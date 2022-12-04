---
author: lam
title: "Correlation of Numeric Variables"
weight: 10
description: >

  So far, we have solved hypotheses testings for condition where we have numeric
  values as our dependent variable and categoric data as our independent
  variables. We have yet to see the solution if we have numeric variables for
  both of the dependent and independent variables. After learning the descriptive
  statistics, we understand that we can observe the spread in our data by
  measuring the variance, i.e. the dispersion of each data element relative to
  the mean value. Similarly, we can understand the dispersion of two numeric
  variables by accounting the covariance.

summary: >

  So far, we have solved hypotheses testings for condition where we have numeric
  values as our dependent variable and categoric data as our independent
  variables. We have yet to see the solution if we have numeric variables for
  both of the dependent and independent variables. After learning the descriptive
  statistics, we understand that we can observe the spread in our data by
  measuring the variance, i.e. the dispersion of each data element relative to
  the mean value. Similarly, we can understand the dispersion of two numeric
  variables by accounting the covariance.

date: 2020-12-03
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 10-cor
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/10-cor/slide)

So far, we have solved hypotheses testings for condition where we have numeric
values as our dependent variable and categoric data as our independent
variables. We have yet to see the solution if we have numeric variables for
both of the dependent and independent variables. After learning the descriptive
statistics, we understand that we can observe the spread in our data by
measuring the variance, i.e. the dispersion of each data element relative to
the mean value. Similarly, we can understand the dispersion of two numeric
variables by accounting the covariance.

# Covariance

The covariance describes the length of relationship between two numeric
variables. In calculating and interpreting covariance, we are interested to
measure the sign, a trend where one variable may vary depends on the other. The
question we wish to address is how a variable `\(y\)` will behave if we know the
value of `\(x\)`. In covariance, the magnitude is not directly interpretable, as it
is not scale free. It is more intuitive to understand covariance when we know
its equation, presented as follow:

`$$\sigma_{x, y} = \frac{\displaystyle \sum_{i=1}^n(x_i - \mu_x)(y_i - \mu_y)}{n}$$`

We see from the enumerator that the covariance takes into account the residual
of each data element relative to the mean value. We can imagine that due to the
difference of scale between `\(x\)` and `\(y\)`, the output of covariance calculation
will not clearly reflect the magnitude of relationship. Suppose we have two
dataset, each with `\(x\)` and `\(y\)` following a different scale. If the first
dataset have `\(x \in [100, 200]\)` and `\(y \in [1000, 2000]\)` while the second one
have `\(x \in [50, 100]\)` and `\(y \in [20, 50]\)`, we will have a different
covariance value even though both has roughly similar linear relationship.
Linearity is the important bit in covariance, where we assume both `\(x\)` and `\(y\)`
follow a similar trend. Please be advised though, as in calculating the
variance, the sampled covariance will require a Bessel’s correction, where we
will use the following equation to measure covariance in a sample:

`$$s_{x, y} = \frac{\displaystyle \sum_{i=1}^n(x_i - {\bar{x}}) (y_i - {\bar{y}})}{({n-1})}$$`

Another important concept we need to understand in calculating covariance is
the covariance matrix. In this matrix, we can take a glimpse of the general
relationship between multiple pairs of numeric variables. Having a covariance
matrix is crucial to determine the overall trend at a glance. Covariance matrix
provides a useful descriptive statistics before designing a complex model,
which we will learn in the next lectures on the linear model (lm) and
generalized linear model (glm).

## Example, please?

``` r
tbl <- subset(iris, select=c(Sepal.Width, Sepal.Length)) %>% str()
```

    | 'data.frame': 150 obs. of  2 variables:
    |  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    |  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...

In this example, we will use a subset of the `iris` dataset, which we have
grown accustomed to. This subset will only include two variables, `Sepal.Width`
and `Sepal.Length`. From this point onwards, we will set `x` to represent the
width and `y` to represent the length. To help us visualize the concept, we
will calculate the residual to the mean as `x.resid` and `y.resid`, each
reflected `\(x_i - \bar{x}\)` and `\(y_i - \bar{y}\)`. The following table describes
the first six rows in our data:

