Melt and Cast The Shape of Your Data-Frame: Exercises
================
sindri
22 June 2018

![](https://www.r-exercises.com/wnw-images/wp-content/uploads/2018/06/melt.jpgmin.jpg)

Data-sets often arrive to us in a form that is different from what we
need for our modeling or visualization functions, which, in turn, don’t
necessarily require the same format.

Reshaping data.frames is a step that all analysts need, but many
struggle with. Practicing this meta-skill will, in the long-run, result
in more time to focus on the actual analysis.

The solutions to this set will rely on data.table, mostly melt() and
dcast(), which are originally from the reshape2 package. However, you
can also get practice out of it using your favorite base-R, tidy-verse
or any other method, then compare the results.

Solutions are available
[here](https://www.r-exercises.com/2018/06/22/11719/).

## Exercise 1

Take the following data.frame from this form:

df \<- data.frame(id = 1:2, q1 = c(“A”, “B”), q2 = c(“C”, “A”),
stringsAsFactors = FALSE) df  
id q1 q2  
1 1 A C  
2 2 B A  
to this:

id question value  
1 1 q1 A  
2 2 q1 B  
3 1 q2 C  
4 2 q2 A

``` r
library(data.table)
library(knitr)
df <- data.frame(id = 1:2, q1 = c("A", "B"), q2 = c("C", "A"), stringsAsFactors = FALSE)
kable(df)

ex1 <- melt(df, id = 1, measure = c("q1", "q2"), variable.name = "question")
kable(ex1)
```

| id | q1 | q2 |
| -: | :- | :- |
|  1 | A  | C  |
|  2 | B  | A  |

| id | question | value |
| -: | :------- | :---- |
|  1 | q1       | A     |
|  2 | q1       | B     |
|  1 | q2       | C     |
|  2 | q2       | A     |

## Exercise 2

Do the opposite; return the data.frame back to it’s original form.

``` r
ex2 <- dcast(ex1, id ~ question)
kable(ex2)
```

| id | q1 | q2 |
| -: | :- | :- |
|  1 | A  | C  |
|  2 | B  | A  |

## Exercise 3

Set up the data.frame in terms of questions, as follows:

question id\_1 id\_2  
1 q1 A B  
2 q2 C A

``` r
ex3 <- dcast(ex1, question ~ paste0("id_", id))
kable(ex3)
```

| question | id\_1 | id\_2 |
| :------- | :---- | :---- |
| q1       | A     | B     |
| q2       | C     | A     |

## Exercise 4

The data entry behind this data.frame went a little bit wrong. Get all
the C and B entries into their corresponding columns:

df2 \<- data.frame(  
A = c(“A1”, “A12”, “A31”, “A4”),  
B = c(“B4”, “C7”, “C3”, “B9”),  
C = c(“C3”, “B16”, “B3”, “C4”)  
)

``` r
df2 <- data.frame(
  A = c("A1", "A12", "A31", "A4"), 
  B = c("B4", "C7", "C3", "B9"), 
  C = c("C3", "B16", "B3", "C4")
)
setDT(df2)

# Add "id" column for melt
df21 <- df2[, id := .I]
kable(df21)
```

| A   | B  | C   | id |
| :-- | :- | :-- | -: |
| A1  | B4 | C3  |  1 |
| A12 | C7 | B16 |  2 |
| A31 | C3 | B3  |  3 |
| A4  | B9 | C4  |  4 |

``` r
df22 <- melt(df21, id.vars = "id")
kable(df22)
```

| id | variable | value |
| -: | :------- | :---- |
|  1 | A        | A1    |
|  2 | A        | A12   |
|  3 | A        | A31   |
|  4 | A        | A4    |
|  1 | B        | B4    |
|  2 | B        | C7    |
|  3 | B        | C3    |
|  4 | B        | B9    |
|  1 | C        | C3    |
|  2 | C        | B16   |
|  3 | C        | B3    |
|  4 | C        | C4    |

``` r
# Substr the first letter of value
df23 <- dcast(df22, id ~ substr(value, 1, 1))
kable(df23)
```

| id | A   | B   | C  |
| -: | :-- | :-- | :- |
|  1 | A1  | B4  | C3 |
|  2 | A12 | B16 | C7 |
|  3 | A31 | B3  | C3 |
|  4 | A4  | B9  | C4 |

``` r
# Delete "id" column
df24 <- df23[, -c("id")]
kable(df24)
```

| A   | B   | C  |
| :-- | :-- | :- |
| A1  | B4  | C3 |
| A12 | B16 | C7 |
| A31 | B3  | C3 |
| A4  | B9  | C4 |

## Exercise 5

Get this data.frame:

df3 \<- data.frame(  
Join\_ID = rep(1:3, each = 2),  
Type = rep(c(“a”, “b”), 3),  
v2 = c(8, 9, 7, 6, 5, 4)\*10  
)

To look like this:

Join\_ID a\_v2 b\_v2  
1 1 80 90  
2 2 70 60  
3 3 50 40

``` r
df3 <- data.frame(
  Join_ID = rep(1:3, each = 2), 
  Type    = rep(c("a", "b"), 3), 
  v2      = c(8, 9, 7, 6, 5, 4)*10
)
setDT(df3)
df31 <- dcast(df3, Join_ID ~ paste0(Type, "_v2"), value.var = "v2")
kable(df31)
```

| Join\_ID | a\_v2 | b\_v2 |
| -------: | ----: | ----: |
|        1 |    80 |    90 |
|        2 |    70 |    60 |
|        3 |    50 |    40 |

## Exercise 6

Revisiting a data-set used in an earlier exercise set on data
exploration, load the AER package and run the command data(“Fertility”),
which loads the data-set Fertility to your work space.  
Melt it into the following format, with one row per child.

head(ferl)  
morekids age afam hispanic other work mother\_id order gender  
1 no 27 no no no 0 1 1 male  
2 no 30 no no no 30 2 1 female  
3 no 27 no no no 0 3 1 male  
4 no 35 yes no no 0 4 1 male  
5 no 30 no no no 22 5 1 female  
6 no 26 no no no 40 6 1 male

``` r
library(AER)
data("Fertility")
head(Fertility)
```

    ##   morekids gender1 gender2 age afam hispanic other work
    ## 1       no    male  female  27   no       no    no    0
    ## 2       no  female    male  30   no       no    no   30
    ## 3       no    male  female  27   no       no    no    0
    ## 4       no    male  female  35  yes       no    no    0
    ## 5       no  female  female  30   no       no    no   22
    ## 6       no    male  female  26   no       no    no   40

``` r
setDT(Fertility)
ex6 <- melt(Fertility[, mother_id := .I], 
            measure.vars = c("gender1", "gender2"), 
            variable.name = "order", 
            value.name = "gender")

kable(head(ex6[, order := substr(order, 7, 8)]))
```

| morekids | age | afam | hispanic | other | work | mother\_id | order | gender |
| :------- | --: | :--- | :------- | :---- | ---: | ---------: | :---- | :----- |
| no       |  27 | no   | no       | no    |    0 |          1 | 1     | male   |
| no       |  30 | no   | no       | no    |   30 |          2 | 1     | female |
| no       |  27 | no   | no       | no    |    0 |          3 | 1     | male   |
| no       |  35 | yes  | no       | no    |    0 |          4 | 1     | male   |
| no       |  30 | no   | no       | no    |   22 |          5 | 1     | female |
| no       |  26 | no   | no       | no    |   40 |          6 | 1     | male   |

## Exercise 7

Take this:

d1 = data.frame(  
ID=c(1,1,1,2,2,4,1,2),  
medication=c(1,2,3,1,2,7,2,8)  
)  
d1  
ID medication  
1 1 1  
2 1 2  
3 1 3  
4 2 1  
5 2 2  
6 4 7  
7 1 2  
8 2 8  
to this form:

ID medications  
1: 1 1, 2, 3, 2  
2: 2 1, 2, 8  
3: 4 7

> Note: the solution doesn’t use melt() nor dcast(), so you might look
> at other options.

``` r
d1 = data.frame(
  ID=c(1,1,1,2,2,4,1,2), 
  medication=c(1,2,3,1,2,7,2,8)
)
setDT(d1)
ex7 <- d1[, .(medications = paste0(medication, collapse = ", ")), by = .(ID)]
kable(ex7)
```

| ID | medications |
| -: | :---------- |
|  1 | 1, 2, 3, 2  |
|  2 | 1, 2, 8     |
|  4 | 7           |

## Exercise 8

Get this:

dfs \<- data.frame(  
Name = c(rep(“name1”,3),rep(“name2”,2)),  
MedName = c(“atenolol 25mg”,“aspirin 81mg”,“sildenafil 100mg”, “atenolol
50mg”,“enalapril 20mg”)  
)  
dfs  
Name MedName  
1 name1 atenolol 25mg  
2 name1 aspirin 81mg  
3 name1 sildenafil 100mg  
4 name2 atenolol 50mg  
5 name2 enalapril 20mg

Into the following format:

  Name medication\_1 medication\_2 medication\_3  
1: name1 atenolol 25mg aspirin 81mg sildenafil 100mg  
2: name2 atenolol 50mg enalapril 20mg

``` r
dfs <- data.frame(
  Name = c(rep("name1",3),rep("name2",2)),
  MedName = c("atenolol 25mg","aspirin 81mg","sildenafil 100mg", "atenolol 50mg","enalapril 20mg")
)
setDT(dfs)

dfs[, medn := paste0("medication_", 1:.N), by = Name]
kable(dfs)
```

| Name  | MedName          | medn          |
| :---- | :--------------- | :------------ |
| name1 | atenolol 25mg    | medication\_1 |
| name1 | aspirin 81mg     | medication\_2 |
| name1 | sildenafil 100mg | medication\_3 |
| name2 | atenolol 50mg    | medication\_1 |
| name2 | enalapril 20mg   | medication\_2 |

``` r
ex8 <- dcast(dfs, Name ~ medn, value.var = "MedName")
kable(ex8)
```

| Name  | medication\_1 | medication\_2  | medication\_3    |
| :---- | :------------ | :------------- | :--------------- |
| name1 | atenolol 25mg | aspirin 81mg   | sildenafil 100mg |
| name2 | atenolol 50mg | enalapril 20mg | NA               |

## Exercise 9

Get the following data.frame organized in standard form:

df7 \<- data.table(  
v1 = c(“name1, name2”, “name3”, “name4, name5”),  
v2 = c(“1, 2”, “3”, “4, 5”),  
v3 = c(1, 2, 3)  
)  
df7  
v1 v2 v3  
1: name1, name2 1, 2 1  
2: name3 3 2  
3: name4, name5 4, 5 3

Expected output:

  v1 v2 v3  
1: name1 1 1  
2: name2 2 1  
3: name3 3 2  
4: name4 4 3  
5: name5 5 3

> The solution doesn’t use melt() nor dcast() and can be suprisingly
> hard.

``` r
df7 <- data.table(
  v1 = c("name1, name2", "name3", "name4, name5"),
  v2 = c("1, 2", "3", "4, 5"), 
  v3 = c(1, 2, 3)
)
setDT(df7)
# `.SD` stands for something like "`S`ubset of `D`ata.table".
ex9 <- df7[, lapply(.SD, tstrsplit, ", "), by = v3]
kable(ex9)
```

| v3 | v1    | v2 |
| -: | :---- | :- |
|  1 | name1 | 1  |
|  1 | name2 | 2  |
|  2 | name3 | 3  |
|  3 | name4 | 4  |
|  3 | name5 | 5  |

``` r
ex9 <- ex9[, .(v1, v2, v3)]
kable(ex9)
```

| v1    | v2 | v3 |
| :---- | :- | -: |
| name1 | 1  |  1 |
| name2 | 2  |  1 |
| name3 | 3  |  2 |
| name4 | 4  |  3 |
| name5 | 5  |  3 |

## Exercise 10

Convert this:

df \<- data.frame(  
Method = c(“10.fold.CV Lasso”, “10.fold.CV.1SE”, “BIC”,
“Modified.BIC”),  
n = c(30, 30, 50, 50, 50, 50, 100, 100),  
lambda = c(1, 3, 1, 2, 2, 0, 1, 2),  
df = c(21, 17, 29, 26, 25, 32, 34, 32) )  
\> df  
Method n lambda df  
1 10.fold.CV Lasso 30 1 21  
2 10.fold.CV.1SE 30 3 17  
3 BIC 50 1 29  
4 Modified.BIC 50 2 26  
5 10.fold.CV Lasso 50 2 25  
6 10.fold.CV.1SE 50 0 32  
7 BIC 100 1 34  
8 Modified.BIC 100 2 32  
Into:

  Method lambda\_30 lambda\_50 lambda\_100 df\_30 df\_50 df\_100  
1 10.fold.CV Lasso 1 2 21 25  
2 10.fold.CV.1SE 3 0 17 32  
3 BIC 1 1 29 34  
4 Modified.BIC 2 2 26 32

``` r
df <- data.frame(
 Method = c("10.fold.CV Lasso", "10.fold.CV.1SE", "BIC", "Modified.BIC"),
 n = c(30, 30, 50, 50, 50, 50, 100, 100),
 lambda = c(1, 3, 1, 2, 2, 0, 1, 2), 
df = c(21, 17, 29, 26, 25, 32, 34, 32) ) 
setDT(df)
ex10 <- melt(df, id.vars = c("Method", "n"))
kable(ex10)
```

| Method           |   n | variable | value |
| :--------------- | --: | :------- | ----: |
| 10.fold.CV Lasso |  30 | lambda   |     1 |
| 10.fold.CV.1SE   |  30 | lambda   |     3 |
| BIC              |  50 | lambda   |     1 |
| Modified.BIC     |  50 | lambda   |     2 |
| 10.fold.CV Lasso |  50 | lambda   |     2 |
| 10.fold.CV.1SE   |  50 | lambda   |     0 |
| BIC              | 100 | lambda   |     1 |
| Modified.BIC     | 100 | lambda   |     2 |
| 10.fold.CV Lasso |  30 | df       |    21 |
| 10.fold.CV.1SE   |  30 | df       |    17 |
| BIC              |  50 | df       |    29 |
| Modified.BIC     |  50 | df       |    26 |
| 10.fold.CV Lasso |  50 | df       |    25 |
| 10.fold.CV.1SE   |  50 | df       |    32 |
| BIC              | 100 | df       |    34 |
| Modified.BIC     | 100 | df       |    32 |

``` r
ex10 <- dcast(ex10, Method ~ variable + n)
kable(ex10)
```

| Method           | lambda\_30 | lambda\_50 | lambda\_100 | df\_30 | df\_50 | df\_100 |
| :--------------- | ---------: | ---------: | ----------: | -----: | -----: | ------: |
| 10.fold.CV Lasso |          1 |          2 |          NA |     21 |     25 |      NA |
| 10.fold.CV.1SE   |          3 |          0 |          NA |     17 |     32 |      NA |
| BIC              |         NA |          1 |           1 |     NA |     29 |      34 |
| Modified.BIC     |         NA |          2 |           2 |     NA |     26 |      32 |
