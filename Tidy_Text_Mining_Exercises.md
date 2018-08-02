Tidy Text Mining Exercises
================
Jakub Kwiecien
12 November 2017

![](https://www.r-exercises.com/wp-content/uploads/2017/10/dictionary-1619740_1920.jpg)

Text mining can be messy. Tokenization, document-term matrices,
lexicons… Lots of data structures and transformations between them.
Fortunately, there is the `tidytext` package, which will help you to
tidy this mess\!

Answers to the exercises are available
[here](http://r-exercises.com/2017/11/12/tidy-text-mining-solutions/).

If you obtained a different (correct) answer than those listed on the
solutions page, please feel free to post your answer as a comment on
that page.

## Exercise 1

Load `tidytext`, `janeaustenr`, `stringr` and `dplyr` packages. Load
Jane Austen\`s books and examine how the data looks.

``` r
library(tidytext)
library(janeaustenr)
library(stringr)
library(dplyr)

books <- austen_books()
head(books)
```

    ## # A tibble: 6 x 2
    ##   text                  book               
    ##   <chr>                 <fct>              
    ## 1 SENSE AND SENSIBILITY Sense & Sensibility
    ## 2 ""                    Sense & Sensibility
    ## 3 by Jane Austen        Sense & Sensibility
    ## 4 ""                    Sense & Sensibility
    ## 5 (1811)                Sense & Sensibility
    ## 6 ""                    Sense & Sensibility

``` r
summary(books)
```

    ##      text                            book      
    ##  Length:73422       Sense & Sensibility:12624  
    ##  Class :character   Pride & Prejudice  :13030  
    ##  Mode  :character   Mansfield Park     :15349  
    ##                     Emma               :16235  
    ##                     Northanger Abbey   : 7856  
    ##                     Persuasion         : 8328

## Exercise 2

Add a column identifying the line in a book. Split text into words.

``` r
books_tokenized <- books %>%
  group_by(book) %>%
  mutate(line = row_number()) %>%
  unnest_tokens(word, text, token = 'words')
head(books_tokenized)
```

    ## # A tibble: 6 x 3
    ## # Groups:   book [1]
    ##   book                 line word       
    ##   <fct>               <int> <chr>      
    ## 1 Sense & Sensibility     1 sense      
    ## 2 Sense & Sensibility     1 and        
    ## 3 Sense & Sensibility     1 sensibility
    ## 4 Sense & Sensibility     3 by         
    ## 5 Sense & Sensibility     3 jane       
    ## 6 Sense & Sensibility     3 austen

## Exercise 3

Remove stopwords.

``` r
length(unique(books_tokenized$word))
```

    ## [1] 14520

``` r
head(stop_words)
```

    ## # A tibble: 6 x 2
    ##   word      lexicon
    ##   <chr>     <chr>  
    ## 1 a         SMART  
    ## 2 a's       SMART  
    ## 3 able      SMART  
    ## 4 about     SMART  
    ## 5 above     SMART  
    ## 6 according SMART

``` r
nrow(stop_words)
```

    ## [1] 1149

``` r
books_tidy <- books_tokenized %>%
  anti_join(stop_words)
```

    ## Joining, by = "word"

``` r
head(books_tidy)
```

    ## # A tibble: 6 x 3
    ## # Groups:   book [1]
    ##   book                 line word       
    ##   <fct>               <int> <chr>      
    ## 1 Sense & Sensibility     1 sense      
    ## 2 Sense & Sensibility     1 sensibility
    ## 3 Sense & Sensibility     3 jane       
    ## 4 Sense & Sensibility     3 austen     
    ## 5 Sense & Sensibility     5 1811       
    ## 6 Sense & Sensibility    10 chapter

``` r
length(unique(books_tidy$word))
```

    ## [1] 13914

## Exercise 4

Create a wordcloud of 100 most frequent words. Hint: Use `wordcloud`
package.

``` r
library(wordcloud2)
library(htmlwidgets)

word_freq <- books_tidy %>%
  count(word)

book <- unique(word_freq$book)

for(i in book) {
  dataset <- word_freq %>% 
    filter(book == i) %>% 
    arrange(desc(n)) %>%
    head(100)
  
  wc <- wordcloud2(data.frame(word = dataset$word, freq = dataset$n))

  saveWidget(wc, paste0(i, ".html"), selfcontained = F)
  output_path <- paste0("Tidy_Text_Mining_Exercises_files/figure-gfm/", i, ".png")
  webshot::webshot(paste0(i, ".html"), 
                   output_path,
                   vwidth = 700, 
                   vheight = 500, 
                   delay = 7)
  cat(paste0("- ", i))
  cat(paste0('![](', output_path,')"\n'))
  if (file.exists(paste0(i, ".html"))) file.remove(paste0(i, ".html"))
  unlink(paste0(i, "_files"), recursive = TRUE, force = TRUE)
}
```

  - Sense &
    Sensibility![](Tidy_Text_Mining_Exercises_files/figure-gfm/Sense%20&%20Sensibility.png)"
  - Pride &
    Prejudice![](Tidy_Text_Mining_Exercises_files/figure-gfm/Pride%20&%20Prejudice.png)"
  - Mansfield
    Park![](Tidy_Text_Mining_Exercises_files/figure-gfm/Mansfield%20Park.png)"
  - Emma![](Tidy_Text_Mining_Exercises_files/figure-gfm/Emma.png)"
  - Northanger
    Abbey![](Tidy_Text_Mining_Exercises_files/figure-gfm/Northanger%20Abbey.png)"
  - Persuasion![](Tidy_Text_Mining_Exercises_files/figure-gfm/Persuasion.png)"

## Exercise 5

Find the most frequent words for each book.

``` r
word_freq %>% 
  group_by(book) %>% 
  arrange(desc(n)) %>% 
  filter(row_number() == 1)
```

    ## # A tibble: 6 x 3
    ## # Groups:   book [6]
    ##   book                word          n
    ##   <fct>               <chr>     <int>
    ## 1 Mansfield Park      fanny       816
    ## 2 Emma                emma        786
    ## 3 Sense & Sensibility elinor      623
    ## 4 Pride & Prejudice   elizabeth   597
    ## 5 Persuasion          anne        447
    ## 6 Northanger Abbey    catherine   428

## Exercise 6

Perform TF-IDF transformation of the books. Find the words with the
highest TF-IDF score for each book.

## Exercise 7

## Exercise 8

## Exercise 9

## Exercise 10
