Descriptive Analytics-Part 1: Data Formatting Exercises
================
Vasileios Tsakalos
26 October 2016

![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2016/10/download.jpgmin.jpg)

Descriptive Analytics is the examination of data or content, usually
manually performed, to answer the question “What happened?”.

This is the first set of exercise of a series of exercises that aims to
provide a descriptive analytics solution to the ‘2008’ data set from
[here](http://stat-computing.org/dataexpo/2009/the-data.html). Download
it and save it as a csv file. This data set which contains the arrival
and departure information for all domestic flights in the US from 2008
has become the “iris” data set for Big Data. In the exercises below we
will try to make the format of the dates adequate for further
processing. Before proceeding, it might be helpful to look over the help
pages for the `str_pad`, `substring`, `paste`, `chron`, `head`.

For this set of exercises you will need to install and load the packages
`stringr`, `chron`.

    install.packages('stringr')
    install.packages('chron')
    library(stringr)
    library(chron)

Answers to the exercises are available
[here](http://www.r-exercises.com/2016/10/26/descriptive-analytics-part-1-data-formatting-solutions/).

## Exercise 1

Print the first five rows of the dataset. What do you think about the
date formatting?

``` r
library(data.table)
library(R.utils)
library(stringr)
library(chron)
if(!file.exists("D://2008.csv")){
  bunzip2("D://2008.csv.bz2", "D://2008.csv", remove = F)
}
flights <- fread("D://2008.csv")
```

    ## 
    Read 0.0% of 7009728 rows
    Read 6.4% of 7009728 rows
    Read 13.1% of 7009728 rows
    Read 20.5% of 7009728 rows
    Read 26.5% of 7009728 rows
    Read 34.1% of 7009728 rows
    Read 40.1% of 7009728 rows
    Read 46.8% of 7009728 rows
    Read 52.8% of 7009728 rows
    Read 56.8% of 7009728 rows
    Read 60.5% of 7009728 rows
    Read 65.8% of 7009728 rows
    Read 66.1% of 7009728 rows
    Read 67.9% of 7009728 rows
    Read 72.2% of 7009728 rows
    Read 76.5% of 7009728 rows
    Read 81.0% of 7009728 rows
    Read 86.2% of 7009728 rows
    Read 90.9% of 7009728 rows
    Read 95.4% of 7009728 rows
    Read 98.6% of 7009728 rows
    Read 7009728 rows and 29 (of 29) columns from 0.642 GB file in 00:00:31

``` r
head(flights, n = 5)
```

    ##    Year Month DayofMonth DayOfWeek DepTime CRSDepTime ArrTime CRSArrTime
    ## 1: 2008     1          3         4    2003       1955    2211       2225
    ## 2: 2008     1          3         4     754        735    1002       1000
    ## 3: 2008     1          3         4     628        620     804        750
    ## 4: 2008     1          3         4     926        930    1054       1100
    ## 5: 2008     1          3         4    1829       1755    1959       1925
    ##    UniqueCarrier FlightNum TailNum ActualElapsedTime CRSElapsedTime
    ## 1:            WN       335  N712SW               128            150
    ## 2:            WN      3231  N772SW               128            145
    ## 3:            WN       448  N428WN                96             90
    ## 4:            WN      1746  N612SW                88             90
    ## 5:            WN      3920  N464WN                90             90
    ##    AirTime ArrDelay DepDelay Origin Dest Distance TaxiIn TaxiOut Cancelled
    ## 1:     116      -14        8    IAD  TPA      810      4       8         0
    ## 2:     113        2       19    IAD  TPA      810      5      10         0
    ## 3:      76       14        8    IND  BWI      515      3      17         0
    ## 4:      78       -6       -4    IND  BWI      515      3       7         0
    ## 5:      77       34       34    IND  BWI      515      3      10         0
    ##    CancellationCode Diverted CarrierDelay WeatherDelay NASDelay
    ## 1:                         0           NA           NA       NA
    ## 2:                         0           NA           NA       NA
    ## 3:                         0           NA           NA       NA
    ## 4:                         0           NA           NA       NA
    ## 5:                         0            2            0        0
    ##    SecurityDelay LateAircraftDelay
    ## 1:            NA                NA
    ## 2:            NA                NA
    ## 3:            NA                NA
    ## 4:            NA                NA
    ## 5:             0                32

## Exercise 2

Create a new objected named `dep_time` and assign the values of
`flights$DepTime` . If the value is less than 4 elements, fill make it a
4-element value with zeros. For example, 123 -\> 0123.

``` r
dep_time <- str_pad(flights$DepTime, width = 4, side = "left", pad = 0)
head(dep_time)
```

    ## [1] "2003" "0754" "0628" "0926" "1829" "1940"

## Exercise 3

Create a new object named `hour` and assign the first two elements of
the `dep_time` object. Can you figure out why I am asking that?

``` r
hour <- substr(dep_time, 1, 2)
head(hour)
```

    ## [1] "20" "07" "06" "09" "18" "19"

## Exercise 4

Create a new object named `minutes` and assign the last two elements of
the `dep_time` object.

``` r
minutes <- substr(dep_time, 3, 4)
head(minutes)
```

    ## [1] "03" "54" "28" "26" "29" "40"

## Exercise 5

Assign to the object `dep_time` the hour in format ‘HH:MM:SS’ , seconds
should be ‘00’ , we make this assumption for the sake of
formatting.

``` r
dep_time <- paste(substr(dep_time, 1, 2), substr(dep_time, 3, 4), "00", sep = ":")
head(dep_time)
```

    ## [1] "20:03:00" "07:54:00" "06:28:00" "09:26:00" "18:29:00" "19:40:00"

## Exercise 6

Change the class of `dep_time` from character to times.

``` r
dep_time <- chron(times. = dep_time)
```

    ## Warning in convert.times(times., fmt): 强制改变过程中产生了NA
    
    ## Warning in convert.times(times., fmt): 强制改变过程中产生了NA

    ## Warning in convert.times(times., fmt): 136767 time-of-day entries out of
    ## range set to NA

``` r
head(dep_time)
```

    ## [1] 20:03:00 07:54:00 06:28:00 09:26:00 18:29:00 19:40:00

## Exercise 7

Print the first 10 rows and then the 10 last rows of the `dep_time`. If
the formatting of the object is ‘HH:MM:SS’(as it should) then assign the
`dep_time` to
    `flights$DepTime`.

``` r
head(dep_time, n = 10)
```

    ##  [1] 20:03:00 07:54:00 06:28:00 09:26:00 18:29:00 19:40:00 19:37:00
    ##  [8] 10:39:00 06:17:00 16:20:00

``` r
tail(dep_time, n = 10)
```

    ##  [1] 10:07:00 06:38:00 07:56:00 06:12:00 07:49:00 10:02:00 08:34:00
    ##  [8] 06:55:00 12:51:00 11:10:00

``` r
flights$DepTime <- dep_time
```

## Exercise 8

Do the exact same process for the `flights$ArrTime` and create the
variable `arr_time`.

``` r
format.time <- function(x) {
  # Padding
  x <- str_pad(x, width = 4, side = "left", pad = 0)
  # Spliting
  x <- paste(substr(x, 1, 2), substr(x, 3, 4), "00", sep = ":")
  # Converting
  x <- chron(times. = x)
  # Return
  return(x)
}
arr_time <- format.time(flights$ArrTime)
```

    ## Warning in convert.times(times., fmt): 强制改变过程中产生了NA
    
    ## Warning in convert.times(times., fmt): 强制改变过程中产生了NA

    ## Warning in convert.times(times., fmt): 154326 time-of-day entries out of
    ## range set to NA

``` r
head(arr_time)
```

    ## [1] 22:11:00 10:02:00 08:04:00 10:54:00 19:59:00 21:21:00

``` r
tail(arr_time)
```

    ## [1] 09:01:00 12:04:00 10:21:00 08:56:00 14:46:00 14:13:00

``` r
flights$ArrTime <- arr_time
```

## Exercise 9

Do the exact same process for the `flights$CRSDepTime` and create the
variable `crs_dep_time`.

``` r
crs_dep_time <- format.time(flights$CRSDepTime)
head(crs_dep_time)
```

    ## [1] 19:55:00 07:35:00 06:20:00 09:30:00 17:55:00 19:15:00

``` r
tail(crs_dep_time)
```

    ## [1] 07:50:00 09:59:00 08:35:00 07:00:00 12:40:00 11:03:00

``` r
flights$CRSDepTime <- crs_dep_time
```

## Exercise 10

Do the exact same process for the `flights$CRSArrTime` and create the
variable
    `crs_arr_time`.

``` r
crs_arr_time <- format.time(flights$CRSArrTime)
```

    ## Warning in convert.times(times., fmt): 547 time-of-day entries out of range
    ## set to NA

``` r
head(crs_arr_time)
```

    ## [1] 22:25:00 10:00:00 07:50:00 11:00:00 19:25:00 21:10:00

``` r
tail(crs_arr_time)
```

    ## [1] 08:59:00 11:50:00 10:23:00 08:56:00 14:37:00 14:18:00

``` r
flights$CRSArrTime <- crs_arr_time
```
