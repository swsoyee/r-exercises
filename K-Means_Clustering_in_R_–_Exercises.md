K-Means Clustering in R – Exercises
================
sindri
13 April 2018

![](https://www.r-exercises.com/wp-content/uploads/2018/04/6966285895_f9a694f2e4_z.jpg)

K-means is efficient, and perhaps, the most popular clustering method.
It is a way for finding natural groups in otherwise *unlabeled* data.
You specify the number of clusters you want defined and the algorithm
minimizes the total within-cluster variance.

In this exercise, we will play around with the base R inbuilt k-means
function on some *labeled* data.

Solutions are available
[here](https://www.r-exercises.com/2018/04/13/11260/).

## Exercise 1

Feed the columns with sepal measurements in the inbuilt `iris` data-set
to the k-means; save the cluster vector of each observation. Use 3
centers and set the random seed to 1 before.

``` r
# Require dataset
d <- iris
# Check all the measurements
colnames(d)
```

    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"

``` r
# Get the columns with sepal measurements
col <- colnames(d)[grepl("sepal", tolower(colnames(d)))]
# Feed the columns into k-means
set.seed(1)
model <- kmeans(d[col], centers = 3)
# Save the cluster vector of each observation
cluster1 <- model$cluster
model
```

    ## K-means clustering with 3 clusters of sizes 50, 47, 53
    ## 
    ## Cluster means:
    ##   Sepal.Length Sepal.Width
    ## 1     5.006000    3.428000
    ## 2     6.812766    3.074468
    ## 3     5.773585    2.692453
    ## 
    ## Clustering vector:
    ##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ##  [36] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 3 2 3 2 3 2 3 3 3 3 3 3 2 3 3 3 3
    ##  [71] 3 3 3 3 2 2 2 2 3 3 3 3 3 3 3 3 2 3 3 3 3 3 3 3 3 3 3 3 3 3 2 3 2 2 2
    ## [106] 2 3 2 2 2 2 2 2 3 3 2 2 2 2 3 2 3 2 3 2 2 3 3 2 2 2 2 2 3 3 2 2 2 3 2
    ## [141] 2 2 3 2 2 2 3 2 2 3
    ## 
    ## Within cluster sum of squares by cluster:
    ## [1] 13.1290 12.6217 11.3000
    ##  (between_SS / total_SS =  71.6 %)
    ## 
    ## Available components:
    ## 
    ## [1] "cluster"      "centers"      "totss"        "withinss"    
    ## [5] "tot.withinss" "betweenss"    "size"         "iter"        
    ## [9] "ifault"

## Exercise 2

Check the proportions of each species by cluster.

``` r
table1 <- table(cluster1, d$Species)
table1 / colSums(table1)
```

    ##         
    ## cluster1 setosa versicolor virginica
    ##        1   1.00       0.00      0.00
    ##        2   0.00       0.24      0.70
    ##        3   0.00       0.76      0.30

## Exercise 3

Make a plot with sepal length on the horizontal axis and width on the
vertical axis. Find a way to visualize both the actual species and the
cluster the algorithm is categorized into.

``` r
d$Cluster <- as.character(cluster1)

# Plotly cannot set color and symbol independently in a easy way, so change to ggplot
# library(plotly)
# plot_ly(data = d,
#         x =~ Sepal.Length,
#         y =~ Sepal.Width,
#         color =~ Species,
#         symbol =~ cluster,
#         type = "scatter",
#         mode = "markers")

library(ggplot2)
ggplot(d, aes(x = Sepal.Length, y = Sepal.Width, color = Species, shape = Cluster)) +
  geom_point() + 
  theme_bw()
```

![](K-Means_Clustering_in_R_–_Exercises_files/figure-gfm/exercise-3-1.png)<!-- -->

## Exercise 4

Repeat the clustering from step one, but include petal measurements
also. Does the clustering reflect the actual species better now?

``` r
# Get the columns with sepal and petal measurements
col <- colnames(d)[grepl("sepal|petal", tolower(colnames(d)))]
# Feed the columns into k-means
set.seed(1)
model <- kmeans(d[col], centers = 3)
# Save the cluster vector of each observation
cluster2 <- model$cluster

table2 <- table(cluster2, d$Species)
table2 / colSums(table2)
```

    ##         
    ## cluster2 setosa versicolor virginica
    ##        1   1.00       0.00      0.00
    ##        2   0.00       0.04      0.72
    ##        3   0.00       0.96      0.28

## Exercise 5

Create a new data-set identical to iris, but multiply the “Petal.Width”
by 2. Are the results different now?

``` r
# Require dataset
d <- iris
# Multiple "Petal.Width" by 2
d$Petal.Width <- d$Petal.Width * 2
# Get the columns with sepal and petal measurements
col <- colnames(d)[grepl("sepal|petal", tolower(colnames(d)))]
# Feed the columns into k-means
set.seed(1)
model <- kmeans(d[col], centers = 3)
# Save the cluster vector of each observation
cluster3 <- model$cluster

table(cluster2, cluster3)
```

    ##         cluster3
    ## cluster2  1  2  3
    ##        1 50  0  0
    ##        2  0  2 36
    ##        3  0 53  9

## Exercise 6

Standardize your new data-set so that each variable has a mean of 0 and
a variance of 1; check once more if multiplying by two makes a
difference.

``` r
# Standardize dataset (with "Petal.Width" doubled)
ds <- data.frame(scale(d[, 1:4]))
# Get the columns with sepal and petal measurements
col <- colnames(ds)[grepl("sepal|petal", tolower(colnames(ds)))]
# Feed the columns into k-means
set.seed(1)
model <- kmeans(ds[col], centers = 3)
# Save the cluster vector of each observation
cluster4 <- model$cluster

# Standardize dataset (original)
d <- data.frame(scale(iris[, 1:4]))
# Get the columns with sepal and petal measurements
col <- colnames(d)[grepl("sepal|petal", tolower(colnames(d)))]
# Feed the columns into k-means
set.seed(1)
model <- kmeans(d[col], centers = 3)
# Save the cluster vector of each observation
cluster5 <- model$cluster

# Check the differences (After Standardization, double any variable has no affection to the result)
table(cluster4, cluster5)
```

    ##         cluster5
    ## cluster4  1  2  3
    ##        1 50  0  0
    ##        2  0 53  0
    ##        3  0  0 47

## Exercise 7

Read in the Titanic `train.csv` data-set from
[Kaggle.com](https://www.kaggle.com/c/titanic/data) (you might have to
sign up first). Turn the sex column into a dummy variable, =1, if it is
male, “0” otherwise, and “Pclass” into a dummy variable for the most
common class 3. Using four columns, `Sex`, `SibSp`, `Parch` and `Fare`,
apply the k-means algorithm to get 4 clusters and use `nstart=20`.
Remember to set the seed to 1 so the results are comparable. Note that
these four variables have no special meaning to the problem and dummy
data in k-means is probably not a good idea in general – we are just
playing around.

``` r
tita <- read.csv("data/tita_train.csv")

# Turn the sex column into a dummy variable, =1, if it is male, “0” otherwise
levels(tita$Sex)
```

    ## [1] "female" "male"

``` r
tita$Sex <- ifelse(tita$Sex == "male", 1, 0)
# And “Pclass” into a dummy variable =1 for the most common class 3
tita$Pclass <- ifelse(tita$Pclass == 3, 1, 0)

tita_si <- scale(tita[, c("Sex", "SibSp", "Parch", "Fare")])
set.seed(1)
tita_kmeans <- kmeans(tita_si, centers = 4)
```

## Exercise 8

Now, how would you describe, in words, each of the clusters and what the
survival rates were?

``` r
tita_kmeans$centers
```

    ##          Sex       SibSp       Parch        Fare
    ## 1 -0.2573209  2.58812815  2.27219730  0.06818506
    ## 2 -1.3548126 -0.07555623  0.02862768 -0.05495253
    ## 3 -0.6250125  0.11621464  0.82490163  3.46425355
    ## 4  0.7372810 -0.27066999 -0.34232475 -0.26226300

``` r
# Cluster description:
# 1: Male, low fare, not travelling with family
# 2: Mostly female, very high fare (rich?), some family
# 3: Female, alone, a bit lower fair
# 4: Individuals with lots of family and about average fare

tita_cluster <- tita_kmeans$cluster
tita_table <- table(tita$Survived, tita_cluster)
tita_table[2, ] / colSums(tita_table) * 100
```

    ##        1        2        3        4 
    ## 11.47541 78.59922 76.74419 18.86792

``` r
# Females had beter survival chance.
```

## Exercise 9

Now, 4 clusters was an arbitrary choice. Apply k-means clustering using
nstart=20, but using k from 2 to 20 and store the
result.

``` r
model <- sapply(2:20, function(x) kmeans(tita_si, centers = x, nstart = 20))
```

## Exercise 10

Plot the percent of variance explained by the clusters (between sum of
squares over the total sum of squares). What seems like a reasonable
number of clusters, according to the [elbow
method](https://en.wikipedia.org/wiki/Determining_the_number_of_clusters_in_a_data_set#The_elbow_method)?

``` r
wss <- sapply(1:19, function(x) model[, x]$betweenss / model[, x]$totss)

# Four clusters is OK
qplot(x = 1:19, y = wss) + 
  geom_line() + 
  ggtitle("Number of Clusters with Sum of Squares value") +
  xlab("Number of Clusters") + 
  ylab("Between Sum of Squares / Total Sum of Squares") +
  theme_bw()
```

![](K-Means_Clustering_in_R_–_Exercises_files/figure-gfm/exercise-10-1.png)<!-- -->
