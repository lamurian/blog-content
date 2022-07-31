---
author: lam
title: "Parametric: Mean in Multiple Groups"
weight: 7
description: >

  The limitation  when using T-Test is its inability to directly compare multiple
  group at once. Often times, we are interested to see whether our groups of
  interest present with at least on differing average value. To alleviate this
  issue, we can assign a generalized form of a T-Test. We will do so by analyzing
  between and within group variances. This analysis resulted in the sum of square
  with two degree of freedoms, one coming from the number of groups and another
  from the calculation of withing group variability.

summary: >

  The limitation  when using T-Test is its inability to directly compare multiple
  group at once. Often times, we are interested to see whether our groups of
  interest present with at least on differing average value. To alleviate this
  issue, we can assign a generalized form of a T-Test. We will do so by analyzing
  between and within group variances. This analysis resulted in the sum of square
  with two degree of freedoms, one coming from the number of groups and another
  from the calculation of withing group variability.

date: 2020-11-09
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 07-par-meandiff-2
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/07-par-meandiff-2/slide)

The limitation when using T-Test is its inability to directly compare multiple
group at once. Often times, we are interested to see whether our groups of
interest present with at least on differing average value. To alleviate this
issue, we can assign a generalized form of a T-Test. We will do so by analyzing
between and within group variances. This analysis resulted in the sum of square
with two degree of freedoms, one coming from the number of groups and another
from the calculation of withing group variability.

# One-Way ANOVA

ANOVA, as it stands for the Analysis of Variance, calculates the relative
variability observed within and between the groups. The perk of using ANOVA is
its ability to distinguish mean difference among multiple groups at once. As it
measures the sum of square differences, the statistics result in ANOVA follows
an F-distribution. One-way ANOVA is a specific type which you only assign one
grouping mechanism as your independent variable. As we have previously done,
we will first declare the hypotheses to test on.

-   `\(H_0:\)` The population mean of all groups are all equal
-   `\(H_a:\)` The population mean of all groups are not all equal

Please note that on the following equation the denominator is a residual of our
observation. F-Test is a quotient of between to within differences, both
measured as a mean square, basically a sum of square divided by the degree of
freedom. In our equation, `\(\bar{X}_j\)` is the mean of a group while `\(\bar{X}\)` is
the mean of all sampled groups. The value of `\(\nu = k-1\)` for between comparison
and `\(\nu = N-k\)` for within comparison, where `\(k\)` stands for the number of
groups and n `\(N\)` for the total number of sample.

`$$F = \frac{\displaystyle \sum_{j=1}^{k} n_j (\bar{X}_j - \bar{X})^2 / (k-1)}{\displaystyle \sum_{j=1}^k \sum_{i=1}^N (X_i - \bar{X}_j)^2 / (N-k)}$$`

In performing a one-way ANOVA, we need to ascertain the following assumptions:

-   I.I.D
-   Normally distributed
-   Homogeneity of variance

As we may recall, we can use Shapiro-Wilk’s statistics as a normality test and
Levene’s as homogeneity of variance test.

## F-Distribution

As the T-distribution derives from the normal distribution to estimate the
mean, the F-distribution derives from the `\(\chi^2\)` distribution to define a
ratio of two variances. We may have a vague recollection that the `\(\chi^2\)`
distribution is a specific case of Gamma distribution from the exponential
family. Interestingly, when we have a data `\(X \sim N(0, 1)\)`, then
`\(X^2 \sim \chi^2(1)\)`. Similarly, when both of normality and homogeneity of
variance assumptions fulfilled, the statistics `\(T^2 = F\)`. That is an
interesting relationship you may occur to observe when performing an one-way
ANOVA in two groups and comparing the result to the Student’s T-Test.

`\begin{align} P(X=x) & = \frac{(\frac{r_1}{r_2})^{\frac{r_1}{2}} \Gamma[(r_1 + r_2) / 2] x^{\frac{r_1}{2}-1}}{\Gamma(\frac{r_1}{2}) \Gamma(\frac{r_2}{2}) [1 + (\frac{r_1 \cdot x}{r_2})]^{\frac{(r_1+r_2)}{2}}} \tag{PDF}\\ X & \sim F(r_1, r_2) \tag{notation} \\ F & = \frac{U / r_1}{V / r_2} \end{align}`

