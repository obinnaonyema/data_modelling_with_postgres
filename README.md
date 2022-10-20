# Introduction
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They want to create a Postgres database with tables designed to optimize queries on song play analysis. In this project, I created a database schema and ETL pipeline for this analysis. There are test scripts provided to test the database and ETL pipeline by running queries developed to validate results that Sparkify expects.

# Data Source
There are 2 datasets, both sourced from the [Million Song Dataset](http://millionsongdataset.com/). Only a subset of the original data has been used for this project. The songs dataset has 71 files and the logs dataset has 30 files.
Set songplay_id to serial type so that it auto increments with every insert