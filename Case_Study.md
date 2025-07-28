# Case study: How does a bike-share navigate speedy success?  

# 1. Introduction
 This Case Study is to demonstrate the skills I learned in the Google Data Analytics Course. In this program, I learned how to clean and organize
 data for analysis, and complete analysis and calculations using spreadsheets, SQL and R programming. I also learned how to visualize and 
 present data findings in dashboards, and in presentations using Tableu.

In this Case Study, I will comlepete an analysis for a ficticious bike-share program named Cyclistic along with some key team members, using 
SQL to clean, organize, analyze, and calulate my findings. I will aslo be using Tableu to create diagrams to share my findings and present my
complete analysis.

# 2. Scenario

I am a junior data analyst working on the marketing analyst team at Cyclistic, a bike-share
company in Chicago. The director of marketing believes the company’s future success
depends on maximizing the number of annual memberships. Therefore, the team wants to
understand how casual riders and annual members use Cyclistic bikes dierently. From these
insights, the team will design a new marketing strategy to convert casual riders into annual
members. But first, Cyclistic executives must approve our recommendations, so they must be
backed up with compelling data insights and professional data visualizations.

### Characters & Teams

* **Cyclistic**   
A bike-share program that features more than 5,800 bicycles and 600
docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand
tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities
and riders who can’t use a standard two-wheeled bike. The majority of riders opt for
traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more
likely to ride for leisure, but about 30% use the bikes to commute to work each day.
* **Lily Moreno**   
The director of marketing and our manager. Moreno is responsible for
the development of campaigns and initiatives to promote the bike-share program.
These may include email, social media, and other channels.
* **Cyclistic Marketing Analytics Team**   
A team of data analysts who are responsible for
collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy.
I joined this team six months ago and have been busy learning about Cyclistic’s
mission and business goals, as well as how I can help Cyclistic achieve them.
* **Cyclistic Executive Team**   
The notoriously detail-oriented executive team will decide
whether to approve the recommended marketing program.

### About the Company

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown
to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across
Chicago. The bikes can be unlocked from one station and returned to any other station in the
system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to
broad consumer segments. One approach that helped make these things possible was the
flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships.
Customers who purchase single-ride or full-day passes are referred to as casual riders.
Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable
than casual riders. Although the pricing flexibility helps Cyclistic attract more customers,
Moreno believes that maximizing the number of annual members will be key to future growth.
Rather than creating a marketing campaign that targets all-new customers, Moreno believes
there is a solid opportunity to convert casual riders into members. She notes that casual riders
are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into
annual members. In order to do that, however, the team needs to beer understand how
annual members and casual riders differ, why casual riders would buy a membership, and how
digital media could affect their marketing tactics. Moreno and her team are interested in
analyzing the Cyclistic historical bike trip data to identify trends.

# 3. Data Analysis Process
In this Case Study, I will be using a 6 step analysis process to solve the problem.
Those steps are as follows:
1. Ask
2. Prepare
3. Process
4. Analyze
5. Share
6. Act

## **Step One: Ask**
The Cyclistic Marketing Analytics Team came up with three questions that will guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes dierently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned me the first question to answer: How do annual members and casual
riders use Cyclistic bikes differently?

## **Step Two: Prepare**
**Data Source**   
Because Cyclistic is a fictitional company, I will be using the January 2024 - December 2024 data from [The City of Chicago’s Divvy bicycle sharing service](https://divvy-tripdata.s3.amazonaws.com/index.html)
to analyze and identify current trends in riders behavior. The data has been made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement).

**Data Organization**  
The data is organized by month, and follows the naming covention of *"YYYYMM-divvy-tripdata"*. The datasets for January - May contains the 
trips that started in their respective months, and the datasets for June - December contain the trips that were concluded in their months. Each 
dataset contains the following data column names:   
* ride_id
* rideable_type
* started_at
* ended_at
* start_station_name
* start_station_id
* end_station_name
* end_station_id
* start_lat
* start_lng
* end_lat
* end_lng
* member_casual

**ROCCC**  
ROCCC is an acronym that is used to help determine weither a dataset is biased. It stands for  

