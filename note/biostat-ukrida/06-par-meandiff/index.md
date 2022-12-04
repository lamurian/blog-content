---
author: lam
title: "Parametric: Mean in Two Groups"
weight: 6
description: >

  When we have a normally-distributed data, parameters `\(\mu\)` and `\(\sigma\)` from
  our PDF can completely explain the behaviour seen in our sample. With `\(\mu\)`
  represents the central tendency and `\(\sigma\)` the spread, we can directly
  compare similarly distributed samples. Often, we need to confirm how much our
  average value differs from other observations. In doing so, we are facing a
  mean difference problem in our venture of statistics. This lecture will help
  us proving mean differences in one-sample and two-sample problems.

summary: >

  When we have a normally-distributed data, parameters `\(\mu\)` and `\(\sigma\)` from
  our PDF can completely explain the behaviour seen in our sample. With `\(\mu\)`
  represents the central tendency and `\(\sigma\)` the spread, we can directly
  compare similarly distributed samples. Often, we need to confirm how much our
  average value differs from other observations. In doing so, we are facing a
  mean difference problem in our venture of statistics. This lecture will help
  us proving mean differences in one-sample and two-sample problems.

date: 2020-10-15
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 06-par-meandiff
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/06-par-meandiff/slide)

When we obtain a normally-distributed data, parameters `\(\mu\)` and `\(\sigma\)` from
our PDF can completely explain the behaviour seen in our sample. With `\(\mu\)`
represents the central tendency and `\(\sigma\)` the spread, we can directly
compare similarly distributed samples. Often, we need to confirm how much our
average value differs from other observations. In doing so, we are facing a
mean difference problem in our venture of statistics. This lecture will help us
proving mean differences in one-sample and two-sample problems.

# Mean Difference

We may have a vivid recollection on previous lectures of data distribution and
the Central Limit Theorem (CLT). For any data following a normal distribution,
centering and scaling according to its `\(\mu\)` and `\(\sigma\)` results in a
standardized normal distribution, i.e. a Z-distribution.

`$$\frac{x - \bar{x}}{s} \sim N(0, 1) \tag{1}$$`

For any given distribution, the mean of such a sample will undergo a
convergence of random variable into a normal distribution with parameters of
`\(\mu\)` and `\(\frac{\sigma}{\sqrt{n}}\)`.

`$$\bar{X} \xrightarrow{d} N(\mu, \frac{\sigma}{\sqrt{n}}) \tag{2}$$`

With known `\(\mu\)` and `\(\sigma\)`, we can make a direct comparison between our data
and the population. However, how if we do not know the parameter `\(\mu\)`? We can
make a close estimate using its statistics, `\(\bar{x}\)`. A mean difference is
simply a result of subtracting sampled mean from the hypothesized parameter,
which corresponds to `\(\bar{x} - \mu_0\)`. However, our sample bounds to have
error, either a systematic or unsystematic (random) ones. An adjustment to such
problems requires us to divide our measures into the standard error. Obtained
quotient is our statistics of interest, which will follow a Z-distribution.

`\begin{align} SE &= \frac{\sigma}{\sqrt{n}} \tag{Standard Error} \\ \\ z  &= \frac{\bar{x} - \mu_0}{SE} \\    &= \frac{\bar{x} - \mu_0}{^{\sigma}/\tiny{\sqrt{n}}} \tag{One-sample Test} \end{align}`

Having our statistics as an element of Z-distribution, we can compute the
p-value by looking at the probability of our statistical value. As always, we
first need to initiate the **significant level** `\(\alpha\)` so we can measure where
our statistics is located in relation to the **significant value**
`\(x: P(X \leqslant x\ |\ 0, 1)=\alpha\)`. We regard this approach as a Z-Test, where the
p-value represents probabilities of `\(P(Z = z\ |\ 0, 1)\)`.

We shall consider the following scenario as an example:

> In a population of third-year electrical engineering students, we know the
> **average final score** of a particular course is **70**. In measuring
> students’ comprehension, UKRIDA has established a standardized examination with
> a **standard deviation** of **10**. We are interested to see whether
> students registered to this year course have different average, where 18
> students averagely scored 75 on the final exam.

Then, we need to formulate our hypotheses:

`\begin{align} H_0 &: \bar{x} = \mu_0 \\ H_a &: \bar{x} \neq \mu_0 \end{align}`

Followed by computing the `\(z\)` statistics:

`\begin{align} SE &= \frac{10}{\sqrt{18}} &= 2.36 \\ z  &= \frac{75 - 70}{2.36} &= 2.12 \end{align}`