Previous equations describe the PDF and notation we can use in defining the
F-distribution, where `\(r_i\)` is the degree of freedom, while `\(U\)` and `\(V\)` are
`\(\chi^2\)` distribution.

## Example, please?

Throughout the lecture, we will use example datasets from `R`. To demonstrate
the one-way ANOVA, we are using a `PlantGrowth` dataset. This dataset simply
describes dried plants weight measured under multiple conditions:

-   `ctrl` for the control (no treatment)
-   `trt1` is the group receiving treatment 1
-   `trt2` is the group receiving treatment 2

We shall first examine our dataset:

``` r
# Data structure
str(PlantGrowth)
```

    ## 'data.frame':    30 obs. of  2 variables:
    ##  $ weight: num  4.17 5.58 5.18 6.11 4.5 4.61 5.17 4.53 5.33 5.14 ...
    ##  $ group : Factor w/ 3 levels "ctrl","trt1",..: 1 1 1 1 1 1 1 1 1 1 ...

``` r
# Descriptive statistics
with(PlantGrowth, tapply(weight, group, summary)) %>% {do.call(rbind, .)}
```

    ##      Min. 1st Qu. Median Mean 3rd Qu. Max.
    ## ctrl 4.17    4.55   5.15 5.03    5.29 6.11
    ## trt1 3.59    4.21   4.55 4.66    4.87 6.03
    ## trt2 4.92    5.27   5.44 5.53    5.73 6.31

Then assessed our assumptions of normality and homogeneity of intergroup
variance:

``` r
with(PlantGrowth, tapply(weight, group, shapiro.test)) %>%
    lapply(broom::tidy) %>% lapply(data.frame) %>% {do.call(rbind, .)}
```

    ##      statistic p.value                      method
    ## ctrl     0.957   0.747 Shapiro-Wilk normality test
    ## trt1     0.930   0.452 Shapiro-Wilk normality test
    ## trt2     0.941   0.564 Shapiro-Wilk normality test

``` r
car::leveneTest(weight ~ group, data=PlantGrowth)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value Pr(>F)
    ## group  2    1.12   0.34
    ##       27

Seeing both p-value from Shapiro-Wilk and Levene’s test being greater than
0.05, we can conclude that our data follow a normal distribution with roughly
equal variances. We may conduct ANOVA by issuing the following command:

``` r
aov.res1 <- aov(weight ~ group, data=PlantGrowth)
anova(aov.res1)
```

    ## Analysis of Variance Table
    ## 
    ## Response: weight
    ##           Df Sum Sq Mean Sq F value Pr(>F)  
    ## group      2   3.77   1.883    4.85  0.016 *
    ## Residuals 27  10.49   0.389                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

We obtain a significant result, brilliant! But we still need to further analyze
its residual by plotting them against the fitted value:

``` r
par(mfrow=c(2, 2)); plot(aov.res1)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/one.way4-1.png" width="100%" />

The residual of our model roughly follows a normal distribution, despite the
presence of outliers in data index number 4, 15 and 17 (see in figure entitled
normal Q-Q). On the left side of our figure, we can observe a homogeneous
residual variances, indicated by the absence of megaphone nor fan effect, i.e.
the residual distributed evenly for all measured groups. Of course we need to
present our data visually as to make it easier for our reader to understand:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.one.way-1.png" width="100%" />

So far, we can conclude that conducting ANOVA requires the following steps to
take:

1.  Normality `\(\to\)` Shapiro-Wilk or Anderson-Darling test
2.  Homogeneity of variance `\(\to\)` Levene’s test
3.  Do modelling and interpretation
4.  Check model goodness of fit

In the next section we will see how to apply such steps to perform two-way and
repeated measure ANOVA.

# Two-Way ANOVA

After demonstrating one-way ANOVA, we are quite satisfied with how it measures
mean differences across multiple groups. However, in some cases, we may need to
consider more than one grouping mechanism. This way, we can also control for
interaction happening among groups. Two-way ANOVA provides a way to perform
such a tedious task. As a side note, we can refer a two-way ANOVA as a
factorial ANOVA. Since it is an extension to one-way ANOVA, it has similar
assumptions of:

-   I.I.D
-   Normality
-   Homogeneity of variance

## Example, please?

