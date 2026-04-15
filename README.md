CIS4400 - Homework 1, Alex Temurov, NYC Yellow Taxi Trip Data  

Business Requirements

Every day in New York City, thousands of yellow taxis transport people citywide. However, often drivers sit idle in low-demand areas while others are crowded, these are patterns that can  be tracked through the data.

This project focuses on designing a data warehouse for NYC yellow taxi trip records and optimizing taxi operations and city planning. The primary objective is to equip taxi drivers and city planners with valuable insights into citywide mobility patterns for more accurate planning and operations.

Three are three main questions for this project:

The primary focus is: When and where does taxi demand is the highest? The goal is to identify the specific hours, days, and locations where taxis are most needed for the regular people, so dispatchers can position drivers in advance rather than reposition themselves after or during the busy hours. How do prices change depending on the area? Trips from Midtown to JFK are very different from trips in the Bronx for example. Looking at fare amounts by neighborhood and time of day can reveal patterns that arent always obvious,
and finally are some routes actually faster or just shorter? By looking at average trip duration on different routes we can identify where traffic is consistently causing delays, which is useful for both drivers and city planners.

Functional Requirements

To make this analysis possible, the system needs to be able to do the following:

Pull trip data from the NYC Open Data API, store everything in a structured data warehouse thats clean and easy to query, let users filter the data by date, time, pickup and dropoff location, calculate metrics like average fare, total trips per day and average trip duration and be accessible through a database client like DataGrip or DbSchema

Data Requirements

- Dataset: NYC Yellow Taxi Trip Data
- Source: data.cityofnewyork.us
- How we get it: NYC Open Data API via Socrata
- Size: 100,000+ rows, 15+ columns
- Data Dictionary: https://docs.google.com/spreadsheets/d/1cMTGQjr-mmM-cfJAtE5sfrETlCic-Q2KVG0O6Gwc7Yo/edit?usp=sharing

Information Architecture

![Information Architecture](Untitled%20Diagram.drawio.png)

Data comes in from the NYC Open Data API and gets stored in the warehouse. Analysts only interact with the warehouse, not the raw data directly. This way the original source stays untouched and results stay consistent.

Data Architecture

![Data Architecture](Untitled%20Diagram.drawio%20(1).png)

The pipeline has three main steps: pulling the data from the API, cleaning it up, and loading it into the warehouse. The warehouse uses a star schema which basically means one central fact table surrounded by dimension tables. Its a simple structure that makes querying faster and easier to maintain.

Dimensional Modeling

The star schema is built around a single fact table that stores the details of each trip. The dimension tables give context around each trip, like when it happened, where it went, and who was involved.

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