R- Reliable  
O- Original  
C- Comprehensive  
C- Current  
C- Cited  

The dataset comes directly from Divvy's parent company Lyft Bikes and Scooters LLC, so I can be sure that the data is reliable, original, and cited.
There are 13 columns of data for each trip so the data is comprehensive, and the datasets are from the past year making them current.  

## **Step Three: Process**  
For this study, I am using Google's BigQuery to complete my analysis.  
  
**Data Combining**  
I've combined the 12 datasets into one dataset I named *"combined_2024_tripdata"* to easily clean and analyze the data. I used the following
code to generate the new table:
```
CREATE TABLE IF NOT EXISTS `cyclistic-466120.combined_2024_tripdata.combined_2024_tripdata` AS 
(
  SELECT * FROM `monthly_2024_tripdata.202401-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202402-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202403-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202404-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202405-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202406-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202407-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202408-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202409-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202410-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202411-divvy-tripdata`
  UNION ALL
  SELECT * FROM `monthly_2024_tripdata.202412-divvy-tripdata`
```
Then I perform the following query to find that we are starting with 5,860,568 rows of data for the entire year.

```
SELECT count(*) as total_records 
FROM `cyclistic-466120.combined_2024_tripdata.combined_2024_tripdata`
```

**Data Cleaning**  
Before we can clean the data I will make a duplicate table named "cleaned_2024_tripdata" so in case I make any irreversable errors, I have a data set that I can return 
to. Now we must ensure that the data is clean, concistant, and does not contain any errors. First I will check the metadata to see if there are any 
inconsistencies.  

```
SELECT column_name, data_type
FROM combined_2024_tripdata.INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'cleaned_2024_tripdata'
```

After checking, I have found no inconsistencies and have identified the ride_id column to be the primary key.
Then I will check for any duplicate values and it appears there are 422 duplicate entries.  

```
SELECT COUNT(ride_id) - COUNT(DISTINCT ride_id) AS duplicate_rows
FROM cyclistic-466120.combined_2024_tripdata.cleaned_2024_tripdata
```
I used the following code to determine which entries were duplicated and to investigate why they were duplicated.
```
SELECT ride_id, COUNT(*) as num_entries
FROM cyclistic-466120.combined_2024_tripdata.cleaned_2024_tripdata
GROUP BY ride_id
HAVING ( COUNT(*) > 1 )
```
I concluded that these duplicates are from the switch in May to June where reporting was switched from trips started in the month to trips 
finished in the month. Trips that were started in May and ended in June were counted twice. I used the following code to take away the 
duplicate entries and continue with the cleaning process.
```
DELETE FROM cyclistic-466120.combined_2024_tripdata.cleaned_2024_tripdata
WHERE ride_id IN (
    SELECT ride_id
    FROM cyclistic-466120.combined_2024_tripdata.cleaned_2024_tripdata
    GROUP BY ride_id
    HAVING COUNT(*) > 1)
```
Now I will look for null values using the following code. It appears ther are 1,073,905 nulls in the start_station_name and start_station_id
columns, 1,104,493 nulls in the end_statioon_name and end_station_id columns, and 7,152 null values for end_lat and end_lng. Although the data 
is null, I will not be removing from the database because it can still provide useful data, and they aren't in a key field.
```
SELECT COUNT(*) - COUNT(ride_id) ride_id,
 COUNT(*) - COUNT(rideable_type) rideable_type,
 COUNT(*) - COUNT(started_at) started_at,
 COUNT(*) - COUNT(ended_at) ended_at,
 COUNT(*) - COUNT(start_station_name) start_station_name,
 COUNT(*) - COUNT(start_station_id) start_station_id,
 COUNT(*) - COUNT(end_station_name) end_station_name,
 COUNT(*) - COUNT(end_station_id) end_station_id,
 COUNT(*) - COUNT(start_lat) start_lat,
 COUNT(*) - COUNT(start_lng) start_lng,
 COUNT(*) - COUNT(end_lat) end_lat,
 COUNT(*) - COUNT(end_lng) end_lng,
 COUNT(*) - COUNT(member_casual) member_casual
FROM cyclistic-466120.combined_2024_tripdata.cleaned_2024_tripdata
```
Next I will calculate some data points that I will use during my analysis. I will use the following code to calulate the hour, the day of the week, 
and the month the trip started, and the trip duration in minutes.

