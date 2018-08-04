Tensorflow – Basics Part 1: Exercises
================
Vasileios Tsakalos
10 December 2017

![](https://www.r-exercises.com/wp-content/uploads/2017/11/unnamed.jpg)

Tensorflow is an open source, software library for numerical computation
using *data flow graphs*. Nodes in the graph are called ops (short for
operations), while the graph edges represent the R multidimensional data
arrays (tensors) communicated between them. An op takes zero or more
Tensors, performs some computation, and produces zero or more Tensors.

In this set of exercises, we will go through the basics of
[Tensorflow](https://www.tensorflow.org/). By the end of this post, you
will be able to perform the basic numerical operations between tensors
and array operations. Before start solving the exercises, it is
recommended to check out the
[tutorial](https://www.r-exercises.com/2017/11/26/tensorflow-basics-12/)
that this post is based on.

Before proceeding, it might be helpful to look over the help pages for
the `tf$add`, `tf$div`, `tf$multiply`, `tf$matmul`, `tf$shape`,
`tf$split`, `tf$concat`.

Moreover, please run the following commands:

``` r
library(tensorflow)
X <- tf$constant(matrix(c(as.integer(rnorm(200, mean = 10, sd = 2)), as.integer(rnorm(200, mean = 50, sd = 10))), nrow = 200, ncol = 2))
a <- tf$constant(matrix(c(3, 12), nrow = 1, ncol = 2), dtype = tf$float32, name = "a")
b <- tf$constant(matrix(c(5, 15), nrow = 1, ncol = 2), dtype = tf$float32, name = "b")
weights <- tf$constant(matrix(c(0.015, 0.02, 0.025, 0.03), nrow = 2, ncol = 2), dtype = tf$float32, name = "weights")
```

Answers to the exercises are available
[here](https://www.r-exercises.com/2017/12/10/tensorflow-basics-part-1-solutions).
If you obtained a different (correct) answer than those listed on the
solutions page, please feel free to post your answer as a comment on
that page.

## Exercise 1

Print out the value of the tensor `a`.

``` r
with(tf$Session() %as% sess, {
  sess$run(a)
})
```

    ##      [,1] [,2]
    ## [1,]    3   12

## Exercise 2

Add the two tensors `a` and `b`.

``` r
addtion <- tf$add(a, b, name = "add")

with(tf$Session() %as% sess, {
  sess$run(addtion)
})
```

    ##      [,1] [,2]
    ## [1,]    8   27

## Exercise 3

Subtract the two tensors `a` and `b`.

``` r
subtraction  <- tf$subtract(a, b, name = "substract")
with(tf$Session() %as% sess, {
  sess$run(subtraction)
})
```

    ##      [,1] [,2]
    ## [1,]   -2   -3

## Exercise 4

Divide the two tensors `a` and `b`.

``` r
div <- tf$div(a, b, name = "div")
with(tf$Session() %as% sess, {
  sess$run(div)
})
```

    ##      [,1] [,2]
    ## [1,]  0.6  0.8

## Exercise 5

Conduct element-wise multiplication between the two tensors `a` and `b`.

``` r
mul <- tf$multiply(a, b, name = "mul")
with(tf$Session() %as% sess, {
  sess$run(mul)
})
```

    ##      [,1] [,2]
    ## [1,]   15  180

## Exercise 6

Conduct a matrix multiplication between the tensors `a` and `b`.

> Hint: You need to transpose `b`.

``` r
matmul <- tf$matmul(a, b, transpose_b = TRUE, name = "matmul")
with(tf$Session() %as% sess, {
  sess$run(matmul)
})
```

    ##      [,1]
    ## [1,]  195

## Exercise 7

Calculate the inner product of `a` and `weights`. Then add the `b`.
Assign this calculation to the object `alpha`.

``` r
with(tf$Session() %as% sess, {
  alpha <- tf$add(tf$matmul(a, weights), b)
  sess$run(alpha)
})
```

    ##       [,1]   [,2]
    ## [1,] 5.285 15.435

## Exercise 8

Print out the shape of `X`.

``` r
with(tf$Session() %as% sess, {
  sess$run(tf$shape(X))
})
```

    ## [1] 200   2

## Exercise 9

Split the tensor X into `train_X`, `validation_X` and `test_x`. The
dimensions should be:  
train\_X – (160, 2) validation\_X – (20, 2) test\_X – (20, 2)

Print out the dimensions of the splits to make sure you got it
right.

``` r
l <- tf$split(value = X, num_or_size_splits = c(160L, 20L, 20L), axis = 0L)
train_X <- l[[1]]
validation_X <- l[[2]]
test_X <- l[[3]]
with(tf$Session() %as% sess, {
  sess$run(c(tf$shape(train_X), tf$shape(validation_X), tf$shape(test_X)))
})
```

    ## [[1]]
    ## [1] 160   2
    ## 
    ## [[2]]
    ## [1] 20  2
    ## 
    ## [[3]]
    ## [1] 20  2

## Exercise 10

Concatenate the tensors `train_X`, `validation_X` and `test_x`,assign it
to the object `concatenated_X`.

Print out the dimensions of the concatenated tensor to make sure you got
it right.

``` r
with(tf$Session() %as% sess, {
  concatenated_X <- tf$concat(c(train_X, validation_X, test_X), axis = 0L, name = "concat")
  sess$run(tf$shape(concatenated_X))
})
```

    ## [1] 200   2
