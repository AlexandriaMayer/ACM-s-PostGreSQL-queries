## PostGIS queries
# December 2025

# Adding Geometry Columns

`SQL`
```ALTER TABLE stops_cmtx ADD COLUMN geom GEOMETRY(Point, 4326);```

This was for a table of bus stops and their attributes.

# Creating SRID

`SQL`
```UPDATE stops_cmtx SET geom = ST_SetSRID(ST_MakePoint(stop_lon, stop_lat), 4326);```

PostGIS requires a Spatial Reference Identifier (basically the ID of the coordinate system used) in order to run spatial queries.
Compare to the "Project" tool in ArcGIS Pro. MakePoint just asks what we're using for our XY coordinates.

# Creating Spatial Index

`SQL`
```CREATE INDEX stops_cmtx_geo_index ON stops_cmtx USING GIST(geom);```

Creating a spatial index makes the tables perform MUCH better when running queries. From what I've read, GIST() is the right method for most GIS use cases.
