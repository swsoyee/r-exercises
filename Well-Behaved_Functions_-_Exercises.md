Well-Behaved Functions – Exercises
================
sindri
27 April 2018

![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2018/04/robots-functions-600x403.jpgmin.jpg)

It is said that, in R, everything that happens is a function call. So,
if we want to improve our ability to make things happen the way we want
them to, maybe it’s worth getting very comfortable with how functions
work in R. 　 In this exercise set, we’ll try to gain better fluency and
deepen our understanding of the R logic by (mostly) writing some
functions.  
Solutions are available
[here](https://www.r-exercises.com/2018/04/27/well-behaved-functions-solutions).

## Exercise 1

Write a function that calculates number `a` to the power of `b`, but let
`b` have a default value of 2.

``` r
ex1.power <- function(a, b = 2) {
  return(a ^ b)
}

ex1.power(4)
```

    ## [1] 16

``` r
ex1.power(2, 3)
```

    ## [1] 8

## Exercise 2

Re-write the function from exercise 1 so that `a` has a default value of
`b+1` already from the formals (from the argument definition.)

``` r
ex2.power <- function(a = b + 1, b = 2) {
  return(a ^ b)
}
ex2.power()
```

    ## [1] 9

## Exercise 3

Write a function `div()` that checks if the first number
[divides](https://math.stackexchange.com/a/452150/103861) the second.

``` r
div <- function(first, second) {
  ifelse(second %% first == 0, TRUE, FALSE)
}
div(2, 3)
```

    ## [1] FALSE

``` r
div(3, 2)
```

    ## [1] FALSE

``` r
div(2, 2)
```

    ## [1] TRUE

## Exercise 4

Write an `infix` function `%div%` that checks if the left-hand side
divides the right-hand side.

    # Example
    3 %div% 42
    [1] TRUE
    3 %div% 13
    [1] FALSE

``` r
`%div%` <- function(first, second) {
  ifelse(second %% first == 0, TRUE, FALSE)
}
3 %div% 42
```

    ## [1] TRUE

``` r
3 %div% 13
```

    ## [1] FALSE

## Exercise 5

Write a function that changes the value of `pi` in the R global
environment to whatever you specify as the argument.

> Note: it is not recommended to re-define the value of “pi” in a
> real-life R program.

    # Example
    pi
    [1] 3.141593
    chpi(4)
    pi
    [1] 4
    chpi("I like cats")
    pi
    [1] "I like cats"

``` r
chpi <- function(p) {
  pi <<- p
  print(pi)
}
pi <- pi
pi
```

    ## [1] 3.141593

``` r
chpi(4)
```

    ## [1] 4

``` r
chpi("I like cats")
```

    ## [1] "I like cats"

## Exercise 6

Write an infix (binary) function that checks if the left hand side (lhs)
is in the range between the maximum and minimum of the rhs.

    set.seed(1)
    rnorm(6) %betw% c(-1, 1)
    [1] TRUE TRUE TRUE FALSE TRUE TRUE

``` r
`%betw%` <- function(lhs, rhs){
  return(lhs > min(rhs) & lhs < max(rhs))
}
set.seed(1)
rnorm(6) %betw% c(-1:1)
```

    ## [1]  TRUE  TRUE  TRUE FALSE  TRUE  TRUE

## Exercise 7

Write another infix operator that pastes two strings and uses it in a
function that takes `ellipsis`. Now, call the function and feed it two
strings and concatenate them to produce some text.

``` r
`%+%` <- function(str1, str2) {
  paste(str1, str2)
}

news <- function(...) {
  strings <- list(...)
  stopifnot(length(strings) == 2)
  "It is" %+% 
    strings[[1]] %+%
    "to practice" %+%
    strings[[2]]
}

news("ok", "writing functions in R")
```

    ## [1] "It is ok to practice writing functions in R"

## Exercise 8

What did you think about the example in exercise 3? What does the
following code (from the [book Advanced R](http://adv-r.had.co.nz/))
return?

    f2 <- function(x = z) {
      z <- 100
      x
    }
    f2()

``` r
f2 <- function(x = z) {
  z <- 100
  x
}
f2()
```

    ## [1] 100

## Exercise 9

Write a function that creates a function analogous to the one from
exercise 1 by feeding 2 as an argument.

> Note: this is not a typo.

    # Example of a function called power()
    powerof2 <- power(2)
    powerof2(3)
    [1] 9

``` r
power <- function(p) {
  function(a, b = p) {
    a^b
  }
}

powerof2 <- power(2)
powerof2(3)
```

    ## [1] 9

## Exercise 10

Make a new `names<-` (replacement) function called `names2<-` that also
allows you to update the names of data structures, except it forces the
names it is given to all-lowercase.

    # Example
    x <- 3:4
    names2(x) <- c("IMPORTANT", "For The CAT do NOT EAT")
    x
    important for the cat do not eat
    3 4

``` r
`names2<-` <- function(x, value) {
  attr(x, "names") <- tolower(value)
  x
}

names2(iris) <- names(iris)
head(iris)
```

    ##   sepal.length sepal.width petal.length petal.width species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa
