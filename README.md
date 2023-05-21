## Cyclistic Bike Share: Case Study

**Introduction and Scenario**

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. 

**About Cyclistic Bike-Share (Company)**

Cyclistic is a bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike.

**Step 1: Ask**

The director of marketing has assigned you the first question to answer: **How do annual members and casual riders use Cyclistic bikes differently?**

**Summary of the Business task: Design marketing strategies aimed at converting casual riders into annual members.**

Who are the Stakeholders? 

Primary stakeholders: The director of marketing and Cyclistic executive team

Secondary stakeholders: Cyclistic marketing analytics team

**Step 2: Prepare**

The data that we will be using is Cyclistic’s Historical Trip data from March-2022 to March-2023. The data has been made available by Motivate International Inc. through this [link](https://divvy-tripdata.s3.amazonaws.com/index.html) and under this [license](https://ride.divvybikes.com/data-license-agreement).

**Limitations of Data:** 

There are some rows with missing values in the start_station_name, end_station_name, start_station_id, and end_station_id columns. 

**Step 3: Process**

**Tools:** R for Data Cleaning and Analysis, Tableau for Visualization

**Load libraries in R**

```
> library(readr)
> library(tidyverse)
> library(dplyr)
> library(lubridate)
> library(skimr)
> library(janitor)
```

**Upload Data Sets to R and Combine into a Single Data Frame**

```
> all_data <- rbind (X202303_divvy_tripdata, X202302_divvy_tripdata_copy, X202301_divvy_tripdata_copy, X202212_divvy_tripdata_copy, X202211_divvy_tripdata_copy, X202210_divvy_tripdata_copy, X202209_divvy_publictripdata_copy, X202208_divvy_tripdata_copy, X202207_divvy_tripdata_copy, X202206_divvy_tripdata_copy, X202205_divvy_tripdata_copy, X202204_divvy_tripdata_copy, X202203_divvy_tripdata_copy)
```

**Removal of the five columns: start_station_name, end_station_name, start_station_id, end_station_id, and ride_length**

```
> all_data$start_station_name <- NULL
> all_data$end_station_name <- NULL 
> all_data$start_station_id <- NULL 
> all_data$end_station_id <- NULL
> all_data$ride_length <- NULL
```

**Creation of new column: ride_time**

```
> all_data$ride_time = all_data$ended_at - all_data$started_at
```

**Further Clean the Data and Prepare for Analysis**

```
> colnames(all_data)
> [1] "ride_id"       
> [2] "rideable_type" 
> [3] "started_at"    
> [4] "ended_at"      
> [5] "start_lat"    
> [6] "start_lng"     
> [7] "end_lat"       
> [8] "end_lng"       
> [9] "member_casual" 
> [10] "day_of_week"  
> [11] "ride_time"  
```

**Addition of columns that list the date, month, day, and year of each ride:**

```
> all_data$date <- as.Date(all_trips$started_at) 
> all_data$month <- format(as.Date(all_data$date), "%m")
> all_data$day <- format(as.Date(all_data$date), "%d")
> all_data$year <- format(as.Date(all_data$date), "%Y")
> all_data$day_of_week <- format(as.Date(all_data$date), "%A")
```

**Convert "ride_time" to numeric so calculations can be run on the data**
```
> is.numeric(all_data$ride_time)
> [1] FALSE
```
```
> all_data$ride_time <- as.numeric(as.character(all_data$ride_time))
```
```
> is.numeric(all_data$ride_time)
> [1] TRUE
```

**Step 4: Analyze** 

```
> summary(all_data$ride_time)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-621201     340     601    1137    1080 2483235 
```

**Compare members and casual users**

```
> aggregate(all_data$ride_time ~ all_data$member_casual, FUN = mean)
1                 casual            1725.24
2                 member             747.99
>
> aggregate(all_data$ride_time ~ all_data$member_casual, FUN = median)
1                 casual                763
2                 member                518
>
> aggregate(all_data$ride_time ~ all_data$member_casual, FUN = max)
1                 casual            2483235
2                 member              93594
>
> aggregate(all_data$ride_time ~ all_data$member_casual, FUN = min)
1                 casual                  1
2                 member                  1
```

**Average daily ride time for members vs casual users**
```
> aggregate(all_data$ride_time ~ all_data$member_casual + all_data$day_of_week, FUN = mean)
```

**Sort the days of the week**
```
> all_data$day_of_week <- ordered(all_data$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```


**Step 5: Share**

**Step 6: Act**




 
