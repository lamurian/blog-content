---
author: lam
title: "Sample Size and Statistical power"
weight: 4
description: >

  Proving or disproving a research hypothesis requires representative evidence
  which may help us substantiate our claims. In general, we need a large amount
  of data following a rigorous and well-documented procedure. However, inviting
  all potential subjects is not feasible due to time and resource constraint.
  Considering the amount of limitation we have, how do we determine the minimum
  required amount of data to answer our research question?

summary: >

  Proving or disproving a research hypothesis requires representative evidence
  which may help us substantiate our claims. In general, we need a large amount
  of data following a rigorous and well-documented procedure. However, inviting
  all potential subjects is not feasible due to time and resource constraint.
  Considering the amount of limitation we have, how do we determine the minimum
  required amount of data to answer our research question?

date: 2020-10-01
categories: ["statistics", "ukrida"]
tags: ["power", "R", "sampling", "hypothesis"]
slug: 04-power
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/04-power/slide)

Proving or disproving a research hypothesis requires representative evidence
which may help us substantiate our claims. In general, we need a large amount
of data following a rigorous and well-documented procedure. However, inviting
all potential subjects is not feasible due to time and resource constraint.
Considering the amount of limitation we have, how do we determine the minimum
required amount of data to answer our research question?

# P-value

We reject our `\(H_0\)` when our statistical test resulted in a p-value \< 0.05, why
do we use 0.05 as our cut-off point? In a simple term, 0.05 simply reflects a
5% chance of having a correct null hypothesis. Though, I am more inclined to
regard it as a probability value. When the probability is small enough, we
reject our `\(H_0\)`. To help us visualize the idea, we can use a formal approach
to answer our hypothesis on (*ahem*) a simple coin toss.

``` r
set.seed(1)
coin <- sample(c("H", "T"), 10, replace=TRUE, prob=rep(1/2, 2)) %T>% print()
```

    |  [1] "T" "T" "H" "H" "T" "H" "H" "H" "H" "T"

As always, we set `H` as our outcome of interest. After observing ten coin
tosses, we may find the appeal to formulate our hypotheses, where we may say:

-   `\(H_0: P(X=x) = 0.5\)`
-   `\(H_a: P(X=x) \neq 0.5\)`

Assuming independence of a fair coin, it is a Bernoulli trial and follows the
binomial distribution. But, does it? :)

``` r
binom.test(x=sum(coin == "H"), n=length(coin), p=0.5)
```

    | 
    |   Exact binomial test
    | 
    | data:  sum(coin == "H") and length(coin)
    | number of successes = 6, number of trials = 10, p-value = 0.8
    | alternative hypothesis: true probability of success is not equal to 0.5
    | 95 percent confidence interval:
    |  0.2624 0.8784
    | sample estimates:
    | probability of success 
    |                    0.6

At this point, we have seen binomial test numerous times, yet we have not
unravel the secret behind this math! Why did we *fail to reject* the `\(H_0\)`? Or
rather, why does the *p-value \> 0.05*? The question we wish to answer is:

> What is the probability of having 6 `H` out of 10 Bernoulli trials? Is it \<
> 5%?

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.binom10-1.png" width="90%" />

To answer our question, we may use Binomial probability function to find our
probability. To find a `\(P(X=6): X \sim B(10, 0.5)\)`, `R` uses following command:

``` r
dbinom(6, 10, 0.5)
```

    | [1] 0.2051

We can manually calculate the p-value as the .amber\[sum\] of `\(P(X \geqslant 6)\)`.

``` r
2 * (dbinom(6:10, 10, 0.5) %>% sum())
```

    | [1] 0.7539

So we could not distinguish a relative probability of 0.6 and 0.5
from a ten consecutive coin tosses. Interesting. How if we preserve the ratio
of event (3:5) using more trials?

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.binom100-1.png" width="90%" />

As previously done, we can find the probability `\(P(X=60): X \sim B(100, 0.5)\)`
using the Binomial probability function:

``` r
dbinom(60, 100, 0.5)
```

    | [1] 0.01084

And the p-value would be:

``` r
2 * (dbinom(60:100, 100, 0.5) %>% sum())
```

    | [1] 0.05689

We preserved the ratio, why has the probability changed? The reason lies in the
number of trial we conducted. We have observed how the peak and distribution
width changes. With *more* trial, the narrower observed distributions get.
That, in itself, does not change our critical value at 5%, but they change
where it is located relative to the mid-point. Theoretically, p-value is
*difficult* to understand. But in practice, it tells you the probability of
having a correct `\(H_0\)`.

> Low p-value `\(\to\)` reject `\(H_0\)`

Cassie Kozyrkov uploaded a splendid example on how to comprehend the p-value:

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/9jW9G8MO4PQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>
</center>

