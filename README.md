# Introduction
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They want to create a Postgres database with tables designed to optimize queries on song play analysis. In this project, I created a database schema and ETL pipeline for this analysis. There are test scripts provided to test the database and ETL pipeline by running queries developed to validate results that Sparkify expects.

# Data Source
There are 2 datasets, both sourced from the [Million Song Dataset](http://millionsongdataset.com/). Only a subset of the original data has been used for this project. The songs dataset has 71 files and the logs dataset has 30 files.

#### Song Data
![song file sample](images/song_data.jpg)

#### Log Data
![log file sample][(images/log_data.jpg)

# Database Schema
A star schema design was developed, with the songplay table as fact table.
#### Songplay table (fact)
* songplay_id serial primary key
* start_time timestamp not null
* user_id int not null
* song_id varchar
* artist_id varchar
* session_id int
* location varchar
* user_agent text

#### Songs table (dimension)
* song_id varchar primary key
* title varchar not null
* artist_id varchar
* year int
* duration decimal not null

#### Users table (dimension)
* user_id int primary key
* first_name varchar
* last_name varchar
* gender varchar
* level varchar

#### Artists table (dimension)
* artist_id varchar primary key
* name varchar not null
* location varchar
* latitude double precision not null 
* longitude double precision not null

#### Time table (dimension)
* start_time time 
* hour int
* day int
* week int
* month int
* year int
* weekday int

# Running the Scripts

`create_tables.py` is used to delete and recreate the database and tables. This is useful for making modifications on a global scale.
`etl.py` will run the etl pipeline to fetch all files, transform and load into the database tables created with `create_tables.py`.
`sql_queries.py` contains SQL queries used in the etl pipeline as well as for validation.
`etl.ipynb` is a development workbook used to create and verify the etl pipeline on a smaller scale.
`test.ipynb` is used to verify final run of the etl. Sanity checks here assess constraints and data types.

# Error Handling
In the songplay table, I set songplay_id to serial type so that it auto increments with every insert. However, for other tables apart from user table, I used the postgres upsert consert to avoid failure if a record already exists during insert. The clause `on conflict [column] do nothing` was added to handle this.

In the user table, there's a possibility that a user level may change between free and paid. Where there's an insert conflict, I have handled it using `on conflict(level) do update set level=EXCLUDED.level`