```
SELECT *,
  TIMESTAMP_DIFF(ended_at, started_at, MINUTE) AS tripduration_mins,
  extract (hour FROM started_at) AS hour,
  format_date('%A', started_at) AS day_of_week,
  format_date('%B', started_at) AS month,
  format_date('%Y', started_at) AS year,
FROM cyclistic-466120.combined_2024_tripdata.cleaned_2024_tripdata


```
I noticed that there are some trips that are shorter than one minute and some are longer than one day, which can affect the average trip 
durations for all the rides. I will delete all of these rows than create a new table called "final_2024_tripdata".
```
DELETE
FROM cyclistic-466120.combined_2024_tripdata.v2_cleaned_2024_tripdata
WHERE tripduration_mins <= 0 OR tripduration_mins >= 1441
```  

## **Step Four: Analyze**  

In this phase of the analysis process, I will begin to analyze the relatonships in the data across the columns, identify trends, and extract datasets
that help me understand the the different habits between casual riders and member riders. These are some of the queries that I ran to analyze the data.

To begin with, I ran this query is to extract the membership status, and total rides for each ride type.
```
SELECT
  member_casual,
  rideable_type,
  COUNT(ride_id) AS rides_total
FROM cyclistic-466120.combined_2024_tripdata.final_2024_tripdata
GROUP BY
  rideable_type,
  member_casual
ORDER BY member_casual
```  
Following this, I ran this query is to find the membership status, ride total, and the average trip duration for each month.
```
SELECT
  month,
  member_casual,
  COUNT(ride_id) AS rides_total,
  AVG(tripduration_mins) AS avg_trip_duration
FROM cyclistic-466120.combined_2024_tripdata.final_2024_tripdata
GROUP BY
  member_casual,
  month
ORDER BY rides_total DESC
```  
Next I ran this query is to find the membership status, ride total, and the average trip duration for each day of the week.
```
SELECT
  day_of_week,
  member_casual,
  COUNT(ride_id) AS rides_total,
  AVG(tripduration_mins) AS avg_trip_duration
FROM cyclistic-466120.combined_2024_tripdata.final_2024_tripdata
GROUP BY
  day_of_week,
  member_casual
ORDER BY rides_total DESC
```
After that, I ran this query to find the membership status, ride total, and the average trip duration for each hour.
```
SELECT
  hour,
  member_casual,
  COUNT(ride_id) AS rides_total,
  AVG(tripduration_mins) AS avg_trip_duration
FROM cyclistic-466120.combined_2024_tripdata.final_2024_tripdata
GROUP BY
  hour,
  member_casual
ORDER BY rides_total DESC
```  
Finally I ran these query to find the most popular starting and ending locations for members and casual riders.
```
SELECT
  start_station_name,
  member_casual,
  COUNT(ride_id) AS rides_total
FROM cyclistic-466120.combined_2024_tripdata.final_2024_tripdata
GROUP BY
  start_station_name,
  member_casual
ORDER BY rides_total DESC

SELECT
  end_station_name,
  member_casual,
  COUNT(ride_id) AS rides_total
FROM cyclistic-466120.combined_2024_tripdata.final_2024_tripdata
GROUP BY
  end_station_name,
  member_casual
ORDER BY rides_total DESC
```

## **Step Five: Share**  

Now that I have analyzed key data points and identified trends, I am now able to exract the data points that I need from the dataset and input 
them into tableau to create visualizations. After I create the vizualizations, I can then present the findings to the shareholders.

First I will examine the types of bikes that each group uses.

<img width="671" height="258" alt="Image" src="https://github.com/user-attachments/assets/3fc0b08d-add0-483e-ac6e-79f857399314" />  

