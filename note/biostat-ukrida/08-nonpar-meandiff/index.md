---
author: lam
title: "Non-parametric: Differences in Two Groups"
weight: 8
description: >

  A parametric test requires us to assume or hypothesize parameters in a
  population. Often, a small sample size or a highly-skewed distribution does not
  resemble a normal distribution. In such a case, it becomes impertinent to
  assume normality in our data. Even though parametric tests are quite robust
  against non-normal data to a certain degree, it still requires a large number
  of sample. With larger `\(n\)` and homogeneous intergroup variance, the parametric
  test may have a sufficient power to correctly reject the `\(H_0\)`. However, if we
  cannot satisfy the required assumption, we need to drop our hypothesized claim
  of population parameters. In other words, we are employing a non-parametric
  test to measure observed differences.

summary: >

  A parametric test requires us to assume or hypothesize parameters in a
  population. Often, a small sample size or a highly-skewed distribution does not
  resemble a normal distribution. In such a case, it becomes impertinent to
  assume normality in our data. Even though parametric tests are quite robust
  against non-normal data to a certain degree, it still requires a large number
  of sample. With larger `\(n\)` and homogeneous intergroup variance, the parametric
  test may have a sufficient power to correctly reject the `\(H_0\)`. However, if we
  cannot satisfy the required assumption, we need to drop our hypothesized claim
  of population parameters. In other words, we are employing a non-parametric
  test to measure observed differences.

date: 2020-11-15
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 08-nonpar-meandiff
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/08-nonpar-meandiff/slide)

A parametric test requires us to assume or hypothesize parameters in a
population. Often, a small sample size or a highly-skewed distribution does not
resemble a normal distribution. In such a case, it becomes impertinent to
assume normality in our data. Even though parametric tests are quite robust
against non-normal data to a certain degree, it still requires a large number
of sample. With larger `\(n\)` and homogeneous intergroup variance, the parametric
test may have a sufficient power to correctly reject the `\(H_0\)`. However, if we
cannot satisfy the required assumption, we need to drop our hypothesized claim
of population parameters. In other words, we are employing a non-parametric
test to measure observed differences.

# Non-Parametric Test

Given a small sample size, it is difficult for us to ascertain normality. As we
have previously discussed, a large sample size merits a high statistical power.
Conversely, when we have a low sample size, it is just natural to expect a
lower statistical power. Asides from normality assumption, parametric tests are
also sensitive to a severe skewness, because the average value will not
represent the central tendency. Non-parametric test neither assume normality
nor symmetricity, thus providing a legible approach for data unsuitable for
parametric tests. However, having a more lenient assumption means to neglect
some information in the data, resulting in a lower statistical power compared to
the parametric test given its assumptions fulfilled. Following figures
represent how the `\(\mu\)` not describing the central tendency in a skewed data
(bottom).

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.data-1.png" width="100%" />

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.data.skew-1.png" width="100%" />

As a summary, we should consider using a non-parametric test in the case of
having a small sample size. If our data is not asymptotically normal, then
employing a non-parametric test might be a more appropriate step. The presence
of extreme outliers or severe skewness may impair the parametric test, so using
a non-parametric test is desirable. In conducting a non-parametric test, we
hypothesize on the difference in our observation compared to its reference. To
describe the difference, we shall use its median value `\(M\)`, therefore:

-   `\(H_0:\ M_1 = M_2\)`
-   `\(H_1:\ M_1 \neq M_2\)`

# One-Sample Test

Similar to the parametric test, in one-sample test we only have one group of
observation. We would like to know whether our group deviates from the
hypothesized median. We may employ two type of test, i.e. a one-sample sign
test and one-sample Wilcoxon signed rank test. Only the one-sample Wilcoxon
test is analogous to the one-sample T-Test

## One-Sample Sign Test

