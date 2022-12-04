---
author: lam
title: "Non-parametric: Differences in Multiple Groups"
weight: 9
description: >

  Even though parametric tests on multiple groups have a favourable statistical
  power in non-normal data, it requires a large enough sample to correctly reject
  the `\(H_0\)`. Moreover, parametric tests only applies on numeric data, as it
  compares the mean between assigned groups. In cases of having an ordinal data
  or data with a low number of sample, non-parametric tests may provide a better
  inference.

summary: >

  Even though parametric tests on multiple groups have a favourable statistical
  power in non-normal data, it requires a large enough sample to correctly reject
  the `\(H_0\)`. Moreover, parametric tests only applies on numeric data, as it
  compares the mean between assigned groups. In cases of having an ordinal data
  or data with a low number of sample, non-parametric tests may provide a better
  inference.

date: 2020-11-24
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 09-nonpar-meandiff-2
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/09-nonpar-meandiff-2/slide)

Even though parametric tests on multiple groups have a favourable statistical
power in non-normal data, it requires a large enough sample to correctly reject
the `\(H_0\)`. Moreover, parametric tests only applies on numeric data, as it
compares the mean between assigned groups. In cases of having an ordinal data
or data with a low number of sample, non-parametric tests may provide a better
inference.

# Unpaired Test

The unpaired non-parametric test is Kruskal-Wallis H, analogous to one-way
ANOVA. It assesses distribution differences among multiple groups. Albeit its
similarity of purpose, Kruskal-Wallis H is fundamentally different from ANOVA.
Kruskal-Wallis is somewhat limited as we cannot assign multiple independent
variables nor adjusting for covariates. Kruskal-Wallis is more of an extension
to unpaired Wilcoxon (Mann-Whitney U test), where it compares the differences
of sum ranked data. Although not assuming normality, Kruskal-Wallis does not
perform better than ANOVA when we have a highly-skewed data. As with other
unpaired tests, Kruskal-Wallis still assume I.I.D dan requires homogeneous
intergroup variances. In cases of having a large-sample data with heterogeneous
intergroup variance, regardless of its normality, please resort to inferring
with Welch’s ANOVA (`oneway.test` in `R`). Though, this method is not available
for factorial ANOVA.

To perform a Kruskal-Wallis test, first we pool and sort all data element. We
shall assign rank to the sorted data, where the smaller value will rank lower.
As in other sum-rank tests, we need and adjustment to tied values, where we take the
mean their sum of rank. Finally, we can calculate the H statistics using the
following equation:

`\begin{align} H = \left[ \frac{12}{{n}({n}+1)} \displaystyle \sum_{i=1}^{k} \frac{{R}_i^2}{{n}_i} \right] - 3 ({n}+1) \end{align}`

`$$H \sim \chi^2(k-1)$$`

-   `\(n\)`: Total observed value
-   `\(k\)`: Total number of groups
-   `\(R\)`: Rank from pooled data

As `\(H\)` follows a `\(\chi^2\)` distribution, we can expect only a one-tailed test in
Kruskal-Wallis. Upon obtaining a significant differences using Kruskal-Wallis,
we need to conduct a pairwise differences post-hoc analysis using the Dunn’s
test.

## Example, please?

In this example, we will use the `DNase` dataset. This is a result obtained
from an ELISA assay, where it has three variables of interest:

-   `Run`: The assay run
-   `conc`: Protein concentration
-   `density`: Optical density in the assay

``` r
str(DNase)
```

    | Classes 'nfnGroupedData', 'nfGroupedData', 'groupedData' and 'data.frame':    176 obs. of  3 variables:
    |  $ Run    : Ord.factor w/ 11 levels "10"<"11"<"9"<..: 4 4 4 4 4 4 4 4 4 4 ...
    |  $ conc   : num  0.0488 0.0488 0.1953 0.1953 0.3906 ...
    |  $ density: num  0.017 0.018 0.121 0.124 0.206 0.215 0.377 0.374 0.614 0.609 ...
    |  - attr(*, "formula")=Class 'formula'  language density ~ conc | Run
    |   .. ..- attr(*, ".Environment")=<environment: R_EmptyEnv> 
    |  - attr(*, "labels")=List of 2
    |   ..$ x: chr "DNase concentration"
    |   ..$ y: chr "Optical density"
    |  - attr(*, "units")=List of 1
    |   ..$ x: chr "(ng/ml)"

Here we have both `conc` and `density` as a numeric variable, while `Run` as a
ordinal variable with 11 levels. `R` use the word `level` to describe the
presence of group in a categorical data. The terminology `level` does not
explicitly imply an order or natural stratification. Instead, it expresses
numerical representation on each category, which of course we can manually set.
In assigning ordinality, we can use `ordered` function in `R`, while the
`factor` function will result in a nominal variable.

