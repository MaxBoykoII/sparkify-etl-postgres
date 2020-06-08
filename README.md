[![CircleCI](https://circleci.com/gh/MaxBoykoII/sparkify-etl-postgres.svg?style=svg)](https://circleci.com/gh/MaxBoykoII/sparkify-etl-postgres)

# Introduction

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They'd like a data engineer to create a Postgres database with tables designed to optimize queries on song play analysis. The purpose of this project is to create a database schema and ETL pipeline for just such an analysis.

## Getting Started

1. Initialize and activate a virtualenv:

   ```sh
   $ cd YOUR_PROJECT_DIRECTORY_PATH/
   $ virtualenv --no-site-packages env
   $ source env/bin/activate
   ```
2. Install the dependencies:
      ```sh
    $ pip install -r requirements.txt
    ```
3. Create a Postgres database named `studentdb` with user `student` and password `student`.
4. (Re)create the `sparkifydb`:
    ```sh
    $ python create_tables.py
    ```
5. Run the etl pipeline:
    ```sh
    $ python etl.py
    ```
6. (Optional) Linting, formatting, and import sorting:
    ```sh
    $ flake8 etl.py create_tables.py
    $ black etl.py create_tables.py
    $ isort etl.py create_tables.py
    ```

## Database Design

To optimize queries on song play analysis, this project implements a star schema design for the `sparkifydb` database, allowing for simplified queries and faster aggregations.

### Fact Table

1. ***songplays*** - records in log data associated with song plays i.e., records with page `NextSong`
          	-  *songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent*

### Dimension Tables
2. ***users*** - users in the app
	- *user_id, first_name, last_name, gender, level*
3. ***songs*** - songs in the music database
	- *song_id, title, artist_id, year, duration*
4. ***artists*** - artists in music database
	- *artist_id, name, location, latitude, longitude*
5. ***time*** - timestamps of records in *songplays* broken down into specific units
	- *start_time, hour, day, week, month, year, weekday*

## Project Structure

```sh
├── README.md
├── .circleci *** Integration with circleci build system
├── data
│   ├── log_data *** JSON logs on user activity on the app
│   ├── song_data *** JSON metadata on the songs available available within the music streaming app   
├── .gitignore
├── create_tables.py *** Script that recreates the sparkify database, dropping existing tables when necessary
├── etl.py *** Script that runs the etl pipeline that populates the sparkify database with the data contained within the data folder
├── requirements.txt *** The dependencies needed to run the project
├── setup.cfg *** Configuration used by flake8 and isort
├── sql_queries.py *** Includes sql queries used by "create_tables.py" and "etl.py"
├── etl.ipynb *** Notebook containing a prototype of the etl pipeline
├── test.ipynb *** Notebook containing sample queries to verify the state of the sparkify database after running "etl.py" or the cells within "etl.ipynb"
```