A one-sample sign test does not assume normality nor symmetric distribution. It
is useful in a skewed data, where the statistics follow a binomial
distribution. In fact, it is an extension to the binomial test, which we have
discussed in the [previous lecture](https://lamurian.rbind.io/note/biostat-ukrida/05-independence/).
In this test, we know that our statistics `\(B_s \sim B(n, p)\)` with `\(0=0.5\)`. We
set the probability `\(p=0.5\)` because the random chance of having `\(M_1 \neq M_0\)`
is 0.5, as the median is the midpoint.

To calculate one-sample sign test, we need to:

-   Find the residual between our observation and hypothesized median
-   Omit all 0
-   Disregard the magnitude, take only its sign
-   Calculate the frequency of positive and negative signs
-   Let `\(B_s\)` be the resultant `\(\to B_s \sim B(n, 0.5)\)`

Essentially, we only have two outcome of interest, i.e. having a positive or
negative sign. Assuming I.I.D in each of our observation, we have a Bernoulli
trial with `\(p=0.5\)`, such that we can model our probability as a Binomial
distribution.

### Example, please?

``` r
# Generate a skewed data using a Chi-squared distribution
set.seed(1)
x <- rchisq(10, 4) %T>% print()
```

    ##  [1] 1.66 7.14 6.93 4.10 7.77 5.08 4.58 2.30 1.36 1.67

In this example, we have `\(X \sim \chi^2(4)\)`, presenting as a skewed data.
Should we let `\(H_0\)` be `\(M=5\)` and we are interested to conduct a two-tailed
test, we can proceed using a one-sample sign test.

``` r
# Set M and find the residual (difference)
M <- 5
diff <- {x - M}

# Make a data frame
tbl <- data.frame(x=x, abs.diff=abs(diff), sign=sign(diff))
```

|    x | abs.diff | sign |
|-----:|---------:|-----:|
| 1.66 |    3.338 |   -1 |
| 7.14 |    2.142 |    1 |
| 6.93 |    1.926 |    1 |
| 4.10 |    0.898 |   -1 |
| 7.77 |    2.771 |    1 |
| 5.08 |    0.081 |    1 |
| 4.58 |    0.424 |   -1 |
| 2.30 |    2.701 |   -1 |
| 1.36 |    3.638 |   -1 |
| 1.67 |    3.329 |   -1 |

``` r
# Perform a binomial test
res <- lapply(c(-1, 1), function(sign) {
    binom.test(sum(tbl$sign==sign), nrow(tbl), 0.5) %>%
        broom::tidy()
})

# Two-tailed test on sign=-1
knitr::kable(res[[1]], format="simple")
```

| estimate | statistic | p.value | parameter | conf.low | conf.high | method              | alternative |
|---------:|----------:|--------:|----------:|---------:|----------:|:--------------------|:------------|
|      0.6 |         6 |   0.754 |        10 |    0.262 |     0.878 | Exact binomial test | two.sided   |

``` r
# Two-tailed test on sign=1
knitr::kable(res[[2]], format="simple")
```

| estimate | statistic | p.value | parameter | conf.low | conf.high | method              | alternative |
|---------:|----------:|--------:|----------:|---------:|----------:|:--------------------|:------------|
|      0.4 |         4 |   0.754 |        10 |    0.122 |     0.738 | Exact binomial test | two.sided   |

As we have demonstrated, a non-parametric test is straightforward and simple.
Statistics obtained from a one-sample sign test only taking into account the
difference and neglecting the magnitude. This test is suitable to avoid
incorrectly rejecting the `\(H_0\)` due to biased calculation in the presence of
severe skewness.

## One-Sample Wilcoxon Signed Rank Test

Similar to the one-sample sign test, one-sample Wilcoxon test also does not
assume normality. Contrarily, it assumes a symmetric distribution because
one-sample Wilcoxon test is not suitable for a severely-skewed data. We may
start to wonder, what distribution has a symmetric shape but not normal? Well,
that is a fair question, because so far we rarely discuss about non-normally
distributed symmetric data. Though, we have briefly mentioned one of them in
the second lecture, the uniform distribution. Of course, there are more
examples such as Cauchy distribution, a generalized normal distribution (which
is not normal!), and so on. We will not dig too much into this topic, but
please be aware that we may have a symmetric but non-normally distributed data.

One-sample Wilcoxon test has a similar procedure to one-sample sign test.
However, we assign ranks based on computed differences. The statistics is the
resultants of signed rank, where we only consider the minimum value between
both the sum of positive and negative ranks. Thus, we sometimes refer
one-sample Wilcoxon as a sum rank signed test.

### Example, please?

To keep a consistent remark, we shall re-use the previous data. Please be
advised, we need to ascertain symmetricity before proceeding with a one-sample
Wilcoxon test. To do so, we may refer to a skewness measure of our data. When
we have a negative value, it means our data is left skewed, *vice versa*.
Acquiring the skewness, we may use the range of `\([-1, 1]\)` to decide that our
data does not present with a severe skewness impairing its symmetricity.

``` r
# Generate a skewed data using a Chi-squared distribution
set.seed(1)
x <- rchisq(10, 4) %T>% print()
```

    ##  [1] 1.66 7.14 6.93 4.10 7.77 5.08 4.58 2.30 1.36 1.67

``` r
# Add columns to data frame
tbl$ranked <- rank(tbl$abs.diff)
```

|    x | abs.diff | sign | ranked |
|-----:|---------:|-----:|-------:|
| 1.66 |    3.338 |   -1 |      9 |
| 7.14 |    2.142 |    1 |      5 |
| 6.93 |    1.926 |    1 |      4 |
| 4.10 |    0.898 |   -1 |      3 |
| 7.77 |    2.771 |    1 |      7 |
| 5.08 |    0.081 |    1 |      1 |
| 4.58 |    0.424 |   -1 |      2 |
| 2.30 |    2.701 |   -1 |      6 |
| 1.36 |    3.638 |   -1 |     10 |
| 1.67 |    3.329 |   -1 |      8 |

``` r
# Calculate the statistics
W <- tapply(tbl$ranked, tbl$sign, sum) %>% min() %T>% print()
```

    ## [1] 17

``` r
# Find the p-value for a two-tailed test
psignrank(W, nrow(tbl)) * 2
```

    ## [1] 0.322

``` r
# Built-in test
wilcox.test(x, data=tbl, mu=5)
```

    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  x
    ## V = 17, p-value = 0.3
    ## alternative hypothesis: true location is not equal to 5

# Two-Sample Test

Mann-Whitney U test is an unpaired two-sample Wilcoxon test. As in other
non-parametric tests, it does not assume normality, although it requires the
data be I.I.D. Mann-Whitney U test can also handle skewed data with a small
sample size. The concept in Mann-Whitney U test is the sum of ranks, where we
pooled all the data elements from both groups. Then we sorted pooled values,
starting from the smallest to largest. Similar to one-sample Wilcoxon test, we
have to assign a rank to each value in order to compare both groups.

## Example, please?

``` r
# We will use x as the first group
x
```

    ##  [1] 1.66 7.14 6.93 4.10 7.77 5.08 4.58 2.30 1.36 1.67

``` r
# Assign x+4 as the second group, make a data frame
tbl <- data.frame(
    obs=c(x, x+4), 
    group=rep(c("1", "2"), each=length(x)) %>% factor()
) %T>% str()
```

    ## 'data.frame':    20 obs. of  2 variables:
    ##  $ obs  : num  1.66 7.14 6.93 4.1 7.77 ...
    ##  $ group: Factor w/ 2 levels "1","2": 1 1 1 1 1 1 1 1 1 1 ...

``` r
# Goodness of fit test to determine the distribution
tapply(tbl$obs, tbl$group, ks.test, pnorm) %>% lapply(broom::tidy) %>%
    lapply(data.frame) %>% {do.call(rbind, .)} %>% kable(format="simple")
```

| statistic | p.value | method                                   | alternative |
|----------:|--------:|:-----------------------------------------|:------------|
|     0.913 |       0 | Exact one-sample Kolmogorov-Smirnov test | two-sided   |
|     1.000 |       0 | Exact one-sample Kolmogorov-Smirnov test | two-sided   |

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.tbl-1.png" width="100%" />

As we can see, both data presented with a non-normal distribution. Thus,
comparing both observations will require a non-parametric test. Here we employ
Mann-Whitney U test and compute the effect size.

``` r
wilcox.test(obs ~ group, data=tbl, conf.int=TRUE)
```

    ## 
    ##  Wilcoxon rank sum exact test
    ## 
    ## data:  obs by group
    ## W = 12, p-value = 0.003
    ## alternative hypothesis: true location shift is not equal to 0
    ## 95 percent confidence interval:
    ##  -6.74 -1.26
    ## sample estimates:
    ## difference in location 
    ##                     -4

``` r
rstatix::wilcox_effsize(obs ~ group, data=tbl)
```

    ## # A tibble: 1 × 7
    ##   .y.   group1 group2 effsize    n1    n2 magnitude
    ## * <chr> <chr>  <chr>    <dbl> <int> <int> <ord>    
    ## 1 obs   1      2        0.642    10    10 large

# Paired Test

In previous tests, we always assume I.I.D. In case of paired data, i.e. when an
observation influencing another, we need to take into consideration their
relationship. Unfortunately, Mann-Whitney U could not discern such a relation
between data point, as it only looks at the difference in rank. In paired
Wilcoxon test, we do not assume I.I.D., and procedure-wise it is akin to
one-sample Wilcoxon test. To understand this concept, please kindly recall how
paired T-Test relates to one-sample T-Test. Although paired Wilcoxon test does
not assume the shape of our distribution, a symmetric data is still a plus.

To conduct a paired Wilcoxon test, we first measure the difference between
paired data points by taking their residuals. Similar observations will result
in 0 residual, which we have to omit out of our observation. We assign rank to
the absolute value of computed residual then calculate the statistics as we
previously demonstrated in the one-sample Wilcoxon test.

## Example, please?

In the previous lecture about ANOVA, we are using `ChickWeight` dataset. We
happened to observe a non-normally distributed data for two group of times.
Here, we will further examine both groups to understand the difference.

``` r
# We will use the ChickWeight dataset
str(ChickWeight)
```

    ## Classes 'nfnGroupedData', 'nfGroupedData', 'groupedData' and 'data.frame':   578 obs. of  4 variables:
    ##  $ weight: num  42 51 59 64 76 93 106 125 149 171 ...
    ##  $ Time  : num  0 2 4 6 8 10 12 14 16 18 ...
    ##  $ Chick : Ord.factor w/ 50 levels "18"<"16"<"15"<..: 15 15 15 15 15 15 15 15 15 15 ...
    ##  $ Diet  : Factor w/ 4 levels "1","2","3","4": 1 1 1 1 1 1 1 1 1 1 ...
    ##  - attr(*, "formula")=Class 'formula'  language weight ~ Time | Chick
    ##   .. ..- attr(*, ".Environment")=<environment: R_EmptyEnv> 
    ##  - attr(*, "outer")=Class 'formula'  language ~Diet
    ##   .. ..- attr(*, ".Environment")=<environment: R_EmptyEnv> 
    ##  - attr(*, "labels")=List of 2
    ##   ..$ x: chr "Time"
    ##   ..$ y: chr "Body weight"
    ##  - attr(*, "units")=List of 2
    ##   ..$ x: chr "(days)"
    ##   ..$ y: chr "(gm)"

``` r
# Assess normality
tapply(ChickWeight$weight, ChickWeight$Time, shapiro.test) %>% lapply(broom::tidy) %>%
    lapply(data.frame) %>% {do.call(rbind, .)} %>% knitr::kable(format="simple")
```

|     | statistic | p.value | method                      |
|-----|----------:|--------:|:----------------------------|
| 0   |     0.890 |   0.000 | Shapiro-Wilk normality test |
| 2   |     0.873 |   0.000 | Shapiro-Wilk normality test |
| 4   |     0.973 |   0.315 | Shapiro-Wilk normality test |
| 6   |     0.982 |   0.648 | Shapiro-Wilk normality test |
| 8   |     0.980 |   0.577 | Shapiro-Wilk normality test |
| 10  |     0.981 |   0.616 | Shapiro-Wilk normality test |
| 12  |     0.983 |   0.686 | Shapiro-Wilk normality test |
| 14  |     0.973 |   0.325 | Shapiro-Wilk normality test |
| 16  |     0.986 |   0.830 | Shapiro-Wilk normality test |
| 18  |     0.991 |   0.975 | Shapiro-Wilk normality test |
| 20  |     0.991 |   0.968 | Shapiro-Wilk normality test |
| 21  |     0.986 |   0.869 | Shapiro-Wilk normality test |

After assessing normality, we see that both `\(T=0\)` and `\(T=2\)` not to follow a
normal distribution.

``` r
# Subset the dataset to exclude normally distributed data
tbl <- subset(ChickWeight, subset={ChickWeight$Time %in% c(0, 2)})

# Make Time as a factor
tbl$Time %<>% factor(levels=c(0, 2))
```

Since we are interested to observe differences in non-normal distribution, here
we subset the data to only include observation in group `\(T_0\)` and `\(T_2\)`. Then
we turn our variables into a factor, where we set `\(T_0\)` as our group of
reference.

``` r
# Perform a paired Wilcoxon test
wilcox.test(weight ~ Time, data=tbl, paired=TRUE, conf.int=TRUE)
```

    ## 
    ##  Wilcoxon signed rank test with continuity correction
    ## 
    ## data:  weight by Time
    ## V = 8, p-value = 1e-09
    ## alternative hypothesis: true location shift is not equal to 0
    ## 95 percent confidence interval:
    ##  -9.0 -7.5
    ## sample estimates:
    ## (pseudo)median 
    ##           -8.5

``` r
rstatix::wilcox_effsize(weight ~ Time, data=tbl, paired=TRUE)
```

    ## # A tibble: 1 × 7
    ##   .y.    group1 group2 effsize    n1    n2 magnitude
    ## * <chr>  <chr>  <chr>    <dbl> <int> <int> <ord>    
    ## 1 weight 0      2        0.862    50    50 large

Lastly, we can perform a paired Wilcoxon test and its associated effect size.