Where does `\(z\)` located in Z-distribution?

<img src="{{< blogdown/postref >}}index_files/figure-html/z.dist1-1.png" width="100%" />

Assigning our significance value on both tails results in:

<img src="{{< blogdown/postref >}}index_files/figure-html/z.dist2-1.png" width="100%" />

Since our `\(H_a\)` assumes non-equality, we can compute the p-value according to
two-tailed test procedures. First we need to find the cumulative probability of
`\(z\)` which satisfies:

`$$P(Z \leqslant 2.12\ |\ \mu,\sigma): Z \sim N(0, 1)$$`

Then we have to subtract `\(P(Z=z)\)` from 1 and multiply the difference by 2 to
obtain the two-tailed p-value.

``` r
2 * {1 - pnorm(2.12, 0, 1)}
```

    ## [1] 0.034

So far, we understand that Z-test requires the sample to follow a normal
distribution. Before conducting any formal test, it is imperative to ascertain
the sampled distribution, i.e. using a goodness of fit or normality test. We do
not need to know the parameter `\(\mu\)` because we can hypothesize the value.
However, we *need* the parameter `\(\sigma\)` to correctly compute `\(z\)`. It is
becoming quite problematic when we do not know the value of `\(\sigma\)`, of which
we often don’t! In such a case, we need to consider using a T-distribution
instead.

# Student’s T-Distribution

Student’s T-distribution only depends on 1 parameter, degree of freedom `\(\nu\)`.
Mathematically, T-distribution has the following notation: `\(X \sim t_{\nu}\)`.
The T-distribution is pivotal to compute statistics in mean difference of a
normally-distributed data. Degree of freedom `\(\nu\)` in T-distribution is simply
`\(n - 1\)`, where `\(n\)` represents the total number of sample.

`\begin{align} Let\ & X \sim t_\nu \tag{Notation} \\ P(X=x) &= \frac{\Gamma \big( \frac{\nu+1}{2} \big)}{\sqrt{\nu \pi}\ \Gamma \big( \frac{\nu}{2} \big)} \bigg( 1 + \frac{x^2}{\nu} \bigg) \tag{PDF} \\ \nu &= n - 1 \\ \\ Let\ & T \sim t_\nu \\ T &= Z \sqrt{\frac{\nu}{V}} \tag{Relationship} \end{align}`

Aside from its relationship with Z-distribution, T-distribution also
independently relates to the `\(chi^2\)` distribution, where they share the same
`\(\nu\)` degree of freedom.

<img src="{{< blogdown/postref >}}index_files/figure-html/t.dist-1.png" width="100%" />

# One Sample T-Test

One sample T-test is analogous to the Z-test, where we use it when we could not
ascertain the `\(\sigma\)`. In place of `\(\sigma\)`, T-test use `\(s\)` as an estimate to
the population parameter. By adapting the `\(z\)` statistics equation, we can
compute `\(t\)` statistics as follow:

`$$t = \frac{\bar{x}-\mu}{^s \big/ \tiny{\sqrt{n}}}$$`

As an example, we shall generate an array of random numbers following a normal
distribution where `\(X \sim N(120, 20)\)`.

``` r
set.seed(1)
x <- rnorm(20, mean=120, sd=20)
```

First, we do some basic exploratory analysis by finding the central tendency
and spread.

``` r
summary(x)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    75.7   112.3   127.2   123.8   135.2   151.9

``` r
sd(x)
```

    ## [1] 18.3

We let `\(x \sim N(120, 20)\)`, yet our `\(\bar{x}\)` is 123.81 with an `\(s\)` of `r sd(s)` and a `\(\nu\)` of 19. Does our statistics differ from the parameter
`\(\mu=120\)`? We can further formulate our question into hypotheses:

`\begin{align} H_0 &: \bar{x} = 120 \\ H_a &: \bar{x} \neq 120 \end{align}`

Then we can determine the `\(t\)` statistics:

``` r
t <- {{mean(x) - 120} / {sd(x) / sqrt(20)}} %T>% print()
```

    ## [1] 0.933

Then we shall locate `\(t\)` statistics into its distribution of `\(t_{19}\)`:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.one.sample1-1.png" width="100%" />

Then we can compute the p-value for a one-tailed test:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.one.sample2-1.png" width="100%" />

``` r
1 - pt(t, df=19)
```

    ## [1] 0.181

Also the p-value for a two-tailed test:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.one.sample3-1.png" width="100%" />

``` r
2 * {1 - pt(t, df=19)}
```

    ## [1] 0.363

How does our calculation compare to the built-in function in `R`?

``` r
t.test(x, mu=120)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  x
    ## t = 0.9, df = 19, p-value = 0.4
    ## alternative hypothesis: true mean is not equal to 120
    ## 95 percent confidence interval:
    ##  115 132
    ## sample estimates:
    ## mean of x 
    ##       124

