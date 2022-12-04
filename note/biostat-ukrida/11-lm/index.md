---
author: lam
title: "Linear Model"
weight: 11
description: >

  Upon seeing a linear trend between two variables, we may guess a potential
  value of interest given a specific data point. When doing so, we are attempting
  an inference on predicted outcomes based on observed events. Given a linear
  trend between multiple variables, such an inference is the basic foundation of
  a linear model, i.e. a mathematical construct in examining the dependent
  variable using known independent variables.

summary: >

  Upon seeing a linear trend between two variables, we may guess a potential
  value of interest given a specific data point. When doing so, we are attempting
  an inference on predicted outcomes based on observed events. Given a linear
  trend between multiple variables, such an inference is the basic foundation of
  a linear model, i.e. a mathematical construct in examining the dependent
  variable using known independent variables.

date: 2020-12-07
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 11-lm
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/11-lm/slide)

Upon seeing a linear trend between two variables, we may guess a potential
value of interest given a specific data point. When doing so, we are attempting
an inference on predicted outcomes based on observed events. Given a linear
trend between multiple variables, such an inference is the basic foundation of
a linear model, i.e. a mathematical construct in examining the dependent
variable using known independent variables.

# Fundamental Concepts

As its name suggests, a *linear* model utilizes *linear* algebra to make
prediction on `\(y\)` given the value of `\(x\)`. Assuming a linear trend between a
pair of `\((x_i, y_i)\)`, then we may calculate `\(y_i\)` as a result of an arbitrary
constant `\(\beta\)` multiplied to `\(x\)`. The value of `\(\beta\)` reflects our estimate
on how `\(x\)` influences `\(y\)`, which termed as an estimate or a slope. However,
sometimes we may observe that `\(x_i=0\)` results in `\(y_i \neq 0\)`. To make a better
estimate, we need to take into account such an occurrence using an intercept
`\(\beta_0\)`. Some limitations in doing an inference, or even designing a model in
general, we can only take into account the estimated value `\(E(y_i)\)`, where our
prediction `\(\hat{y}\)` poses a randomly-distributed error `\(\epsilon\)`. The term
error does not mean an error in computation, rather, it is the residuals
between `\(\hat{y}_i\)` and `\(y\)`.

`\begin{align} \hat{y}_i &= \beta_0 + \beta_1 x_i \\ y_i &= \hat{y} + \epsilon_i \end{align}`

In short, we may learn that a linear model is an extension to the correlation
analysis, where it explains the linearity between `\(x\)` and `\(y\)`. The important
bit about statistical model is its capability to act as an explanatory and
predictive model. An explanatory model aims to explain how independent
variables influence the dependent variable. In doing so, we need to minimize
the multicollinearity so that we are confident each of our independent
variables directly influence the dependent variable without receiving
influences from other independent variables. On the other hand, a predictive
model aims to deliver the closest possible prediction of `\(\hat{y}_i\)`, i.e.
minimizing the error `\(\epsilon\)`.

## Example, please?

``` r
str(iris)
```

    | 'data.frame': 150 obs. of  5 variables:
    |  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
    |  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
    |  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
    |  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
    |  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

``` r
subset(iris, select=-Species) %>% cor()
```

    |              Sepal.Length Sepal.Width Petal.Length Petal.Width
    | Sepal.Length         1.00       -0.12         0.87        0.82
    | Sepal.Width         -0.12        1.00        -0.43       -0.37
    | Petal.Length         0.87       -0.43         1.00        0.96
    | Petal.Width          0.82       -0.37         0.96        1.00

There is a seemingly good correlation between sepal length and petal width. We
will use both variables as our dependent and independent variable,
respectively. We will initially perform a correlation analysis, using the
`cor.test()` function in `R`.

``` r
with(iris, cor.test(Sepal.Length, Petal.Length))
```

    | 
    |   Pearson's product-moment correlation
    | 
    | data:  Sepal.Length and Petal.Length
    | t = 22, df = 148, p-value <2e-16
    | alternative hypothesis: true correlation is not equal to 0
    | 95 percent confidence interval:
    |  0.83 0.91
    | sample estimates:
    |  cor 
    | 0.87

Then we will model the linearity between sepal length and petal length using
`lm()`.

``` r
mod1 <- lm(Sepal.Length ~ Petal.Length, data=iris) %T>% {print(summary(.))}
```

    | 
    | Call:
    | lm(formula = Sepal.Length ~ Petal.Length, data = iris)
    | 
    | Residuals:
    |     Min      1Q  Median      3Q     Max 
    | -1.2468 -0.2966 -0.0152  0.2768  1.0027 
    | 
    | Coefficients:
    |              Estimate Std. Error t value Pr(>|t|)    
    | (Intercept)    4.3066     0.0784    54.9   <2e-16 ***
    | Petal.Length   0.4089     0.0189    21.6   <2e-16 ***
    | ---
    | Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    | 
    | Residual standard error: 0.41 on 148 degrees of freedom
    | Multiple R-squared:  0.76,    Adjusted R-squared:  0.758 
    | F-statistic:  469 on 1 and 148 DF,  p-value: <2e-16

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.mod1-1.png" width="90%" />