|   x |   y | x.resid | y.resid |
|----:|----:|--------:|--------:|
| 5.1 | 3.5 |   -0.74 |    0.44 |
| 4.9 | 3.0 |   -0.94 |   -0.06 |
| 4.7 | 3.2 |   -1.14 |    0.14 |
| 4.6 | 3.1 |   -1.24 |    0.04 |
| 5.0 | 3.6 |   -0.84 |    0.54 |
| 5.4 | 3.9 |   -0.44 |    0.84 |

Now, we will make a function describing the covariance equation. As previously
described, this function will take two arguments, `x` and `y`. Then it will
calculate the mean and residual to the mean for each `x` and `y`. Here, the
value of both inputs are vectors. Then we get the product of each residual from
`x` and `y`, where the addition of all products will divide by `\(n-1\)`.

``` r
covariance <- function(x, y) {
    n <- length(x) # Length of x must be = length of y
    {(x - mean(x)) * (y - mean(y))} %>% sum() %>% divide_by(n-1)
}
```

We can compare the covariance from our function and the covariance
resulted from a built-in function in `R`.

``` r
covariance(tbl$x, tbl$y)
```

    | [1] -0.042

``` r
cov(tbl$x, tbl$y) # Built-in function
```

    | [1] -0.042

It is also interesting to explore what will happen if we calculate covariances of
the same variable. As it turns out, the covariance of the same variable is its
variance!

``` r
covariance(tbl$x, tbl$x)
```

    | [1] 0.69

``` r
var(tbl$x) # Variance of x
```

    | [1] 0.69

It is pretty straightforward if we closely evaluate the equation:

`$$s_{x, x} = \frac{\displaystyle \sum_{i=1}^n(x_i - {\bar{x}}) (x_i - {\bar{x}})}{({n-1})}$$`

Previously, we mentioned a concept on covariance matrices. Of course we can do
that to in `R` using the `cov` function.

``` r
tbl <- subset(iris, select=-Species) %T>% str()
```

    | 'data.frame': 150 obs. of  4 variables:
    |  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
    |  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    |  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
    |  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...

``` r
cov(tbl)
```

    |              Sepal.Length Sepal.Width Petal.Length Petal.Width
    | Sepal.Length        0.686      -0.042         1.27        0.52
    | Sepal.Width        -0.042       0.190        -0.33       -0.12
    | Petal.Length        1.274      -0.330         3.12        1.30
    | Petal.Width         0.516      -0.122         1.30        0.58

# Pearson’s `\(r\)`

Correlation is a step after covariance, where we can employ Pearson’s method to
measure the trend and magnitude of a relationship between two numeric
variables. Correlation is dimension free, as it takes into account the standard
deviation on how both variables differ from each other. In a way, correlation
is the degree of relationship between two standardized score, both estimated in
a Z distribution.

`\begin{align} r &= \frac{{s_{x,y}}}{s_x \cdot s_y} \\   &= \displaystyle \sum_{i=1}^n \frac{{(x-\bar{x}) (y-\bar{y})}} {{(n-1)} \cdot s_x \cdot s_y} \\   &= \displaystyle \sum_{i=1}^n \frac{\big( \frac{x-\bar{x}}{s_x} \big) \cdot \big( \frac{y-\bar{y}}{s_y} \big)}{n-1} \\   &= \frac{Z_x \cdot Z_y}{n-1} \\ \\ \nu &= n - 2 \tag{DoF} \end{align}`

As it employs a Z-score to measure the relationship between two values,
normally-distributed variables will have their association fully described
using Pearson’s `\(r\)` method. Please recall that `\(Z \sim N(0,1)\)`, since $r \to Z$, `\(r\)` does not care for the measurement unit, i.e. scale free.

`$$t = \frac{r}{\sqrt{\frac{1-r^2}{n-2}}}$$`

To denote significance, we shall calculate the `\(t: t \sim T(\nu)\)` statistics.
Please be advised, there exists another method of determining the significance,
but we only consider the simplest method available. In calculating the
correlation, we assume:

