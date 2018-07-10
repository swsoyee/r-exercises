Data Manipulation with Data Table - Part 2
================
Biswarup Ghosh
29 June 2017

![](https://www.r-exercises.com/wp-content/uploads/2016/11/Occasional-Table-Group.jpg)

In the last set of
[exercise](http://www.r-exercises.com/2017/06/15/data-manipulation-with-data-table-part-1/)
of data.table, we saw some interesting features of data.table. In this
set we will cover some of the advanced features like set operation, join
in data.table. You should ideally complete the first part before
attempting this one.  
Answers to the exercises are available
[here](http://www.r-exercises.com/2017/06/29/data-manipulation-with-data-table-solution-part-2).

If you obtained a different (correct) answer than those listed on the
solutions page, please feel free to post your answer as a comment on
that page.

## Exercise 1

Create a `data.table` from `diamonds` dataset, create key using setkey
over `cut` and `color`. Now select first entry of the groups `Ideal` and
`Premium`.

``` r
library(data.table)
library(ggplot2)

dt <- diamonds
setDT(dt)

setkey(dt, cut, color)
dt[c("Ideal","Premium"), mult = "first"]
```

    ##    carat     cut color clarity depth table price    x    y    z
    ## 1:  0.30   Ideal     D     SI1  62.5    57   552 4.29 4.32 2.69
    ## 2:  0.22 Premium     D     VS2  59.3    62   404 3.91 3.88 2.31

## Exercise 2

With the same dataset, select the first and last entry of the groups
`Ideal` and `Premium`.

``` r
dt[c("Ideal", "Premium"), .SD[c(1, .N)], by = .EACHI]
```

    ##        cut carat color clarity depth table price    x    y    z
    ## 1:   Ideal  0.30     D     SI1  62.5    57   552 4.29 4.32 2.69
    ## 2:   Ideal  0.83     J     VS2  62.3    55  2742 6.01 6.03 3.75
    ## 3: Premium  0.22     D     VS2  59.3    62   404 3.91 3.88 2.31
    ## 4: Premium  0.90     J     SI2  63.0    59  2717 6.14 6.11 3.86

## Exercise 3

Earlier we have seen how we can create/update columns by reference using
`:=`. However there is a lower over head, faster alternative in
`data.table`. This is achieved by `SET` and `Loop` in `data.table`,
however this is meant for simple operations and will not work in grouped
operation. Now take the `diamonds` `data.table` and make columns `x`,
`y`, `z` value squared. For example if the value is currently 10, the
resulting value would be 100. You are awesome if you find out all
alternative answer and check the time using `system.time`.

``` r
system.time(dt1 <- dt[, `:=` (x_sqr = x^2, y_sqr = y^2, z_sqr = z^2)])
```

    ##    user  system elapsed 
    ##    0.01    0.00    0.01

``` r
dt1

# cols = c("x","y","z")
# system.time(for (i in cols) dt2 <- set(dt, j = i, value = dt[[i]]^2))
# dt2
```

## Exercise 4

In the same dataset, capitalize first letter of column names.

``` r
library(R.utils)
setnames(dt, names(dt), capitalize(names(dt)))
dt
```

    ##        Carat   Cut Color Clarity Depth Table Price    X    Y    Z   X_sqr
    ##     1:  0.75  Fair     D     SI2  64.6    57  2848 5.74 5.72 3.70 32.9476
    ##     2:  0.71  Fair     D     VS2  56.9    65  2858 5.89 5.84 3.34 34.6921
    ##     3:  0.90  Fair     D     SI2  66.9    57  2885 6.02 5.90 3.99 36.2404
    ##     4:  1.00  Fair     D     SI2  69.3    58  2974 5.96 5.87 4.10 35.5216
    ##     5:  1.01  Fair     D     SI2  64.6    56  3003 6.31 6.24 4.05 39.8161
    ##    ---                                                                   
    ## 53936:  0.71 Ideal     J     SI1  60.6    57  2700 5.78 5.83 3.52 33.4084
    ## 53937:  0.81 Ideal     J     VS2  62.1    56  2708 5.92 5.97 3.69 35.0464
    ## 53938:  0.84 Ideal     J     VS2  61.1    57  2709 6.09 6.12 3.73 37.0881
    ## 53939:  0.82 Ideal     J     VS2  61.6    56  2741 6.00 6.04 3.71 36.0000
    ## 53940:  0.83 Ideal     J     VS2  62.3    55  2742 6.01 6.03 3.75 36.1201
    ##          Y_sqr   Z_sqr
    ##     1: 32.7184 13.6900
    ##     2: 34.1056 11.1556
    ##     3: 34.8100 15.9201
    ##     4: 34.4569 16.8100
    ##     5: 38.9376 16.4025
    ##    ---                
    ## 53936: 33.9889 12.3904
    ## 53937: 35.6409 13.6161
    ## 53938: 37.4544 13.9129
    ## 53939: 36.4816 13.7641
    ## 53940: 36.3609 14.0625

## Exercise 5

Reordering column sometimes is necessary, however if your data frame is
of several GBs it might be a overhead to create new data frame with new
order. `Data.Table` provides features to overcome this. Now reorder your
`diamonds` `data.table`’s column by sorting alphabetically.

``` r
setcolorder(dt, sort(names(dt)))
dt
```

    ##        Carat Clarity Color   Cut Depth Price Table    X   X_sqr    Y
    ##     1:  0.75     SI2     D  Fair  64.6  2848    57 5.74 32.9476 5.72
    ##     2:  0.71     VS2     D  Fair  56.9  2858    65 5.89 34.6921 5.84
    ##     3:  0.90     SI2     D  Fair  66.9  2885    57 6.02 36.2404 5.90
    ##     4:  1.00     SI2     D  Fair  69.3  2974    58 5.96 35.5216 5.87
    ##     5:  1.01     SI2     D  Fair  64.6  3003    56 6.31 39.8161 6.24
    ##    ---                                                              
    ## 53936:  0.71     SI1     J Ideal  60.6  2700    57 5.78 33.4084 5.83
    ## 53937:  0.81     VS2     J Ideal  62.1  2708    56 5.92 35.0464 5.97
    ## 53938:  0.84     VS2     J Ideal  61.1  2709    57 6.09 37.0881 6.12
    ## 53939:  0.82     VS2     J Ideal  61.6  2741    56 6.00 36.0000 6.04
    ## 53940:  0.83     VS2     J Ideal  62.3  2742    55 6.01 36.1201 6.03
    ##          Y_sqr    Z   Z_sqr
    ##     1: 32.7184 3.70 13.6900
    ##     2: 34.1056 3.34 11.1556
    ##     3: 34.8100 3.99 15.9201
    ##     4: 34.4569 4.10 16.8100
    ##     5: 38.9376 4.05 16.4025
    ##    ---                     
    ## 53936: 33.9889 3.52 12.3904
    ## 53937: 35.6409 3.69 13.6161
    ## 53938: 37.4544 3.73 13.9129
    ## 53939: 36.4816 3.71 13.7641
    ## 53940: 36.3609 3.75 14.0625

## Exercise 6

If you are not convinced with the powerful intuitive features of
`data.table` till now, I am pretty sure you will by the end of THIS.
Suppose I want to have a metric on `diamonds` where I want to find for
each group of `cut` `maximum of x * mean of depth` and name it
`my_int_feature` and also I want another metric which is `my_int_feature
\* maximum of y` again for each group of `cut`. This is achievable by
chaining but also with a single operation without chaining which is the
expected answer.

``` r
dt <- diamonds
setDT(dt)
dt1 <- dt
# Method 1
dt1[, .(my_int_feature = my_int_feature <- max(x) * mean(depth), 
        my_int_feature2 = my_int_feature * max(y)), 
    by = cut]
```

    ##          cut my_int_feature my_int_feature2
    ## 1:      Fair       687.8076        7249.492
    ## 2:      Good       588.7339        5522.324
    ## 3: Very Good       618.8009        6150.881
    ## 4:   Premium       621.2238       36590.081
    ## 5:     Ideal       595.4957       18936.764

``` r
# Method 2
dt2 <- dt
dt2[, {m = mean(depth)
       my_int_feature = max(x) * m
       my_int_feature2 = max(y) * my_int_feature
       list("my_int_feature" = my_int_feature, 
            " my_int_feature2" = my_int_feature2
            )
       },
    by = cut]
```

    ##          cut my_int_feature  my_int_feature2
    ## 1:      Fair       687.8076         7249.492
    ## 2:      Good       588.7339         5522.324
    ## 3: Very Good       618.8009         6150.881
    ## 4:   Premium       621.2238        36590.081
    ## 5:     Ideal       595.4957        18936.764

## Exercise 7

Suppose we want to merge `iris` and `airquality`, akin to the
functionlaity of `rbind`. We want to do it fast and want to keep track
of the rows with their original dataset, and keep all the columns of
both the data set in the merged data set as well, How do we achieve
that?

``` r
iris_dt <- data.table(iris)
aq <- data.table(airquality)

rbindlist(list("iris" = iris_dt,"aq" = aq), fill = TRUE, idcol = "id")
```

    ##        id Sepal.Length Sepal.Width Petal.Length Petal.Width Species Ozone
    ##   1: iris          5.1         3.5          1.4         0.2  setosa    NA
    ##   2: iris          4.9         3.0          1.4         0.2  setosa    NA
    ##   3: iris          4.7         3.2          1.3         0.2  setosa    NA
    ##   4: iris          4.6         3.1          1.5         0.2  setosa    NA
    ##   5: iris          5.0         3.6          1.4         0.2  setosa    NA
    ##  ---                                                                     
    ## 299:   aq           NA          NA           NA          NA      NA    30
    ## 300:   aq           NA          NA           NA          NA      NA    NA
    ## 301:   aq           NA          NA           NA          NA      NA    14
    ## 302:   aq           NA          NA           NA          NA      NA    18
    ## 303:   aq           NA          NA           NA          NA      NA    20
    ##      Solar.R Wind Temp Month Day
    ##   1:      NA   NA   NA    NA  NA
    ##   2:      NA   NA   NA    NA  NA
    ##   3:      NA   NA   NA    NA  NA
    ##   4:      NA   NA   NA    NA  NA
    ##   5:      NA   NA   NA    NA  NA
    ##  ---                            
    ## 299:     193  6.9   70     9  26
    ## 300:     145 13.2   77     9  27
    ## 301:     191 14.3   75     9  28
    ## 302:     131  8.0   76     9  29
    ## 303:     223 11.5   68     9  30

## Exercise 8

The Next 3 exercises are on rolling Join like features of `data.table`,
which is useful in time series like data. Create a `data.table` with the
following

``` 
set.seed(1024)
x <- data.frame(rep(letters[1:2],6),c(1,2,3,4,6,7),sample(100,6))
names(x) <- c("id","day","value")
test_dt <- setDT(x)  
```

Now this mimics a sales data of `7` days for `a` and `b`. Notice that
day `5` is not present for both `a` and `b`. This is not desirable in
many situations, A common practise is to use the previous days data. How
do we get previous days data for the `id a`, You should ideally set keys
and do it using join features.

``` r
set.seed(1024)
x <- data.frame(rep(letters[1:2],6), c(1,2,3,4,6,7), sample(100,6))
names(x) <- c("id", "day", "value")
test_dt <- setDT(x)  
test_dt
```

    ##     id day value
    ##  1:  a   1    22
    ##  2:  b   2    98
    ##  3:  a   3    35
    ##  4:  b   4    37
    ##  5:  a   6     3
    ##  6:  b   7    72
    ##  7:  a   1    22
    ##  8:  b   2    98
    ##  9:  a   3    35
    ## 10:  b   4    37
    ## 11:  a   6     3
    ## 12:  b   7    72

``` r
setkey(test_dt, id, day)
test_dt
```

    ##     id day value
    ##  1:  a   1    22
    ##  2:  a   1    22
    ##  3:  a   3    35
    ##  4:  a   3    35
    ##  5:  a   6     3
    ##  6:  a   6     3
    ##  7:  b   2    98
    ##  8:  b   2    98
    ##  9:  b   4    37
    ## 10:  b   4    37
    ## 11:  b   7    72
    ## 12:  b   7    72

``` r
test_dt[.("a", 5), roll = TRUE]
```

    ##    id day value
    ## 1:  a   5    35

## Exercise 9

May be you dont want the previous day’s data, you may want to copy the
nearest value for day `5`, How do we achieve that?

``` r
test_dt[.("a", 5), roll = "nearest"]
```

    ##    id day value
    ## 1:  a   5     3

## Exercise 10

Now there may be a case when you don’t want to copy any value if the
date is beyond last observation. Use your answer for question 8 to find
the value for `day` `5` and `9` for `b`, Now since `9` falls beyond last
observation of 7 you might want to avoid copying it. How do you
explicitly tell your `data.table` to stop when it sees last observation
and don’t copy previous value. This may not seem useful since you know
that here `9` falls beyond `7`, but imagine you have a series of data
points and you don’t really want to copy data to observations after your
last observation. This might come handy in such cases.

``` r
test_dt[.("b", c(5, 9)), roll = TRUE, rollends = FALSE]
```

    ##    id day value
    ## 1:  b   5    37
    ## 2:  b   9    NA
