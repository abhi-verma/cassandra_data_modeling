# Data Modeling with Apache Cassandra

## Purpose 

The project demonstrates data modeling in a Cassandra keyspace for a music streaming company called Sparkify. Raw event data is in CSV format and the files are parsed and then fed into an ETL pipeline to create a keyspace with tables modeled on different queries.


## Dataset

The dataset consists of event_data files which have the following fields:
- artist: string
- auth: string
- firstName: string
- gender: char
- itemInSession: int
- lastName: string
- length: float
- level: string
- location: string
- method: string
- page: string
- registration: float
- sessionId: int
- song: string
- status: int
- ts: float
- userId: int


## Data Model

###### Query 1: Find artist, song title and song length that was heard during sessionId=338, and itemInSession=4.
###### Query 2: Find name of artist, song (sorted by itemInSession) and user (first and last name) for userid=10, sessionId=182.
###### Query 3: Find every user name (first and last) who listened to the song 'All Hands Against His Own'.

The data model for the keyspace is modeled depending on the queries we want to run on the data. Each table is created based on one query.
1. song_history table is created for Query 1 with session_id as Partition Key and item_in_session as Clustering column. A partition will contain rows specific to a session_id and then rows are ordered by item_in_session within each partition.
2. user_details table is created for Query 2 with user_id and session_id as Partition Key and item_in_session as Clustering column. A partition will contain rows specific to a user_id and session_id and then rows are ordered by item_in_session within each partition.
3. user_listened_to_song table is created for Query 3 with song as Partition Key and user_id as Clustering column. A partition will contain rows specific to a song and then rows are ordered by user_id within each partition.


## Process Overview

1. The event_data folder is scanned to get all csv files.
2. Each csv file is parsed row-wise and a data row list is created containing the data from all the csv files.
3. A smaller csv file, event_datafile_new.csv is then created which contains only the rows where artist is not blank 
4. Sparkify keyspace is created and ETL pipeline is built to read the data from the event_datafile_new.csv and insert it into different tables, as per the data model.
5. Queries are run against these tables to validate the results.
