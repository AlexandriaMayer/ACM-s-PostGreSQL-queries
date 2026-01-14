# PostGIS Queries: Jan 2026

## Creating buffers

`SQL`

`CREATE TABLE cmtx_400m_stop_buffer AS
SELECT 
stop_id,
stop_name,
ST_Buffer(geom::geography, 400)::geometry as geom
FROM stops_cmtx;`

What happened: buffers are going to be a new feature class, so it's a new table. AS to specificy what's in the new table.

geom[etry] is cast as geography for more accurate calculations, then cased back into geom[etry] for QGIS compatibility reasons.