-   I.I.D
-   Univariate normality
-   Bivariate normality
-   Has a linear relationship

The important concept we need to highlight in this section is the presence of a
joint distribution, i.e. the bivariate normality assumption. Suppose we have
two variables of `\(x\)` and `\(y\)`, then we understand each variables has its own
distribution. However, when we *jointly* distribute each data point, i.e. by
assigning a pair of `\((x_i, y_i)\)`, we will obtain a joint distribution. A joint
distribution may follow a certain probability characteristic, and to utilize
the full extent of Pearson’s `\(r\)` correlation, we expect to have a normal joint
distribution of `\(x\)` and `\(y\)`. Another important concept here is linearity, where
we can eyeball the relationship using a scatter plot. In linearity assumption,
we shall visualize a linear pattern depicting a relationship of both variables.
Fulfilling required assumptions, we can proceed with formulating the
hypotheses:

-   `\(H_0\)`: Both variables do not have a linear relationship
-   `\(H_1\)`: Both variables have a linear relationship

## Example, please?

This example will use variables from the previous one, where we first tested
for the univariate normality.

``` r
lapply(tbl, shapiro.test) %>% lapply(broom::tidy) %>% lapply(data.frame) %>%
    {do.call(rbind, .)} %>% kable(format="simple")
```

|              | statistic | p.value | method                      |
|--------------|----------:|--------:|:----------------------------|
| Sepal.Length |      0.98 |    0.01 | Shapiro-Wilk normality test |
| Sepal.Width  |      0.98 |    0.10 | Shapiro-Wilk normality test |
| Petal.Length |      0.88 |    0.00 | Shapiro-Wilk normality test |
| Petal.Width  |      0.90 |    0.00 | Shapiro-Wilk normality test |

<img src="{{< blogdown/postref >}}index_files/figure-html/pearson2-1.png" width="100%" />

From both Shapiro-Wilk test and a visual examination, we may conclude that the
`Sepal.Width` follows a normal distribution and `Sepal.Length` is seemingly
close to the normal distribution. Using a QQ-plot, we do not observe many
normality violations in the `Sepal.Length` variable. Then we proceeded
with the bivariate normality assumption.

``` r
subset(tbl, select=c(Sepal.Length, Sepal.Width)) %>%
    MVN::mvn() # Multivariate normality
```

    | $multivariateNormality
    |            Test  HZ p value MVN
    | 1 Henze-Zirkler 2.9   8e-07  NO
    | 
    | $univariateNormality
    |               Test     Variable Statistic   p value Normality
    | 1 Anderson-Darling Sepal.Length      0.89     0.022    NO    
    | 2 Anderson-Darling Sepal.Width       0.91     0.020    NO    
    | 
    | $Descriptives
    |                n Mean Std.Dev Median Min Max 25th 75th Skew Kurtosis
    | Sepal.Length 150  5.8    0.83    5.8 4.3 7.9  5.1  6.4 0.31    -0.61
    | Sepal.Width  150  3.1    0.44    3.0 2.0 4.4  2.8  3.3 0.31     0.14

Using the Mardia’s test to calculate skewness and kurtosis of a joint
distribution, we see that a joint distribution of `Sepal.Width` and
`Sepal.Length` follows the normal distribution. In `R`, calculating correlation
is a simple task using the `cor.test()` function, where it takes two necessary
arguments of sample variables.

``` r
cor.test(tbl$Sepal.Length, tbl$Sepal.Width)
```

    | 
    |   Pearson's product-moment correlation
    | 
    | data:  tbl$Sepal.Length and tbl$Sepal.Width
    | t = -1, df = 148, p-value = 0.2
    | alternative hypothesis: true correlation is not equal to 0
    | 95 percent confidence interval:
    |  -0.273  0.044
    | sample estimates:
    |   cor 
    | -0.12

The correlation coefficient `\(r\)` will have a range of value `\([-1, 1]\)`, where it
reflects the trend and magnitude of a relationship between two numeric
variables.

