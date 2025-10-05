# Spotify data analysis using SQL
![Spotify_logo](https://github.com/prachisharma52833-art/Spotify_new/blob/main/spotify_logo.jpg)
## Objective
The objective of this project is to analyse Spotify music data using SQL to extract meaningful insights about songs,artists etc.The project aims to demonstrate data analysis skills by writing optimized SQL queries to identify key trends such as the most popular tracks,top artists,, song, duration,etc.


## Dataset
The data for this project is sourced from the kaggle dataset:
-**Dataset Link:**https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset

## Schema

```sql
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
```

## Bussiness Problems and solution

### 1.Retrieve the names of all tracks that have more than 1 billion streams.

```sql
SELECT*FROM spotify
WHERE stream>1000000000;
```
### 2.List all albums along with their respective artists.

```sql
SELECT 
   DISTINCT album,artist
FROM spotify
ORDER BY 1;
```
### 3.Get the total number of comments for tracks where licensed = TRUE.

```sql
SELECT 
   SUM(comments) AS total_comments
FROM spotify
WHERE licensed='true';
```

### 4.Find all tracks that belong to the album type single.

```sql
SELECT *FROM spotify
WHERE album_type='single';
```

### 5.Count the total number of tracks by each artist.

```sql
SELECT 
    artist,
	COUNT(*) AS total_no_songs
FROM spotify
GROUP BY artist
```

### 6.Calculate the average danceability of tracks in each album.

```sql
SELECT 
     album,
	 avg(danceability)AS avg_danceability
FROM spotify
GROUP BY 1
```

### 7.Find the top 5 tracks with the highest energy values.

```sql
SELECT 
     track,
	 avg(energy)
FROM spotify
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

### 8.List all tracks along with their views and likes where official_video = TRUE.

```sql
SELECT 
    track,
	SUM(views)as total_views,
	SUM(likes)as total_likes
FROM spotify
WHERE official_video='TRUE'
GROUP BY 1
ORDER BY 2 DESC
```

### 9.For each album, calculate the total views of all associated tracks.

```sql
SELECT 
   album,
   track,
   SUM(views)
FROM spotify
GROUP BY 1,2
```

### 10.Retrieve the track names that have been streamed on Spotify more than YouTube.

```sql
SELECT*FROM
(SELECT
     track,
	  COALESCE(SUM(CASE WHEN most_played_on='Youtube'THEN stream END),0) as streamed_on_youtube,
	  COALESCE(SUM(CASE WHEN most_played_on='Spotify'THEN stream END),0) as streamed_on_spotify
FROM spotify
GROUP BY 1
) AS t1
WHERE
    streamed_on_spotify>streamed_on_youtube
	AND
	streamed_on_youtube<>0
```

### 11.Find the top 3 most-viewed tracks for each artist using window functions.

```sql
WITH ranking_artist
AS
(SELECT
    artist,
	track,
	SUM(views)as total_view,
	DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views)DESC)AS rank
FROM spotify
GROUP BY 1,2
ORDER BY 1,3 DESC
)
SELECT*FROM ranking_artist
WHERE rank<=3
```

### 12.Write a query to find tracks where the liveness score is above the average.

```sql
SELECT 
    track,
	artist,
	liveness
FROM spotify
WHERE liveness>(SELECT AVG(liveness)FROM spotify)
```

### 13.Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.

```sql
WITH cte
AS
(SELECT
     album,
	 MAX(energy)as highest_energy,
	 MIN(energy)as lowest_energy
FROM spotify
GROUP BY 1
)
SELECT
    album,
	highest_energy-lowest_energy as energy_difference
FROM cte
ORDER BY 2 DESC
```

### Conclusion:
Through this SQL-based analysis,meaningful insights were derived about spotify's music trends.This project demonstrates how SQL can be effectively used to extract , analyze,and interpret large datasets to uncover data-driven insights in the music industry.

