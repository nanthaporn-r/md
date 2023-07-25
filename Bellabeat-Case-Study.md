Bellabeat Case Study
================
Nanthaporn R.
2023-07-25

This case study is hands-on activities for Grow with Google’s Data
Analytics certificate intended for educational purposes in answering
business tasks. All data used by the author is from [FitBit Fitness
Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) (CC0:
Public Domain, dataset made available through Mobius). According to the
scenario details as follows:

**Given Scenario**: You are a junior data analyst working on the
marketing analyst team at Bellabeat, a high-tech manufacturer of
health-focused products for women. Bellabeat is a successful small
company, but they have the potential to become a larger player in the
global smart device market. Urška Sršen, cofounder and Chief Creative
Officer of Bellabeat, believes that analyzing smart device fitness data
could help unlock new growth opportunities for the company. You have
been asked to focus on one of Bellabeat’s products and analyze smart
device data to gain insight into how consumers are using their smart
devices. The insights you discover will then help guide marketing
strategy for the company. You will present your analysis to the
Bellabeat executive team along with your high-level recommendations for
Bellabeat’s marketing strategy

To answer business tasks, I will follow the steps of the data analysis
process: **ask**, **prepare**, **process**, **analyze**, **share**, and
**act**.

### 1. Ask

1)  What are some trends in smart device usage?
2)  How could these trends apply to Bellabeat customers?
3)  How could these trends help influence Bellabeat marketing strategy?

### 2. Prepare

