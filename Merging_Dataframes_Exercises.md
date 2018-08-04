Merging Dataframes Exercises
================
John Akwei
14 April 2016

![](http://www.r-exercises.com/wp-content/uploads/2016/04/Merge_Vertical_2_Blue.gif)

When combining separate dataframes, (in the R programming language),
into a single dataframe, using the cbind() function usually requires use
of the “Match()” function. To simulate the database joining
functionality in SQL, the “Merge()” function in R accomplishes dataframe
merging with the following protocols;

“Inner Join” where the left table has matching rows from one, or more,
key variables from the right table. “Outer Join” where all the rows from
both tables are joined. “Left Join” where all rows from the left table,
and any rows with matching keys from the right table are returned.
“Right Join” where all rows from the right table, and any rows with
matching keys from the left table are returned.

Answers to the exercises are available
[here](http://www.r-exercises.com/2016/4/14/merging-dataframes-solutions/).

## Exercise 1

Create the dataframes to
    merge:

    buildings <- data.frame(location=c(1, 2, 3), name=c("building1", "building2", "building3"))
    data <- data.frame(survey=c(1,1,1,2,2,2), location=c(1,2,3,2,3,1),
    efficiency=c(51,64,70,71,80,58))

The dataframes, buildings and data have a common key variable called,
`location`. Use the `merge()` function to merge the two dataframes by
`location`, into a new dataframe, `buildingStats`.

``` r
library(data.table)
buildings <- data.frame(location = c(1, 2, 3), 
                        name = c("building1", "building2", "building3")
                        )
data <- data.frame(survey = c(1,1,1,2,2,2), 
                   location = c(1,2,3,2,3,1),
                   efficiency=c(51,64,70,71,80,58)
                   )
head(buildings)
```

    ##   location      name
    ## 1        1 building1
    ## 2        2 building2
    ## 3        3 building3

``` r
head(data)
```

    ##   survey location efficiency
    ## 1      1        1         51
    ## 2      1        2         64
    ## 3      1        3         70
    ## 4      2        2         71
    ## 5      2        3         80
    ## 6      2        1         58

``` r
buildingStats <- merge(x = buildings, y = data, by = "location")
buildingStats
```

    ##   location      name survey efficiency
    ## 1        1 building1      1         51
    ## 2        1 building1      2         58
    ## 3        2 building2      1         64
    ## 4        2 building2      2         71
    ## 5        3 building3      1         70
    ## 6        3 building3      2         80

## Exercise 2

Give the dataframes different key variable
    names:

    buildings <- data.frame(location=c(1, 2, 3), name=c("building1", "building2", "building3"))
    data <- data.frame(survey=c(1,1,1,2,2,2), LocationID=c(1,2,3,2,3,1),
    efficiency=c(51,64,70,71,80,58))

The dataframes, buildings and data now have corresponding variables
called, `location`, and `LocationID`. Use the `merge()` function to
merge the columns of the two dataframes by the corresponding variables.

``` r
buildings <- data.frame(location = c(1, 2, 3), 
                        name = c("building1", "building2", "building3")
                        )
data <- data.frame(survey = c(1,1,1,2,2,2), 
                   LocationID = c(1,2,3,2,3,1),
                   efficiency = c(51,64,70,71,80,58)
                   )
merge(x = buildings, y = data, by.x = "location", by.y = "LocationID")
```

    ##   location      name survey efficiency
    ## 1        1 building1      1         51
    ## 2        1 building1      2         58
    ## 3        2 building2      1         64
    ## 4        2 building2      2         71
    ## 5        3 building3      1         70
    ## 6        3 building3      2         80

## Exercise 3

Inner Join: The R `merge()` function automatically joins the frames by
common variable names. In that case, demonstrate how you would perform
the merge in Exercise 1 without specifying the key variable.

``` r
buildings <- data.frame(location = c(1, 2, 3), 
                        name = c("building1", "building2", "building3")
                        )
data <- data.frame(survey = c(1,1,1,2,2,2), 
                   location = c(1,2,3,2,3,1),
                   efficiency=c(51,64,70,71,80,58)
                   )
merge(x = buildings, y = data)
```

    ##   location      name survey efficiency
    ## 1        1 building1      1         51
    ## 2        1 building1      2         58
    ## 3        2 building2      1         64
    ## 4        2 building2      2         71
    ## 5        3 building3      1         70
    ## 6        3 building3      2         80

## Exercise 4

Outer Join: Merge the two dataframes from Exercise 1. Use the `all =`
parameter in the `merge()` function to return all records from both
tables. Also, merge with the key variable, `location`.

``` r
merge(x = buildings, y = data, by = "location", all = TRUE)
```

    ##   location      name survey efficiency
    ## 1        1 building1      1         51
    ## 2        1 building1      2         58
    ## 3        2 building2      1         64
    ## 4        2 building2      2         71
    ## 5        3 building3      1         70
    ## 6        3 building3      2         80

## Exercise 5

Left Join: Merge the two dataframes from Exercise 1, and return all rows
from the left table. Specify the matching key from Exercise 1.

``` r
merge(x = buildings, y = data, by = "location", all.x = TRUE)
```

    ##   location      name survey efficiency
    ## 1        1 building1      1         51
    ## 2        1 building1      2         58
    ## 3        2 building2      1         64
    ## 4        2 building2      2         71
    ## 5        3 building3      1         70
    ## 6        3 building3      2         80

## Exercise 6

Right Join: Merge the two dataframes from Exercise 1, and return all
rows from the right table. Use the matching key from Exercise 1 to
return matching rows from the left table.

``` r
merge(x = buildings, y = data, by = "location", all.y = TRUE)
```

    ##   location      name survey efficiency
    ## 1        1 building1      1         51
    ## 2        1 building1      2         58
    ## 3        2 building2      1         64
    ## 4        2 building2      2         71
    ## 5        3 building3      1         70
    ## 6        3 building3      2         80

## Exercise 7

Cross Join: Merge the two dataframes from Exercise 1, into a “Cross
Join” with each row of `buildings` matched to each row of `data`. What
new column names are created in `buildingStats`?

``` r
merge(x = buildings, y = data, by = NULL)
```

    ##    location.x      name survey location.y efficiency
    ## 1           1 building1      1          1         51
    ## 2           2 building2      1          1         51
    ## 3           3 building3      1          1         51
    ## 4           1 building1      1          2         64
    ## 5           2 building2      1          2         64
    ## 6           3 building3      1          2         64
    ## 7           1 building1      1          3         70
    ## 8           2 building2      1          3         70
    ## 9           3 building3      1          3         70
    ## 10          1 building1      2          2         71
    ## 11          2 building2      2          2         71
    ## 12          3 building3      2          2         71
    ## 13          1 building1      2          3         80
    ## 14          2 building2      2          3         80
    ## 15          3 building3      2          3         80
    ## 16          1 building1      2          1         58
    ## 17          2 building2      2          1         58
    ## 18          3 building3      2          1         58

## Exercise 8

Merging Dataframe rows: To join two data frames (datasets) vertically,
use the `rbind` function. The two data frames must have the same
variables, but they do not have to be in the same order.

Merge the rows of the following two
    dataframes:

    buildings <- data.frame(location=c(1, 2, 3), name=c("building1", "building2", "building3"))
    buildings2 <- data.frame(location=c(5, 4, 6), name=c("building5", "building4", "building6"))

Also, specify a new dataframe, `allBuidings`.

``` r
buildings <- data.frame(location = c(1, 2, 3), 
                        name = c("building1", "building2", "building3")
                        )
buildings2 <- data.frame(location = c(5, 4, 6), 
                         name = c("building5", "building4", "building6")
                         )
allBuidings <- rbind(buildings, buildings2)
allBuidings
```

    ##   location      name
    ## 1        1 building1
    ## 2        2 building2
    ## 3        3 building3
    ## 4        5 building5
    ## 5        4 building4
    ## 6        6 building6

## Exercise 9

A new dataframe, `buildings3`, has variables not found in the previous
dataframes

    buildings3 <- data.frame(location=c(7, 8, 9),
    name=c("building7", "building8", "building9"),
    startEfficiency=c(75,87,91))

Create a new `buildings3` without the extra variables.

``` r
buildings3 <- data.frame(location = c(7, 8, 9),
                         name = c("building7", "building8", "building9"),
                         startEfficiency = c(75,87,91)
                         )
buildings3[, -3]
```

    ##   location      name
    ## 1        7 building7
    ## 2        8 building8
    ## 3        9 building9

## Exercise 10

Instead of deleting the extra variables from `buildings3`. append the
`buildings`, and `buildings2` with the new variable in `buildings3`,
(from Exercise 9). Set the new data in `buildings` and `buildings2`,
(from Exercise 8), to `NA`.

``` r
buildings$startEfficiency <- NA
buildings2$startEfficiency <- NA
```
