Fighting Factors With Cats: Exercises
================
sindri
20 July 2018

![](https://www.r-exercises.com/wp-content/uploads/2018/07/logo.png)

In this exercise set, we will practice using the forcats factor
manipulation package by Hadley Wickham. In the [last exercise
set](https://www.r-exercises.com/2018/07/06/facing-the-facts-about-factors-exercises/),
we saw that it is entirely possible to deal with factors in base R, but
also that things can get a bit involved and un-intuitive. Forcats
simplifies many common factor manipulation tasks. It is worth mastering
if you cannot avoid using factors in your work. Also, study the package
and then its [source code](https://github.com/tidyverse/forcats) can
give you ideas for writing your own custom function to simplify everyday
tasks that you think can be dealt with in a better way.

Solutions are available
[here](https://www.r-exercises.com/2018/07/20/fighting-factors-with-cats-solutions).

## Exercise 1

Load the `gapminder` data-set from the `gapminder` package, as well as
forcats. Check what the levels of the continent factor variable are and
their frequency in the data.

``` r
library(gapminder)
library(forcats)

# Method 1
library(data.table)
gp <- gapminder
setDT(gp)
gp[, .N, by = continent][order(-N)]
```

    ##    continent   N
    ## 1:    Africa 624
    ## 2:      Asia 396
    ## 3:    Europe 360
    ## 4:  Americas 300
    ## 5:   Oceania  24

``` r
# forcats Method
fct_count(gp$continent)
```

    ## # A tibble: 5 x 2
    ##   f            n
    ##   <fct>    <int>
    ## 1 Africa     624
    ## 2 Americas   300
    ## 3 Asia       396
    ## 4 Europe     360
    ## 5 Oceania     24

## Exercise 2

Notice that one continent, Antarctica, is missing – add it as the last
level of six.

``` r
# Method 1
# levels(gp$continent) <- c(levels(gp$continent), "Antartica")
# gp[, .N, by = continent][order(-N)]

# forcats Method
gp$continent <- fct_expand(gp$continent, "Antarctica")
fct_count(gp$continent)
```

    ## # A tibble: 6 x 2
    ##   f              n
    ##   <fct>      <int>
    ## 1 Africa       624
    ## 2 Americas     300
    ## 3 Asia         396
    ## 4 Europe       360
    ## 5 Oceania       24
    ## 6 Antarctica     0

## Exercise 3

Actually, you change your mind. There is no permanent human population
on Antarctica. Drop this (unused) level from your factor.

``` r
# Method 1
# gp$continent <- droplevels(gp$continent)
# Method 2
# gp$continent <- factor(gp$continent)
# Method 3
# gp$continent  <- gp$continent[, drop = TRUE]
# forcats Method
gp$continent <- fct_drop(gp$continent)
fct_count(gp$continent)
```

    ## # A tibble: 5 x 2
    ##   f            n
    ##   <fct>    <int>
    ## 1 Africa     624
    ## 2 Americas   300
    ## 3 Asia       396
    ## 4 Europe     360
    ## 5 Oceania     24

## Exercise 4

Again, modify the continent factor, making it more precise. Add two new
levels: instead of Americas, add North America and South America. The
countries in the following vector should be classified as South America
and the rest as North America.

    c("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador",
    "Paraguay", "Peru", "Uruguay", "Venezuela")

``` r
gp$continent <- fct_expand(gp$continent, "South America", "North America")
sAmerica <- c("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador",
"Paraguay", "Peru", "Uruguay", "Venezuela")

gp$continent[gp$country %in% sAmerica] <- "South America"
gp$continent[gp$continent == "Americas"] <- "North America"

gp$continent <- fct_drop(gp$continent)
fct_count(gp$continent)
```

    ## # A tibble: 6 x 2
    ##   f                 n
    ##   <fct>         <int>
    ## 1 Africa          624
    ## 2 Asia            396
    ## 3 Europe          360
    ## 4 Oceania          24
    ## 5 South America   120
    ## 6 North America   180

## Exercise 5

Arrange the levels of the continent factor in alphabetical order.

``` r
# Method 1
# levels(gp$continent)
# gp$continent <- factor(gp$continent, levels = sort(levels(gp$continent)))
# levels(gp$continent)
# forcats Method
gp$continent <- fct_relevel(gp$continent, sort(levels(gp$continent)))
fct_count(gp$continent)
```

    ## # A tibble: 6 x 2
    ##   f                 n
    ##   <fct>         <int>
    ## 1 Africa          624
    ## 2 Asia            396
    ## 3 Europe          360
    ## 4 North America   180
    ## 5 Oceania          24
    ## 6 South America   120

## Exercise 6

Re-order the continent levels again so that they appear in order of
total population in 2007.

``` r
gp$continent <- fct_relevel(gp$continent, 
                            levels(gp[year == 2007,                         
                                      sum(as.numeric(pop)), 
                                      by = continent][order(-V1)]$continent
                                   )
                            )
fct_count(gp$continent)
```

    ## # A tibble: 6 x 2
    ##   f                 n
    ##   <fct>         <int>
    ## 1 Africa          624
    ## 2 Asia            396
    ## 3 Europe          360
    ## 4 North America   180
    ## 5 Oceania          24
    ## 6 South America   120

## Exercise 7

Reverse the order of the factors.

``` r
gp$continent <- fct_rev(gp$continent)
fct_count(gp$continent)
```

    ## # A tibble: 6 x 2
    ##   f                 n
    ##   <fct>         <int>
    ## 1 South America   120
    ## 2 Oceania          24
    ## 3 North America   180
    ## 4 Europe          360
    ## 5 Asia            396
    ## 6 Africa          624

## Exercise 8

Make continents, again, an un-ordered factor. Set North America as the
first level, therefore interpreted as a reference group in modeling
functions such as `lm()`.

``` r
gp$continent <- fct_relevel(gp$continent, "North America")
```

## Exercise 9

Turn the following messy vector into a factor with two levels: “Female”
and “Male” using the factor function. Use the labels argument in the
`factor()`
function.

``` 
gender <- c("f", "m ", "male ","male", "female", "FEMALE", "Male", "f", "m")  
```

``` r
# Method 1
# gender <- c("f", "m ", "male ","male", "female", "FEMALE", "Male", "f", "m")
# g <- tolower(substr(gender, 1, 1))
# g <- as.factor(ifelse(g == "f", "Female", "Male"))

# forcats Method 
gender <- c("f", "m ", "male ","male", "female", "FEMALE", "Male", "f", "m")
gender <- as_factor(gender)
gender <- fct_collapse(
  gender,
  Female = c("f", "female", "FEMALE"),
  Male   = c("m ", "m", "male ", "male", "Male")
)
fct_count(gender)
```

    ## # A tibble: 2 x 2
    ##   f          n
    ##   <fct>  <int>
    ## 1 Female     4
    ## 2 Male       5

## Exercise 10

Gender can be considered sensitive data. Convert the gender variable
into a factor that takes the integer values “1” and “2”, where one
integer represents female and the other male, but make the choice
randomly.

``` r
# Method 1
# choice <- c(1,2)
# Gender Male
# g1 <- sample(choice, 1, replace = TRUE)
# Gender Female
# g2 <- choice[!g1 == choice]
# factor(ifelse(gender == "Male", g1, g2))

# forcats Method 
gender <- fct_anon(gender)
fct_count(gender)
```

    ## # A tibble: 2 x 2
    ##   f         n
    ##   <fct> <int>
    ## 1 1         4
    ## 2 2         5
