# PostGIS queries
## December 2025

### Creating a table
`SQL`

`CREATE TABLE IF NOT EXISTS shapes_cmtx(
  shape_id TEXT,
	shape_pt_lat NUMERIC,
	shape_pt_lon NUMERIC,
	shape_pt_sequence INTEGER,
	shape_dist_traveled NUMERIC
);`

### Adding a primary key after the creation of the table.

Claude suggested a composite primary key, as the result is unique and relies on the existing GTFS data.

`SQL`

`ALTER TABLE shapes_cmtx ADD PRIMARY KEY (shape_id, shape_pt_sequence)`


### Adding Geometry Columns

`SQL`

```ALTER TABLE stops_cmtx ADD COLUMN geom GEOMETRY(Point, 4326);```

This was for a table of bus stops and their attributes.

### Creating SRID

`SQL`

```UPDATE stops_cmtx SET geom = ST_SetSRID(ST_MakePoint(stop_lon, stop_lat), 4326);```

PostGIS requires a Spatial Reference Identifier (basically the ID of the coordinate system used) in order to run spatial queries.
Compare to the "Project" tool in ArcGIS Pro. MakePoint just asks what we're using for our XY coordinates.

### Creating Spatial Index

`SQL`

```CREATE INDEX stops_cmtx_geo_index ON stops_cmtx USING GIST(geom);```

Creating a spatial index makes the tables perform MUCH better when running queries. From what I've read, GIST() is the right method for most GIS use cases.
