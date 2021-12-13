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

### Inputs

[Simple Network CSV](simple_network.csv)

### Outputs

- [Freeway Interchanges CSV](freeway_interchanges.csv)
- [Freeway Interchanges GeoJSON](freeway_interchanges.geojson)
- [Interactive Map of Freeway Interchanges and Freeway/Ramp Network](https://joshuacroff.github.io/MTC-Modeling-Code-Challenge/network_interactive_map)


 