# Significance Level

In interpreting p-value, we use a threshold of 0.05, which reflect our
significance level `\(\alpha\)`. With a higher `\(\alpha\)`, we have more chance to
reject the `\(H_0\)`, and so does our chance get higher when we have more sample.
However, we may need to worry about incorrect rejection, *viz.* what is the
chance of we incorrectly reject a true `\(H_0\)`?

Suppose we are conducting a study on a potential cancer therapy. We knew giving
the patient a placebo may affect their recovery rate by 50%. We are certain
that giving the new treatment will increase the probability. Tested on 50
patients, 35 showed signs of better quality of life.

Considering I.I.D, we may model our problem as a Binomial distribution $Cured \sim B(50, 0.5)$, so we can state our hypothesis as:

`\begin{align} H_0 &: P(X=35) = 0.5 \\ H_a &: P(X=35) > 0.5 \end{align}`

Since the data follows the Binomial distribution, we can assess our hypothesis
using the Binomial test:

``` r
binom.test(35, 50, 0.5, alternative="greater")
```

    | 
    |   Exact binomial test
    | 
    | data:  35 and 50
    | number of successes = 35, number of trials = 50, p-value = 0.003
    | alternative hypothesis: true probability of success is greater than 0.5
    | 95 percent confidence interval:
    |  0.5763 1.0000
    | sample estimates:
    | probability of success 
    |                    0.7

With a nice visualization, we can see where our hypothesis located relative to
the midpoint (hint: the blue dot).

<img src="{{< blogdown/postref >}}index_files/figure-html/stat.err.eg2-1.png" width="90%" />

We are assuming `\(H_a > H_0\)`, how do we picture `\(\alpha\)` in our figure? Since
`\(\alpha\)` is the significance level of 0.05, we need to find a point of
`\(x : P(X=0.05 | 50, 0.5)\)`. After doing so, we can find our *significance value*
in the x-axis (the red dashed line).

<img src="{{< blogdown/postref >}}index_files/figure-html/stat.err.eg3-1.png" width="90%" />

Area to the right of our significance value determines the probability of
getting a type I error `\(\alpha\)`. If we were to make a second distribution
reflecting our `\(H_a\)`, we may observe intersected area reflecting our
probability of getting a type II error `\(\beta\)`. Assuming `\(H_a\)` coming from
similar distribution as `\(H_0\)`, we just need to determine tis parameter.
However, the value of `\(\beta\)` depends on the `\(H_a\)` distribution, and we only
stated `\(H_a > H_0\)`. It means, the parameter in `\(H_a\)` can be any value higher
than 0.5, either be 0.6, 0.7, 0.8, and so on. For our convenience, we will
assign 0.7 as our `\(H_a\)` so we can construct the second binomial distribution.

<img src="{{< blogdown/postref >}}index_files/figure-html/stat.err.eg4-1.png" width="90%" />

The area to the left of our significance value in the *second* `\(H_a\)`
distribution is the probability of getting a type II error `\(\beta\)`. We can
summarize the characteristic of type I and type II statistical error using
following anecdote:

<img src="https://mk0codingwithmaxskac.kinstacdn.com/wp-content/uploads/2019/12/type-1-error-type-2-statistical-power-comic.png" width="100%">

Type I statistical error:

-   Incorrectly rejecting the `\(H_0\)`
-   Reflected as `\(\alpha \to\)` shaded area to the right of `\(H_0\)` distribution
-   A false positive

Type II statistical error:

-   Incorrectly accepting the `\(H_0\)`
-   Reflected as `\(\beta \to\)` shaded area to the left of `\(H_a\)` distribution
-   A false negative

# Power Analysis

So far, we have learnt there ought to be some probability of incorrectly
rejecting the `\(H_0\)`, which highly depends on the significance level `\(\alpha\)`,
rate of type II error `\(\beta\)`, and the sample size. We can adjust `\(\alpha\)` to
reduce the risk of having type I error at the cost of increasing the risk for
type II error. Such a problem is the art of performing statistics, where we may
tend to justify one type of error instead of another. Imagine this case in a
court as a statistical problem. We have charged someone as guilty of robbery
and we need to prove their guilt before sending them to jail. After collecting
some evidence, we may have two possible error:

-   We incorrectly put an innocent person in jail (type I error)
-   We failed to identify the culprit and let them free (type II error)

What is the more appropriate case to justify, letting a criminal free or
arresting an innocent citizen? That is the question we need to deal with in
statistics, of whether we should accept a type I or type II error. However, if
we have more data (analogous to more evidence in court settings), we may reduce
**both** error at the expense of time and effort. A good research demand a
sufficient amount of data to prevent both type of error. Though, please be
advised, we are not getting anywhere near to type III and other special errors
in statistics. For now, understanding the relation between statistical errors
and sample size is enough to introduce us a new topic: statistical power.

