### Project 3 - Data Warehouse

#### Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, I will be building an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. I tested my database and ETL pipeline by running test queries and compared my results with the expected results.

#### Project Datasets

The two datasets that reside in S3 with links to each below:

    Song data: s3://udacity-dend/song_data
    Log data: s3://udacity-dend/log_data

Log data json path: s3://udacity-dend/log_json_path.json

#### Song Dataset

The first dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

#### Log Dataset

The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings.

The log files in the dataset are partitioned by year and month. For example, here are filepaths to two files in this dataset.

`log_data/2018/11/2018-11-12-events.json`
`log_data/2018/11/2018-11-13-events.json`

And below is an example of what the data in a log file, 2018-11-12-events.json, looks like.

![Schema](https://video.udacity-data.com/topher/2019/February/5c6c3ce5_log-data/log-data.png)

### Redshift staging tables

The song data is loaded from s3 into `staging_songs` and the log data is loaded into `staging_events`.
Below is a diagram of the two staging tables 

![alt text](staging_tables.png)

#### Schema for Song Play Analysis

Using the song and event datasets, the following star-schema was created for songplay analysis with the tables listed below.  Data for these tables are loaded from the 2 staging tables, namely `staging_songs` and `staging_events`

Fact Table

    1. songplays - records in event data associated with song plays i.e. records with page NextSong
        songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

Dimension Tables

    2. users - users in the app
        user_id, first_name, last_name, gender, level
    3. songs - songs in music database
        song_id, title, artist_id, year, duration
    4. artists - artists in music database
        artist_id, name, location, lattitude, longitude
    5. time - timestamps of records in songplays broken down into specific units
        start_time, hour, day, week, month, year, weekday

![Schema](https://udacity-reviews-uploads.s3.us-west-2.amazonaws.com/_attachments/38715/1599555988/Song_ERD.png "Star Database Schema")

### Project Template

The following files python files are included in the repository for this project.

- `sql_queries.py` - SQL statement definitions for creating and inserting data staging tables plus the fact and dimensions on Redshift.  This file will be imported into the two other files below.
- `create_table.py` -  Runs the commands inside `sql_queries.py` to create the necessary tables in Redshift.
- `etl.py` - Runs the actual ETL process where it loads the data from S3 into staging tables on Redshift and then process that data into the analytics tables on Redshift.
- 'README.md' - Readme file with information about this project

### Running files for this project

Before running the above python scripts, the Redshift cluster will need to be created first with the propoer database and access credentials and the appropriate IAM role defined.  After this step is completed, the 3 python scripts above are to be executed from the command prompt in the order below.

`python sql_queries.py`

`python create_tables.py`

`python etl.py`


### Sample Queries

`select count (*) from songplays`

`select count (*) from users`

`select count (*) from songs`

`select count (*) from artists`

`select count (*) from time`