At this point, we may have realised `R` only prints the rounded value for
acquired computation. If we are interested to see the actual value, we may save
our test result as an object, then directly call the specific result. In
following demonstration, we shall save the T-test and obtain its p-value.

``` r
t.result <- t.test(x, mu=120)
t.result$p.value
```

    ## [1] 0.363

Comparing our computation and the result acquired from `R` built-in function,
we failed to reject our `\(H_0\)`, so we conclude `\(\bar{x}=\mu_0=120\)`.

# Unpaired T-Test

When we two samples, we can compare the central tendency of both samples by
computing the mean difference. If both data follow a normal distribution, we
are conducting a two-sample T-test. As in previous examples, unpaired T-Test
(or rather, T-Test in general) assumes normality. Moreover, there are further
assumptions when conducting T-test, resulting in two distinctive types of said
test, i.e. a Student’s and Welch’s approach. T-test is arguably robust in
non-normally distributed data to a certain degree, where skewedness and
outliers highly influence its robustness. The problem with robustness is the
way we properly evaluate how T-test may provide a correct inference in the
event of having non-normal data. A few simulations have demonstrated how T-test
robust against `\(\chi^2\)` distribution with a *specific* range of `\(\nu\)`. However,
when we have a real-world data, we often do not know their underlying
parameters. In such cases, it is safe to follow normality assumption to avoid
type-I error inflations. To test mean difference between two samples, we
formulate following hypotheses:

`\begin{align} H_0 &: \bar{x}_1 - \bar{x}_2 = d \\ H_a &: \bar{x}_1 - \bar{x}_2 \neq d \\ d &= \mu_1 - \mu_2 = 0 \end{align}`

As outlined above, our hypotheses depends on the value of `\(d\)`, which defaulted
to `\(0\)`. In some rare occasion, we may apply a different value of `\(d\)`, but for now
we will stick with `\(d=0\)`.

## Student’s T-Test

Student’s approach in T-test is a test with pooled variance. We may conduct
this test when we know that variances in both samples are comparably similar.
We denote similarity as a homogeneity of variance, which we can prove using the
Levene’s test.

`\begin{align} t &= \frac{\bar{x}_1 - \bar{x}_2 - d}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}} \tag{Statistics} \\ s_p &= \sqrt{\frac{(n_1 - 1) s_1^2 + (n_2 - 1) s_2^2}{\nu}} \tag{Pooled variance} \\ \nu &= n_1 + n_2 - 2 \tag{Degree of freedom} \end{align}`

However, if our data fail to fulfill homogeneity of variance assumption, we
shall appropriately use a Welch’s T-test.

## Welch’s T-Test

Welch’s approach still assume normality, but give more leniency to equality of
variance. As a result, Welch modified the equation to compute the `\(t\)`
statistics, where we may find:

`\begin{align} t &= \frac{\bar{x}_1 - \bar{x}_2 - d} {\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}} \tag{Statistics} \\ \nu &= \frac{(n_1-1)(n_2-1)}{(n_2-1)C^2 + (1-C^2)(n_1-1)} \tag{Degree of freedom} \\ \\ C &= \frac{\frac{s_1^2}{n_1}}{\frac{s_1^2}{n_1} \frac{s_2^2}{n_2}} \end{align}`

Compared to Student’s T-test, Welch’s provide a different measure of `\(\nu\)` to
adjust for differences in sample variances. As an example, we may consider
following situation:

> Suppose we are collecting data on body height. Our population of interest will
> be students registered in UKRIDA, where we categorize sex as female and male.
> We acquire a normally distributed data from both sexes, where:
>
> -   `\(female \sim N(155, 15)\)`
> -   `\(male \sim N(170, 12)\)`
>
> We have a sample of 25 females and 30 males, and would like conduct a
> hypothesis test on mean difference.

``` r
set.seed(5)
tbl <- data.frame(
    "height" = c(rnorm(30, 170, 8), rnorm(25, 155, 16)),
    "sex" = c(rep("male", 30), rep("female", 25))
)

tapply(tbl$height, tbl$sex, summary)
```

    ## $female
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     123     146     154     158     173     190 
    ## 
    ## $male
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     152     165     168     170     177     184