In statistics, we define power as a simple equation of `\(1 - \beta\)`. However,
determining the value of `\(\beta\)` is a bit tricky, as we have visually observed.
In essence, a good statistical power helps us to correctly reject the `\(H_0\)`
when it is actually false. As a rule of thumb, we expect to have a statistical
power in the range of 0.8 - 0.9 (higher is better). We can analyze the power
prospectively *or* retrospectively, depends on the context of what we would
like to prove. When done prospectively, a thorough power analysis can also help
us determining the minimum required sample.

However, conducting a power analysis is not all roses, since it comes with
several caveats to consider. First of all, power analysis depends on formal
methods to use when we design the statistical analysis pipeline. Since it
depends on our statistical methods and study designs, it often does not
generalize that well. It means that, if we were to propose a different
statistical method, we may need to adjust our power analysis. And lastly, power
analysis only tells us the best case scenario *estimate*.

There are four linked concepts to comprehend the statistical power:

-   Power
-   Effect size
-   Sample size
-   Alpha

So far, we have discussed what the other three are, but it is the first time we
hear about effect size. As a disclaimer though, this post only aims to give a
brief overview of what effect size is. We may not delve in further to formally
prove the equation we will later use. In a layman term, effect size measures a
true difference between two hypotheses. Hitherto, numerous conventions exist to
calculate the effect size. The higher the effect size we have reflects in more
statistical power. And as all good thing goes, effect size is one of the most
difficult to obtain.

<img src="https://sayingimages.com/wp-content/uploads/one-simply-can-never-have-power-meme.png" width="100%">

To obtain an effect size, we may initially perform a thorough literature
review, conduct a pilot study, *or* follow Cohen’s recommendation. When
reviewing published articles, we may find some with similar methods as we
proposed. In such cases, we can use their data to estimate the desired effect
size. Meta-analysis technique is sometimes applicable to make a better
estimate. When we fail to get published articles with similar methods and
elaborate results, we may find the appeal of conducting a pilot study. By doing
so, we can get data reflecting our future study. It **is** time consuming, but
it gives us a closer estimate of how our data will be. Also, a pilot study is a
good chance to resolve any unforeseen issue. Following Cohen’s recommendation
may help us determine what effect size we can regard as being adequate in a
prospective power analysis. Still, it depends on what formal test to use, where
we can use a certain threshold to separate a small, medium and large effect
size.

All words and no number would not help us much to understand the context, so we
shall dig into an example! We will use our previous case of a novel cancer
drug trial. Can we recall this figure?

<img src="{{< blogdown/postref >}}index_files/figure-html/stat.err.eg4-1.png" width="90%" />

We can calculate power when we know the probability function *and* its
parameters, as such:

`$$Let\ X \sim B(n, p)$$`

`\begin{align} sig &= x:P(X=1-\alpha\ |\ n, H_0) \\ \beta &= P(X \leqslant sig\ |\ n, H_1) \\ Power &= 1 - \beta \end{align}`

Plain math does not help us to stick in with the idea, how about some code?

``` r
# Set H0, sample size, significance level (alpha)
h0 <- 0.5; size <- 50; alpha.rate <- 0.05

# Find significance value
alpha.value <- qbinom(1 - alpha.rate, size, prob=h0) %T>% print()
```

    | [1] 31

``` r
# Determine H1
h1 <- 0.7

# Calculate beta
beta.value <- dbinom(0:alpha.value, size, prob=h1) %>% sum() %T>% print()
```

    | [1] 0.1406

``` r
# Calculate power
1 - beta.value
```

    | [1] 0.8594

Now, that is better :) If you find it hard to understand what’s going on, you
may want to focus on commented section (any line started by `#`), as they
indicates what action we do. Thankfully, we have some ready-to-use packages to
do the computation for us (yay to the devs!), so we *do not need* to reinvent
the wheel every time.

# Equation in Calculating Sample Size

As in calculating effect sizes, we have numerous equations to find a minimum
sample size. None fits all needs, as it depends on our search context. We will
see popular ones used in general and biomedical science.

## General Equation

`$$n = \bigg( \frac{Z_{1 - \frac{\alpha}{2}} + Z_{1-\beta}}{ES} \bigg)^2$$`

In general, the culprit causing us much headaches when calculating the sample
size is effect sizes. The equation itself only consists of plain arithmetical
operations. We shall understand each symbols first before looking the different
effect size measures.

`\(n\)`: Number of minimal sample size  
`\(Z_{1 - \frac{\alpha}{2}}\)`: Significance value in a standardized normal distribution  
`\(Z_{1-\beta}\)`: Power value in a standardized normal distribution  
`\(ES\)`: Effect size

