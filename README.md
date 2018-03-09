# Count of Oil and Gas Wells in the US by County

## Explanation of Project

While working as an intern for an environmental economics organization, one of the projects I was a part of was a rangeland/cropland conservation project throughout several US states (Texas, Wyoming, New Mexico, Oklahoma, Colorado, Kansas, and Nebraska). The level of granularity for the project was at the county level, and we needed a way to differentiate the counties based on their oil and natural gas output.

The first way I did this was by finding production amounts by county for oil and natural gas, which were readily available online. After completing this, I was also asked to verify/support this approach by also figuring out the total number of oil and natural gas wells per county.

While this projet has already been completed and visualized and completed on the first website below, the data from that website is raw and only labels the wells by Latitude and Longitude, without their corresponding county name. Because of that, my approach to labeling the wells with their county name and aggregating that is layed out below. The total number of wells was 1.7 million, which, as shown below, put a constraint on the realistic methods I could use to assign the correct county names to each latitude/longitude

## Data Sources

I accessed the well data from this website: https://www.fractracker.org/2015/08/1-7-million-wells/
It has a listing of all the wells in the United States, excluding offshore, North Carolina, and Texas

To get the data for Texas, I went to this site: http://www.rrc.state.tx.us/oil-gas/research-and-statistics/well-information/well-distribution-by-county-well-counts/

The well data for Texas was already aggregated by county, so it is not included as part of assigning the county names to the latitudes and longitudes.

## Process to Aggregate the Data

It took a little bit of trial and error to find the best way to get the county information from the Lat/Longs

I found a python library that could do it with something called Noominatim from open street map, but it had a request limit that was prohibitive for the amount of entries I had. The process of using Noominatim to query the GEOID information is not included in this notebook, but it is esentially the same as querying the US Census website

I then found the Census API where you can use a website link with specific Lat and Longs inputted to return a JSON of descriptive data about the location, including county name and GEOID. This method was faster, but still too slow to process the entire dataset.

So my next step was to download a dataset of Latitude/Longitude pairs with associated county information from the US Census into a text file. These Lat and Long pairs that had GEOID labels could be used to to train a K-Nearest Neighbors algorithm to predict the remainder of the un-labeled Lat/Longs. I chose K-Nearest Neighbors as opposed to another algorithm because the input feature set was two dimensional, and the problem itself was inherently spatial, so I thought it would be a good fit.

*As a note, the part of this project that is not included in this notebook is the final solution I used to get the county names. I used QGIS along with a shapefile which had polygon boundaries of each county, so that a specific Lat/Long point could be place within a certain county. This is important, as the nearest neighbor algorithm could get it wrong if there is not a high enough density of points along the border between two counties, and the closest point is within the incorrect county*

## Methodolgy and High-Level Explanation of Notebook

While the final method used in QGIS to get the GEOID information from the Lat/Longs was used instead of the K-Nearest Neighbors algorithm, the notebook below shows my process. After first setting aside a testing set to test the KNN algorithm on, and seeing where it failed, I was interested to see how many new points it would take (by querying the census API) to get to a certain acceptable level of accuracy with the KNN algorithm. I created some functions to do this (as well as using the census API to generate testing data), and after a couple of iterations, realized it was prohibitively slow.

*With that, the file I save below called 'GEOIDS_Progress.csv' is just a checkpoint so that I can shutdown the notebook and keep the extra labeled Lat/Longs that I had already queried from the census API and not have to start over with redefining the original versions of each of the dataframes.*

*The file: 'wells_original.csv' is just an aggregation of the 3 raw data files taken from the website, trimmed down to just the states I was looking at for this project.*

*The file: 'Original_Census_Data.csv' is the original text file with Lat/Longs and GEOID information taken from the census website, trimmed down to just the states in this project.*

*The file: 'Data_for_GIS.csv' is the well data trimmmed down to just the Lat/Longs, the state code, and the state abbreviation (with duplicates removed), to be fed into QGIS along with the county shapefile.*

*The file: 'predicted.csv' has the predicted county ID's from the KNN model for each Lat/Long combination.*
