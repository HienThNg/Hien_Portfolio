# Hien_Portfolio
---
title: 'Google Data Analytics - Case Study: Bellabeat'
Author: Nguyen Thuy Hien
Date: July 8th 2022
output:
  pdf_document: default  
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo=TRUE)
```

# How Can a Wellness Technology Company Play It Smart? 

## Introduction

Welcome to my Bellabeat data analysis case study. In this case study, I will perform the knowledge that I learned from Google Data Analytics Professional Certificate. In order to answer the key business questions, I will follow the steps of the data analysis process:  ask,  prepare,  process,  analyze, share, and act. Along the way, the Case Study Roadmap tables — including guiding questions and key tasks.

## About the company

Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that manufactures health-focused smart products. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women. By 2016, Bellabeat had opened offices around the world and launched multiple products. Bellabeat products became available through a growing number of online retailers in addition to their own e-commerce channel on their website. 

## Ask Phase

### Identify the business task

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

The first thing is to recognize who are the potential customers of Bellabeat based on their usage of their fitness smart devices. Next is the answer is there any relationship between customer behaviors and data we have.  After that, what is the effect of those trends on Bellabeat’s marketing strategies.

### Consider key stakeholders

The main stakeholders are Urška Sršen and Sando Mur, the founders of Bellabeat. The other stakeholders are Bellabeat marketing team and maybe there is also my manager.

## Prepare Phase

Choosing the suitable dataset.

Sršen encourages to use public data that explores smart device users’ daily habits. She points you to a specific data set: [FitBit Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit) (CC0: Public Domain, dataset made available through [Mobius](https://www.kaggle.com/arashnic)):This Kaggle data set contains personal fitness tracker from thirty fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits.

I will use R programing language to work with this dataset.
  
### Installing and loading common packages and libraries

```{r results = FALSE}
install.packages('tinytex', repos = "http://cran.us.r-project.org")
```

```{r results = FALSE}
install.packages('plyr', repos = "http://cran.us.r-project.org")
```

```{r results = FALSE}
install.packages("tidyverse", repos = "http://cran.us.r-project.org")
install.packages("lubridate", repos = "http://cran.us.r-project.org")
install.packages("dplyr", repos = "http://cran.us.r-project.org")
install.packages("ggplot2", repos = "http://cran.us.r-project.org")
install.packages("tidyr", repos = "http://cran.us.r-project.org")
install.packages("here", repos = "http://cran.us.r-project.org")
install.packages("skimr", repos = "http://cran.us.r-project.org")
install.packages("janitor", repos = "http://cran.us.r-project.org")
```

```{r}
library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
library(tidyr)
library(here)
library(skimr)
library(janitor)
library(readr)
```

### Importing dataset

In this step, I will import all datasets that I need to use for this project.

  * dailyActivity_merged.csv
  
```{r}
Activity <- read_csv("~/BellaBeat-Case Study/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
head(Activity)
```

```{r}
colnames(Activity)
```

```{r}
str(Activity)
```
  * heartrate_seconds_merged.csv
  
```{r}
Heartrate <- read_csv("~/BellaBeat-Case Study/Fitabase Data 4.12.16-5.12.16/heartrate_seconds_merged.csv")
head(Heartrate)
```

```{r}
colnames(Heartrate)
```

```{r}
str(Heartrate)
```
  
  * sleepDay_merged.csv
  
```{r}
Sleep <- read_csv("~/BellaBeat-Case Study/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
head(Sleep)
```

```{r}
colnames(Sleep)
```

```{r}
str(Sleep)
```  

  * weightLogInfo_merged.csv  

```{r}
Weight <- read_csv("~/BellaBeat-Case Study/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
head(Weight)
```

```{r}
colnames(Weight)
```

```{r}
str(Weight)
```   
  
  * hourlyIntensities_merged.csv

```{r}
HourlyIntensities <- read_csv("~/BellaBeat-Case Study/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
head(HourlyIntensities)
```

```{r}
colnames(HourlyIntensities)
```

```{r}
str(HourlyIntensities)
```   

## Process Phase

### Cleaning the dataset

I used functions like skim_without_charts() to review the datasets. I also used clean_name() to clean names.

```{r}
clean_names(Activity)
clean_names(Heartrate)
clean_names(Sleep)
clean_names(Weight)
clean_names(HourlyIntensities)
```

```{r}
skim_without_charts(Activity)
skim_without_charts(Heartrate)
skim_without_charts(Sleep)
skim_without_charts(Weight)
skim_without_charts(HourlyIntensities)
```
  * There are 65 missing values in total 67 values of column fat in Weight dataframe.
  
  * For Activity dataframe: I did not find any Spelling error, Misfield value, Missing value, Extra or blank space. 
  
To find and remove duplicate values, I identified them by duplicated() and distinct():

```{r}
get_dupes(Sleep)
```
  
  * There are 3 duplicate rows in Sleep dataframe, so that I removed them with distinct() function:
  
```{r}
distinct(Sleep)
```

### Formatting dataset:

I am going to change the data type from character to date time and split to date and time

Activity dataframe:
```{r}
Activity$date <- mdy(Activity$ActivityDate)
glimpse(Activity$date)
```

Heartrate dataframe:
```{r}
Heartrate$Date_Time <- mdy_hms(Heartrate$Time,tz=Sys.timezone())
glimpse(Heartrate$Date_Time)
Heartrate$Date <- as.Date(Heartrate$Date_Time)
glimpse(Heartrate$Date)
```

Sleep dataframe:
```{r}
Sleep$Date_Time <- mdy_hms(Sleep$SleepDay,tz=Sys.timezone())
glimpse(Sleep$Date_Time)
Sleep$Date <- as.Date(Sleep$Date_Time)
glimpse(Sleep$Date)
```
Weight dataframe:
```{r}
Weight$Date_Time <- mdy_hms(Weight$Date,tz=Sys.timezone())
glimpse(Weight$Date_Time)
Weight$Day <- as.Date(Weight$Date_Time)
glimpse(Weight$Day)
```
HourlyIntensities dataframe
```{r}
HourlyIntensities$Date_Time <- mdy_hms(HourlyIntensities$ActivityHour,tz=Sys.timezone())
glimpse(HourlyIntensities$Date_Time)
HourlyIntensities$Time <- format(as.POSIXct(HourlyIntensities$Date_Time),format = "%H:%M:%S")
glimpse(HourlyIntensities$Time)
```

## Analyze Phase

### The total number of participants in each data set

```{r}
n_distinct(Activity$Id)
```

```{r}
n_distinct(Heartrate$Id)
```

```{r}
n_distinct(Sleep$Id)
```

```{r}
n_distinct(Weight$Id)
```

There are 33 participants in Activity dataframes. 24 participants in Sleep dataframe. Heartrate and Weight dataframes only have 14 and 8 participants. 

### Sumary dataset

Activity dataframe:
```{r}
Activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes,
         VeryActiveMinutes, 
         FairlyActiveMinutes, 
         LightlyActiveMinutes, 
         SedentaryMinutes,
         Calories) %>%
  summary()
```
Sleep dataframe:
```{r}
Sleep %>%
  select(TotalSleepRecords, 
         TotalMinutesAsleep, 
         TotalTimeInBed) %>%
  summary()
```

Heartrate dataframe:
```{r}
Heartrate %>%
  select(Value) %>%
  summary()
```

Weight dataframe:
```{r}
Weight %>%
  select(WeightKg,
         BMI) %>%
  summary()
```

### Key finding from these summar

Average steps per day is 7638.

People consumed 2304 calories a day.

Participants’ average sleep time is 6.98 hours a day.

The average light activity (192.8 minutes) is considerably higher than very active (21.16 minutes) and fairly active (13.56 minutes). 

The number of sedentary time is very high more than 16 hours.

## Data Visualization (Share and Act Phases)

#### Relationship between steps taken in a day and sedentary minutes:

```{r}
ggplot(data=Activity, aes(x=TotalSteps, y=SedentaryMinutes, color = VeryActiveMinutes)) + 
  geom_point() + 
  geom_smooth() + 
  labs(title="Total Steps and Sedentaty Minutes")
```

There is a negative relationship between two figures. The more sedentary time participants have, the less steps they take a day. 

#### The relationship between minutes asleep and time in bed

```{r}
ggplot(data=Sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed, color = TotalSleepRecords)) + 
  geom_point() +
  geom_smooth() + 
  labs(title="Minutes Asleep and Time In Bed")
```

Time in bed and Total minutes asleep have a nearly perfect positive correlation.

#### Relationship between Total Steps and Very Active Minutes:

```{r}
ggplot(data=Activity, aes(x=TotalSteps, y=VeryActiveMinutes)) + 
  geom_point() + 
  geom_smooth() + 
  labs(title="Total Steps vs Very Active Minutes ")
```
The graph indicate that there is a positive correlation between Total Steps and Very Active Minutes a day by participants. That means the more steps they walk per day, the more likely the sleep time to increase. 

#### Intensity hour:

```{r}
ggplot(HourlyIntensities) + 
  stat_summary(aes(x=Time,y=TotalIntensity),fun=mean,geom="bar", 
                          fill="lightblue",col="grey50") +
  theme(axis.text.x = element_text(angle = 90)) +
  labs(title="Total Intensity vs Time ")
```
Look at the chart, I notice that participants use the app mostly after work from 5pm to 7pm.

## Act

### Conclusion

After analyzing and visualizing FitBit Fitness Tracker Data set, I found some insights that would help Bellabeat improve their business:

  * The negative relationship between sedentary time and Total Steps, and the number of sedentary time per day. This figure shows that the company needs to promote more about the benefit of the number of steps should take a, day especially for people who have high sedentary time. 
  
  * Time in bed and Total minutes asleep have a nearly perfect positive correlation. Through that, the company may consider adding more features to remind customers what time they need to go to bed and what sleep time they should take.
  
  * The graph indicates that there is a positive correlation between Total Steps and Very Active Minutes a day by participants.  That means the company can use it to encourage app users to do intense activity in order to increase the number of steps because 10,000 steps per day are good for people's health. 
  
  * Look at the chart, I notice that participants use the app mainly after work from 5 pm to 7 pm. The company app can use this figure to remind and motivate users to go for exercise.

### Target Audiences

People who want to use the app remind or encourage to do exercise for keeping weight or for health purposes. Especially people who have a full-time job and want to run or walk after work.

### Recommendation

The average number of steps per day is 7638, this figure needs to increase. According to CDC research, people should walk 10,000 steps a day. That is a number said to help reduce certain health conditions, such as high blood pressure and heart disease.

People consumed 2304 calories a day. Regarding NHS, the recommended daily calorie intake is 2,000 calories a day for women and 2,500 for men. We could use [BMR method](https://www.k-state.edu/paccats/Contents/PA/PDF/Physical%20Activity%20and%20Controlling%20Weight.pdf) to calculate the calories need to consume a day. For example, Women: BMR = 655 + (9.6 x wt in kg) + (1.8 x ht in cm) - (4.7 x age in years). For a 30 year old female, 167.6 cm tall and weigh 54.5 kg, she need 1339 calories/day.

Participants’ average sleep time is 6.98 hours a day, it is a little bit lower than the recommendation. For adults, getting less than seven hours of sleep a night on a regular basis has been linked with poor health, including weight gain, having a body mass index of 30 or higher, diabetes, high blood pressure, heart disease, stroke, and depression.

The average light activity is considerably higher than very and fairly active. [A study](https://www.dovepress.com/replacing-sedentary-time-with-physical-activity-a-15-year-follow-up-of-peer-reviewed-fulltext-article-CLEP) by Dr. Maria Hagströmer confirms that replacing sedentary time with moderate- or higher-intensity physical activity has an even greater effect on reducing deaths linked to cardiovascular disease. 

The number of sedentary time is very high more than 16 hours. The study by Dr.Maria Hagströmer also recommend that replacing sedentary time with just 10 minutes of either moderate- or vigorous-intensity activity each day was linked to a 38 percent reduced risk of death from cardiovascular disease, while 30 minutes per day was linked to a 77 percent reduction.