### Dichotomous outcome, one sample

`\begin{align} H_0 &: p = p_0 \\ ES &= \frac{p_1 - p_0}{\sqrt{p(1-p)}} \end{align}`

### Dichotomous outcome, two independent samples

`\begin{align} H_0 &: p_1 = p_2 \\ ES &= \frac{|p_1 = p_2|}{\sqrt{p(1-p)}} \end{align}`

### Continuous outcome, one sample

`\begin{align} H_0 &: \mu = \mu_0 \\ ES &= \frac{|\mu_1 = \mu_0|}{\sigma} \end{align}`

### Continuous outcome, two independent samples

`\begin{align} H_0 &: \mu_1 = \mu_2 \\ ES &= \frac{|\mu_1 = \mu_2|}{\sigma} \end{align}`

### Continuous outcome, two matched samples

`\begin{align} H_0 &: \mu_d = 0 \\ ES &= \frac{\mu_d}{\sigma_d} \end{align}`

Different study designs may require different solution, where different field
of science contributed to countless preferences in performing statistics. What
do we do as biomedical scientist? (Charan and Biswas, 2013)

## Cross-Sectional

### Qualitative variable

`$$n = \frac{Z_{1-\frac{\alpha}{2}}^2 \cdot p (1-p)}{d^2}$$`

## Quantitative variable

`$$n = \frac{Z_{1-\frac{\alpha}{2}}^2 \cdot \sigma^2}{d^2}$$`

`\(Z_{1 - \frac{\alpha}{2}}\)`: Significance value in a standardized normal distribution  
`\(d\)`: Absolute error as determined by the researcher  
`\(p\)`: Estimated proportion  
`\(\sigma\)`: Standard deviation

## Case-Control

### Qualitative variable

`$$n = \frac{r+1}{r} \frac{(p^*)(1-p^*)(Z_{\beta} + Z_{\frac{\alpha}{2}})^2}{(p_1 - p_2)^2}$$`

### Quantitative variable

`$$n = \frac{r+1}{r} \frac{\sigma^2(Z_{\beta} + Z_{\frac{\alpha}{2}})^2}{(p_1 - p_2)^2}$$`

`\(r\)`: Ratio of control to case  
`\(p^*\)`: Average of exposed samples proportion  
`\(\sigma\)`: Standard deviation from previous publication  
`\(p_1 - p_2\)`: Difference in proportion as previously reported  
`\(Z_{\beta}\)`: `\(\beta\)` value in a standardized normal distribution

## Clinical Trial / Experimental

### Qualitative variable

`$$n = \frac{2 P(1-P) \cdot (Z_{\frac{\alpha}{2}} + Z_{\beta})^2}{(p_1-p_2)^2}$$`

### Quantitative variable

`$$n = \frac{2\sigma^2 \cdot (Z_{\frac{\alpha}{2}} + Z_{\beta})^2}{d^2}$$`

`\(\sigma\)`: Standard deviation from previous publication  
`\(P\)`: Pooled prevalence from both groups  
`\(p_1 - p_2\)`: Difference in proportion as previously reported

# Random Sampling

After understanding what p-value is and calculating the minimum sample size, we
are one step closer to conducting our own investigation. We have previously
discussed the importance of having independent and identically distributed
data, where random sampling is one method to fulfill such an assumption. In
random sampling, we can choose either a non-probability or a probability method
to acquire our sample. In following sections, we will have a brief explanation
on each of available methods.

## Non-Probability Random Sampling

### Convenience

-   Based on availability
-   Representativeness is unknown
-   Useful in preliminary study

### Quota

-   As in convenient sampling
-   We set the desired proportion of our sample
-   Proportion based on specific criteria, e.g. age, sex, etc.

## Probability Random Sampling

### Simple

-   Random sample from a list of all subjects in a population
-   Each subject has an equal chance to participate
-   Useful in a small population

### Systematic

-   Subject selection not entirely random
-   As in random sampling, requires an enumeration of all subjects
-   Systematically select the subject based on a certain criteria, e.g. every `\(n_{th}\)` subject

### Stratified / cluster

-   Split subjects into stratified / clustered groups
-   Do random sampling from each group
-   Stratified `\(\to\)` preserves ordinality, i.e. the order is important

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-Charan2013" class="csl-entry">

Charan, J. and Biswas, T. (2013) How to calculate sample size for different study designs in medical research? *Indian Journal of Psychological Medicine* \[online\]. 35 (2), pp. 121. Available from: <https://doi.org/10.4103/0253-7176.116232>doi:[10.4103/0253-7176.116232](https://doi.org/10.4103/0253-7176.116232).

</div>

</div>
