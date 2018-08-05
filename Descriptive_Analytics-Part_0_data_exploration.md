Descriptive Analytics-Part 0: data exploration
================
Vasileios Tsakalos
19 October 2016

![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2016/10/download.jpgmin.jpg)

Descriptive Analytics is the examination of data or content, usually
manually performed, to answer the question “What happened?”.

This is the first set of exercise of a series of exercises that aims to
provide a descriptive analytics solution to the ‘2008’ data set from
[here](http://stat-computing.org/dataexpo/2009/the-data.html). Download
it and save it as a csv file. This data set which contains the arrival
and departure information for all domestic flights in the US from 2008
has become the “iris” data set for Big Data. In the exercises below we
cover the basics of data exploration. I chose it to be the ‘part 0’ of
the descriptive analytics solution, because in order to proceed to the
data pre-processing and then description you need to get to know your
data set while it is not formally on the value chain of descriptive
analytics process. Before proceeding, it might be helpful to look over
the help pages for the `str`, `summary`, `dim`, `nrow`, `ncol`, `names`,
`is.na`, `match` functions.

Answers to the exercises are available
[here](http://www.r-exercises.com/2016/10/19/descriptive-analytics-part-0-data-exploration-solution/).

If you obtained a different (correct) answer than those listed on the
solutions page, please feel free to post your answer as a comment on
that page.

Load the data before proceeding. Let’s name the dataset as `flights`

``` 
flights <- read.csv('2008.csv')  
```

## Exercise 1

Print the structure of the data. What do you think about it?

``` r
library(data.table)
library(R.utils)
if(!file.exists("D://2008.csv")){
  bunzip2("D://2008.csv.bz2", "D://2008.csv", remove = F)
}
flights <- fread("D://2008.csv")
```

    ## 
    Read 0.0% of 7009728 rows
    Read 4.9% of 7009728 rows
    Read 10.4% of 7009728 rows
    Read 16.0% of 7009728 rows
    Read 19.0% of 7009728 rows
    Read 23.0% of 7009728 rows
    Read 29.5% of 7009728 rows
    Read 34.8% of 7009728 rows
    Read 41.4% of 7009728 rows
    Read 47.1% of 7009728 rows
    Read 53.1% of 7009728 rows
    Read 58.6% of 7009728 rows
    Read 64.1% of 7009728 rows
    Read 64.3% of 7009728 rows
    Read 70.5% of 7009728 rows
    Read 76.9% of 7009728 rows
    Read 83.7% of 7009728 rows
    Read 91.0% of 7009728 rows
    Read 97.6% of 7009728 rows
    Read 7009728 rows and 29 (of 29) columns from 0.642 GB file in 00:00:27

``` r
str(flights)
```

    ## Classes 'data.table' and 'data.frame':   7009728 obs. of  29 variables:
    ##  $ Year             : int  2008 2008 2008 2008 2008 2008 2008 2008 2008 2008 ...
    ##  $ Month            : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ DayofMonth       : int  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ DayOfWeek        : int  4 4 4 4 4 4 4 4 4 4 ...
    ##  $ DepTime          : int  2003 754 628 926 1829 1940 1937 1039 617 1620 ...
    ##  $ CRSDepTime       : int  1955 735 620 930 1755 1915 1830 1040 615 1620 ...
    ##  $ ArrTime          : int  2211 1002 804 1054 1959 2121 2037 1132 652 1639 ...
    ##  $ CRSArrTime       : int  2225 1000 750 1100 1925 2110 1940 1150 650 1655 ...
    ##  $ UniqueCarrier    : chr  "WN" "WN" "WN" "WN" ...
    ##  $ FlightNum        : int  335 3231 448 1746 3920 378 509 535 11 810 ...
    ##  $ TailNum          : chr  "N712SW" "N772SW" "N428WN" "N612SW" ...
    ##  $ ActualElapsedTime: int  128 128 96 88 90 101 240 233 95 79 ...
    ##  $ CRSElapsedTime   : int  150 145 90 90 90 115 250 250 95 95 ...
    ##  $ AirTime          : int  116 113 76 78 77 87 230 219 70 70 ...
    ##  $ ArrDelay         : int  -14 2 14 -6 34 11 57 -18 2 -16 ...
    ##  $ DepDelay         : int  8 19 8 -4 34 25 67 -1 2 0 ...
    ##  $ Origin           : chr  "IAD" "IAD" "IND" "IND" ...
    ##  $ Dest             : chr  "TPA" "TPA" "BWI" "BWI" ...
    ##  $ Distance         : int  810 810 515 515 515 688 1591 1591 451 451 ...
    ##  $ TaxiIn           : int  4 5 3 3 3 4 3 7 6 3 ...
    ##  $ TaxiOut          : int  8 10 17 7 10 10 7 7 19 6 ...
    ##  $ Cancelled        : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ CancellationCode : chr  "" "" "" "" ...
    ##  $ Diverted         : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ CarrierDelay     : int  NA NA NA NA 2 NA 10 NA NA NA ...
    ##  $ WeatherDelay     : int  NA NA NA NA 0 NA 0 NA NA NA ...
    ##  $ NASDelay         : int  NA NA NA NA 0 NA 0 NA NA NA ...
    ##  $ SecurityDelay    : int  NA NA NA NA 0 NA 0 NA NA NA ...
    ##  $ LateAircraftDelay: int  NA NA NA NA 32 NA 47 NA NA NA ...
    ##  - attr(*, ".internal.selfref")=<externalptr>

> Function `fread` can’t not read `csv.bz2` directly. it will cause
> *mmap’d region has EOF at the end* error.

## Exercise 2

Print the `summary` statistics of the data. What do you think about the
values? (format, consistency, completeness)

``` r
summary(flights)
```

    ##       Year          Month          DayofMonth      DayOfWeek    
    ##  Min.   :2008   Min.   : 1.000   Min.   : 1.00   Min.   :1.000  
    ##  1st Qu.:2008   1st Qu.: 3.000   1st Qu.: 8.00   1st Qu.:2.000  
    ##  Median :2008   Median : 6.000   Median :16.00   Median :4.000  
    ##  Mean   :2008   Mean   : 6.375   Mean   :15.73   Mean   :3.924  
    ##  3rd Qu.:2008   3rd Qu.: 9.000   3rd Qu.:23.00   3rd Qu.:6.000  
    ##  Max.   :2008   Max.   :12.000   Max.   :31.00   Max.   :7.000  
    ##                                                                 
    ##     DepTime         CRSDepTime      ArrTime         CRSArrTime  
    ##  Min.   :   1     Min.   :   0   Min.   :   1     Min.   :   0  
    ##  1st Qu.: 928     1st Qu.: 925   1st Qu.:1107     1st Qu.:1115  
    ##  Median :1325     Median :1320   Median :1512     Median :1517  
    ##  Mean   :1334     Mean   :1326   Mean   :1481     Mean   :1495  
    ##  3rd Qu.:1728     3rd Qu.:1715   3rd Qu.:1909     3rd Qu.:1907  
    ##  Max.   :2400     Max.   :2359   Max.   :2400     Max.   :2400  
    ##  NA's   :136246                  NA's   :151649                 
    ##  UniqueCarrier        FlightNum      TailNum          ActualElapsedTime
    ##  Length:7009728     Min.   :   1   Length:7009728     Min.   :  12.0   
    ##  Class :character   1st Qu.: 622   Class :character   1st Qu.:  77.0   
    ##  Mode  :character   Median :1571   Mode  :character   Median : 110.0   
    ##                     Mean   :2224                      Mean   : 127.3   
    ##                     3rd Qu.:3518                      3rd Qu.: 157.0   
    ##                     Max.   :9743                      Max.   :1379.0   
    ##                                                       NA's   :154699   
    ##  CRSElapsedTime      AirTime          ArrDelay          DepDelay      
    ##  Min.   :-141.0   Min.   :   0     Min.   :-519.00   Min.   :-534.00  
    ##  1st Qu.:  80.0   1st Qu.:  55     1st Qu.: -10.00   1st Qu.:  -4.00  
    ##  Median : 110.0   Median :  86     Median :  -2.00   Median :  -1.00  
    ##  Mean   : 128.9   Mean   : 104     Mean   :   8.17   Mean   :   9.97  
    ##  3rd Qu.: 159.0   3rd Qu.: 132     3rd Qu.:  12.00   3rd Qu.:   8.00  
    ##  Max.   :1435.0   Max.   :1350     Max.   :2461.00   Max.   :2467.00  
    ##  NA's   :844      NA's   :154699   NA's   :154699    NA's   :136246   
    ##     Origin              Dest              Distance          TaxiIn      
    ##  Length:7009728     Length:7009728     Min.   :  11.0   Min.   :  0.00  
    ##  Class :character   Class :character   1st Qu.: 325.0   1st Qu.:  4.00  
    ##  Mode  :character   Mode  :character   Median : 581.0   Median :  6.00  
    ##                                        Mean   : 726.4   Mean   :  6.86  
    ##                                        3rd Qu.: 954.0   3rd Qu.:  8.00  
    ##                                        Max.   :4962.0   Max.   :308.00  
    ##                                                         NA's   :151649  
    ##     TaxiOut         Cancelled       CancellationCode      Diverted       
    ##  Min.   :  0.00   Min.   :0.00000   Length:7009728     Min.   :0.000000  
    ##  1st Qu.: 10.00   1st Qu.:0.00000   Class :character   1st Qu.:0.000000  
    ##  Median : 14.00   Median :0.00000   Mode  :character   Median :0.000000  
    ##  Mean   : 16.45   Mean   :0.01961                      Mean   :0.002463  
    ##  3rd Qu.: 19.00   3rd Qu.:0.00000                      3rd Qu.:0.000000  
    ##  Max.   :429.00   Max.   :1.00000                      Max.   :1.000000  
    ##  NA's   :137058                                                          
    ##   CarrierDelay      WeatherDelay        NASDelay       SecurityDelay    
    ##  Min.   :   0      Min.   :   0      Min.   :   0      Min.   :  0      
    ##  1st Qu.:   0      1st Qu.:   0      1st Qu.:   0      1st Qu.:  0      
    ##  Median :   0      Median :   0      Median :   6      Median :  0      
    ##  Mean   :  16      Mean   :   3      Mean   :  17      Mean   :  0      
    ##  3rd Qu.:  16      3rd Qu.:   0      3rd Qu.:  21      3rd Qu.:  0      
    ##  Max.   :2436      Max.   :1352      Max.   :1357      Max.   :392      
    ##  NA's   :5484993   NA's   :5484993   NA's   :5484993   NA's   :5484993  
    ##  LateAircraftDelay
    ##  Min.   :   0     
    ##  1st Qu.:   0     
    ##  Median :   0     
    ##  Mean   :  21     
    ##  3rd Qu.:  26     
    ##  Max.   :1316     
    ##  NA's   :5484993

> Too many NA’s values.

## Exercise 3

Print the dimensionality of the data (number of rows and columns)

``` r
dim(flights)
```

    ## [1] 7009728      29

## Exercise 4

Print the number of rows. This may seem like a silly command, but it is
quite useful for loops and if statements.

``` r
nrow(flights)
```

    ## [1] 7009728

## Exercise 5

Print the number of columns.

``` r
ncol(flights)
```

    ## [1] 29

## Exercise 6

Print the names of the variables.

``` r
names(flights)
```

    ##  [1] "Year"              "Month"             "DayofMonth"       
    ##  [4] "DayOfWeek"         "DepTime"           "CRSDepTime"       
    ##  [7] "ArrTime"           "CRSArrTime"        "UniqueCarrier"    
    ## [10] "FlightNum"         "TailNum"           "ActualElapsedTime"
    ## [13] "CRSElapsedTime"    "AirTime"           "ArrDelay"         
    ## [16] "DepDelay"          "Origin"            "Dest"             
    ## [19] "Distance"          "TaxiIn"            "TaxiOut"          
    ## [22] "Cancelled"         "CancellationCode"  "Diverted"         
    ## [25] "CarrierDelay"      "WeatherDelay"      "NASDelay"         
    ## [28] "SecurityDelay"     "LateAircraftDelay"

## Exercise 7

Print whether the first column has missing values (NAs). Try to answer
this question with two ways. \> Hint: %in%

``` r
# Method 1
sum(is.na(flights[, 1]))
```

    ## [1] 0

``` r
# Method 2
TRUE %in% is.na(flights[, 1])
```

    ## [1] FALSE

## Exercise 8

Print the number of variables that contain missing values.

``` r
na.cal <- sapply(flights, function(x) sum(is.na(x)))
na.cal
```

    ##              Year             Month        DayofMonth         DayOfWeek 
    ##                 0                 0                 0                 0 
    ##           DepTime        CRSDepTime           ArrTime        CRSArrTime 
    ##            136246                 0            151649                 0 
    ##     UniqueCarrier         FlightNum           TailNum ActualElapsedTime 
    ##                 0                 0                 0            154699 
    ##    CRSElapsedTime           AirTime          ArrDelay          DepDelay 
    ##               844            154699            154699            136246 
    ##            Origin              Dest          Distance            TaxiIn 
    ##                 0                 0                 0            151649 
    ##           TaxiOut         Cancelled  CancellationCode          Diverted 
    ##            137058                 0                 0                 0 
    ##      CarrierDelay      WeatherDelay          NASDelay     SecurityDelay 
    ##           5484993           5484993           5484993           5484993 
    ## LateAircraftDelay 
    ##           5484993

``` r
sum(na.cal > 0)
```

    ## [1] 14

## Exercise 9

Find the portion of the variables that contain missing values. What do
you think about it?

``` r
na.portion <- sprintf("%.2f %%", round(na.cal / nrow(flights) * 100, 2))
names(na.portion) <- names(na.cal)
na.portion
```

    ##              Year             Month        DayofMonth         DayOfWeek 
    ##          "0.00 %"          "0.00 %"          "0.00 %"          "0.00 %" 
    ##           DepTime        CRSDepTime           ArrTime        CRSArrTime 
    ##          "1.94 %"          "0.00 %"          "2.16 %"          "0.00 %" 
    ##     UniqueCarrier         FlightNum           TailNum ActualElapsedTime 
    ##          "0.00 %"          "0.00 %"          "0.00 %"          "2.21 %" 
    ##    CRSElapsedTime           AirTime          ArrDelay          DepDelay 
    ##          "0.01 %"          "2.21 %"          "2.21 %"          "1.94 %" 
    ##            Origin              Dest          Distance            TaxiIn 
    ##          "0.00 %"          "0.00 %"          "0.00 %"          "2.16 %" 
    ##           TaxiOut         Cancelled  CancellationCode          Diverted 
    ##          "1.96 %"          "0.00 %"          "0.00 %"          "0.00 %" 
    ##      CarrierDelay      WeatherDelay          NASDelay     SecurityDelay 
    ##         "78.25 %"         "78.25 %"         "78.25 %"         "78.25 %" 
    ## LateAircraftDelay 
    ##         "78.25 %"

``` r
sum(na.cal > 0) / ncol(flights)
```

    ## [1] 0.4827586

## Exercise 10

Print the names of the variables that contain missing values.

``` r
names(na.cal[na.cal > 0])
```

    ##  [1] "DepTime"           "ArrTime"           "ActualElapsedTime"
    ##  [4] "CRSElapsedTime"    "AirTime"           "ArrDelay"         
    ##  [7] "DepDelay"          "TaxiIn"            "TaxiOut"          
    ## [10] "CarrierDelay"      "WeatherDelay"      "NASDelay"         
    ## [13] "SecurityDelay"     "LateAircraftDelay"