<img src="{{< blogdown/postref >}}index_files/figure-html/pearson4-1.png" width="100%" />

This plot visualizes the linearity and the extent of correlation between
`Sepal.Length` and `Sepal.Width`.

# Spearman’s `\(\rho\)`

As Pearson’s `\(r\)` is a parametric test, we have stringent assumptions to follow.
In case we could not satisfy the assumption, i.e. when we have an ordinal data
or the relationship is not completely linear, it is best to employ a
non-parametric method. Spearman’s `\(\rho\)` is the non-parametric correlation
test, suitable to handle ordinal data. In some cases, Spearman’s `\(\rho\)`
performs considerably well to measure correlation between non-normally
distributed numeric data. However, Spearman’s `\(\rho\)` method is not sufficient
to correctly handle tied values, as we will see in its equation.

`\begin{align} \rho &= 1 - \frac{6 \sum (R_x - R_y)^2}{n (n^2 - 1)} \\ \nu  &= n - 2 \tag{DoF} \end{align}`

In calculating `\(\rho\)`, the value of `\(R_{x, y}\)` is the rank for variables `\(x\)`
and `\(y\)`. Ranking in Spearman’s method follows an order within one variable,
i.e. not the pooled data. By assigning rank, we can address non-linearity to a
certain degree. As an alternative to this equation, we can use Pearson’s
method while substituting the actual value to its rank. This method handles
tied values by taking the average rank (kindly refer to our discussion in
non-parametric mean difference topics).

`$$t = \frac{\rho}{\sqrt{\frac{1-\rho^2}{n-2}}}$$`

As in Pearson’s `\(r\)`, we measure the significance by calculating the `\(t\)`
statistics. In the presence of tie, Spearman’s method faces difficulties in
confidently determine the p-value. Even though not assuming the normality,
there be a few assumption we need to satisfy so that we can use the Spearman’s
method correctly:

-   I.I.D
-   Monotonic trend
-   Has a natural order

Here we introduced a new terminology, *viz.* monotonic trend. As to make things
a bit easier to understand, please know that linearity is a subset of a
monotonic trend. Monotonic trend simply describes a consistent trend between
two variables, either be an upward or downward slope. Since monotonic trend is
a general form of a linear trend, it does not have to fully address linearity.
It means that we can expect a slight curvature in a monotonic trend as long as
it presents with a overtime consistent change.

## Example, please?

This example is only for an illustrative purpose, as we agreed that Spearman’s
method finds its best uses to measure correlation in an ordinal data. We keep
on using the subset of an `iris` dataset just to keep this example simple.

``` r
tbl <- subset(iris, select=-Species) %T>% str()
```

    | 'data.frame': 150 obs. of  4 variables:
    |  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
    |  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    |  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
    |  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...

``` r
cov(tbl)
```

    |              Sepal.Length Sepal.Width Petal.Length Petal.Width
    | Sepal.Length        0.686      -0.042         1.27        0.52
    | Sepal.Width        -0.042       0.190        -0.33       -0.12
    | Petal.Length        1.274      -0.330         3.12        1.30
    | Petal.Width         0.516      -0.122         1.30        0.58

``` r
cor.test(tbl$Sepal.Length, tbl$Sepal.Width, method="spearman")
```

    | 
    |   Spearman's rank correlation rho
    | 
    | data:  tbl$Sepal.Length and tbl$Sepal.Width
    | S = 7e+05, p-value = 0.04
    | alternative hypothesis: true rho is not equal to 0
    | sample estimates:
    |   rho 
    | -0.17

As in `\(r\)`, the value of `\(\rho\)` will fall in the range between `\([-1, 1]\)`, where
it respectively describes a negative and positive trend. The magnitude in
Spearman’s method describes the degree of correlation between the *ranks*, not
the actual value. This is one drawback of choosing Spearman’s instead of
Pearson’s correlation.

# Kendall’s `\(\tau\)`

