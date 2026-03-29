Technical Methodology: Spatial Aggregation in PostGIS
To process the 200,000+ tree records, I performed a spatial join and used the WITHIN GROUP clause to extract neighborhood-level statistics from the raw Open Data.

CREATE TABLE neighborhood_tree_analysis AS
SELECT 
    n.name, n.geom, 
    COUNT(t.id) AS total_trees,
    mode() WITHIN GROUP (ORDER BY t.common_name) AS dominant_species
FROM philly_neighborhoods n
LEFT JOIN ppr_trees t ON ST_Intersects(n.geom, t.geom)
GROUP BY n.name, n.geom;
