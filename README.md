CIS4400 - Homework 1

Student: Alex Temurov Dataset: NYC Yellow Taxi Trip Data

---

Business Requirements

Each night in New York City, thousands of yellow taxis traverse the grid, picking up and dropping off passengers, or remaining idle in less optimal locations while demand exists nearby. These inefficiencies are not random; they follow discernible patterns that are evident in the data.

This project involves building a data warehouse from NYC yellow taxi trip records. The objective is to provide taxi operators and city planners with a clear, queryable representation of passenger movement, enabling proactive vehicle positioning to efficiently meet demand.

Three questions drive everything:

1. When and where is demand highest? The analysis aims to identify specific intersections, hours, and days that experience increased demand, such as during inclement weather or peak times. This information enables dispatchers to position drivers proactively, rather than reactively.
2. How do fares vary across the city? For example, trips from Midtown to JFK differ significantly from those between Inwood and the Bronx. Analyzing fare amounts by neighborhood and time of day reveals pricing patterns that may not be apparent to drivers or passengers.
3. Are certain routes genuinely faster, or merely shorter in distance? Analyzing average trip duration by corridor identifies areas where traffic consistently causes delays. This information is valuable for optimizing routing and for broader urban analysis.

---

Functional Requirements

To enable this analysis, the system must reliably perform five key functions:

1. Pull live trip data directly from the Store and store it in a structured warehouse that is organized, clean, and easily queryable.
2. Enable users to filter data by date, time, and pickup or dropoff location without accessing the raw source.
3. Generate key metrics, including average fare, total daily trips, and average trip duration. Stay accessible through standard database clients — DataGrip, DbSchema, whatever the analyst already has open

The primary value lies in the data structure rather than in specialized tooling.

---

Data Requirements

* Dataset: NYC Yellow Taxi Trip Data
* Source: data.cityofnewyork.us
* Ingestion method: NYC Open Data API via Socrata
* Scale: 100,000+ rows, 15+ columns
* Data dictionary: (link coming soon)

---

Information Architecture

(diagram will be added)

Data is ingested from the NYC Open Data API and stored in the warehouse. Analysts access and query data exclusively through the warehouse, ensuring the raw source remains unaltered and analyses remain consistent.

---

Data Architecture

(diagram will be added)

The data pipeline consists of three main stages: API extraction, data cleaning, and warehouse loading. The warehouse employs a star schema, featuring a central fact table surrounded by dimension tables. This familiar structure facilitates efficient querying and simplifies maintenance.

---

The star schema is organized around a single fact table that records the details of each trip. Dimension tables provide contextual information, addressing aspects such as who, when, and where.n, or where.

Fact Table: fact_trips

Column	Type	Description	Column 4
trip_id (SK)	INT	Surrogate key	
date_id (FK)	INT	Links to dim_date	
location_id (FK)	INT	Links to dim_location	
passenger_id (FK)	INT	Links to dim_passenger	
vendor_id (FK)	INT	Links to dim_vendor	
trip_distance	FLOAT	Distance in miles	
fare_amount	FLOAT	Base fare charged	
tip_amount	FLOAT	Tip amount	
total_amount	FLOAT	Total charged	
trip_duration	INT	Duration in minutes	


dim_date — date_id, date, year, month, day, hour, day_of_week

dim_location — location_id, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, borough

dim_passenger — passenger_id, passenger_count

dim_vendor — vendor_id, vendor_name
