## Google-Cyclistic-2023
# Google_Data_Analytics_Capstone_Cyclistic_2023

Course: [Google Data Analytics Capstone: Complete a Case
Study](https://www.coursera.org/learn/google-data-analytics-capstone/)

![cyclistic_case_study](https://github.com/user-attachments/assets/b77dbfdb-2daa-4ae9-9183-7fd7b9b8c798)


## Introduction

Welcome to the Cyclistic bike-share analysis case study! In this case
study, I worked for a fictional company, Cyclistic, along with some key
team members. In order to answer the business questions, I have followed
the steps of the data analysis process: **Ask, Prepare, Process,
Analyze, Share,** and **Act.**

## Scenario

I am assuming myself as a junior data analyst working on the marketing
analyst team at Cyclistic, a bike-share company in Chicago. The director
of marketing Ms. Lily Moreno believes the company’s future success
depends on maximizing the number of annual memberships.

Therefore, my team wants to understand how casual riders and annual
members use Cyclistic bikes differently. From these insights, we will
design a new marketing strategy to convert casual riders into annual
members. But first, Cyclistic executives must approve our team's
recommendations, so they must be backed up with compelling data insights
and professional data visualizations.

### About the company

In 2016, Cyclistic launched a successful bike-share offering. Since
then, the program has grown to a fleet of 5,824 bicycles that are geo
tracked and locked into a network of 692 stations across Chicago. The
bikes can be unlocked from one station and returned to any other station
in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general
awareness and appealing to broad consumer segments. One approach that
helped make these things possible was the flexibility of its pricing
plans: single-ride passes, full-day passes, and annual memberships.
Customers who purchase single-ride or full-day passes are referred to as
casual riders.Customers who purchase annual memberships are Cyclistic
members.

Cyclistic’s finance analysts have concluded that annual members are much
more profitable than casual riders. Although the pricing flexibility
helps Cyclistic attract more customers, Moreno believes that maximizing
the number of annual members will be key to future growth. Rather than
creating a marketing campaign that targets all-new customers, Moreno
believes there is a solid opportunity to convert casual riders into
members. Moreno has set a clear goal: Design marketing strategies aimed
at converting casual riders into annual members.

## Ask

### Business Task

Help to design marketing strategies to convert casual riders to members.

### Analysis Questions

Three questions will guide the future marketing program:

1.  How do annual members and casual riders use Cyclistic bikes
    differently?

2.  Why would casual riders buy Cyclistic annual memberships?

3.  How can Cyclistic use digital media to influence casual riders to
    become members?

Moreno has assigned my team the first question to answer: How do annual
members and casual riders use Cyclistic bikes differently?

## Prepare

### Data Source

I used Cyclistic’s historical trip data to analyze and identify trends
from Jan 2023 to Dec 2023 which can be downloaded from
[divvy_tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html).
The data has been made available by Motivate International Inc. under
this [license](https://divvybikes.com/data-license-agreement).

This is public data that can be used to explore how different customer
types are using Cyclistic bikes. But note that data-privacy issues
prohibit from using riders’ personally identifiable information. This
means that we won’t be able to connect pass purchases to credit card
numbers to determine if casual riders live in the Cyclistic service area
or if they have purchased multiple single passes.

### Data Organization

There are 12 files with naming convention of YYYYMM-divvy-tripdata and
each file includes information for one month, such as the ride id, bike
type, start time, end time, start station, end station, start location,
end location, and whether the rider is a member or not. The
corresponding column names are ride_id, rideable_type, started_at,
ended_at, start_station_name, start_station_id, end_station_name,
end_station_id, start_lat, start_lng, end_lat, end_lng and
member_casual.

## Process

BigQuery is used to combine the various datasets into one dataset and
clean it. Reason: A worksheet can only have 1,048,576 rows in Microsoft
Excel because of its inability to manage large amounts of data. Because
the Cyclistic dataset has more than 6 million rows, it is essential to
use a platform like BigQuery that supports huge volumes of data.

### Combining the Data

SQL Query: [Data
Combining](https://console.cloud.google.com/bigquery?ws=!1m7!1m6!12m5!1m3!1slofty-fort-427707-j0!2sus-central1!3s426c52ea-3526-4025-9f5a-d6ea218b237a!2e1)

Monthly wise starting from January-2023 and ending at December-2023; 12
csv files are uploaded as tables in the dataset '202301_tripdata',
202302_tripdata, 202303_tripdata and so on. Another table named
"2023_combined_data" is created, containing 6,048,834 rows of data for
the entire year.

### Data Exploration

SQL Query: [Data
Exploration](https://console.cloud.google.com/bigquery?ws=!1m7!1m6!12m5!1m3!1slofty-fort-427707-j0!2sus-central1!3s25e59f32-557b-4747-b68c-bf1157875e73!2e1)

Before cleaning the data, I am familiarizing myself with the data to
find the inconsistencies.

Observations:

1.  The table below shows the all column names and their data types. The
    ride_id column is our primary key.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_schema.png)

2.  The following table shows number of null values in each column.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_nulls.png)
*Note*: After checking the above results, I found out that the number of
NULLS of columns start_station_name, start_station_id, end_station_name,
end_station_id, end_lat and end_lng are not matching to the numbers of
NULLS(328957) of remaining other columns(please refer the image). This
may be due to missing information in the same row i.e. station's name
and id for the same station and latitude and longitude for the same
ending station.

3.  Missing values/nulls related to start_staion_name and
    start_staion_id.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_nulls_start_station.png)

So, these 875848 rows have both start_station_name and start_station_id
missing needs to be removed.

4.  Missing values/nulls related to end_staion_name and end_staion_id.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_nulls_end_station.png)

So, these 929343 rows have both end_station_name and end_station_id
missing needs to be removed.

5.  Missing values/nulls related to end_lat and end_lng

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_nulls_end_locations.png)