``` r
with(DNase, tapply(density, Run, shapiro.test)) %>%
    lapply(broom::tidy) %>% lapply(data.frame) %>% {do.call(rbind, .)} %>%
    knitr::kable(format="simple") %>% kable_minimal()
```

|     | statistic | p.value | method                      |
|-----|----------:|--------:|:----------------------------|
| 10  |     0.891 |   0.059 | Shapiro-Wilk normality test |
| 11  |     0.888 |   0.051 | Shapiro-Wilk normality test |
| 9   |     0.889 |   0.053 | Shapiro-Wilk normality test |
| 1   |     0.883 |   0.044 | Shapiro-Wilk normality test |
| 4   |     0.877 |   0.035 | Shapiro-Wilk normality test |
| 8   |     0.876 |   0.033 | Shapiro-Wilk normality test |
| 5   |     0.879 |   0.037 | Shapiro-Wilk normality test |
| 7   |     0.883 |   0.043 | Shapiro-Wilk normality test |
| 6   |     0.880 |   0.039 | Shapiro-Wilk normality test |
| 2   |     0.869 |   0.027 | Shapiro-Wilk normality test |
| 3   |     0.880 |   0.039 | Shapiro-Wilk normality test |

By employing SW as our normality test of choice, we can see that our data does
not follow the normal distribution.

``` r
with(DNase, car::leveneTest(conc, Run))
```

    | Levene's Test for Homogeneity of Variance (center = median)
    |        Df F value Pr(>F)
    | group  10       0      1
    |       165

From the Levene’s test, we found out that our data has a roughly equal
intergroup variances. So we have fulfilled two important assumptions in
conducting Kruskal-Wallis test, i.e. I.I.D and homogeneity of intergroup
variance.

``` r
kruskal.test(conc ~ Run, data=DNase)
```

    | 
    |   Kruskal-Wallis rank sum test
    | 
    | data:  conc by Run
    | Kruskal-Wallis chi-squared = 0, df = 10, p-value = 1

``` r
rstatix::kruskal_effsize(conc ~ Run, data=DNase)
```

    | # A tibble: 1 × 5
    |   .y.       n effsize method  magnitude
    | * <chr> <int>   <dbl> <chr>   <ord>    
    | 1 conc    176 -0.0606 eta2[H] moderate

Then we can proceed by calculating the H statistics, its p-value and the effect
size.

``` r
dunn.test::dunn.test(DNase$conc, DNase$Run)
```

    |   Kruskal-Wallis rank sum test
    | 
    | data: x and group
    | Kruskal-Wallis chi-squared = 0, df = 10, p-value = 1
    | 
    | 
    |                            Comparison of x by group                            
    |                                 (No adjustment)                                
    | Col Mean-|
    | Row Mean |          1         10         11          2          3          4
    | ---------+------------------------------------------------------------------
    |       10 |   0.000000
    |          |     0.5000
    |          |
    |       11 |   0.000000   0.000000
    |          |     0.5000     0.5000
    |          |
    |        2 |   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000
    |          |
    |        3 |   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000
    |          |
    |        4 |   0.000000   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000     0.5000
    |          |
    |        5 |   0.000000   0.000000   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000     0.5000     0.5000
    |          |
    |        6 |   0.000000   0.000000   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000     0.5000     0.5000
    |          |
    |        7 |   0.000000   0.000000   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000     0.5000     0.5000
    |          |
    |        8 |   0.000000   0.000000   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000     0.5000     0.5000
    |          |
    |        9 |   0.000000   0.000000   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000     0.5000     0.5000
    | Col Mean-|
    | Row Mean |          5          6          7          8
    | ---------+--------------------------------------------
    |        6 |   0.000000
    |          |     0.5000
    |          |
    |        7 |   0.000000   0.000000
    |          |     0.5000     0.5000
    |          |
    |        8 |   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000
    |          |
    |        9 |   0.000000   0.000000   0.000000   0.000000
    |          |     0.5000     0.5000     0.5000     0.5000
    | 
    | alpha = 0.05
    | Reject Ho if p <= alpha/2

Lastly, we conducted a Dunn’s test to ascertain pairwise differences in our
data.

# Paired Test

The paired non-parametric test is useful to analyze ordinal data which assume
non-independence, as analogous to repeated measure ANOVA. This statistical test
aims to calculate the within-subject and between-group differences. The
statistical test we will use is Friedman test, where we rank observation
*only* in the same subjects. Afterward, we sum all ranks within the same group,
followed by calculating the statistics.

