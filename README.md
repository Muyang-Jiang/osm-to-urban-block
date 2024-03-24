# osm-to-urban-block

This project aim to delineate urban blocks using OpenStreetMap road network and land-cover/land-use data. It is being conducted in major cities in China, with potential for expansion to cities worldwide.

![Example of blocks Xi'an, China](pic "https://github.com/Muyang-Jiang/osm-to-urban-block/tree/main/pics/blocks-Xi_an.png")

## Environment Requirement

This project is developed using the ArcGIS Pro Model Builder, which is subsequently converted into Python code through ArcPy. Therefore it is recommended to either install ArcGIS Pro (version 3.1.0 or later) to directly use the toolbox, or to import ArcPy into your Python environment and then run the python code.

## Data

- OSM road network (Shapefile)
- Land-use/Land-cover (Raster)
- OSM Water surface cover (Shapefile)
- Administrative boundaries (Shapefile)

If you are interested please DM and I will share with the data for China.

## How the tool works

In this project, urban blocks are defined as polygons delimited by major roads. The steps are:

1. **Selecting major roads** by attribute "fclass". When delineating urban blocks, roads classified as *footway, residential, service, steps, living street, path, cycleway and track* are excluded because they are considered as roads within blocks.  The classification of *pedestrian and unclassified* roads is left to the preference of the user, depending on whether they prefer the resulting blocks to be at a finer or coarser level.
2. **Preprocessing road network data** to make it optimal for block delineation, which involves: 
    1. Extending roads by 20 meters using "Extent line" tool to connected roads, ensuring block enclosure. 
    2. Removing dead-end streets.
3. **Erasing** processed road network from the city polygon defined by the administrative boundary. This results in primary blocks.
4. **Filtering and processing of blocks**. This include:
    1. Erasing water surfaces classified as *riverbank, reservoir and wetland* using OSM Water surface cover data*.*
    2. Buffering roads to simulate road spaces and erase them from the blocks. The buffering distance varies based on the class of roads: road classified as *primary, motorway and trunk road* are buffered with 6 meters, p*edestrian, tertiary* and *unclassified* roads are not buffered, and all other roads are buffered with 3 meters. 
    3. merging small blocks (default value is one hectare) with the neighboring block with the longest shared border using "Eliminate" tool. To be safe this step is conducted twice.
    4. Calculate the built-up ratio of blocks using Land-use/Land-cover data and select blocks where over certain percentage (depending on the city, default value is 20%) of the area are built.


    

## How to use the ArcGIS Pro tool

1. Create a new project in ArcGIS Pro and copy the toolbox to the folder.
2. Within the ArcGIS Pro Catalog view, access the toolbox where you will find the model titled "OSM Road to Block". Right-click on the model, opt to "open", and navigate to adjust parameters, select the input data source and choose output path within the right panel.