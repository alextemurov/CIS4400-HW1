CIS4400 - Homework 1, Alex Temurov, NYC Yellow Taxi Trip Data  

Business Requirements

Every night in New York City, thousands of yellow taxis are picking up and dropping off passengers across the city. A lot of the time though, drivers are sitting idle in areas where theres not much demand, while other areas are packed. These patterns are not random and can actually be tracked through the data.

This project is about building a data warehouse using NYC yellow taxi trip records. The goal is to give taxi operators and city planners a way to better understand how people are moving around the city so they can make smarter decisions.

Three main questions drive this project:

1. When and where is demand the highest? The idea is to find the specific hours, days and locations where taxis are most needed, so dispatchers can position drivers ahead of time instead of reacting after the fact.
2. How do fares change depending on the area? Trips from Midtown to JFK are very different from trips in the Bronx for example. Looking at fare amounts by neighborhood and time of day can reveal patterns that arent always obvious.
3. Are some routes actually faster or just shorter? By looking at average trip duration on different routes we can identify where traffic is consistently causing delays, which is useful for both drivers and urban planners.

Functional Requirements

To make this analysis possible, the system needs to be able to do the following:

1. Pull trip data from the NYC Open Data API
2. Store everything in a structured data warehouse thats clean and easy to query
3. Let users filter the data by date, time, pickup and dropoff location
4. Calculate metrics like average fare, total trips per day and average trip duration
5. Be accessible through a database client like DataGrip or DbSchema


Data Requirements

- Dataset: NYC Yellow Taxi Trip Data
- Source: data.cityofnewyork.us
- How we get it: NYC Open Data API via Socrata
- Size: 100,000+ rows, 15+ columns
- Data Dictionary: link coming soon

Information Architecture

(diagram will be added)

Data comes in from the NYC Open Data API and gets stored in the warehouse. Analysts only interact with the warehouse, not the raw data directly. This way the original source stays untouched and results stay consistent.

Data Architecture

(diagram will be added)

The pipeline has three main steps: pulling the data from the API, cleaning it up, and loading it into the warehouse. The warehouse uses a star schema which basically means one central fact table surrounded by dimension tables. Its a simple structure that makes querying faster and easier to maintain.

Dimensional Modeling

The star schema is built around a single fact table that stores the details of each trip. The dimension tables give context around each trip, like when it happened, where it went, and who was involved.

Fact Table: fact_trips

Fact Table: fact_trips
| Column | Type | Description |
|--------|------|-------------|
| trip_id (SK) | INT | surrogate key |
| date_id (FK) | INT | links to dim_date |
| location_id (FK) | INT | links to dim_location |
| passenger_id (FK) | INT | links to dim_passenger |
| vendor_id (FK) | INT | links to dim_vendor |
| trip_distance | FLOAT | distance of the trip in miles |
| fare_amount | FLOAT | the fare charged |
| tip_amount | FLOAT | tip amount |
| total_amount | FLOAT | total amount charged |
| trip_duration | INT | how long the trip took in minutes |

dim_date: date_id, date, year, month, day, hour, day_of_week

dim_location: location_id, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, borough

dim_passenger: passenger_id, passenger_count

dim_vendor: vendor_id, vendor_name