I decide to apply the publicly available [FitBit Fitness Tracker
Data](https://www.kaggle.com/datasets/arashnic/fitbit) that explores
existing smart device users’ daily habits as a reference to discover
trends in smart device use and incorporate them into Bellabeat’s
marketing strategy.

### 3. Process

With the certain limitation and huge amount of data entries, I therefore
choose the R language programming to use for this processing and
analyzing.

#### 3.1 Upload raw data file into RStudio

Upload 3 raw data files into RStudio

- dailyAcitivity_merged
- sleepDay_merged
- weightLogInfo_merged.csv

#### 3.2 Use the `install.packages()` and `library()` functions to install and load packages once for all following steps of the entire case study

``` r
install.packages("tidyverse")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.3'
    ## (as 'lib' is unspecified)

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readr)
library(dplyr)
library(lubridate)
library(ggplot2)
```

#### 3.3 Import raw data into dataset using the `read_csv()` function

``` r
activity <- read_csv("dailyActivity_merged.csv")
```

    ## Rows: 940 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(activity)
```

    ## # A tibble: 6 × 15
    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
    ## 1 1503960366 4/12/2016         13162          8.5             8.5 
    ## 2 1503960366 4/13/2016         10735          6.97            6.97
    ## 3 1503960366 4/14/2016         10460          6.74            6.74
    ## 4 1503960366 4/15/2016          9762          6.28            6.28
    ## 5 1503960366 4/16/2016         12669          8.16            8.16
    ## 6 1503960366 4/17/2016          9705          6.48            6.48
    ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
    ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
    ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

``` r
sleep <- read_csv("sleepDay_merged.csv")
```

    ## Rows: 413 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(sleep)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
    ##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:0…                 1                327            346
    ## 2 1503960366 4/13/2016 12:0…                 2                384            407
    ## 3 1503960366 4/15/2016 12:0…                 1                412            442
    ## 4 1503960366 4/16/2016 12:0…                 2                340            367
    ## 5 1503960366 4/17/2016 12:0…                 1                700            712
    ## 6 1503960366 4/19/2016 12:0…                 1                304            320

``` r
weightlog <- read_csv("weightLogInfo_merged.csv")
```

    ## Rows: 67 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(weightlog)
```

    ## # A tibble: 6 × 8
    ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
    ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
    ## 1 1503960366 5/2/2016 …     52.6         116.    22  22.6 TRUE           1.46e12
    ## 2 1503960366 5/3/2016 …     52.6         116.    NA  22.6 TRUE           1.46e12
    ## 3 1927972279 4/13/2016…    134.          294.    NA  47.5 FALSE          1.46e12
    ## 4 2873212765 4/21/2016…     56.7         125.    NA  21.5 TRUE           1.46e12
    ## 5 2873212765 5/12/2016…     57.3         126.    NA  21.7 TRUE           1.46e12
    ## 6 4319703577 4/17/2016…     72.4         160.    25  27.5 TRUE           1.46e12

#### 3.4 Use the `typeof()` to check type of variables and `as.Date()` to convert them properly for compatibility

``` r
typeof(activity$ActivityDate)
```

    ## [1] "character"

``` r
activity$new_date <- as.Date(activity$ActivityDate, format='%m/%d/%Y')
```

``` r
sleep$SleepDay=as.POSIXct(sleep$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
sleep$SleepTime <- format(sleep$SleepDay, format = "%H:%M:%S")
sleep$SleepDate <- format(sleep$SleepDay, format = "%m/%d/%Y")

typeof(sleep$SleepDate)
```

    ## [1] "character"

``` r
sleep$new_date <- as.Date(sleep$SleepDate, format='%m/%d/%Y')
```

``` r
weightlog$Date=as.POSIXct(weightlog$Date, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
weightlog$Time <- format(weightlog$Date, format = "%H:%M:%S")
weightlog$NewDate <- format(weightlog$Date, format = "%m/%d/%y")

typeof(weightlog$NewDate)
```

    ## [1] "character"

``` r
weightlog$new_date <- as.Date(weightlog$NewDate, format='%m/%d/%Y')
```

#### 3.5 Use the `select()` function to filter useful information and required for further analysis

``` r
activity_df <- activity %>% 
  select(Id, new_date, TotalSteps, VeryActiveMinutes, SedentaryMinutes, Calories)

sleep_df <- sleep %>% 
  select(Id, new_date, TotalMinutesAsleep, TotalTimeInBed)

weightlog_df <- weightlog %>% 
  select(Id, new_date, WeightKg, BMI)
```

### 4. Analyze

#### 4.1 Use the `full_join()` function to combine 3 data frames into one

``` r
summary_activity <- activity_df %>% 
  full_join(sleep_df,
            by = c("Id"="Id", "new_date"="new_date")) %>% 
  full_join(weightlog_df,
            by = c("Id"="Id", "new_date"="new_date"))
```

#### 4.2 Use the `select()` function to select quantitative data and `summary()` function to draw preliminary summary

``` r
summary_activity %>%
  select(TotalSteps, VeryActiveMinutes, SedentaryMinutes, Calories, TotalMinutesAsleep, TotalTimeInBed, WeightKg, BMI) %>% 
  summary()
```

    ##    TotalSteps    VeryActiveMinutes SedentaryMinutes    Calories   
    ##  Min.   :    0   Min.   :  0.00    Min.   :   0.0   Min.   :   0  
    ##  1st Qu.: 3795   1st Qu.:  0.00    1st Qu.: 729.0   1st Qu.:1830  
    ##  Median : 7439   Median :  4.00    Median :1057.0   Median :2140  
    ##  Mean   : 7652   Mean   : 21.24    Mean   : 990.4   Mean   :2308  
    ##  3rd Qu.:10734   3rd Qu.: 32.00    3rd Qu.:1229.0   3rd Qu.:2796  
    ##  Max.   :36019   Max.   :210.00    Max.   :1440.0   Max.   :4900  
    ##  NA's   :67      NA's   :67        NA's   :67       NA's   :67    
    ##  TotalMinutesAsleep TotalTimeInBed     WeightKg           BMI       
    ##  Min.   : 58.0      Min.   : 61.0   Min.   : 52.60   Min.   :21.45  
    ##  1st Qu.:361.0      1st Qu.:403.0   1st Qu.: 61.40   1st Qu.:23.96  
    ##  Median :433.0      Median :463.0   Median : 62.50   Median :24.39  
    ##  Mean   :419.5      Mean   :458.6   Mean   : 72.04   Mean   :25.19  
    ##  3rd Qu.:490.0      3rd Qu.:526.0   3rd Qu.: 85.05   3rd Qu.:25.56  
    ##  Max.   :796.0      Max.   :961.0   Max.   :133.50   Max.   :47.54  
    ##  NA's   :597        NA's   :597     NA's   :943      NA's   :943

#### Key takeaways

After proceed to the analyze step, I found some key insights of Fitbit
users to help answer Bellabeat’s business questions.

- The average weight of a Fitbit user is 74.02 kg with a BMI of 25.19,
  which exceeds the standard value
- The average of daily active vs sedentary Time (minutes) differs up to
  4562.90%
- There’s a gap between total time in bed and asleep make it possible to
  interpret that some users don’t fall asleep right after going to bed

### 5. Share

#### 5.1 Use the `ggplot()` function to plot the data viz about the Fitbit Users’ Weight

``` r
ggplot(summary_activity, aes(x=WeightKg)) +
  geom_bar() + labs(title="Weight of Fitbit Users",
                    x="Weight (kg)", y="Number of user (person)")
```

    ## Warning: Removed 943 rows containing non-finite values (`stat_count()`).

![](Bellabeat-Case-Study_files/figure-gfm/plot%20data%20viz%20about%20the%20Fitbit%20users%20weight-1.png)<!-- -->

#### 5.2 Use the `ggplot()` function to plot the data viz about Daily Steps vs Calories

``` r
ggplot(data = summary_activity, aes(x=TotalSteps, y=Calories)) +
  geom_smooth(method="gam") + geom_point() + labs(title="No. of Steps vs Calories")
```

    ## `geom_smooth()` using formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 67 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 67 rows containing missing values (`geom_point()`).

![](Bellabeat-Case-Study_files/figure-gfm/plot%20data%20viz%20about%20daily%20steps%20vs%20calories-1.png)<!-- -->

#### 5.3 Use the `ggplot()` function to plot the data viz about Daily Active vs Sedentary Time (minutes)

``` r
ggplot(data = summary_activity, aes(x=VeryActiveMinutes, y=SedentaryMinutes)) +
  geom_jitter(alpha=0.5, color="purple") + labs(title="Daily Active vs Sedentary Time (minutes)")
```

    ## Warning: Removed 67 rows containing missing values (`geom_point()`).

![](Bellabeat-Case-Study_files/figure-gfm/plot%20the%20data%20viz%20about%20aaily%20active%20vs%20eedentary%20time-1.png)<!-- -->

#### 5.4 Use the `ggplot()` function to plot the data viz about Total Minutes Asleep vs Time in Bed

``` r
ggplot(data = summary_activity, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) +
  geom_point(alpha=0.2, color="blue") + labs(title="Total Minutes Asleep vs Time in Bed")
```

    ## Warning: Removed 597 rows containing missing values (`geom_point()`).

![](Bellabeat-Case-Study_files/figure-gfm/plot%20the%20data%20viz%20about%20total%20minutes%20asleep%20vs%20time%20in%20bed-1.png)<!-- -->

### 6. Act

#### Conclusion

After perform data analysis and gain insight into how people use their
smart devices from FitBit Fitness Tracker Data. There’re some suggestion
for Bellabeat marketing strategies:

1.  Add ‘Alerts function’ when users stay in one position for long
    period to remind users to change their posture during the day and
    reduce sedentary minutes
2.  Launch new smart device to improve sleep quality e.g. pillow, sleep
    eye mask, ear plug and close a gap between total time in bed vs time
    asleep

#### Next Steps

1.  Collect detailed demographic information to study specifically among
    women which are the target customers of Bellabeat
2.  Track how user drink water to analyze water intake throughout the
    day and find areas for ‘Spring’ improvement
3.  Track the food that user take at each meal to analyze proportions
    and nutrition according to weight and BMI data