So, these 6990 rows have both end_lat and end_lng missing needs to be
removed.

6.  As ride_id has no null values, let's use it to check for duplicates.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_ride_id_duplicates.png)

There are no duplicate rows in the data.

7.  All ride_id values have length of 16 so no need to clean it.

8.  There are 3 unique types of bikes(rideable_type) in our data.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclitic_2023_bike_types.png)

9.  The started_at and ended_at shows start and end time of the trip in
    YYYY-MM-DD hh:mm:ss UTC format. New column ride_length can be
    created to find the total trip duration. There are 6419 trips which
    has duration longer than a day and 161112 trips having less than a
    minute duration or having end time earlier than start time so need
    to remove them. Other columns day_of_week and month can also be
    helpful in analysis of trips at different times in a year.

10. member_casual column has 2 uniqued values as member or casual rider.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclitic_2023_rider_types.png)

11. Columns that need to be removed are start_station_id and
    end_station_id as they do not add value to analysis of our current
    problem. Longitude and latitude location columns may not be used in
    analysis but can be used to visualize a map.

### Data Cleaning

SQL Query: [Data
Cleaning](https://console.cloud.google.com/bigquery?ws=!1m7!1m6!12m5!1m3!1slofty-fort-427707-j0!2sus-central1!3s7e21c2c1-264c-4f46-9a26-ef2e79d55725!2e1)

1.  All the rows having missing values are deleted.

2.  3 more columns ride_length for duration of the trip, day_of_week and
    month are added.

3.  Trips with duration less than a minute and longer than a day are
    excluded.

4.  Total 1,811,546 rows are removed in this step.

## Analyze and Share

SQL Query: [Data
Analysis](https://console.cloud.google.com/bigquery?ws=!1m7!1m6!12m5!1m3!1slofty-fort-427707-j0!2sus-central1!3s9cc6fa9e-dbd7-4e77-88ea-ac1854422015!2e1)

Data Visualization:
[Tableau](https://public.tableau.com/app/profile/shrawan.singh6518/viz/GoogleCaseStudyCyclistic_17317739104460/Story1)

The data is stored appropriately and is now prepared for analysis. I
queried multiple relevant tables for the analysis and visualized them in
Tableau. The analysis question is: How do annual members and casual
riders use Cyclistic bikes differently?

First of all, member and casual riders are compared by the type of bikes
they are using.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_tableau_01.png)

The members make 64.53% of the total while remaining 35.47% constitutes
casual riders. Each bike type chart shows percentage from the total.
Most used bike is classic bike followed by the electric bike. Docked
bikes are used the least by only casual riders.

Next the number of trips distributed by the months, days of the week and
hours of the day are examined.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_tableau_02.png)
*Months*: When it comes to monthly trips, both casual and members
exhibit comparable behavior, with more trips in the spring and summer
and fewer in the winter. The gap between casuals and members is closest
in the month of July in summer.

*Days of Week*: When the days of the week are compared, it is discovered
that casual riders make more journeys on the weekends while members show
a decline over the weekend in contrast to the other days of the week.

*Hours of the Day*: The members shows 2 peaks throughout the day in
terms of number of trips. One is early in the morning at around 6 am to
8 am and other is in the evening at around 4 pm to 8 pm while number of
trips for casual riders increase consistently over the day till evening
and then decrease afterwards.

We can infer from the previous observations that member may be using
bikes for commuting to and from the work in the week days while casual
riders are using bikes throughout the day, more frequently over the
weekends for leisure purposes. Both are most active in summer and
spring.

Ride duration of the trips are compared to find the differences in the
behavior of casual and member riders.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_tableau_03.png)
Take note that casual riders tend to cycle longer than members do on
average. The length of the average journey for members doesn't change
throughout the year, week, or day. However, there are variations in how
long casual riders cycle. In the spring and summer, on weekends, and
from 10 am to 2 pm during the day, they travel greater distances.
Between five and eight in the morning, they have brief trips.

These findings lead to the conclusion that casual commuters travel
longer (approximately 2x more) but less frequently than members. They
make longer journeys on weekends and during the day outside of commuting
hours and in spring and summer season, so they might be doing so for
recreation purposes.

To further understand the differences in casual and member riders,
locations of starting and ending stations can be analysed. Stations with
the most trips are considered using filters to draw out the following
conclusions.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_tableau_04.png)
Casual riders have frequently started their trips from the stations in
vicinity of museums, parks, beach, harbor points and aquarium while
members have begun their journeys from stations close to universities,
residential areas, restaurants, hospitals, grocery stores, theatre,
schools, banks, factories, train stations, parks and plazas.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_tableau_05.png)
Similar trend can be observed in ending station locations. Casual riders
end their journay near parks, museums and other recreational sites
whereas members end their trips close to universities, residential and
commmercial areas. So this proves that casual riders use bikes for
leisure activities while members extensively rely on them for daily
commute.

![](https://431702c6caf44d879636c14111d98f01.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2FCourse+7%2Fcyclistic_2023_summary.png)

## Act 
After identifying the differences between casual and member
riders, marketing strategies to target casual riders can be developed to
persuade them to become members.

**Recommendations**: Marketing campaigns might be conducted in spring and
summer at tourist/recreational locations popular among casual riders.
Casual riders are most active on weekends and during the summer and
spring, thus they may be offered seasonal or weekend-only memberships.
Casual riders use their bikes for longer duration than members. Offering
discounts for longer rides may incentivize casual riders and entice
members to ride for longer periods of time.

