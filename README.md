**How does rideable_type usage vary between the 2 categories of riders?**

SELECT
 member_casual,
 rideable_type,
 COUNT(*) AS ride_count
FROM
 my-data-project-401909.riders_data.membership_2021_01
WHERE
 member_casual IN ('member', 'casual')
GROUP BY
 member_casual,
 rideable_type
ORDER BY
 member_casual, 
 rideable_type;

I selected the 2 columns i.e. rideable_type and member_casual which I then ‘GROUPED BY’ and ‘ORDERED BY’ in order to group the data and specify the order in which the groups or rows should be presented. 

**How does rideable_type vary by start station?**

SELECT
  start_station_name,
  rideable_type,
  COUNT(*) AS ride_count
FROM
  my-data-project-401909.riders_data.membership_2021_01
WHERE
 start_station_name IS NOT NULL 
GROUP BY
  start_station_name, 
  rideable_type
ORDER BY
  start_station_name,
  rideable_type
LIMIT
 30

Using the ‘WHERE’ statement, I eliminated these values to ensure consistency.

**How does rideable_type vary by days of the week?**

SELECT
  EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
  rideable_type,
  COUNT(*) AS ride_count
FROM
  my-data-project-401909.riders_data.membership_2021_01
GROUP BY
  day_of_week, 
  rideable_type
ORDER BY
  day_of_week, 
  rideable_type
LIMIT 21;

Using the ‘EXTRACT’ function, I was able to extract the days of the week from the started_at column whose values consist of date and time. I then named the new column ‘start_day_of_week’. Using the count (*) function, I was able to count the number of rides and insert the results into a new column called ‘ride_count’. I also added the ‘GROUP BY’ and ‘ORDER BY’ function to group and specify the order of the results. To limit the result set, I applied a ‘LIMIT’ of 21.

**What are the most active start_stations for the 2 categories of riders?**

SELECT 
 start_station_name,
 COUNT(*) as preferred_station_casual
FROM 
 my-data-project-401909.riders_data.membership_2021_01
WHERE 
 member_casual = 'casual'
 AND start_station_name IS NOT NULL
GROUP BY 
 start_station_name
ORDER BY 
 preferred_station_casual DESC
LIMIT 10;

SELECT 
 start_station_name,
 COUNT(*) as preferred_station_member
FROM 
 my-data-project-401909.riders_data.membership_2021_01
WHERE 
 member_casual = 'member'AND
 start_station_name IS NOT NULL
GROUP BY 
 start_station_name
ORDER BY 
 preferred_station_member DESC
LIMIT 10;

I applied the ‘IS NOT NULL’ function to ensure that null results wouldn’t be returned. Further to this, I used the ‘LIMIT’ function to restrict the result set to the 10 most popular start stations for both members and casual riders. 

**What are the most active end_stations for the 2 categories of riders?**

SELECT 
 end_station_name,
 COUNT(*) as preferred_station_casual
FROM 
 my-data-project-401909.riders_data.membership_2021_01
WHERE 
 member_casual = 'casual'
 AND end_station_name IS NOT NULL
GROUP BY 
 end_station_name
ORDER BY 
 preferred_station_casual DESC
LIMIT 10;

SELECT 
 end_station_name,
 COUNT(*) as preferred_station_member
FROM 
 my-data-project-401909.riders_data.membership_2021_01
WHERE 
 member_casual = 'member'
 AND end_station_name IS NOT NULL
GROUP BY 
 end_station_name
ORDER BY 
 preferred_station_member DESC
LIMIT 10;

I grouped and specified the order of results using ‘GROUP BY’ AND ‘ORDER BY’ function. Using DESC, the results are listed from most popular. I also ensured to add the ‘LIMIT’ function to restrict the result set to the 10 most popular end stations for both members and casual riders. 

**What are the least active start_stations for the 2 different groups of riders?**

 SELECT 
 start_station_name,
 COUNT(*) as least_used_stations
FROM 
 my-data-project-401909.riders_data.membership_2021_01
WHERE 
 member_casual = 'casual'
GROUP BY 
 start_station_name
HAVING
 least_used_stations <5
ORDER BY 
 least_used_stations asc
LIMIT 5;

SELECT 
 start_station_name,
 COUNT(*) as least_used_stations
FROM 
 my-data-project-401909.riders_data.membership_2021_01
WHERE 
 member_casual = 'member'
GROUP BY 
 start_station_name
HAVING
 least_used_stations <5
ORDER BY 
 least_used_stations asc
LIMIT 5;

I used the ‘HAVING’ function in conjunction with the ‘GROUP BY’ function in order to filter results after grouping and aggregation has taken place.

**How does bike usage vary during the different days of the months for the 2 groups?**

i used the ‘EXTRACT’ function in order to extract ‘days of the week’ from the ‘started_at column’. This then created 1 new column which I named start_day_of_week. 

SELECT
  *,
  EXTRACT(DAYOFWEEK FROM started_at) AS start_day_of_week,
  EXTRACT(DAYOFWEEK FROM ended_at) AS end_day_of_week
FROM
  my-data-project-401909.riders_data.membership_2020_04;

I then run a separate query where used the count (*) to establish the number of days. I also used the ‘GROUP BY’ and ‘ORDER BY’ function to group and specify the order of result in DESC order. 

SELECT
  member_casual,
  start_day_of_week,
  COUNT(*) AS day_count
FROM
  my-data-project-401909.riders_data.updated_weekday_2021_01
GROUP BY
  member_casual, 
  start_day_of_week
ORDER BY
  member_casual, 
  day_count DESC,
  start_day_of_week;

**What is the average trip duration for the 2 categories of riders across the different starting and ending_stations?**

SELECT
  member_casual,
  AVG(TIME_DIFF(end_time, start_time, MINUTE)) AS average_trip_duration_minutes
FROM
 my-data-project-401909.riders_data.updated_2021_01
GROUP BY
 member_casual;

This calculates the average trip duration in minutes by finding the time difference between end_time and start_time for each record and then computing the average of these differences across the entire dataset. 

**What are the peak riding hours for the 2 categories of riders?**

SELECT
  member_casual,
  EXTRACT(HOUR FROM started_at) AS start_hour,
  COUNT(*) AS ride_count
FROM
  my-data-project-401909.riders_data.membership_2021_01
WHERE
 member_casual = 'casual'
GROUP BY
  member_casual,
  start_hour
ORDER BY
  member_casual, 
  ride_count DESC
LIMIT 10;

SELECT
  member_casual,
  EXTRACT(HOUR FROM started_at) AS start_hour,
  COUNT(*) AS ride_count
FROM
  my-data-project-401909.riders_data.membership_2021_01
WHERE
 member_casual = 'casual'
GROUP BY
  member_casual,
  start_hour
ORDER BY
  member_casual, 
  ride_count DESC
LIMIT 10;

I applied a ‘LIMIT’ of 7 to establish the peak hours for both categories. 