We have previously discussed that the intercept `\(\beta_0\)` and slope `\(\beta_i\)`
determine how to predict `\(y\)` using `\(x\)`. But, how do we calculate each value of
`\(\beta\)`? We can derive `\(\beta\)` from our data using two methods: Ordinary
Least Square (OLS) and Maximum Likelihood Estimation (MLE). Both techniques are
solvable using a partial derivative or matrices operation. In this note, we
will only consider the former to build a firm foundation on how they work.

When calculating the optimal value of `\(\beta\)`, we aim to find the best line
fitting our data. Optimal `\(\beta\)` will result in the least amount of error
`\(\epsilon\)`. It also generalizes the model to adapt to unforeseen data. Since a
statistical model calculates the expected value of `\(E(y)\)`, we can anticipate
that a model will draw a line passing through the centroid of
`\((\bar{x},\bar{y})\)`.

## Ordinary Least Square

The Ordinary Least Square (OLS) method looks for the most appropriate model by
minimizing the error `\(\epsilon\)`. A pair of `\((x_i, y_i)\)` are constants relative
to the index `\(i\)`. In case of a simple linear regression, the error only depends
on `\(\beta_{0,1}\)`. We can use partial derivatives to find the optimal values of
`\(\beta\)` in minimizing the error `\(\epsilon\)`.

`\begin{align} \epsilon &= \displaystyle \sum_{i=1}^n (y_i - \hat{y}_i)2 \\ &= \displaystyle \sum_{i=1}^n (y_i - (\beta_0 + \beta_1 x_i))^2 \end{align}`

You may recall learning about derivatives in you high school years. The partial
derivative is similar, the only difference is it allows a partial assignment.
When partially solving one variable, we regard the other as a constant.

`\begin{align} \epsilon &= \displaystyle \sum_{i=1}^n (y_i - (\beta_0 + \beta_1 x_i))^2 \\ \frac{\partial \epsilon}{\partial \beta_0} &= \displaystyle \sum_{i=1}^n -2 (y_i - (\beta_0 + \beta_1 x_i)) \\ \frac{\partial \epsilon}{\partial \beta_1} &= \displaystyle \sum_{i=1}^n -2 x_i (y_i - (\beta_0 + \beta_1 x_i)) \end{align}`

We shall solve the `\(\beta_0\)` as follow:

`\begin{align} \frac{\partial \epsilon}{\partial \beta_0} = \displaystyle \sum_{i=1}^n -2 (y_i - (\beta_0 + \beta_1 x_i)) &= 0\\ -2 \bigg( \displaystyle \sum_{i=1}^n y_i - \sum_{i=1}^n \beta_0 - \sum_{i=1}^n \beta_1 x_i \bigg) &= 0 \\ \displaystyle \sum_{i=1}^n y_i - n \beta_0 - \sum_{i=1}^n \beta_1 x_i &= 0 \\ \beta_0 &= \frac{1}{n} \bigg( \displaystyle \sum_{i=1}^n y_i - \beta_1 \sum_{i=1}^n x_i \bigg) \\ \beta_0 &= \bar{y} - \beta_1 \bar{x} \end{align}`

Then we can proceed by solving the `\(\beta_1\)`:

`\begin{align} \frac{\partial \epsilon}{\partial \beta_1} = \displaystyle \sum_{i=1}^n -2 x_i (y_i - (\beta_0 + \beta_1 x_i)) &= 0 \\ -2 \bigg( \displaystyle \sum_{i=1}^n x_i (y_i - (\bar{y} - \beta_1 \bar{x} + \beta_1 x_i)) \bigg) &= 0\\ \displaystyle \sum_{i=1}^n x_i (y_i - \bar{y}) - \sum_{i=1}^n \beta_1 x_i (x_i - \bar{x}) &= 0 \\ \beta_1 \displaystyle \sum_{i=1}^n x_i (x_i - \bar{x}) &= \sum_{i=1}^n x_i (y_i - \bar{y}) \\ \beta_1 &= \displaystyle \sum_{i=1}^n \frac{x_i(y_i - \bar{y})}{x_i(x_i - \bar{x})} \\ \beta_1 &= \displaystyle \sum_{i=1}^n \frac{(x_i - \bar{x})(y_i - \bar{y})}{(x_i - \bar{x})(x_i - \bar{x})} \\ \end{align}`

As you may have noticed, `\(\beta_1\)` solved into the quotient of covariance and
variance.
