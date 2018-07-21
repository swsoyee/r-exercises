Practice Your ggplot Skills: Exercises
================
Jakub Kwiecien
23 February 2018

![](https://www.r-exercises.com/wp-content/uploads/2018/02/iris.png)

`ggplot2` is a great tool for complex data visualization. Let’s practice
it a bit\!

Answers to these exercises are available
[here](http://r-exercises.com/2018/02/23/practice-you-ggplot-skills-solutions/).

For each exercise, please replicate the given graph. Some exercises
require additional data wrangling. If you obtained a different (correct)
answer than those listed on the solutions page, please feel free to post
your answer as a comment on that page.

## Exercise 1

Fancy the `iris`
dot-plot.  
![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2018/02/ggplot-exercises-1.pngmin.png)

``` r
library(ggplot2)
library(dplyr)

ggplot(data = iris, aes(x = Sepal.Length, 
                        y = Sepal.Width,
                        color = Species,
                        shape = Species)) + 
  geom_point() + 
  stat_density2d() +
  theme_light() +
  ggtitle("IRIS")
```

![](Beyond_the_basics_of_data.table_Smooth_data_exploration_files/figure-gfm/exercise-1-1.png)<!-- -->

## Exercise 2

Faceted smoothing (`iris`, once
again).  
![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2018/02/ggplot-exercises-2.pngmin.png)

``` r
iris %>%
  mutate(Species = 'ALL') %>%
  bind_rows(iris) %>%
  ggplot(aes(x = Sepal.Length, y = Sepal.Width, color = Species)) + 
  geom_point() + 
  geom_smooth() +
  facet_wrap(facets =~ Species, scales = "free") +
  theme_bw()
```

    ## Warning in bind_rows_(x, .id): binding character and factor vector,
    ## coercing into character vector

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Beyond_the_basics_of_data.table_Smooth_data_exploration_files/figure-gfm/exercise-2-1.png)<!-- -->

## Exercise 3

Tufte style
mtcars.  
![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2018/02/ggplot-exercises-3.pngmin.png)

``` r
mtcars %>%
  mutate(rank = rank(mpg, ties.method = "first"), name = rownames(mtcars)) %>%
  ggplot(aes(x = mpg, y = rank, label = name)) +
  geom_point() +
  geom_text(nudge_x = .2, hjust = "left") +
  xlab('Miles per gallon fuel consumption') +
  xlim(10, 40) +
  theme_classic() +
  theme(plot.title = element_text(hjust = 0, size = 16),
        axis.title.x = element_text(face = 'bold'),
        axis.title.y = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks.y = element_blank(),
        axis.line.y = element_blank())
```

![](Beyond_the_basics_of_data.table_Smooth_data_exploration_files/figure-gfm/exercise-3-1.png)<!-- -->

## Exercise 4

`mtcars`
bubble-plot.  
![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2018/02/ggplot-exercises-4.pngmin.png)

``` r
mtcars %>%
  ggplot(aes(x = mpg, y = qsec, size = disp, color = as.factor(am))) + 
  geom_point() +
  scale_colour_discrete(name  ="Gear",
                        breaks=c(0, 1),
                        labels=c("Manual", "Automatic")) +
  scale_size_continuous(name = 'Displacement') +
  xlab("Miles per gallon") +
  ylab("1/4 mile time") +
  theme_light()
```

![](Beyond_the_basics_of_data.table_Smooth_data_exploration_files/figure-gfm/exercise-4-1.png)<!-- -->

## Exercise 5

Polar barplot of the mean diamond `price` per `cut` and
`color`.  
![](https://www.r-exercises.com/wp-content/uploads/2018/02/ggplot-exercises-5.png)

``` r
# Copy from solution page 
diamonds2plot <- diamonds %>%
  group_by(cut, color) %>%
  summarise(price = mean(price)) %>%
  arrange(color, price) %>%
  ungroup() %>%
  mutate(id = row_number(),
         angle = 90 - 360 * (id - 0.5) / n())

diamonds2plot  %>%
  ggplot(aes(factor(id), price, fill = color, group = cut, label = cut)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  geom_text(hjust = 0, angle = diamonds2plot$angle, alpha = .5) +
  coord_polar() +
  ggtitle('Mean dimond price') +
  ylim(-3000, 7000) +
  theme_void() +
  theme(plot.title = element_text(hjust = 0.5, size = 16, face = 'bold'))
```

![](Beyond_the_basics_of_data.table_Smooth_data_exploration_files/figure-gfm/exercise-5-1.png)<!-- -->

## Exercise 6

Economist style `economics` time series. (Hint: you will need the
`ggthemes`
package.)  
![](https://www.r-exercises.com/wp-content/uploads/2018/02/ggplot-exercises-6.png)

``` r
economics %>%
  ggplot(aes(x = date, y = uempmed)) + 
  geom_line() +
  expand_limits(y  = 0) +
  ggtitle("Median duration of unemployment [weeks]") +
  ylab("Median duration of unemployment [weeks]") +
  ggthemes::theme_economist_white() +
  theme(axis.title.x = element_blank())
```

![](Beyond_the_basics_of_data.table_Smooth_data_exploration_files/figure-gfm/exercise-6-1.png)<!-- -->