``` r
tapply(tbl$height, tbl$sex, sd)
```

    ## female   male 
    ##  17.98   7.93

<img src="{{< blogdown/postref >}}index_files/figure-html/two.sample.t2-1.png" width="100%" />

Do both groups in our sample follow a normal distribution?

``` r
tapply(tbl$height, tbl$sex, shapiro.test)
```

    ## $female
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  X[[i]]
    ## W = 1, p-value = 0.7
    ## 
    ## 
    ## $male
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  X[[i]]
    ## W = 1, p-value = 0.2

Considering both p-values being `\(\geqslant 0.05\)` in Shapiro-Wilk test, we can
ascertain their normality. We then tested for homogeneity of variance using
Lavene’s test:

``` r
car::leveneTest(tbl$height ~ tbl$sex)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value  Pr(>F)    
    ## group  1    14.3 0.00039 ***
    ##       53                    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Interpreting a low p-value, we can conclude variances in both groups are not
equal to one another. In this case, we will follow Welch’s method.

``` r
t.test(height ~ sex, data=tbl, var.equal=FALSE)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  height by sex
    ## t = -3, df = 32, p-value = 0.004
    ## alternative hypothesis: true difference in means between group female and group male is not equal to 0
    ## 95 percent confidence interval:
    ##  -19.87  -4.07
    ## sample estimates:
    ## mean in group female   mean in group male 
    ##                  158                  170

Just for curiosity sake, we may want to try Student’s method as well and see
how it is different from Welch’s:

``` r
t.test(height ~ sex, data=tbl, var.equal=TRUE)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  height by sex
    ## t = -3, df = 53, p-value = 0.002
    ## alternative hypothesis: true difference in means between group female and group male is not equal to 0
    ## 95 percent confidence interval:
    ##  -19.27  -4.67
    ## sample estimates:
    ## mean in group female   mean in group male 
    ##                  158                  170

The Student’s T-test reported a lower p-value compared to Welch’s T-test
when we have unequal variance. A low p-value is not a bad sign per se, but we
need to be wary when we violated required assumptions. A low p-value may
indicate an inflation in statistical error. After conducting our test, we can
summarize our findings by visualizing them.

<img src="{{< blogdown/postref >}}index_files/figure-html/two.sample.t7-1.png" width="100%" />

Visualizing our results is important when conducting a statistical inference,
since it gives the reader a clearer representation on what we observed in our
data. Both figures give similar information, yet conveyed in different
fashions.

# Paired T-Test

The equation in unpaired T-test implicitly imply independence of each data
point, where we could not correctly infer a paired data. We may consider using
a paired T-test when we have following situations:

-   Difference between multiple measurements
-   Probability events where each instance influence another

In measuring mean differences between paired data, first we need to reduce its
complexity. Suppose `\(\mu_1\)` and `\(\mu_2\)` represent measurement in `\(t_1\)` and
`\(t_2\)`. Both measures represent same subject (within comparison). We can calculate
the difference between both samples:

`$$\mu_d = \mu_1 - \mu_2$$`

Then, we only needed to take into account one-sample difference, where we
hypothesize:

`\begin{align} H_0 &: \mu_d = 0 \\ H_a &: \mu_d \neq 0 \end{align}`

Does it seem familiar? Because it is! By reducing the complexity, we can infer
differences in paired data using a one-sample T-test to `\(mu_d\)`. Following
example will help us visualizing the concept:

> In the current investigation, we are looking for the effect of a certain
> anti-hipertensive drug. First we measure the blood pressure baseline, then
> prescribe the drug to all subjects. Then, we re-measure the blood pressure
> after one month. Each subject has a unique identifier, so we can specify mean
> differences within paired samples. Suppose we have the following scenario in 30
> sampled subjects:
>
> -   `\(X_1 \sim N(140, 12)\)`
> -   `\(X_2 \sim N(130, 17)\)`

We then set our hypotheses:

`\begin{align} H_0 &: \bar{x}_d = 0 \\ H_a &: \bar{x}_d \neq 0 \end{align}`

``` r
set.seed(1)
tbl <- data.frame(
    "bp" = c(rnorm(30, 140, 12), rnorm(30, 133, 17)),
    "time" = c(rep("Before", 30), rep("After", 30)) %>%
        factor(levels=c("Before", "After"))
)

# Measure the mean of mean difference
md <- with(tbl, bp[time=="Before"] - bp[time=="After"])

