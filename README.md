
# Analyzing Spotify Dataset Using SQL
![logo](https://github.com/Mgit125/Spotify-SQL-Prject/blob/main/Picture1.jpg)

### Project Overview
This project analyzes a publicly available Spotify dataset containing various attributes of tracks, albums, and artists. The aim is to derive insights related to artist popularity, song duration, audio features, and listener engagement through advanced SQL queries and data analysis techniques.

### Dataset Overview
Spotify Dataset on Kaggle : https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset

#### Key Features:
Artist: Name of the artist.

Track: Name of the song.

Album: Album containing the track.

Duration_min: Length of the song in minutes.

Danceability, Energy, Loudness: Various audio features.

Stream, Views, Likes, Comments: Listener engagement metrics.


### Step-by-Step Process for Analyzing the Spotify Dataset
#### 1. Data Understanding
Objective: Explore the structure and key features of the dataset.
Key Features: Track, artist, album, duration, popularity, audio features.
#### 2. Data Cleaning and Preparation
Handle missing values and remove duplicates.
Standardize formats for consistency.
Perform descriptive statistics on numerical columns.
#### 3. Data Normalization
Objective: Optimize query performance.
Process: Transform denormalized tables into a normalized structure by creating separate tables for artists, albums, and tracks.
#### 4. Exploratory Data Analysis (EDA)
Conduct bivariate analysis (e.g., Popularity vs. Tempo).
Analyze trends based on release years.
Use multivariate analysis to explore factors influencing song popularity.
#### 5. Data Exploration
Gain insights into artist trends and listener behavior.
Count tracks per artist and analyze release years.
#### 6. SQL Queries
Easy Queries: Count total tracks, albums, and unique artists.
Intermediate Queries: Analyze top artists based on popularity.
Advanced Queries: Analyze song duration and its impact on popularity.
#### 7. Key Business Problems to Solve
Identify top artists based on the number of songs and popularity.
Analyze how song length impacts popularity.
Study the effects of collaborations on track performance.
#### 8. Visualization & Insights
Tools: Use Power BI or Tableau for visualization.
Charts: Total Sales by Fat Content (Donut chart), Most Played Tracks (Bar Chart), Stream Trends by Month (Line Chart).
Insights: Provide actionable insights for music industry stakeholders.
#### 9. Optimization Techniques
Implement indexing on frequently queried columns (e.g., artist, track).
Optimize query performance using JOIN operations and CTEs.
#### 10. Real-Life Applications
Music Recommendations: Build a recommendation system based on popularity and genre.
Business Decision Support: Provide insights into optimizing song releases for record labels.

## Schema

```sql
CREATE TABLE Spotify (

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
	likes BIGINT,
	comments BIGINT,
	licensed BOOLEAN,
	official_video BOOLEAN, 
	stream BIGINT,
	energy_liveness FLOAT,
	most_played_on VARCHAR(100)
);

```



### Analysis

#### Easy Level Questions
1. Top 5 Songs by Energy
```sql
SELECT track, energy FROM Spotify
ORDER BY track DESC
LIMIT 5;
```

2. Most Played Songs by Platform
```sql
	----------------- Using window function : ROW_NUMBER()	

WITH RANKED_TRACKERS_PLATFORM_wise AS (

	SELECT
		track,
		views,
		most_played_on,
		ROW_NUMBER() OVER ( PARTITION BY most_played_on ORDER BY views DESC) AS max_view_rank
	FROM Spotify
	WHERE 
		most_played_on IN ('Spotify', 'Youtube')
)
SELECT
	track,
	views,
	most_played_on
FROM RANKED_TRACKERS_PLATFORM_wise
WHERE 
	max_view_rank =1;

```
3. Duration and Popularity of Songs by Artist
 ```sql
								----------------- Using window function : ROW_NUMBER()				

WITH POP_ARTIST_DURATION AS (
	SELECT 
		artist,
		track,
		duration_min,
		stream,
		ROW_NUMBER() OVER (PARTITION BY artist ORDER BY stream DESC) AS popu_artist_rank,
		ROW_NUMBER() OVER (PARTITION BY artist ORDER BY duration_min DESC) AS dur_artist_rank
	FROM Spotify
)
SELECT
	artist,
	track,
	duration_min,
	stream
FROM POP_ARTIST_DURATION
WHERE
	popu_artist_rank = 1 OR dur_artist_rank =1
ORDER BY duration_min DESC, stream DESC
LIMIT 5;

 ```
4. Average energy_liveness & Average liveness by each Track
```sql
SELECT 
    track,
    AVG(energy_liveness) AS avg_energy_liveness,
    AVG(liveness) AS avg_liveness
FROM Spotify
GROUP BY track
ORDER BY avg_energy_liveness DESC, avg_liveness DESC
LIMIT 10;
```
5. Songs with Highest Danceability and Acousticness
```sql
			---------------- Using window function
WITH high_dan_aco AS (
	SELECT 
		track,
		danceability,
		acousticness,
		ROW_NUMBER() OVER (ORDER BY danceability DESC) AS high_dan_rank,
		ROW_NUMBER() OVER (ORDER BY acousticness DESC) AS high_aco_rank
	FROM Spotify
)
SELECT 
	track,
	danceability,
	acousticness	
FROM high_dan_aco
WHERE
	high_dan_rank = 1 OR high_aco_rank =1;
```
6. Retrieve tracks with more than 1 billion streams
```sql
SELECT 
	track,
	stream
FROM Spotify
WHERE
	stream > 1000000000
ORDER BY stream DESC;
```
7. List all albums along with respective artists
```sql
SELECT 
	album,
	artist
FROM Spotify;
```
8. Get the total number of comments for tracks where licensed = TRUE.
```sql
SELECT 
		track,
		SUM(comments) AS total_comments
	FROM Spotify
	WHERE
		licensed = TRUE
	GROUP BY 1
	ORDER BY 2 DESC;
```
9. Find all tracks that belong to the album type single.
```sql
	SELECT 
		track
	FROM Spotify
	WHERE
		album_type = 'single';
```

10. Count the total number of tracks by each artist.
```sql
	SELECT 
		artist,
		COUNT(track) AS total_no_of_songs_artist
	FROM Spotify
	GROUP BY artist;
```
11. Total number of tracks in the dataset
```sql
	SELECT 
		COUNT(track) 
	FROM Spotify;
```
12. Distinct artists in the dataset
```sql
	SELECT 
		DISTINCT(COUNT(artist)) 
	FROM Spotify;

```
13. Top 10 most popular songs
```sql
	SELECT 
		track,
		stream
	FROM Spotify
	ORDER BY stream DESC
	LIMIT 10;
```
14. Total number of albums in the dataset
```sql
	SELECT 
	COUNT(album) AS total_albums
	FROM Spotify
```
15. Tracks with duration greater than 5 minutes
```sql
	SELECT
		track,
		duration_min
	FROM Spotify
	WHERE
		duration_min > 5
	ORDER BY duration_min DESC;
```
16. List all songs by a specific artist
```sql
	SELECT
		artist,
		track
	FROM Spotify
	WHERE
		artist = 'Gorillaz'
	ORDER BY artist;

```
#### Medium Level Questions

1. Average danceability of tracks in each album
   
2. Top 5 tracks with the highest energy values
   
3. Tracks along with views and likes where official_video = TRUE
 
4. Total views of all tracks per album
 
5. Track names streamed more on Spotify than YouTube
 
6. Total Views and Likes by Platform
 
7. Songs with High Popularity and Low Acousticness

8. Top 5 Most Lively Songs by Track
 
9. Top 5 artists with the most songs
   
#### Advanced Level Questions

1. Top 5 Artists by Total Stream Count for Official Videos

2. Songs with the Highest Difference Between Views and Likes

3. Correlating Popularity (stream) and Danceability Across All Songs

4. Most played tracks in specific channels (top 10)

5. Cluster songs based on audio features (e.g., danceability, energy)


### Conclusion
This structured approach allows for comprehensive analysis of the Spotify dataset, leading to meaningful insights into music trends, artist performance, and listener behavior. By leveraging SQL and visualization tools, this project aims to provide valuable recommendations for stakeholders in the music industry.


### Author - Srinivas
This project is a significant component of my portfolio, demonstrating my proficiency in SQL, which is essential for data analyst roles. It showcases my ability to write complex queries, manage and manipulate databases, and extract meaningful insights from data.

### Contact
If you have any questions, feedback, or if you're interested in discussing potential collaborations, please feel free to contact me. I'm eager to connect and explore opportunities to leverage data for strategic decision-making.


