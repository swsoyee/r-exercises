Programmatically Creating Text Outputs in R: Exercises
================
sindri
25 May 2018

![](https://www.r-exercises.com/wp-content/uploads/2018/05/33824145905_d6dc3515f8_z.jpg)

In the age of Rmarkdown and Shiny, or when making any custom output from
your data, you want your output to look consistent and neat. Also, when
writing your output, you often want it to obtain a specific (decorative)
format defined by the html or LaTeX engine. These exercises are an
opportunity to refresh our memory on functions, such as paste, sprintf,
formatC and others that are convenient tools to achieve these ends. All
of the solutions rely partly on the ultra flexible sprintf(), but there
are no-doubt many ways to solve the exercises with other functions. Feel
free to share your solutions in the comment section.

Example solutions are available
[here](https://www.r-exercises.com/2018/05/25/programmatically-creating-text-output-in-r-solutions).

## Exercise 1

Print out the following vector as prices in dollars (to the nearest
cent):
`c(14.3409087337707, 13.0648270623048, 3.58504267621646, 18.5077076398145, 16.8279241011882)`.  
Example:
$14.34

``` r
ex1 <- c(14.3409087337707, 13.0648270623048, 3.58504267621646, 18.5077076398145, 16.8279241011882)
# Method 1
paste0("$", round(ex1, 2))
```

    ## [1] "$14.34" "$13.06" "$3.59"  "$18.51" "$16.83"

``` r
# Method 2
sprintf("$%.2f", ex1)
```

    ## [1] "$14.34" "$13.06" "$3.59"  "$18.51" "$16.83"

## Exercise 2

Using these numbers, `c(25, 7, 90, 16)`, make a vector of filenames in
the following format: file\_025.txt. Left pad the numbers so they are
all three digits.

``` r
ex2 <- c(25, 7, 90, 16)
# Method 1
paste0("file_", sprintf("%03d", ex2), ".txt")
```

    ## [1] "file_025.txt" "file_007.txt" "file_090.txt" "file_016.txt"

``` r
# Method 2
sprintf("file_%03d.txt", ex2)
```

    ## [1] "file_025.txt" "file_007.txt" "file_090.txt" "file_016.txt"

## Exercise 3

Actually, if we are only dealing with numbers less than one hundred,
file\_25.txt would have been enough. Change the code from the last
exercise so that the padding is pro-grammatically decided by the biggest
number in the vector.

``` r
# Method 1
sprintf(paste0("file_%0", length(max(ex2)), "d.txt"), ex2)
```

    ## [1] "file_25.txt" "file_7.txt"  "file_90.txt" "file_16.txt"

``` r
# Method 2
sprintf("file_%0*d.txt", nchar(max(ex2)), ex2)
```

    ## [1] "file_25.txt" "file_07.txt" "file_90.txt" "file_16.txt"

## Exercise 4

Print out the following haiku on three lines, right aligned, with the
help of cat: `c("Stay the patient course.", "Of little worth is your
ire.", "The network is
down.")`.

``` r
ex4 <- c("Stay the patient course.", "Of little worth is your ire.", "The network is down.")
cat(sprintf("%*s", max(nchar(ex4)), ex4), sep = "\n")
```

    ##     Stay the patient course.
    ## Of little worth is your ire.
    ##         The network is down.

## Exercise 5

Write a function that converts a number to its hexadecimal
representation. This is a useful skill when converting bmp colors from
one representation to another. Example output:

    tohex(12)
    [1] "12 is c in hexadecimal"

``` r
tohex <- function(x) {
  paste0(x, " is ", as.hexmode(x), " in hexadecimal")
}
tohex(12)
```

    ## [1] "12 is c in hexadecimal"

## Exercise 6

Take a string and pro-grammatically surround it with the html header tag
`h1`.

``` r
sprintf("<h1>%s</h1>", "Yes")
```

    ## [1] "<h1>Yes</h1>"

## Exercise 7

Back to the poem from exercise 4, let R convert to html unordered list
so that it would appear like the following in a browser:

  - Stay the patient course
  - Of little worth is your ire
  - The network is down

<!-- end list -->

``` r
cat(sprintf("<li>%s</li>", ex4))
```

<li>

Stay the patient course.

</li>

<li>

Of little worth is your ire.

</li>

<li>

The network is down.

</li>

## Exercise 8

Here is a list of the current top 5 movies on imdb.com in terms of
rating `c("The Shawshank Redemption", "The Godfather", "The Godfather:
Part II", "The Dark Knight", "12 Angry Men", "Schindler's List")`.
Convert them into a list compatible with the written text.

Example output:

\[1\] “The top ranked films on imdb.com are The Shawshank Redemption,
The Godfather, The Godfather: Part II, The Dark Knight, 12 Angry Men and
Schindler’s
List”

``` r
ex8 <- c("The Shawshank Redemption", "The Godfather", "The Godfather: Part II", "The Dark Knight", "12 Angry Men", "Schindler's List")
paste("The top ranked films on imdb.com are", paste(ex8, collapse = ", "))
```

    ## [1] "The top ranked films on imdb.com are The Shawshank Redemption, The Godfather, The Godfather: Part II, The Dark Knight, 12 Angry Men, Schindler's List"

## Exercise 9

Now, you should be able to solve this quickly: write a function that
converts a proportion to a percentage that takes as input number of
decimal places. An input of 0.921313 and 2 decimal places should return
“92.13%”.

``` r
ex9 <- function(number, decimal) {
  sprintf(paste0("%.", decimal, "f%%"), number * 100)
}
ex9(0.921313, 2)
```

    ## [1] "92.13%"

## Exercise 10

Improve the function from the last exercise so that the percentage
consistently takes 10 characters by doing some left padding. Raise an
error if the percentage already happens to be longer than 10.

``` r
ex10 <- function(number, decimal) {
  p <- sprintf("%.*f%%", decimal, number * 100)
  if(any(nchar(p) > 10)) {
    return("Too long percentage")
  }
  sprintf("%10s", p)
}

set.seed(1)
cat(ex10(rnorm(10), 1), sep="\n")
```

    ##     -62.6%
    ##      18.4%
    ##     -83.6%
    ##     159.5%
    ##      33.0%
    ##     -82.0%
    ##      48.7%
    ##      73.8%
    ##      57.6%
    ##     -30.5%

``` r
ex10(999, 4)
```

    ## [1] "Too long percentage"