To establish an understanding on conducting a factorial ANOVA, we will use
`ToothGrowth` dataset as it is readily available in `R`. This dataset also
presents in JASP if you prefer to use them for a convenient purpose.
The `ToothGrwoth` dataset has three variables of

-   `len`: Tooth length
-   `supp`: Supplement given
-   `dose`: Supplement dose

As we previously did in one-way ANOVA, we shall start our inference by
inspecting the data:

``` r
# Data structure
str(ToothGrowth)
```

    ## 'data.frame':    60 obs. of  3 variables:
    ##  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
    ##  $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...

``` r
# Set dose as a factor
ToothGrowth$dose %<>% factor(levels=c(0.5, 1.0, 2.0))
```

Then we may want to measure the descriptive statistics:

``` r
# Grouped by supplement type
with(ToothGrowth, tapply(len, supp, summary)) %>% {do.call(rbind, .)}
```

    ##    Min. 1st Qu. Median Mean 3rd Qu. Max.
    ## OJ  8.2    15.5   22.7 20.7    25.7 30.9
    ## VC  4.2    11.2   16.5 17.0    23.1 33.9

``` r
# Grouped by prescribed dose
with(ToothGrowth, tapply(len, dose, summary)) %>% {do.call(rbind, .)}
```

    ##     Min. 1st Qu. Median Mean 3rd Qu. Max.
    ## 0.5  4.2    7.22   9.85 10.6    12.2 21.5
    ## 1   13.6   16.25  19.25 19.7    23.4 27.3
    ## 2   18.5   23.53  25.95 26.1    27.8 33.9

And of course, a normality test:

``` r
# Grouped by supplement type
with(ToothGrowth, tapply(len, supp, shapiro.test)) %>%
    lapply(broom::tidy) %>% lapply(data.frame) %>% {do.call(rbind, .)}
```

    ##    statistic p.value                      method
    ## OJ     0.918  0.0236 Shapiro-Wilk normality test
    ## VC     0.966  0.4284 Shapiro-Wilk normality test

``` r
# Grouped by prescribed dose
with(ToothGrowth, tapply(len, dose, shapiro.test)) %>%
    lapply(broom::tidy) %>% lapply(data.frame) %>% {do.call(rbind, .)}
```

    ##     statistic p.value                      method
    ## 0.5     0.941   0.247 Shapiro-Wilk normality test
    ## 1       0.931   0.164 Shapiro-Wilk normality test
    ## 2       0.978   0.902 Shapiro-Wilk normality test

Oh no! The data in `OJ` group does not follow a normal distribution, what
should we do? Fret not ;) With an increasing number of sample, we often see
that our data will not follow a normal distribution. In such a case, it is
always good to perform a visual examination to determine whether our data
contains outliers. In case of extreme outliers, we may opt to remove such an
entry so that we can safely conduct an ANOVA.

<img src="{{< blogdown/postref >}}index_files/figure-html/two.way4-1.png" width="100%" />

We have outliers in data index 7 and 8, but it is still within a safe boundary
of normal distribution theoretical quantiles. So it is alright to continue our
inference and fulfilling the homogeneity of variance assumption.

``` r
# Grouped by supplement type
car::leveneTest(len ~ supp, data=ToothGrowth)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value Pr(>F)
    ## group  1    1.21   0.28
    ##       58

``` r
# Grouped by prescribed dose
car::leveneTest(len ~ dose, data=ToothGrowth)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value Pr(>F)
    ## group  2    0.65   0.53
    ##       57

The data presents with a homogeneous intergroup variance, so we can fit an
ANOVA model to understand the mean difference.

``` r
aov.res2 <- aov(len ~ supp + dose, data=ToothGrowth)
anova(aov.res2)
```

    ## Analysis of Variance Table
    ## 
    ## Response: len
    ##           Df Sum Sq Mean Sq F value  Pr(>F)    
    ## supp       1    205     205    14.0 0.00043 ***
    ## dose       2   2426    1213    82.8 < 2e-16 ***
    ## Residuals 56    820      15                    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Next, we will do a goodness of fit evaluation by analyzing the residual.

