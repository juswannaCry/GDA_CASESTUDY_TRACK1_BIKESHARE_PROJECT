# **Cyclistic Bike-Share: Case Study**

_This document is created as part of the capstone project of the Google Data Analytics Professional Certificate._

## Introduction and Scenario
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

### **About the Company**
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geo-tracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

The project follows the six step data analysis process: **ask, prepare, process, analyze, share, and act**.

## **PHASE 1: Ask** 
Three questions will guide the future marketing program:
 1. How do annual members and casual riders use Cyclistic bikes
    differently? 
 2. why would casual riders buy Cyclistic annual memberships?
 3. How can Cyclistic use digital media to influence casual
        riders to become members?

    The director of marketing has assigned you the first question to answer: 
**How do annual members and casual riders use Cyclistic bikes differently?**

**Summary of Business Task**

The goal of this case study is to identify how do annual members and casual riders use Cyclistic bikes differently.

This comparison along with other tasks will later be used by marketing department for developing strategies aimed at converting casual riders into members

**Stakeholders:**

Primary stakeholders: The director of marketing and Cyclistic executive team

Secondary stakeholders: Cyclistic marketing analytics team

## **PHASE 2: Data Preparation**

The data that we will be using is Cyclistic’s historical trip data from last 12 months (May-2020 to Apr-2021). The data has been made available by Motivate International Inc. on this [link](https://divvy-tripdata.s3.amazonaws.com/index.html) under this [license](https://www.divvybikes.com/data-license-agreement).

The dataset consists of 12 CSV files (each for a month) with 13 columns and more than 4 million rows.

ROCCC approach is used to determine the credibility of the data

-   **R**eliable – It is complete and accurate and it represents all bike rides taken in the city of Chicago for the selected duration of our analysis.
-   **O**riginal - The data is made available by Motivate International Inc. which operates the city of Chicago’s Divvy bicycle sharing service which is powered by Lyft.
-   **C**omprehensive - the data includes all information about ride details including starting time, ending time, station name, station ID, type of membership and many more.
-   **C**urrent – It is up-to-date as it includes data until end of May 2021
-   **C**ited - The data is cited and is available under Data License Agreement.
-   **Data Limitation**

A quick filtering and checking data for completeness shows that “start station name and ID” and “end station name and ID” for some rides are missing. Further observations suggest that the most missing data about “start station name” belongs to “electric bikes” as 201,975 out of 888,490 electric ride shares have missing data and it accounts for 22% of total electric-bike ride shares.

This limitation could slightly affect our analysis for finding stations where most electric-bikes are taken but we can use “end station names” to locate our customers and this can be used for further analysis and potential marketing campaigns.
<![endif]-->

## **PHASE 3: Process**

Before we start analyzing, it is necessary to make sure data is clean, free of error and in the right format.
### **Tasks:**

## **PHASE 3: Process**

Before we start analyzing, it is necessary to make sure data is clean, free of error and in the right format.
### **Tasks:**

Tools: R Programming is used for its ability to handle huge datasets efficiently. Microsoft Excel is used for further analysis and visualization. 

# Load Packages
library(readr)
library(tidyverse)
library(dplyr)
library(lubridate)
library(skimr)
library(janitor)

**Preparing for analysis**

# upload clean csv.

# Sort days of the week
df$day_of_week <- ordered(df$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))

# EXPORT
# Total and Average number of weekly rides by rider type
summary_wd <- df %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  
  group_by(member_casual, weekday) %>%  
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>%    
  arrange(member_casual, weekday)
write_csv(summary_wd, "summary_ride_length_weekday.csv")

# Total and Average number of monthly rides by rider type
summary_month <- df %>% 
  mutate(month = month(started_at, label = TRUE)) %>%  
  group_by(month,member_casual) %>%  
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>%    
  arrange(month, member_casual)
write_csv(summary_month, "summary_ride_length_month.csv")

# Stations most used by each user group
summary_station <- df %>% 
  mutate(station = start_station_name) %>%
  drop_na(start_station_name) %>% 
  group_by(start_station_name, member_casual) %>%  
  summarise(number_of_rides = n()) %>%    
  arrange(number_of_rides)
write_csv(summary_station, "summary_stations.csv")

