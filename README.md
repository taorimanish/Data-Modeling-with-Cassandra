# Data Modeling with Apache Cassandra

- [project rubric](https://review.udacity.com/#!/rubrics/2475/view)


This **Udacity Data Engineering nanodegree** project creates an Apache Cassandra database `sparkifyks` for a music app, *Sparkify*. The purpose of the NoSQL database is to answer queries on song play data. The data model includes a table for each of the following queries:

1. Give me the artist, song title and song's length in the music app history that was heard during  sessionId = 338, and itemInSession  = 4

2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182
    
3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'


## Data pre-processing, ETL pipeline, and data modeling

The data are stored as a collection of csv files partitioned by date. The ETL pipeline and data modeling are written in a single jupyter notebook, **Project_1B_Project_Template.ipynb**.

ETL copies data from the date-partitioned csv files to a single csv file **event_datafile_new.csv** which is used to populate the denormalized Cassandra tables optimised for the 3 queries above. The 3 tables in the model are named after the song play query they are created to solve:


<br>

## Queries

For NoSQL databases, we design the schema based on the queries we know we want to perform. For this project, we have three queries:

1. Find artist, song title and song length that was heard during sessionId=338, and itemInSession=4.
    - `SELECT artist, song, length from table_1 WHERE sessionId=338 AND itemInSession=4`
2. Find name of artist, song (sorted by itemInSession) and user (first and last name) for userid=10, sessionId=182.
    - `SELECT artist, song, firstName, lastName FROM table_2 WHERE userId=10 and sessionId=182`
3. Find every user name (first and last) who listened to the song 'All Hands Against His Own'.
    - `SELECT firstName, lastName WHERE song='All Hands Againgst His Own'`

## Schema

For the first query: 
- Column 1 = sessionId
- Column 2 = itemInSession
- Column 3 = artist
- Column 4 = song
- Column 5 = length
- Primary key = (sessionId, itemInSession)

For the second query:
- Column 1 = userId
- Column 2 = sessionId
- Column 3 = artist
- Column 4 = song
- Column 5 = itemInSession
- Column 6 = firstName
- Column 7 = lastName
- Primary key = (userId, sessionId) with clustering column itemInSession

For the third query:
- Column 1 = song
- Column 2 = firstName
- Column 3 = lastName
- Primary key = (song)

## ETL Pipeline

1. Start with the raw csv data files as described in Dataset
2. For each row of the csv data, insert the data in the appropriate column as described in Schema
3. Perform the Select query as described in Queries