``` r
par(mfrow=c(2,2)); plot(aov.res2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/two.way7-1.png" width="100%" />

Then we can plot our statistically-proven mean difference using a nice figure.

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.two.way-1.png" width="100%" />

# Repeated Measure ANOVA

When the independence assumption violated, we need to perform a paired instead
of unpaired test. It is the same thing in ANOVA, where we can apply both of
one-way and factorial repeated measure ANOVA. However, please bear in mind that
we still assume normality without the presence of an extreme outlier. Instead
of homogeneity of variance, we need to prove a sphericity assumption, a
homogeneity of between group variances.

## Example, please?

Up to this point, we are pretty much accustomed to measuring mean differences
and have a practical knowledge on how ANOVA works. For this demonstration, we
will use `ChickWeight` dataset to conduct a repeated measure ANOVA. In this
dataset, we have an observation of chicken weight in grams measured overtime.
As we will se on the initial inspection, the dataset contains following
variables:

-   `weight`: chicken weight in grams
-   `Time`: number of days since birth
-   `Chick`: unique identifier on the chicken
-   `Diet`: type of diet given

``` r
# Data structure
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

As previously conducted, we shall test for data normality.

``` r
with(ChickWeight, tapply(weight, Time, shapiro.test)) %>%
    lapply(broom::tidy) %>% lapply(data.frame) %>% {do.call(rbind, .)}
```

    ##    statistic  p.value                      method
    ## 0      0.890 2.22e-04 Shapiro-Wilk normality test
    ## 2      0.873 6.86e-05 Shapiro-Wilk normality test
    ## 4      0.973 3.15e-01 Shapiro-Wilk normality test
    ## 6      0.982 6.48e-01 Shapiro-Wilk normality test
    ## 8      0.980 5.77e-01 Shapiro-Wilk normality test
    ## 10     0.981 6.16e-01 Shapiro-Wilk normality test
    ## 12     0.983 6.86e-01 Shapiro-Wilk normality test
    ## 14     0.973 3.25e-01 Shapiro-Wilk normality test
    ## 16     0.986 8.30e-01 Shapiro-Wilk normality test
    ## 18     0.991 9.75e-01 Shapiro-Wilk normality test
    ## 20     0.991 9.68e-01 Shapiro-Wilk normality test
    ## 21     0.986 8.69e-01 Shapiro-Wilk normality test

The data in `\(t_0\)` and `\(t_2\)` does not seem right. We have a few options to
select on:

-   Transform the data, at the cost of losing information on measured units
-   Filter and delete row entry with existing outliers contributing to skewness
-   Drop out observed variables (columns) not fulfilling the normality assumption

Since we have so many observation of interest, it will not be a problem to drop
two columns out of 12 observation periods. Of course, you may want to suit your
choice with how you would like to interpret your data. But here we shall
demonstrate a convenient method of handling non-normal variables.

``` r
tbl <- subset(ChickWeight, subset=!{ChickWeight$Time==0 | ChickWeight$Time==2})
with(tbl, tapply(weight, Time, shapiro.test)) %>%
    lapply(broom::tidy) %>% lapply(data.frame) %>% {do.call(rbind, .)}
```

    ##    statistic p.value                      method
    ## 4      0.973   0.315 Shapiro-Wilk normality test
    ## 6      0.982   0.648 Shapiro-Wilk normality test
    ## 8      0.980   0.577 Shapiro-Wilk normality test
    ## 10     0.981   0.616 Shapiro-Wilk normality test
    ## 12     0.983   0.686 Shapiro-Wilk normality test
    ## 14     0.973   0.325 Shapiro-Wilk normality test
    ## 16     0.986   0.830 Shapiro-Wilk normality test
    ## 18     0.991   0.975 Shapiro-Wilk normality test
    ## 20     0.991   0.968 Shapiro-Wilk normality test
    ## 21     0.986   0.869 Shapiro-Wilk normality test

Now they look better :) Next, we will check for any extreme outlier.

``` r
# Use `weight` variable as a reference to identify outliers
rstatix::identify_outliers(tbl, variable="weight")
```

    ##   weight Time Chick Diet is.outlier is.extreme
    ## 1    331   21    21    2       TRUE      FALSE
    ## 2    327   20    34    3       TRUE      FALSE
    ## 3    341   21    34    3       TRUE      FALSE
    ## 4    332   18    35    3       TRUE      FALSE
    ## 5    361   20    35    3       TRUE      FALSE
    ## 6    373   21    35    3       TRUE      FALSE
    ## 7    321   21    40    3       TRUE      FALSE
    ## 8    322   21    48    4       TRUE      FALSE