# Calculate t-statistics
t <- {mean(md)} / {sd(md) / sqrt(30)} %T>% print()
```

    ## [1] 3.12

``` r
# Obtain p-value for a two-sided test
2 * {1 - pt(t, df=29)}
```

    ## [1] 0.076

``` r
# Comparison to built-in one-sample T-Test
t.test(md, mu=0)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  md
    ## t = 2, df = 29, p-value = 0.08
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.64 12.10
    ## sample estimates:
    ## mean of x 
    ##      5.73

``` r
# Comparison to built-in paired T-Test
t.test(bp ~ time, data=tbl, paired=TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  bp by time
    ## t = 2, df = 29, p-value = 0.08
    ## alternative hypothesis: true mean difference is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.64 12.10
    ## sample estimates:
    ## mean difference 
    ##            5.73

## Choosing an Appropriate Test

All tests explained in this post assume data normality, both for T-test and
Z-test. As a general rule of thumb, we may use following conventions:

-   One-sample test:
    -   Known `\(\sigma \to\)` use Z-Test
    -   Unknown `\(\sigma \to\)` use one-sample T-Test
-   Two-sample test `\(\to\)` do Levene’s test
    -   Equal variance: Student’s method (pooled variance)
    -   Unequal variance: Welch’s method
-   Paired T-Test: Basically one-sample T-Test on sampled differences

# Effect Size

The previous lecture on sample size equation served as a brief introduction on
statistical power, which presented us a new concept of effect size. There are
two related concepts to effect size and statistical power, *viz.* sample size
`\(n\)` and significance level `\(\alpha\)`. Effect size calculation varies on the type
of data we consider and how we conduct our formal test. In this section, we
will focus on measuring effect size in a mean difference between two groups
using Cohen’s distance `\(d\)`.

`\begin{align} d &= \frac{\bar{x}_1 - \bar{x}_2}{s_p} \tag{Cohen's D} \\ s_p &= \sqrt{\frac{(s_1^2 + s_2^2)}{2}} \tag{Pooled SD} \end{align}`

As an example, we will re-use previous scenario:

``` r
set.seed(1)
tbl <- data.frame(
    "bp" = c(rnorm(30, 140, 12), rnorm(30, 133, 17)),
    "time" = c(rep("Before", 30), rep("After", 30)) %>%
        factor(levels=c("Before", "After"))
)

# Measure the mean of mean difference
md <- with(tbl, bp[time=="Before"] - bp[time=="After"])

# Calculate t-statistics
t <- {mean(md)} / {sd(md) / sqrt(30)} %T>% print()
```

    ## [1] 3.12

``` r
# Obtain p-value for a two-sided test
2 * {1 - pt(t, df=29)}
```

    ## [1] 0.076

We will calculate `\(d\)` by issuing following command in `R`. Beware though,
different method of computing also exists as we can be more flexible in
expressing our code. I prefer this method because it clearly explains what we
do at the expense of the need to understand `apply` family of functions in `R`,
which is a good thing to know if you were to learn `R` (and I hope you will!)

``` r
# Calculate pooled standard deviation
sp <- sqrt({with(tbl,
    tapply(bp, time, var, simplify=FALSE)) %>% {do.call(add, .)}
} / 2) %T>% print()
```

    ## [1] 12.4

``` r
# Measure Cohen's distance
{with(tbl,
    tapply(bp, time, mean, simplify=FALSE)) %>% {do.call(subtract, .)}
} / sp
```

    ## [1] 0.464

As a comparison, we can also compute `\(d\)` using the `psych` package.

``` r
# Calculate power using the `psych` package
d <- psych::cohen.d(tbl ~ time) %T>% print()
```

    ## Call: psych::cohen.d(x = tbl ~ time)
    ## Cohen d statistic of difference between two means
    ##    lower effect upper
    ## bp -0.98  -0.47  0.04
    ## 
    ## Multivariate (Mahalanobis) distance between groups
    ## [1] 0.47
    ## r equivalent of difference between two means
    ##    bp 
    ## -0.23

Finally, we can apply `\(d\)` to computing the statistical power:

``` r
# Power analysis using previous information
pwr::pwr.t.test(n=30, d=d$cohen.d[[2]], sig.level=0.05, type="paired")
```

    ## 
    ##      Paired t test power calculation 
    ## 
    ##               n = 30
    ##               d = 0.472
    ##       sig.level = 0.05
    ##           power = 0.704
    ##     alternative = two.sided
    ## 
    ## NOTE: n is number of *pairs*
