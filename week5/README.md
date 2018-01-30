# MUSA-620-Week-5

Intro to Databases / NYC Taxi data with Google BigQuery ([notes](https://github.com/MUSA-620-Fall-2017/MUSA-620-Week-5/blob/master/week-5-big-datasets.pptx))

![Distribution of NYC Taxi Pickups](https://blueshift.io/nyctaxipickups.png "Distribution of NYC Taxi Pickups")

##### Links from class:
- [The BigQuery project we used in class](https://bigquery.cloud.google.com/welcome/bigquery-158617?pli=1)
- [Google Cloud storage for the project](https://console.cloud.google.com/storage/browser?project=bigquery-158617)
- [BigQuery SQL reference](https://cloud.google.com/bigquery/docs/reference/legacy-sql)
- [NYC Taxi trip data](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml)

##### Some interesting projects based this data:
- [Passenger Privacy in the NYC Taxicab Dataset](https://research.neustar.biz/2014/09/15/riding-with-the-stars-passenger-privacy-in-the-nyc-taxicab-dataset/)
- [A Day in the Life of an NYC Taxi Driver](http://chriswhong.github.io/nyctaxi/)
- [Analyzing 1.1 Billion NYC Taxi and Uber Trips, with a Vengeance](http://toddwschneider.com/posts/analyzing-1-1-billion-nyc-taxi-and-uber-trips-with-a-vengeance/)

## Assignment

This assignment is not required. You may turn it in by email (galkamaxd at gmail) or in person at class.

**Due:** by the end of class next week, 22-Feb

### Task:

- **Option 1**: How does the usage of NYC Taxis look when you adjust for population differences?  Using population data from the U.S. Census, color each census tract according to the number of taxi pickups normalized by population.
- **Option 2**: How long does it take to get to JFK Airport from different parts of New York? For each Census tract, aggregate all the taxi trips that went to JFK Airport and color the map according to the average trip duration.

### Deliverable: A choropleth map, similar to the one we made in class.

- **Option 1:** For this assignment, you will need to create a "GeoID" (county code + tract code) to join the census data to the shapefile. The shapefile posted here does not give the county code, but it does give the NYC borough, which is equivalent. You can convert between boroughs and counties using this table.

| Borough	| Borough Code | County | County Code |
|-----|------|-------|-------|
|Bronx|2|Bronx County|5|
|Brooklyn	|3	|Kings County	|47|
|Manhattan	|1	|New York County	|61|
|Queens|	4|	Queens County	|81|
|Staten Island|	5	|Richmond County	|85|

Some Census tracts may have very small or 0 population. It's up to you how to deal with those cases (e.g. leave them in, throw them out, identify them separately on the map), whatever you feel is most relevant given the purpose.


- **Option 2:** For the second assignment option, you will need to add two additional pieces to your query (see below). It must be restricted to include only taxi trips going to JFK Airport and it must return information about the trip duration.

> SELECT ROUND(Pickup_latitude, 3) AS lat, ROUND(Pickup_longitude, 3) AS lon, COUNT( * ) AS num_trips, SUM(TIMESTAMP_TO_SEC(TIMESTAMP(tpep_dropoff_datetime))-TIMESTAMP_TO_SEC(TIMESTAMP(tpep_pickup_datetime))) AS total_duration_in_seconds

> FROM [TaxiTrips.yellow_taxi_jun]

> WHERE Dropoff_longitude > -73.823 AND Dropoff_longitude < -73.749 AND Dropoff_latitude > 40.618 AND Dropoff_latitude < 40.667

> GROUP BY lat, lon

Using a spatial join, you can sum up the total trip time (total_duration_in_seconds) and the number of trips (num_trips) for each Census tract. Dividing one by the other will give the average time it takes to drive from each Census tract to JFK Airport.