Sure there are outliers, but none of them are extreme. With a large number of
observation, ANOVA is quite robust against data not following a normal
distribution. However, the presence of extreme outliers or severely-skewed data
compromise this robustness. We have fulfilled two of the three assumptions, now
we need to fit in our ANOVA model and assess for its sphericity assumption
using Mauchly’s test.

``` r
rstatix::anova_test(data=tbl, dv=weight, wid=Chick, within=Time)
```

    ## ANOVA Table (type III tests)
    ## 
    ## $ANOVA
    ##   Effect DFn DFd   F         p p<.05   ges
    ## 1   Time   9 396 196 8.03e-140     * 0.617
    ## 
    ## $`Mauchly's Test for Sphericity`
    ##   Effect        W         p p<.05
    ## 1   Time 6.67e-13 1.48e-209     *
    ## 
    ## $`Sphericity Corrections`
    ##   Effect   GGe     DF[GG]    p[GG] p[GG]<.05   HFe      DF[HF]    p[HF]
    ## 1   Time 0.136 1.22, 53.7 3.46e-21         * 0.137 1.24, 54.43 1.92e-21
    ##   p[HF]<.05
    ## 1         *

Interpreting the Mauchly’s test, we may conclude that our data violated the
sphericity assumption. As such, we will use the corrected p-value as presented
at the bottom of our results.

``` r
rstatix::anova_test(data=tbl, dv=weight, wid=Chick, within=Time, between=Diet)
```

    ## ANOVA Table (type III tests)
    ## 
    ## $ANOVA
    ##      Effect DFn DFd      F         p p<.05   ges
    ## 1      Diet   3  41   5.01  5.00e-03     * 0.185
    ## 2      Time   9 369 234.20 1.21e-146     * 0.685
    ## 3 Diet:Time  27 369   3.52  2.77e-08     * 0.089
    ## 
    ## $`Mauchly's Test for Sphericity`
    ##      Effect        W         p p<.05
    ## 1      Time 8.13e-13 1.97e-190     *
    ## 2 Diet:Time 8.13e-13 1.97e-190     *
    ## 
    ## $`Sphericity Corrections`
    ##      Effect  GGe      DF[GG]    p[GG] p[GG]<.05   HFe      DF[HF]    p[HF]
    ## 1      Time 0.14 1.26, 51.64 9.97e-23         * 0.142 1.28, 52.51 4.53e-23
    ## 2 Diet:Time 0.14 3.78, 51.64 1.40e-02         * 0.142 3.84, 52.51 1.40e-02
    ##   p[HF]<.05
    ## 1         *
    ## 2         *

What have we learnt so far? We know that to conduct a parametric hypothesis
test we need to fulfill some stringent assumptions. Our demonstration so far
has delineated the difference between a one-way and factorial ANOVA. In case of
independence assumption violation, we need to employ a repeated measure ANOVA.

# Post-Hoc Analysis

When assessing the mean difference across multiple groups, it might be
intriguing to further infer which pairwise comparison actually present with a
statistically significant mean difference. After conducting an ANOVA, it is
always a good practice to apply a post-hoc analysis using Tukey’s range test,
or also called a Tukey’s Honest Significant Difference (HSD). After
understanding the role of T-test and ANOVA, it is reasonably tempting to use
T-test right after ANOVA. However, it turned out we have a higher probability
of getting a type-I statistical error when applying multiple T-Test as a
post-hoc analysis.

Tukey’s HSD alleviates such a problem by calculating the actual mean difference
between two groups and dividing the residual by a square root of a quotient
between the mean square within and the number of samples in one of observed
group. Please note that Tukey’s HSD is best applied for cases where numbers of
observation for each group are equal. In case you have differing number of
sample size, we need to use a Tukey-Kramer method. You may also want to
consider using the Scheffe test in the case of unequal sample size. However, to
keep this post brief, we will stick with Tukey’s HSD and Tukey-Kramer.

## Example, please?

We will have the previous result of one-way ANOVA as our example. By imputing
the model into `TukeyHSD` function in `R`, we will have the following output.

