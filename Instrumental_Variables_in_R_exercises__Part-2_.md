Instrumental Variables in R exercises (Part-2)
================
Bassalat Sajjad
22 May 2017

![](http://www.r-exercises.com/wp-content/uploads/2017/05/Rplot_set1.jpeg)

This is the second part of the series on Instrumental Variables. For
other parts of the series follow the tag [instrumental
variables](http://www.r-exercises.com/tag/instrumental-variables/).

In this exercise set we will build on the example from
[part-1](http://www.r-exercises.com/2017/05/15/instrumental-variables-in-r-exercises-part-1/).  
We will now consider an over-identified case i.e. we have multiple IVs
for an endogenous variable. We will also look at tests for endogeneity
and over-identifying restrictions.

Answers to the exercises are available
[here](http://r-exercises.com/2017/05/22/instrumental-variables-in-r-exercises-part-2-solutions/).

## Exercise 1

Load the `AER` package (package description:
[here](https://cran.r-project.org/web/packages/AER/AER.pdf)). Next, load
`PSID1976` dataset provided with the AER package. This has data
regarding labor force participation of married women.  
Define a new data-frame that has data for all married women that were
employed. This data-frame will be used for the following exercises.

``` r
library(data.table)
library(AER)

data("PSID1976")
setDT(PSID1976)

df <- PSID1976[participation == "yes"]
```

## Exercise 2

We will use a more elaborate model as compared to the previous set.  
Regress `log(wage)` on `education`, `experience` and
`experience`^2.

``` r
model2 <- lm(log(wage) ~ education + experience + I(experience^2), data = df)
summary(model2)
```

    ## 
    ## Call:
    ## lm(formula = log(wage) ~ education + experience + I(experience^2), 
    ##     data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -3.08404 -0.30627  0.04952  0.37498  2.37115 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     -0.5220406  0.1986321  -2.628  0.00890 ** 
    ## education        0.1074896  0.0141465   7.598 1.94e-13 ***
    ## experience       0.0415665  0.0131752   3.155  0.00172 ** 
    ## I(experience^2) -0.0008112  0.0003932  -2.063  0.03974 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6664 on 424 degrees of freedom
    ## Multiple R-squared:  0.1568, Adjusted R-squared:  0.1509 
    ## F-statistic: 26.29 on 3 and 424 DF,  p-value: 1.302e-15

## Exercise 3

Previously, we identified `feducation` and `meducation` as possible IVs
for `education`.  
Regress `education` on `experience`, `experience`^2, `feducation` and
`meducation`. Comment on your
results.

``` r
model3 <- lm(education ~ feducation + meducation + experience + I(experience^2), data = df)
summary(model3)
```

    ## 
    ## Call:
    ## lm(formula = education ~ feducation + meducation + experience + 
    ##     I(experience^2), data = df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -7.8057 -1.0520 -0.0371  1.0258  6.3787 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)      9.102640   0.426561  21.340  < 2e-16 ***
    ## feducation       0.189548   0.033756   5.615 3.56e-08 ***
    ## meducation       0.157597   0.035894   4.391 1.43e-05 ***
    ## experience       0.045225   0.040251   1.124    0.262    
    ## I(experience^2) -0.001009   0.001203  -0.839    0.402    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.039 on 423 degrees of freedom
    ## Multiple R-squared:  0.2115, Adjusted R-squared:  0.204 
    ## F-statistic: 28.36 on 4 and 423 DF,  p-value: < 2.2e-16

> Both `feducation` and `meducation` are individually significant.

## Exercise 4

Test the hypothesis that `feducation` and `meducation` are jointly equal
to
zero.

``` r
linearHypothesis(model3, c("feducation = 0", "meducation = 0"), test="F")
```

    ## Linear hypothesis test
    ## 
    ## Hypothesis:
    ## feducation = 0
    ## meducation = 0
    ## 
    ## Model 1: restricted model
    ## Model 2: education ~ feducation + meducation + experience + I(experience^2)
    ## 
    ##   Res.Df    RSS Df Sum of Sq    F    Pr(>F)    
    ## 1    425 2219.2                                
    ## 2    423 1758.6  2    460.64 55.4 < 2.2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

> Both variables are jointly significant in the model.

## Exercise 5

Use 2SLS to estimate the IV based return to
`education`.

``` r
model5 <- lm(log(wage) ~ experience + I(experience^2) + fitted.values(model3), data = df)
summary(model5)
```

    ## 
    ## Call:
    ## lm(formula = log(wage) ~ experience + I(experience^2) + fitted.values(model3), 
    ##     data = df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.1631 -0.3539  0.0326  0.3818  2.3727 
    ## 
    ## Coefficients:
    ##                         Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)            0.0481003  0.4197565   0.115  0.90882   
    ## experience             0.0441704  0.0140844   3.136  0.00183 **
    ## I(experience^2)       -0.0008990  0.0004212  -2.134  0.03338 * 
    ## fitted.values(model3)  0.0613966  0.0329624   1.863  0.06321 . 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.7075 on 424 degrees of freedom
    ## Multiple R-squared:  0.04978,    Adjusted R-squared:  0.04306 
    ## F-statistic: 7.405 on 3 and 424 DF,  p-value: 7.615e-05

## Exercise 6

Use the `ivreg` command to get the same results as in Exercise-5. Note
that standard errors would be
different.

``` r
model6 <- ivreg(log(wage) ~ education + experience + I(experience^2) | feducation + meducation + experience + I(experience^2), data = df)
summary(model6)
```

    ## 
    ## Call:
    ## ivreg(formula = log(wage) ~ education + experience + I(experience^2) | 
    ##     feducation + meducation + experience + I(experience^2), data = df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.0986 -0.3196  0.0551  0.3689  2.3493 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)      0.0481003  0.4003281   0.120  0.90442   
    ## education        0.0613966  0.0314367   1.953  0.05147 . 
    ## experience       0.0441704  0.0134325   3.288  0.00109 **
    ## I(experience^2) -0.0008990  0.0004017  -2.238  0.02574 * 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6747 on 424 degrees of freedom
    ## Multiple R-Squared: 0.1357,  Adjusted R-squared: 0.1296 
    ## Wald test: 8.141 on 3 and 424 DF,  p-value: 2.787e-05

> The estimated coefficient of ‘feducation’ is positive and significant.

## Exercise 7

There is a short form for the `ivreg` command which saves time when we
are dealing with numerous variables.  
Try the short format and check that you get the same results as in
Exercise-6.

``` r
model7 <- ivreg(log(wage) ~ education + experience + I(experience^2) | .- education +    feducation + meducation,  data = df)
summary(model7)
```

    ## 
    ## Call:
    ## ivreg(formula = log(wage) ~ education + experience + I(experience^2) | 
    ##     . - education + feducation + meducation, data = df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.0986 -0.3196  0.0551  0.3689  2.3493 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)      0.0481003  0.4003281   0.120  0.90442   
    ## education        0.0613966  0.0314367   1.953  0.05147 . 
    ## experience       0.0441704  0.0134325   3.288  0.00109 **
    ## I(experience^2) -0.0008990  0.0004017  -2.238  0.02574 * 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6747 on 424 degrees of freedom
    ## Multiple R-Squared: 0.1357,  Adjusted R-squared: 0.1296 
    ## Wald test: 8.141 on 3 and 424 DF,  p-value: 2.787e-05

## Exercise 8

Regress `log(wage)` on `education`, `experience`, `experience`^2 and
residuals from the model estimated in Exercise-3.  
Use your result to test for the endogeneity of
`education`.

``` r
model8 <- lm(log(wage) ~ education + experience + I(experience^2) +              residuals(model3), data = df)
summary(model8)
```

    ## 
    ## Call:
    ## lm(formula = log(wage) ~ education + experience + I(experience^2) + 
    ##     residuals(model3), data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -3.03743 -0.30775  0.04191  0.40361  2.33303 
    ## 
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)        0.0481003  0.3945753   0.122 0.903033    
    ## education          0.0613966  0.0309849   1.981 0.048182 *  
    ## experience         0.0441704  0.0132394   3.336 0.000924 ***
    ## I(experience^2)   -0.0008990  0.0003959  -2.271 0.023672 *  
    ## residuals(model3)  0.0581666  0.0348073   1.671 0.095441 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.665 on 423 degrees of freedom
    ## Multiple R-squared:  0.1624, Adjusted R-squared:  0.1544 
    ## F-statistic:  20.5 on 4 and 423 DF,  p-value: 1.888e-15

> The coefficient for the residuals is significant at the 10% level. We
> can conclude that education is endogenous.

## Exercise 9

Regress the residuals from exercise-7 on `experience`, `experience`^2,
`feducation` and `meducation`.  
Use your result to test for over-identifying
restrictions.

``` r
model9 <- lm(residuals(model7) ~ experience + I(experience^2) + meducation + feducation,
             data = df)
names(summary(model9))
```

    ##  [1] "call"          "terms"         "residuals"     "coefficients" 
    ##  [5] "aliased"       "sigma"         "df"            "r.squared"    
    ##  [9] "adj.r.squared" "fstatistic"    "cov.unscaled"

``` r
pchisq(summary(model9)$r.squared*428, 1, lower.tail = FALSE)
```

    ## [1] 0.5386372

> p-value is higher than 5%. So, `meducation` and `feducation` clear the
> over-identification criteria.

## Exercise 10

The two tests we did in exercises 8 and 9 can be conveniently obtained
using the `summary` command with diagnostics turned on. Verify that you
get the same test results with the `summary` command.

``` r
summary(model7, diagnostics = TRUE)
```

    ## 
    ## Call:
    ## ivreg(formula = log(wage) ~ education + experience + I(experience^2) | 
    ##     . - education + feducation + meducation, data = df)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.0986 -0.3196  0.0551  0.3689  2.3493 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)      0.0481003  0.4003281   0.120  0.90442   
    ## education        0.0613966  0.0314367   1.953  0.05147 . 
    ## experience       0.0441704  0.0134325   3.288  0.00109 **
    ## I(experience^2) -0.0008990  0.0004017  -2.238  0.02574 * 
    ## 
    ## Diagnostic tests:
    ##                  df1 df2 statistic p-value    
    ## Weak instruments   2 423    55.400  <2e-16 ***
    ## Wu-Hausman         1 423     2.793  0.0954 .  
    ## Sargan             1  NA     0.378  0.5386    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6747 on 424 degrees of freedom
    ## Multiple R-Squared: 0.1357,  Adjusted R-squared: 0.1296 
    ## Wald test: 8.141 on 3 and 424 DF,  p-value: 2.787e-05
