
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
4. Duration and Popularity of Songs by Artist
  ```sql
   ```
5. Average energy_liveness & Average liveness by each Track
  ```sql
   ```
6. Songs with Highest Danceability and Acousticness
  ```sql
   ```
7. Retrieve tracks with more than 1 billion streams
  ```sql
   ```
8. List all albums along with respective artists
  ```sql
   ```
9. Total number of comments for licensed tracks
  ```sql
   ```
10. Tracks belonging to album type single
  ```sql
   ```
11. Total number of tracks by each artist
  ```sql
   ```
12. Total number of tracks in the dataset
  ```sql
   ```
13. Distinct artists in the dataset
  ```sql
   ```
14. Top 10 most popular songs
  ```sql
   ```
15. Total number of albums in the dataset
  ```sql
   ```
16. Tracks with duration greater than 5 minutes
  ```sql
   ```
17. List all songs by a specific artist

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


