Sharpening The Knives in The data.table Toolbox: Exercises
================
sindri
8 June 2018

![](https://www.r-exercises.com/wp-content/uploads/2018/06/tools_med.jpg)

If knowledge is power, then knowledge of data.table is something of a
super power, at least in the realm of data manipulation in R.

In this exercise set, we will use some of the more obscure functions
from the data.table package. The solutions will use set(), inrange(),
chmatch(), uniqueN(), tstrsplit(), rowid(), shift(), copy(), address(),
setnames() and last(). You are free to use more, as long as they are
part of data.table. The objective is to get (more) familiar with these
functions and be able to call on them in real-life, giving us fewer
reasons to leave the fast and neat data.table universe.

Solutions are available
[here](http://r-exercises.com/2018/06/08/sharpening-the-knives-in-the-data-table-toolbox-solutions/).

PS. If you are unfamiliar with data.table, we recommend you start with
the exercises covering the basics of
[data.table](https://www.r-exercises.com/2017/08/23/basics-of-data-table-smooth-data-exploration/).

## Exercise 1

Load the gapminder data-set from the gapminder package. Save it to an
object called “gp” and convert it to a data.table. How many different
countries are covered by the data?

``` r
library(gapminder)
library(data.table)

gp <- gapminder
setDT(gp)

gp
```

    ##           country continent year lifeExp      pop gdpPercap
    ##    1: Afghanistan      Asia 1952  28.801  8425333  779.4453
    ##    2: Afghanistan      Asia 1957  30.332  9240934  820.8530
    ##    3: Afghanistan      Asia 1962  31.997 10267083  853.1007
    ##    4: Afghanistan      Asia 1967  34.020 11537966  836.1971
    ##    5: Afghanistan      Asia 1972  36.088 13079460  739.9811
    ##   ---                                                      
    ## 1700:    Zimbabwe    Africa 1987  62.351  9216418  706.1573
    ## 1701:    Zimbabwe    Africa 1992  60.377 10704340  693.4208
    ## 1702:    Zimbabwe    Africa 1997  46.809 11404948  792.4500
    ## 1703:    Zimbabwe    Africa 2002  39.989 11926563  672.0386
    ## 1704:    Zimbabwe    Africa 2007  43.487 12311143  469.7093

``` r
gp[, uniqueN(country)]
```

    ## [1] 142

## Exercise 2

Create a lag term for GDP per capita. That is the value of GDP at the
last observation (which are 5 years apart) for each country.

``` r
gp[, gdpPercap_lag := shift(gdpPercap), by = country]
gp
```

    ##           country continent year lifeExp      pop gdpPercap gdpPercap_lag
    ##    1: Afghanistan      Asia 1952  28.801  8425333  779.4453            NA
    ##    2: Afghanistan      Asia 1957  30.332  9240934  820.8530      779.4453
    ##    3: Afghanistan      Asia 1962  31.997 10267083  853.1007      820.8530
    ##    4: Afghanistan      Asia 1967  34.020 11537966  836.1971      853.1007
    ##    5: Afghanistan      Asia 1972  36.088 13079460  739.9811      836.1971
    ##   ---                                                                    
    ## 1700:    Zimbabwe    Africa 1987  62.351  9216418  706.1573      788.8550
    ## 1701:    Zimbabwe    Africa 1992  60.377 10704340  693.4208      706.1573
    ## 1702:    Zimbabwe    Africa 1997  46.809 11404948  792.4500      693.4208
    ## 1703:    Zimbabwe    Africa 2002  39.989 11926563  672.0386      792.4500
    ## 1704:    Zimbabwe    Africa 2007  43.487 12311143  469.7093      672.0386

## Exercise 3

Using the data.table syntax, calculate the GDP per capita growth from
2002 to 2007 for each country. Extract the one with the highest value
for each
continent.

``` r
gp[year == 2007, .(country, continent, growth = (gdpPercap / gdpPercap_lag) - 1)][order(growth), .(country = last(country), growth = last(growth)), continent]
```

    ##    continent             country    growth
    ## 1:      Asia            Cambodia 0.9122171
    ## 2:    Africa              Angola 0.7297996
    ## 3:  Americas Trinidad and Tobago 0.5713408
    ## 4:    Europe          Montenegro 0.4112585
    ## 5:   Oceania           Australia 0.1221208

## Exercise 4

Save the column names in a vector named “temp” and change the name of
the year column in “gp” to “anno” (just because); print the temp. Oh my,
what just happened? Check the memory address of temp and names(gp),
respectively.

``` r
temp <- names(gp)
setnames(gp, "year", "anno")
temp
```

    ## [1] "country"       "continent"     "anno"          "lifeExp"      
    ## [5] "pop"           "gdpPercap"     "gdpPercap_lag"

``` r
address(temp)
```

    ## [1] "0000000017F34B28"

``` r
address(names(gp))
```

    ## [1] "0000000017F34B28"

## Exercise 5

Overwrite “gp” with the original data again. Now make a copy passed by
value into temp (before you change the year to anno) so you can keep the
original variable names. Check the addresses again. Also, change factors
to characters and don’t forget to convert to data.table again.

``` r
gp <- gapminder
setDT(gp)
temp <- copy(names(gp))
setnames(gp, "year", "anno")
temp
```

    ## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"

``` r
names(gp)
```

    ## [1] "country"   "continent" "anno"      "lifeExp"   "pop"       "gdpPercap"

``` r
address(temp)
```

    ## [1] "00000000180E7C70"

``` r
address(names(gp))
```

    ## [1] "000000001811BA20"

``` r
str(gp)
```

    ## Classes 'data.table' and 'data.frame':   1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ anno     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...
    ##  - attr(*, ".internal.selfref")=<externalptr>

``` r
factcols <- sapply(gp, is.factor)
factcols <- names(factcols)[factcols]
str(gp[, (factcols) := lapply(.SD, as.character), .SDcols = factcols])
```

    ## Classes 'data.table' and 'data.frame':   1704 obs. of  6 variables:
    ##  $ country  : chr  "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
    ##  $ continent: chr  "Asia" "Asia" "Asia" "Asia" ...
    ##  $ anno     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...
    ##  - attr(*, ".internal.selfref")=<externalptr>

## Exercise 6

A data.table of the number of goals each team in group A made in the
FIFA world championship is given below. Import this into R and add a
column with the countries’ population in 2017 to the data.table, rounded
to the nearest million.

gA\_2014 \<- data.table(  
country = c(“Brazil”, “Mexico”, “Croatia”, “Cameroon”),  
goals2014 = c(7, 4, 6, 1)  
)  
gA\_2014  
country goals2014  
1: Brazil 7  
2: Mexico 4  
3: Croatia 6  
4: Cameroon 1

``` r
gA_2014 <- data.table(  
  country   = c("Brazil", "Mexico", "Croatia", "Cameroon"),    
  goals2014 = c(7, 4, 6, 1)  
) 
gA_2014
```

    ##     country goals2014
    ## 1:   Brazil         7
    ## 2:   Mexico         4
    ## 3:  Croatia         6
    ## 4: Cameroon         1

``` r
ex6 <- merge(gA_2014, gp[anno == "2007"], by = "country")[, .(country, goals2014, pop_mill = round(pop / 1e6))]
ex6
```

    ##     country goals2014 pop_mill
    ## 1:   Brazil         7      190
    ## 2: Cameroon         1       18
    ## 3:  Croatia         6        4
    ## 4:   Mexico         4      109

## Exercise 7

Calculate the number of years since the country reached $8k in GDP per
capita at each relevant observation as accurately as the data allows.

``` r
gp <- gp[order(country, anno)]
gp[, years_from8k := anno - anno[which(gdpPercap >= 8e3)[1]], by = country
   ][years_from8k < 0, years_from8k := NA]
gp
```

    ##           country continent anno lifeExp      pop gdpPercap years_from8k
    ##    1: Afghanistan      Asia 1952  28.801  8425333  779.4453           NA
    ##    2: Afghanistan      Asia 1957  30.332  9240934  820.8530           NA
    ##    3: Afghanistan      Asia 1962  31.997 10267083  853.1007           NA
    ##    4: Afghanistan      Asia 1967  34.020 11537966  836.1971           NA
    ##    5: Afghanistan      Asia 1972  36.088 13079460  739.9811           NA
    ##   ---                                                                   
    ## 1700:    Zimbabwe    Africa 1987  62.351  9216418  706.1573           NA
    ## 1701:    Zimbabwe    Africa 1992  60.377 10704340  693.4208           NA
    ## 1702:    Zimbabwe    Africa 1997  46.809 11404948  792.4500           NA
    ## 1703:    Zimbabwe    Africa 2002  39.989 11926563  672.0386           NA
    ## 1704:    Zimbabwe    Africa 2007  43.487 12311143  469.7093           NA

## Exercise 8

Add a subtly different variable using rowid(). That is the number of the
observations among observations where the GDP is below 8k up to and
including the given observation. Which country, in each continent, has
the most observations above 8k? If there are ties, then list all of the
those tied at the top.

``` r
gp[gdpPercap >= 8e3, obs8k_numb := rowid(country)]
gp[anno == 2007 & !is.na(obs8k_numb)
   ][order(obs8k_numb),
     .(country[obs8k_numb == max(obs8k_numb)], obs8k_numb[obs8k_numb == max(obs8k_numb)]),
     continent
     ]
```

    ##     continent             V1 V2
    ##  1:  Americas         Canada 12
    ##  2:  Americas  United States 12
    ##  3:    Africa          Gabon  9
    ##  4:    Africa          Libya  9
    ##  5:    Europe        Belgium 12
    ##  6:    Europe        Denmark 12
    ##  7:    Europe    Netherlands 12
    ##  8:    Europe         Norway 12
    ##  9:    Europe         Sweden 12
    ## 10:    Europe    Switzerland 12
    ## 11:    Europe United Kingdom 12
    ## 12:      Asia        Bahrain 12
    ## 13:      Asia         Kuwait 12
    ## 14:   Oceania      Australia 12
    ## 15:   Oceania    New Zealand 12

## Exercise 9

Use inrange() to extract countries that have their life expectancy
either below 40 or above 80 in 2002.

``` r
gp[anno == 2002 & !inrange(lifeExp, 40, 80)]
```

    ##             country continent anno lifeExp       pop  gdpPercap
    ## 1:        Australia   Oceania 2002  80.370  19546792 30687.7547
    ## 2: Hong Kong, China      Asia 2002  81.495   6762476 30209.0152
    ## 3:          Iceland    Europe 2002  80.500    288030 31163.2020
    ## 4:            Italy    Europe 2002  80.240  57926999 27968.0982
    ## 5:            Japan      Asia 2002  82.000 127065841 28604.5919
    ## 6:           Sweden    Europe 2002  80.040   8954175 29341.6309
    ## 7:      Switzerland    Europe 2002  80.620   7361757 34480.9577
    ## 8:           Zambia    Africa 2002  39.193  10595811  1071.6139
    ## 9:         Zimbabwe    Africa 2002  39.989  11926563   672.0386
    ##    years_from8k obs8k_numb
    ## 1:           50         11
    ## 2:           30          7
    ## 3:           45         10
    ## 4:           40          9
    ## 5:           35          8
    ## 6:           50         11
    ## 7:           50         11
    ## 8:           NA         NA
    ## 9:           NA         NA

## Exercise 10

Now, the soccer/football data from exercise 6 came with goals made and
goals made against each team as the following:

gA\_2014b \<- data.table(  
country = c(“Brazil”, “Mexico”, “Croatia”, “Cameroon”),  
goals2014 = c(“7-2”, “4-1”, “6-6”, “1-9”)  
)  
How can you split the goals column into two relevant columns?

``` r
gA_2014b <- data.table(  
  country   = c("Brazil", "Mexico", "Croatia", "Cameroon"),    
  goals2014 = c("7-2", "4-1", "6-6", "1-9")  
)  
gA_2014b[, c("t1", "t2") := tstrsplit(goals2014, "-")]
gA_2014b
```

    ##     country goals2014 t1 t2
    ## 1:   Brazil       7-2  7  2
    ## 2:   Mexico       4-1  4  1
    ## 3:  Croatia       6-6  6  6
    ## 4: Cameroon       1-9  1  9