Kendall’s `\(\tau\)` is another non-parametric correlation test. However, a bit
different from Spearman’s and Pearson’s, Kendall’s method measure the degree of
concordance between two variables. Kendall’s `\(\tau\)` is most suitable to handle
an ordinal data, where its variant provides a good measure on data with tied
values. Kendall’s `\(\tau\)` presents with three methods of `\(\tau_A\)`, `\(\tau_B\)` and
`\(\tau_C\)`. The preference of choosing which method to employ should align with
the shape of our data. Both of `\(\tau_A\)` and `\(\tau_B\)` are suitable to handle
ordinal data with the same measurement scale, e.g. both are using 10-point
Likert scale. While in `\(\tau_C\)`, we can use two ordinal variables with
different measurement scale. It is easier to understand this concept by
examining the equation. However, before that, it is important to understand
what we mean by concordant and discordant data.

For `\(i, j \in X, Y: i \neq j,\ \exists\ (x_{i, j}, y_{i, j})\)`. For each pair of
`\((x_{i, j}, y_{i, j})\)`, we regard concordance as $(x_i < x_j \ \texttt{and}\
y_i < y_j) \lor (x_i > x_j \ \texttt{and}\  y_i > y_j)$, or otherwise a
discordance. In other words, we have a concordant pair when we observe the same
relationship for index `\(i\)` and `\(j\)` in both variables.

`\begin{align} \tau_a &= \frac{n_c - n_d}{n}\\ \tau_b &= \frac{n_c - n_d}{\sqrt{(n + X_0) (n + Y_0)}} \\ \tau_c &= \frac{2(n_c - n_d)}{n^2 \frac{(m-1)}{m}} \\ n      &= \binom{n}{2} \end{align}`

Here, `\(n_c\)` and `\(n_d\)` are the numbers of concordant and discordant data,
respectively. In the second equation of `\(\tau_B\)`, we also count for $X_0 and Y_0$, which is the number of ties. In `\(\tau_C\)`, we have `\(m\)` which reflects the
minimum number of row and column between two different measurement scales. In
brief, we know how to use each method and we can conclude that:

-   `\(\tau_a\)`: Square table (same measurement scale)
-   `\(\tau_b\)`: Square table, handles tie
-   `\(\tau_c\)`: Rectangular table (different scale), handles tie

## Example, please?

Again, this example will use the same data subset.

``` r
tbl <- subset(iris, select=-Species) %T>% str()
```

    | 'data.frame': 150 obs. of  4 variables:
    |  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
    |  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    |  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
    |  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...

``` r
cov(tbl)
```

    |              Sepal.Length Sepal.Width Petal.Length Petal.Width
    | Sepal.Length        0.686      -0.042         1.27        0.52
    | Sepal.Width        -0.042       0.190        -0.33       -0.12
    | Petal.Length        1.274      -0.330         3.12        1.30
    | Petal.Width         0.516      -0.122         1.30        0.58

``` r
cor.test(tbl$Sepal.Length, tbl$Sepal.Width, method="kendall")
```

    | 
    |   Kendall's rank correlation tau
    | 
    | data:  tbl$Sepal.Length and tbl$Sepal.Width
    | z = -1, p-value = 0.2
    | alternative hypothesis: true tau is not equal to 0
    | sample estimates:
    |    tau 
    | -0.077

The value of `\(\tau\)` is in the range of `\([0, 1]\)`, with any sign of `\(\tau < 0\)` being an
artefact. So, when interpreting the `\(\tau\)`, we will only take the absolute
value, `\(|\tau|\)`. The base installation of `R` only implements `\(\tau_A\)`
calculation. If needed, other packages are available to provide a different
Kendall’s approach.

# Recap

-   Check normality
-   Check linearity
-   Non-parametric test: determine the presence of tie
-   Perform correlation
-   Create the plot (if necessary)

# Caveats

-   We only discussed *some* of the popular correlation test
-   All discussed methods assume I.I.D
-   Paired data is suitable for none of discussed methods
-   Time series data requires a different approach
-   Correlation `\({\neq}\)` Causation

# More on Correlations

-   Concordance correlation coefficient
-   Intraclass correlation
-   Partial correlation
-   Zero-order correlation
-   The list goes on…
