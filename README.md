# MTC Modeling Code Challenge

Locate freeway to freeway interchanges. 

## Problem Statement

You are given a simplified version of a travel model network of the Bay Area. Thisnetwork is made up of links and nodes. Each link can beuniquely identified by itsA node and B node, with trafficflowing from A to B. Each link is also characterized by itsfacility type (FT). The facility type definitions are shown in the table below:

|Facility Type    |FT   |
|-----------------|-----|
|Freeway|1|
|Expressway|2|
|Ramp|3|
|Divided Arterial|4|
|Undivided Arterial|5|
|Collector|6|
|Local|7|

Two different formatsof the network are given to you: (i) a csv version, which gives you all you need to complete this exercise; and (ii) a shapefile version, as you may find it helpful to have the network visualized.In this exercise, we would like you to write a script, either in R or in python, to identify all the freeway-to-freeway interchange nodes in the network. Such interchanges are defined as: any node X where there is a freeway link ending in X1 followed by one or more ramp links ending in X2, followed by another freeway link. Here, both X1 and X2 are interchange nodes.

![img](img/fwy_interchange_example.png)

The script should outputa csv file with a single column listing all freeway-to-freeway interchangenodes. Please submit your scriptand the output file(and any visualization you may want to share). We are interested in the following:

- Does the code work?
- Are the results accurate?
- Is the code easy to read and maintain?

## Solution

[Find Freeway Interchanges](Find_Freeway_Interchanges.ipynb)

### Methodology

1. Create network geodataframe

For this step, one function was defined called `create_network_geodataframe(file_path,crs=2230)`. The function accepts a csv and returns a geodataframe with a default coordinate reference system of [EPSG 2230](https://epsg.io/2230).  


2. Find intersections using geopandas overlay

For this step, one function was defined called `find_freeway_network_interchanges(freeway_nw_gdf,ramp_nw_gdf)`. The function accepts two network datasets: a freeway network dataset and a ramp dataset and returns a geodataframe with only point geometries. The intersections data is filtered to include only point geometries, and to only include interchanges with are either the end of a ramp or the start of a ramp. Ramp types are also added. Ramps are classified as 'on' ramp if A_1 = B_2, and 'off' if B_1 = A_2. 

3. Determine if a intersection node is a freeway to freeway node

For this step, two functions were was created called `is_node_connected_to_freeway(row,nw_gdf)` and `flag_freeway_interchanges(interchanges,freeway_nw_gdf)`. 

**`is_node_connected_to_freeway(row,nw_gdf)`**

This function uses recusion to traverse the freeway network, using an interchange node geodataframe. The function takes accepts a row as a parameter from the interchange dataset, using the 'RAMP_TYPE' attribute to determine the direction of the ramp. 

The network is traversed backwards or forwards depending on ramp direction. If "on", the function looks backwards at network links using the interchange node 'A' as the starting point. If the ramp direction is "off", the function looks forward using the interchange node 'B' as the starting point. 

Base cases include the following:
- Check if ramp is connected to any other links. Return False if not, meaning that the link is not connected to a freeway. 
- If ramp is connected to another link, check the link facility type. If 'FT' == 1, return True meaning that the ramp link is an freeway interchange. 
- If ramp is not connected to another ramp link of 'FT' == 3, return False meaning that the ramp link is not a freeway interchange. 
    
Recursive case includes the following:

 If the ramp is connected to another ramp link, check all ramp links (backwards if on ramp, forward if off) using recusion to determine if they are connected to a freeway and return True if connected to a freeway link. Also uses a list to remember which links were already looked at. Solves for cases where ramp link is both an on ramp and an off ramp and cases where off ramp and on ramp links intersect, which would cause an infinate looping backwards or forwards.

**`flag_freeway_interchanges(interchanges,freeway_nw_gdf)`**

This function iterates over the interchange geodatagrame created in previous steps, and passes rows to the `is_node_connected_to_freeway(row,nw_gdf)` function as well as the network geodataframe created in previous steps. The function returns a geodataframe containing a flagged column indicating whether or not the interchange is a valid freeway to freeway interchange. 

This step also outputs the valid freeway to freeway interchange results as a csv with one column, which is a wkt geography column. [See Outputs](#outputs)

4. Visualize Results

Results are visualized on an interactive map using the Folium library. The results are also saved as a html and [hosted on a github site for exploration](https://joshuacroff.github.io/MTC-Modeling-Code-Challenge/network_interactive_map). 

### Inputs

Simple Network CSV (Private Link)

### Outputs

- [Freeway Interchanges CSV](freeway_interchanges.csv)
- [Freeway Interchanges GeoJSON (EPSG:4326)](freeway_interchanges.geojson)
- [Interactive Map of Freeway Interchanges and Freeway Ramp Network](https://joshuacroff.github.io/MTC-Modeling-Code-Challenge/network_interactive_map)


 