``` r
aov.res1 %>% anova() %>% broom::tidy()
```

    ## # A tibble: 2 × 6
    ##   term         df sumsq meansq statistic p.value
    ##   <chr>     <int> <dbl>  <dbl>     <dbl>   <dbl>
    ## 1 group         2  3.77  1.88       4.85  0.0159
    ## 2 Residuals    27 10.5   0.389     NA    NA

``` r
TukeyHSD(aov.res1) %>% broom::tidy()
```

    ## # A tibble: 3 × 7
    ##   term  contrast  null.value estimate conf.low conf.high adj.p.value
    ##   <chr> <chr>          <dbl>    <dbl>    <dbl>     <dbl>       <dbl>
    ## 1 group trt1-ctrl          0   -0.371   -1.06      0.320      0.391 
    ## 2 group trt2-ctrl          0    0.494   -0.197     1.19       0.198 
    ## 3 group trt2-trt1          0    0.865    0.174     1.56       0.0120

As another demonstration, we can also use the our second model using factorial
ANOVA.

``` r
aov.res2 %>% anova() %>% broom::tidy()
```

    ## # A tibble: 3 × 6
    ##   term         df sumsq meansq statistic   p.value
    ##   <chr>     <int> <dbl>  <dbl>     <dbl>     <dbl>
    ## 1 supp          1  205.  205.       14.0  4.29e- 4
    ## 2 dose          2 2426. 1213.       82.8  1.87e-17
    ## 3 Residuals    56  820.   14.7      NA   NA

``` r
TukeyHSD(aov.res2) %>% broom::tidy()
```

    ## # A tibble: 4 × 7
    ##   term  contrast null.value estimate conf.low conf.high adj.p.value
    ##   <chr> <chr>         <dbl>    <dbl>    <dbl>     <dbl>       <dbl>
    ## 1 supp  VC-OJ             0    -3.70    -5.68     -1.72    4.29e- 4
    ## 2 dose  1-0.5             0     9.13     6.22     12.0     1.32e- 9
    ## 3 dose  2-0.5             0    15.5     12.6      18.4     7.31e-12
    ## 4 dose  2-1               0     6.37     3.45      9.28    6.98e- 6

# Effect Size

When conducting a mean difference using T-test methods (note the plural), we
also computed the effect size using Cohen’s `\(d\)`. However, `\(d\)` is limited to
measure distance in a two-sample mean difference. As ANOVA is a generalized
form of a T-test, measuring its effect size also requires a different approach.
Here we shall discuss `\(\eta^2\)` and partial `\(\eta^2\)` to measure the effect size
on ANOVA and repeated ANOVA, respectively.

`\begin{align} \eta^2 &= \frac{SS_{effect}}{SS_{total}} \\ Partial\ \eta^2 &= \frac{SS_{effect}}{SS_{effect} + SS_{error}} \end{align}`

## Example, please?

``` r
heplots::etasq(aov.res1, partial=FALSE)
```

    ##           eta^2
    ## group     0.264
    ## Residuals    NA

``` r
heplots::etasq(aov.res2, partial=FALSE)
```

    ##            eta^2
    ## supp      0.0595
    ## dose      0.7029
    ## Residuals     NA

After acquiring the effect size, we can use it to calculate the power of our
statistical inference.

## Power analysis

``` r
pwr::pwr.anova.test(k=3, n=10, f=0.264)
```

    ## 
    ##      Balanced one-way analysis of variance power calculation 
    ## 
    ##               k = 3
    ##               n = 10
    ##               f = 0.264
    ##       sig.level = 0.05
    ##           power = 0.213
    ## 
    ## NOTE: n is number in each group

``` r
pwr::pwr.anova.test(k=2, n=30, f=0.0595)
```

    ## 
    ##      Balanced one-way analysis of variance power calculation 
    ## 
    ##               k = 2
    ##               n = 30
    ##               f = 0.0595
    ##       sig.level = 0.05
    ##           power = 0.0739
    ## 
    ## NOTE: n is number in each group

``` r
pwr::pwr.anova.test(k=3, n=20, f=0.7029)
```

    ## 
    ##      Balanced one-way analysis of variance power calculation 
    ## 
    ##               k = 3
    ##               n = 20
    ##               f = 0.703
    ##       sig.level = 0.05
    ##           power = 0.999
    ## 
    ## NOTE: n is number in each group
