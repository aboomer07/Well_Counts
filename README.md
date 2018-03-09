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