Member users make up about 64% of total of total rides and casual users make up the remaining 36% of total rides. Electric bikes are the most 
popular ride type, making up a total of 50% of total rides. However, electric scooters were only introduced in September so it us unknown if 
scooters would have a larger effect on total ride share during the spring and summer months. Members make up 64% of both classic bikes and 
electric bikes, showing that member and don't have a preference for either classic bikes or electric bikes. However, casual members do have a 
preference towards electric scooters, as they make up 59% of all scooter users.  

Following this, I will analyze how the different user types use the bikes over different amount of time parameters.

<img width="1920" height="1010" alt="Image" src="https://github.com/user-attachments/assets/3cf257d0-e36e-4d0a-8263-34c785d3cf97" />  

For monthly trips, both member and casual exhibit similar trends. Both rise in total rides in the spring and sumer months, peaking in 
September, and fall in the fall and winter months, with a low in January. During weekly trips, member rides tend to take more rides during the 
week, and casual riders have peaks on the weekends. Trips per hour is where you find the most difference. Member users have two peak times 
at 8AM and 5PM, while casual users have a steady growth throughout the day, peaking at 5PM, then slowing down into the evening.  

From these findings, we can conclude that member users may be using the bikes to make their daily commutes, as their peak rides are during 
the work week and at work hours. We can also conclude that casual users are more like to use the bikes for leisure, as their peak times are on 
the weekends and are more steady throughout the day.  

Now that we understand the differences between members and casual users total trips, I'll compare the average trip durations.

<img width="1920" height="1013" alt="Image" src="https://github.com/user-attachments/assets/5270651f-c2e1-4549-bbf8-2d14d4fe95f3" />  

One ofthe first things you notice is that casual users have a much higher average trip duration. Members users have about the same trip 
duration throughout the different time parameters, where casual users have highly varying times. Casual users have a higher trip duration 
during the spring and summer months as well as the weekends. Casual users also have a peak in ride duration from 10AM till 1PM. 

We can conclude that casual riders take trips that are about twice as long as member riders, while taking fewer trips. These trips are also taken
on the weekends and are mainly not during commuting hours, which supports our previous observation that casual users trips are primarily for leisure.  

Lastly I will look at the difference in the most popular starting and ending locations for member and casual users by looking at some of the 
most popular locations.

<img width="1917" height="913" alt="Image" src="https://github.com/user-attachments/assets/9fc1c79b-b686-4eb4-859f-0d98e33004ed" />  

Both member and casual riders tend to start and end in the same locations, however the locations that the different groups go to are very 
different. Member users are more likely to go to locations that are near to locations like grocery stores, banks, factories, train stations, 
commercial areas , and the University of Chicago. Casual users are more likely to go to locations like museums, aquariums, parks, and beaches.
These observations continue to support the conclusion that casual riders utilize the bike share network for leisure purpouses and members use 
it for commuting.

### Summary ###  

*Member Users*  

Member users are more likely to use the bikes to commute throughout the city. The peak times that members use the bikes is during the week 
and at peak commute times. They take shorter trip times, on average taking half as long of trips. Members also go to locations such as grocery 
stores, banks, factories, train stations, commercial areas , and the University of Chicago. This indicates that members tend to use the bikes as
part of their daily commute.  

*Casual Riders*  

Casual riders throughout the data has shown that they use the bikes for leisure, peaking on the weekends and in the summer months. They are 
also willing to travel farther than member users, on average traveling twice as far as members. This is also supported by looking at where casual 
users tend to travel to. Casual users are most likely to go to locations like museums, aquariums, parks, beaches and other leisurely locations.  

## **Step Six: Act**  

Now that we have observed the differences between casual and member users, we can help create marketing stratagies that will encourage 
casual users to convert into members. Here are some of the following recomendations that could be made:

1. A marketing campaign could be launched in the spring and summer months thats targeted at tourist and casual rider hotspot.
2. Create new weekend and seasonal membership plans to target the influx of casual riders on the weekends and warmer seasons.
3. Provide discount for longer trips or electric scooters to incentivize casual rides that take longer trips or perfer scooters.
4. Introduce a referral program that incentivizes members to refer friends and family to become members.