$$ Q = \left[ \frac{12 {N}}{{N} {k} ( {k}+1)} \displaystyle \sum_{i=1}^{{k}} {R}_i^2 \right] - 3 {N}( {k}+1) $$
`$$Q \sim \chi^2(k-1)$$`

Where:
- `\(N\)`: Number of rows (block)
- `\(k\)`: Number of columns (treatment / repetition)
- `\(R\)`: Ranked values

## Example, please?

For demonstration purposes, we shall use the `warpbreaks` dataset as it is
readily available in `R`.

``` r
str(warpbreaks)
```

    | 'data.frame': 54 obs. of  3 variables:
    |  $ breaks : num  26 30 54 25 70 52 51 26 67 18 ...
    |  $ wool   : Factor w/ 2 levels "A","B": 1 1 1 1 1 1 1 1 1 1 ...
    |  $ tension: Factor w/ 3 levels "L","M","H": 1 1 1 1 1 1 1 1 1 2 ...

This dataset consists of three variables, namely `breaks`, `wool` and
`tension`. The `warpbreak` dataset describes how a different type of wool
presents with varying breaking points given a particular pulling tension.
Judging from its structure, the variable `breaks` is numeric, while `wool` and
`tension` are nominal.

``` r
wp <- aggregate(warpbreaks$breaks,
    by = list(
        w = warpbreaks$wool,
        t = warpbreaks$tension
    ), FUN = mean
) %T>% print()
```

    |   w t    x
    | 1 A L 44.6
    | 2 B L 28.2
    | 3 A M 24.0
    | 4 B M 28.8
    | 5 A H 24.6
    | 6 B H 18.8

``` r
friedman.test(x ~ w | t, data=wp)
```

    | 
    |   Friedman rank sum test
    | 
    | data:  x and w and t
    | Friedman chi-squared = 0.3, df = 1, p-value = 0.6

``` r
rstatix::friedman_effsize(x ~ w | t, data=wp)
```

    | # A tibble: 1 × 5
    |   .y.       n effsize method    magnitude
    | * <chr> <int>   <dbl> <chr>     <ord>    
    | 1 x         3   0.111 Kendall W small

Friedman’s test determines the difference among various blocks. Essentially, a
block is a description of two interacting variables, which in this case are
wool and tension. It sounds confusing and not too straightforward, but please
imagine it this way:

-   Each type of wool has different breaking point
-   Each of tractional tension will affect when the wool breaks
-   In `warpbreak` dataset, the `wool` variable has two levels of `A` and `B`
-   While the `tension` variable has three levels of `L`, `M` and `H`
-   So we will have six blocks of observation: wool A + tension L, wool A +
    tension M, and so on

So we could not directly use the function `friedman.test` in `R`, as we need to
change our dataset to summarize the block-like design. We use the `aggregate`
function solely for that purpose, where we group all observation based on its
wool type and tractional tension, then we calculate the mean of breaks
happening in such a block.

# Final Excerpts

As we have discussed in the last two lectures, non-parametric tests have a
simpler approach and more lenient assumptions. However, neglecting some
assumptions impede the test from acquiring an optimal statistical power. In
case of conducting a test on multiple groups, the non-parametric variants also
have several limitations compared to the parametric test. Whenever possible, it
is a good practice to use a parametric test (as appropriate). However, the
non-parametric test is useful in an ordinal data, where using parametric tests
would not make much sense.

When we have our data not following a normal distribution, we can still
consider employing a parametric test with careful consideration. In the
previous lecture, we already discussed how test of normality may results in a
`\(H_0\)` rejection when we have a large enough sample. Contrarily, according to
the CLT, when we have `\(n \to \infty\)`, our sampled mean will undergo a
convergence of random variables into a normal distribution. In other words,
when we have a large sample size, we can compare the mean between observed
groups using a parametric test so long that all groups have a roughly equal
intergroup variance.

As we have briefly discussed both parametric and non-parametric test, and we
also have mentioned how parametric test is applicable for non-normal data, it
is of essence to discuss further analysis we can do after ANOVA. Post-hoc
analysis is of course a must, but we need to get more accustomed to the
residual analysis to measure how well our model fitted the dataset. In short,
with residual analysis we will satisfy two assumptions in ANOVA:

-   Normality of the residual
-   Homogeneity of residual variance

Hold up, why does it sound so familiar? Well, because it is the *actual*
assumptions in ANOVA. In the previous lecture, we learnt that we require our
data to follow a normal distribution and has a homogeneous intergroup variance.
The normality assumption exist only to simplify the actual assumption of the
model residuals! We will delve further in satisfying residual assumptions when
we get into lectures on (generalized) linear model :)
