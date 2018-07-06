Basics of data.table: Smooth data exploration
================
sindri
23 August 2017

![](https://www.r-exercises.com/wp-content/uploads/2017/08/speed.jpg)

The data.table package provides perhaps the fastest way for data
wrangling in R. The syntax is concise and is made to resemble SQL. After
studying the basics of data.table and finishing this exercise set
successfully you will be able to start easing into using data.table for
all your data manipulation needs.

We will use data drawn from the 1980 US Census on married women aged
21–35 with two or more children. The data includes gender of first and
second child, as well as information on whether the woman had more than
two children, race, age and number of weeks worked in 1979. For more
information please refer to the [reference manual for the package
AER](https://cran.r-project.org/web/packages/AER/AER.pdf).

Answers are available
[here](http://www.r-exercises.com/2017/08/23/basics-of-data-table-smooth-data-exploration-solution).

## Exercise 1

Load the data.table package. Furtermore (install and) load the AER
package and run the command data(“Fertility”) which loads the dataset
Fertility to your workspace. Turn it into a data.table object.

``` r
library(data.table)
library(AER)

data("Fertility")
setDT(Fertility)

Fertility
```

    ##         morekids gender1 gender2 age afam hispanic other work
    ##      1:       no    male  female  27   no       no    no    0
    ##      2:       no  female    male  30   no       no    no   30
    ##      3:       no    male  female  27   no       no    no    0
    ##      4:       no    male  female  35  yes       no    no    0
    ##      5:       no  female  female  30   no       no    no   22
    ##     ---                                                      
    ## 254650:      yes  female  female  35   no       no    no    0
    ## 254651:      yes    male    male  29   no       no    no    0
    ## 254652:      yes  female    male  34   no       no    no   38
    ## 254653:      yes  female  female  30   no       no    no   26
    ## 254654:      yes  female  female  35   no       no    no    0

## Exercise 2

Select rows 35 to 50 and print to console its age and work entry.

``` r
Fertility[35:50, .(age, work)]
```

    ##     age work
    ##  1:  28   20
    ##  2:  33   12
    ##  3:  32    0
    ##  4:  26   52
    ##  5:  32   52
    ##  6:  28    0
    ##  7:  32   40
    ##  8:  35    0
    ##  9:  33    0
    ## 10:  32   42
    ## 11:  29    0
    ## 12:  29   52
    ## 13:  31    0
    ## 14:  30   51
    ## 15:  28    0
    ## 16:  29    0

## Exercise 3

Select the last row in the dataset and print to console.

``` r
Fertility[.N]
```

    ##    morekids gender1 gender2 age afam hispanic other work
    ## 1:      yes  female  female  35   no       no    no    0

## Exercise 4

Count how many women proceeded to have a third child.

``` r
Fertility[morekids == "yes", .N]
```

    ## [1] 96912

## Exercise 5

There are four possible gender combinations for the first two children.
Which is the most common? Use the by argument.

``` r
Fertility[, .N, by = .(gender1, gender2)]
```

    ##    gender1 gender2     N
    ## 1:    male  female 63185
    ## 2:  female    male 62724
    ## 3:  female  female 60946
    ## 4:    male    male 67799

## Exercise 6

By racial composition what is the proportion of woman working four weeks
or less in 1979?

``` r
Fertility[, mean(work <= 4), by = .(afam, hispanic, other)]
```

    ##    afam hispanic other        V1
    ## 1:   no       no    no 0.5088019
    ## 2:  yes       no    no 0.3030864
    ## 3:   no       no   yes 0.4696925
    ## 4:   no      yes    no 0.5242422
    ## 5:  yes      yes    no 0.4540816
    ## 6:   no      yes   yes 0.5060654

## Exercise 7

Use %between% to get a subset of woman between the age of 22 and 24 and
calculate the proportion who had a boy as their firstborn.

``` r
Fertility[age %between% c(22, 24), mean(gender1 == "male")]
```

    ## [1] 0.5036608

## Exercise 8

Add a new column, age squared, to the dataset.

``` r
Fertility[, age_squared := age^2]
```

## Exercise 9

Out of all the racial composition in the dataset which had the lowest
proportion of boys for their firstborn. With the same command display
the number of observation in each category as
well.

``` r
Fertility[, .(.N, mean(gender1 == "male")), by = .(afam, hispanic, other)]
```

    ##    afam hispanic other      N        V2
    ## 1:   no       no    no 216033 0.5146436
    ## 2:  yes       no    no  12960 0.5089506
    ## 3:   no       no   yes   6764 0.5198108
    ## 4:   no      yes    no  11117 0.5119187
    ## 5:  yes      yes    no    196 0.5612245
    ## 6:   no      yes   yes   7584 0.5130538

## Exercise 10

Calculate the proportion of women who have a third child by gender
combination of the first two children?

``` r
Fertility[, mean(morekids == "yes"), by = .(gender1, gender2)]
```

    ##    gender1 gender2        V1
    ## 1:    male  female 0.3463005
    ## 2:  female    male 0.3465500
    ## 3:  female  female 0.4247859
    ## 4:    male    male 0.4042095
