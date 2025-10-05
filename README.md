# Spotify data analysis using SQL
![Spotify_logo](https://github.com/prachisharma52833-art/Spotify_new/blob/main/spotify_logo.jpg)
## Objective
The objective of this project is to analyse Spotify music data using SQL to extract meaningful insights about songs,artists etc.The project aims to demonstrate data analysis skills by writing optimized SQL queries to identify key trends such as the most popular tracks,top artists,, song, duration,etc.


##Dataset
The data for this project is sourced from the kaggle dataset:
Dataset Link:https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset

##Schema

DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes FLOAT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
