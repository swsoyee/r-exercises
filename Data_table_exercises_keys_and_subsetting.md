Data table exercises: keys and subsetting
================
Han de Vries
21 March 2016

![](https://www.r-exercises.com/wp-content/uploads/2016/03/Fotolia_20180554_XS.jpg)

The data.table package is a popular R package that facilitates fast
selections, aggregations and joins on large data sets. It is
well-documented through several vignettes, and even has its own
interactive course, offered by Datacamp. For those who want to build
some mileage practising the use of data.table, there’s good news\! In
the coming weeks, we’ll dive into the package with several exercise
sets. We’ll start with the first set today, focusing on creating
data.tables, defining keys and subsetting. Before proceeding, make sure
you have installed the data.table package from CRAN and studied the
vignettes.

Answers to the exercises are available
[here](http://www.r-exercises.com/2016/03/21/data-table-exercises-keys-and-subsetting-solutions/).
For the other (upcoming) exercise sets on data.table, check back next
week [here](http://www.r-exercises.com/tag/data-table/). If there are
any particular topics/problems related to data.table, you’d like to see
included in subsequent exercise sets, please post as a comment below.

## Exercise 1

Setup: Read the wine quality dataset from the uci repository as a
data.table (available for download from:
<http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv>)
into an object named df. To demonstrate the speed of data.table, we’re
going to make this dataset much bigger, with:

    df <- df[rep(1:nrow(df), 1000), ]

Check that the resulting data.table has 1.2 mln. rows and 12 variables.

``` r
library(data.table)

df <- fread("http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv")
df <- df[rep(1:nrow(df), 1000), ]
dim(df)
```

    ## [1] 4898000      12

``` r
df
```

    ##          fixed acidity volatile acidity citric acid residual sugar
    ##       1:           7.0             0.27        0.36           20.7
    ##       2:           6.3             0.30        0.34            1.6
    ##       3:           8.1             0.28        0.40            6.9
    ##       4:           7.2             0.23        0.32            8.5
    ##       5:           7.2             0.23        0.32            8.5
    ##      ---                                                          
    ## 4897996:           6.2             0.21        0.29            1.6
    ## 4897997:           6.6             0.32        0.36            8.0
    ## 4897998:           6.5             0.24        0.19            1.2
    ## 4897999:           5.5             0.29        0.30            1.1
    ## 4898000:           6.0             0.21        0.38            0.8
    ##          chlorides free sulfur dioxide total sulfur dioxide density   pH
    ##       1:     0.045                  45                  170 1.00100 3.00
    ##       2:     0.049                  14                  132 0.99400 3.30
    ##       3:     0.050                  30                   97 0.99510 3.26
    ##       4:     0.058                  47                  186 0.99560 3.19
    ##       5:     0.058                  47                  186 0.99560 3.19
    ##      ---                                                                
    ## 4897996:     0.039                  24                   92 0.99114 3.27
    ## 4897997:     0.047                  57                  168 0.99490 3.15
    ## 4897998:     0.041                  30                  111 0.99254 2.99
    ## 4897999:     0.022                  20                  110 0.98869 3.34
    ## 4898000:     0.020                  22                   98 0.98941 3.26
    ##          sulphates alcohol quality
    ##       1:      0.45     8.8       6
    ##       2:      0.49     9.5       6
    ##       3:      0.44    10.1       6
    ##       4:      0.40     9.9       6
    ##       5:      0.40     9.9       6
    ##      ---                          
    ## 4897996:      0.50    11.2       6
    ## 4897997:      0.46     9.6       5
    ## 4897998:      0.46     9.4       6
    ## 4897999:      0.38    12.8       7
    ## 4898000:      0.32    11.8       6

## Exercise 2

Check if `df` contains any keys. If no keys are present, create a key
for the `quality` variable. Confirm that the key has been set.

``` r
haskey(df)
```

    ## [1] FALSE

``` r
setkey(df, quality)
key(df)
```

    ## [1] "quality"

## Exercise 3

Create a new data.table `df2`, containing the subset of `df` with
`quality` equal to `9`. Use `system.time()` to measure run-time.

``` r
# Method 1
system.time(df2 <- subset(df, quality == 9))
```

    ##    user  system elapsed 
    ##    0.11    0.00    0.13

``` r
# Method 2
system.time(df2 <- df[.(9)])
```

    ##    user  system elapsed 
    ##       0       0       0

## Exercise 4

Remove the `key` from `df`, and repeat exercise 3. How much slower is
this? Now, repeat exercise 3 once more and check timing. Explain the
difference in speed.

> hint: use the key2() function.  
> key2() will be deprecated in the next relase. Please use indices()
> instead.

``` r
setkey(df, NULL)
key(df)
```

    ## NULL

``` r
indices(df) 
```

    ## NULL

``` r
system.time(df2 <- df[quality == 9, ])
```

    ##    user  system elapsed 
    ##    0.01    0.00    0.01

## Exercise 5

Create a new data.table `df2`, containing the subset of df with
`quality` equal to `7`, `8` or `9`. First without setting keys, then
with setting keys and compare run-time.

``` r
system.time(df2 <- df[quality %in% 7:9, ])
```

    ##    user  system elapsed 
    ##    0.03    0.05    0.08

``` r
setkey(df, quality)
system.time(df2 <- df[.(7:9)])
```

    ##    user  system elapsed 
    ##    0.06    0.04    0.09

## Exercise 6

Create a new data.table `df3` containing the subset of observations from
`df` with: `fixed acidity` \< 8 and `residual sugar` \< 5 and `pH` \< 3.
First without setting keys, then with setting keys and compare run-time.
Explain why differences are
small.

``` r
system.time(df3 <- df[.(`fixed acidity` < 8, `residual sugar` < 5, pH < 3)])
```

    ##    user  system elapsed 
    ##    0.42    0.29    0.72

``` r
setkeyv(df, c('fixed acidity', 'residual sugar', 'pH'))
system.time(df3 <- df[.(`fixed acidity` < 8, `residual sugar` < 5, pH < 3)])
```

    ##    user  system elapsed 
    ##    1.08    0.50    1.58

## Exercise 7

Take a bootstrap sample (i.e., with replacement) of the full df
data.table without keys, and record run-time. Then, convert to a regular
data frame, and repeat. What is the difference in speed? Is there any
(speed) benefit in creating a new variable `id` equal to the row number,
creating a key for this variable, and use this key to select the
bootstrap?

``` r
setkey(df, NULL)
system.time(df3 <- df[sample(.N, .N, replace=TRUE)])
```

    ##    user  system elapsed 
    ##    1.49    0.14    1.69

``` r
df <- as.data.frame(df)
system.time(df3 <- df[sample(nrow(df), nrow(df), replace=TRUE), ])   
```

    ##    user  system elapsed 
    ##   24.26    1.75   26.68

``` r
df$id <- 1:nrow(df)
df <- data.table(df, key='id')
system.time(df3 <- df[.(sample(.N, .N, replace=TRUE))]) 
```

    ##    user  system elapsed 
    ##    2.42    0.27    2.80
