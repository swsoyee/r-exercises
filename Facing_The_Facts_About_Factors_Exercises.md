Facing The Facts About Factors: Exercises
================
sindri
6 July 2018

Factor variables in R can be mind-boggling. Often, you can just avoid
them and use characters vectors instead – just don’t forget to set
stringsAsFactors=FALSE. They are, however, very useful in [some
circumstances](https://stackoverflow.com/questions/3445316/factors-in-r-more-than-an-annoyance),
such as statistical modelling and presenting data in graphs and tables.
Relying on factors but misunderstanding them has been known to “eat up
hours of valuable time in any given analysis”, as one member of the
community [put
it](https://www.stat.ubc.ca/~jenny/STAT545A/block08_bossYourFactors.html#factors-are-high-maintenance-variables).
It is therefore a good investment to get them straight as soon as
possible on your R journey.

The intent behind these exercises is to help you find and fill in the
cracks and holes in your relationship with factor variables.

Solutions are available
[here](https://www.r-exercises.com/2018/07/06/11753).

## Exercise 1

Load the `gapminder` data-set from the `gapminder` package. Save it to
an object called `gp`. Check programmatically how many factors it
contains and how many levels each factor has.

``` r
library(gapminder)
gp <- gapminder
faccol <- sapply(gp, is.factor)
faccol
```

    ##   country continent      year   lifeExp       pop gdpPercap 
    ##      TRUE      TRUE     FALSE     FALSE     FALSE     FALSE

``` r
levelCount <- sapply(gp[faccol], levels)
levelCount
```

    ## $country
    ##   [1] "Afghanistan"              "Albania"                 
    ##   [3] "Algeria"                  "Angola"                  
    ##   [5] "Argentina"                "Australia"               
    ##   [7] "Austria"                  "Bahrain"                 
    ##   [9] "Bangladesh"               "Belgium"                 
    ##  [11] "Benin"                    "Bolivia"                 
    ##  [13] "Bosnia and Herzegovina"   "Botswana"                
    ##  [15] "Brazil"                   "Bulgaria"                
    ##  [17] "Burkina Faso"             "Burundi"                 
    ##  [19] "Cambodia"                 "Cameroon"                
    ##  [21] "Canada"                   "Central African Republic"
    ##  [23] "Chad"                     "Chile"                   
    ##  [25] "China"                    "Colombia"                
    ##  [27] "Comoros"                  "Congo, Dem. Rep."        
    ##  [29] "Congo, Rep."              "Costa Rica"              
    ##  [31] "Cote d'Ivoire"            "Croatia"                 
    ##  [33] "Cuba"                     "Czech Republic"          
    ##  [35] "Denmark"                  "Djibouti"                
    ##  [37] "Dominican Republic"       "Ecuador"                 
    ##  [39] "Egypt"                    "El Salvador"             
    ##  [41] "Equatorial Guinea"        "Eritrea"                 
    ##  [43] "Ethiopia"                 "Finland"                 
    ##  [45] "France"                   "Gabon"                   
    ##  [47] "Gambia"                   "Germany"                 
    ##  [49] "Ghana"                    "Greece"                  
    ##  [51] "Guatemala"                "Guinea"                  
    ##  [53] "Guinea-Bissau"            "Haiti"                   
    ##  [55] "Honduras"                 "Hong Kong, China"        
    ##  [57] "Hungary"                  "Iceland"                 
    ##  [59] "India"                    "Indonesia"               
    ##  [61] "Iran"                     "Iraq"                    
    ##  [63] "Ireland"                  "Israel"                  
    ##  [65] "Italy"                    "Jamaica"                 
    ##  [67] "Japan"                    "Jordan"                  
    ##  [69] "Kenya"                    "Korea, Dem. Rep."        
    ##  [71] "Korea, Rep."              "Kuwait"                  
    ##  [73] "Lebanon"                  "Lesotho"                 
    ##  [75] "Liberia"                  "Libya"                   
    ##  [77] "Madagascar"               "Malawi"                  
    ##  [79] "Malaysia"                 "Mali"                    
    ##  [81] "Mauritania"               "Mauritius"               
    ##  [83] "Mexico"                   "Mongolia"                
    ##  [85] "Montenegro"               "Morocco"                 
    ##  [87] "Mozambique"               "Myanmar"                 
    ##  [89] "Namibia"                  "Nepal"                   
    ##  [91] "Netherlands"              "New Zealand"             
    ##  [93] "Nicaragua"                "Niger"                   
    ##  [95] "Nigeria"                  "Norway"                  
    ##  [97] "Oman"                     "Pakistan"                
    ##  [99] "Panama"                   "Paraguay"                
    ## [101] "Peru"                     "Philippines"             
    ## [103] "Poland"                   "Portugal"                
    ## [105] "Puerto Rico"              "Reunion"                 
    ## [107] "Romania"                  "Rwanda"                  
    ## [109] "Sao Tome and Principe"    "Saudi Arabia"            
    ## [111] "Senegal"                  "Serbia"                  
    ## [113] "Sierra Leone"             "Singapore"               
    ## [115] "Slovak Republic"          "Slovenia"                
    ## [117] "Somalia"                  "South Africa"            
    ## [119] "Spain"                    "Sri Lanka"               
    ## [121] "Sudan"                    "Swaziland"               
    ## [123] "Sweden"                   "Switzerland"             
    ## [125] "Syria"                    "Taiwan"                  
    ## [127] "Tanzania"                 "Thailand"                
    ## [129] "Togo"                     "Trinidad and Tobago"     
    ## [131] "Tunisia"                  "Turkey"                  
    ## [133] "Uganda"                   "United Kingdom"          
    ## [135] "United States"            "Uruguay"                 
    ## [137] "Venezuela"                "Vietnam"                 
    ## [139] "West Bank and Gaza"       "Yemen, Rep."             
    ## [141] "Zambia"                   "Zimbabwe"                
    ## 
    ## $continent
    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

``` r
lapply(levelCount, length)
```

    ## $country
    ## [1] 142
    ## 
    ## $continent
    ## [1] 5

2 factors are contained in this dataset.  
And the number of levels are:  
142, 5

## Exercise 2

Notice that one continent, Antarctica, is missing from the corresponding
factor – add it as the last level of six.

``` r
levels(gp$continent)
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

``` r
levels(gp$continent) <- c(levels(gp$continent), "Antartica")
levels(gp$continent)
```

    ## [1] "Africa"    "Americas"  "Asia"      "Europe"    "Oceania"   "Antartica"

## Exercise 3

Actually, you change your mind. There is no permanent human population
on Antarctica. Drop this (unused) level from your factor. Can you find
three ways to do this, then you are an expert.

``` r
# Method 1
gp$continent <- droplevels(gp$continent)
levels(gp$continent)
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

``` r
# Method 2
gp$continent <- factor(gp$continent)
levels(gp$continent)
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

``` r
# Method 3
gp$continent  <- gp$continent[, drop = TRUE]
levels(gp$continent)
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

## Exercise 4

Again, modify the continent factor, making it more precise. Add two new
levels instead of Americas, North-America and South-America. The
countries in the following vector should be classified as South-America
and the rest as North-America.

`c("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador",
"Paraguay", "Peru", "Uruguay", "Venezuela")`

``` r
# Add new levels, else if will generate <NA>
levels(gp$continent) <- c(levels(gp$continent), "North-America", "South-America")
sAmerica <- c("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Ecuador",
"Paraguay", "Peru", "Uruguay", "Venezuela")

gp[gp$country %in% sAmerica, ]$continent <- as.factor("South-America")
gp[gp$continent == "Americas", ]$continent <- "North-America"
gp$continent <- droplevels(gp$continent)
table(gp$continent)
```

    ## 
    ##        Africa          Asia        Europe       Oceania North-America 
    ##           624           396           360            24           180 
    ## South-America 
    ##           120

## Exercise 5

Get the levels of the factor in alphabetical
    order.

``` r
sort(levels(gp$continent))
```

    ## [1] "Africa"        "Asia"          "Europe"        "North-America"
    ## [5] "Oceania"       "South-America"

``` r
gp$continent <- factor(gp$continent, levels = sort(levels(gp$continent)))
levels(gp$continent)
```

    ## [1] "Africa"        "Asia"          "Europe"        "North-America"
    ## [5] "Oceania"       "South-America"

## Exercise 6

Re-order the continent levels again so that they appear in order of
total population in 2007.

``` r
library(data.table)
setDT(gp)
gp$continent <- factor(gp$continent, 
                       levels = (gp[year == 2007, 
                                    sum(pop), 
                                    by = continent][order(-V1)]$continent
                                 )
                       )
levels(gp$continent)
```

    ## [1] "Asia"          "Africa"        "Europe"        "North-America"
    ## [5] "South-America" "Oceania"

## Exercise 7

Reverse the order of the factor and define continents as an ordered
factor.

``` r
gp$continent <- factor(gp$continent,
                       levels = rev(levels(gp$continent)),
                       ordered = TRUE
                       )
levels(gp$continent)
```

    ## [1] "Oceania"       "South-America" "North-America" "Europe"       
    ## [5] "Africa"        "Asia"

## Exercise 8

Make the continent an unordered factor again and set North-America as
the first level, thus interpreted as a reference group in modelling
functions such as lm().

``` r
class(gp$continent)
```

    ## [1] "ordered" "factor"

``` r
class(gp$continent) <- "factor"
class(gp$continent)
```

    ## [1] "factor"

``` r
gp$continent <- relevel(gp$continent, ref = "North-America")
levels(gp$continent)
```

    ## [1] "North-America" "Oceania"       "South-America" "Europe"       
    ## [5] "Africa"        "Asia"

## Exercise 9

Turn the following messy vector into a factor with two levels: Female
and Male, using the factor function. Use the labels argument in the
factor() function (ps: you can save some time by applying tolower() and
trimws() before you apply factor()).  
`gender <- c("f", "m ", "male ","male", "female", "FEMALE", "Male", "f",
"m")`

``` r
gender <- c("f", "m ", "male ","male", "female", "FEMALE", "Male", "f", "m")
g <- tolower(substr(gender, 1, 1))
g <- as.factor(ifelse(g == "f", "Female", "Male"))
g
```

    ## [1] Female Male   Male   Male   Female Female Male   Female Male  
    ## Levels: Female Male

## Exercise 10

Use the fact that factors are built on top of integers and create a
dummy (binary) variable male that takes the value 1 if the gender has
the value “Male.”

``` r
as.factor(ifelse(g == "Male", 1, 0))
```

    ## [1] 0 1 1 1 0 0 1 0 1
    ## Levels: 0 1
