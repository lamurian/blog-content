---
author: lam
title: "Hypothesis Test: Proportional Difference"
weight: 5
description: >

  In statistics, proving or rejecting an assumption requires a rigorous formal
  approach, which highly depends on the formulated hypothesis. The current post
  will help us in conducting a statistical test to measure hypothesized
  proportional difference among categorical variables. Another common name
  assigned to such a formal step is the test of independence. We shall see
  various tests we have, how they work and when to use them.

summary: >

  In statistics, proving or rejecting an assumption requires a rigorous formal
  approach, which highly depends on the formulated hypothesis. The current post
  will help us in conducting a statistical test to measure hypothesized
  proportional difference among categorical variables. Another common name
  assigned to such a formal step is the test of independence. We shall see
  various tests we have, how they work and when to use them.

date: 2020-10-09
categories: ["statistics", "ukrida"]
tags: ["R", "hypothesis"]
slug: 05-independence
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/05-independence/slide)

In statistics, proving or rejecting an assumption requires a rigorous formal
approach, which highly depends on the formulated hypothesis. The current post
will help us in conducting a statistical test to measure hypothesized
proportional difference among categorical variables. Another common name
assigned to such a formal step is the test of independence. We shall see
various tests we have, how they work and when to use them.

# Proportional Difference

We may recall from the first lecture that we can measure proportion in a
population or sample as a ratio of an event to the total space. As an example,
imagine we have 100 participants in a survey, where 60 involved respondents are male
and the rest are female. We can calculate the proportion of male participants
as `\(\frac{60}{100} = 0.6\)`. In previous lectures, we also performed a number of
experiments involving coin flipping and calculating the conditional probability
of `\(P(X=x|n, p)\)`, by considering `\(X \sim B(n, p)\)`. What we previously did was
proving a `\(H_0: \hat{p} = p\)`, with `\(\hat{p}\)` being the proportion seen in our
sample and `\(p\)` is the assumed natural occurrence of probability in a fair coin,
0.5. We have yet to coin the term yet (pun intended), but we have grown
familiar with a form of hypothesis testing using a binomial test.

Binomial test is a form of exact hypothesis test for one group proportion. With
an exact test, we can expect the computed probability of proposed `\(H_0\)` being
true reflects the actual probability. In later sections, we shall observe how
calculating an exact probability gets more tedious with larger sample size and
more complex comparison. As a solution, we often rely on an approximation, e.g.
using proportion test for one group proportion problem as an alternative to
binomial test. As we have gotten quite comfortable with the idea of flipping a
coin, we may use this example as a proof of concept in distinguishing an exact
test from its approximation.

`$$Let X \sim B(n, p)$$`
`$$\texttt{Test the probability of having: } P(X=6 \ |\ 10, 0.5)$$`

`\begin{align} H_0 &: P(X=6) = 0.5 \\ H_a &: P(X=6) \neq 0.5 \end{align}`

| estimate | statistic | p.value | parameter | conf.low | conf.high | method              | alternative |
|---------:|----------:|--------:|----------:|---------:|----------:|:--------------------|:------------|
|      0.6 |         6 |   0.754 |        10 |    0.262 |     0.878 | Exact binomial test | two.sided   |

(#tab:prop.1)Binomial test

| estimate | statistic | p.value | parameter | conf.low | conf.high | method                                               | alternative |
|---------:|----------:|--------:|----------:|---------:|----------:|:-----------------------------------------------------|:------------|
|      0.6 |       0.1 |   0.752 |         1 |    0.274 |     0.863 | 1-sample proportions test with continuity correction | two.sided   |

(#tab:prop.1)Proportion test

Using an approximation, the proportion test gives a slightly different results
compared to the binomial test (hint: look at the p-value and confidence
interval). Now we may perform the same procedure, but using a higher sample
space, where we indicate the problem as follows:

`$$Let X \sim B(n, p)$$`
`$$\texttt{Test the probability of having: } P(X=60 \ |\ 100, 0.5)$$`

`\begin{align} H_0 &: P(X=60) = 0.5 \\ H_a &: P(X=60) \neq 0.5 \end{align}`

| estimate | statistic | p.value | parameter | conf.low | conf.high | method              | alternative |
|---------:|----------:|--------:|----------:|---------:|----------:|:--------------------|:------------|
|      0.6 |        60 |   0.057 |       100 |    0.497 |     0.697 | Exact binomial test | two.sided   |

(#tab:prop.2)Binomial test

| estimate | statistic | p.value | parameter | conf.low | conf.high | method                                               | alternative |
|---------:|----------:|--------:|----------:|---------:|----------:|:-----------------------------------------------------|:------------|
|      0.6 |      3.61 |   0.057 |         1 |    0.497 |     0.695 | 1-sample proportions test with continuity correction | two.sided   |

(#tab:prop.2)Proportion test

With a larger sample size, the proportion test gives a better estimation of the
binomial test. Notice how the p-value and confidence interval produced by the
proportion test gets closer to the binomial test, as compared to our previous
test. In fact, with sample size of `\(n \to \infty\)`, we can expect a closer
approximation to the exact test. The perk of using approximation is its
comparably lower computational power and less stringent assumptions.

However, we are often curious to observe multiple variables, i.e.??a
proportional difference in multiple groups. In such cases, neither of binomial
nor proportion test can help us! To identify the problem, first we need to
visualize each observation into a contingency table. Then we may apply a more
appropriate test, e.g.??with Fisher???s exact test or an approximation using
Pearson???s Chi-square.

Contingency table is a table with `\(m \times n\)` cells, usually takes form as a
`\(2 \times 2\)` table. The row and column of a contingency table represents two
variables of interest, respectively. With an arbitrary element, we can draw a
contingency table as follows:

|            | Outcome 1 | Outcome 2 |
|------------|:----------|:----------|
| Exposure 1 | a         | b         |
| Exposure 2 | c         | d         |

To help us visualize a contingency table, suppose we are conducting a market
research in Jakarta, where we aim to see how people express their preferences
in choosing chain store outlets. We categorized participants based on their
place of residency, i.e.??in suburban and urban area. The mini-market chain of
our interest would be Indomaret and Alfamart. We observed 30 out of 50
respondents in suburban area choose Indomaret, compared to 20 out of 50
respondents in urban area.

|          | Indomaret | Alfamart |
|----------|----------:|---------:|
| Suburban |        30 |       20 |
| Urban    |        20 |       30 |

As a test of proportional difference, both Fisher???s exact test and Pearson???s
Chi-square can aid us in inferring current data on our market research. On
later section we will see their limitation and what use cases are available.

# Fisher???s Exact Test

Fisher???s test provides an exact p-value calculation, where it follows a
hypergeometric distribution. From the previous lecture, we have learnt what
geometric distribution is, i.e.??a specific form of binomial distribution, where
we are interested to calculate the probability of having one event in `\(n\)`
number of trials. The hypergeometric distribution is somewhat similar to the
binomial distribution, with each instance not being identical. In the binomial
distribution, we can expect each trial following a Bernoulli trial with
identical probability of success. A hypergeometric distribution assumes an
event with replacement (kindly recall probability concepts from the urn
problem). In other word:

-   Binomial distribution solves the probability of having `\(k\)` successes
    within `\(n\)` number of trials without replacement
-   Geometry distribution finds the probability of having 1 success within `\(n\)`
    number of trials without replacement
-   Hypergeometry distribution looks for the probability of having `\(k\)` successes
    within `\(n\)` number of trials *with* replacement

Looking at proportional differences in general, we can formulate our
hypotheses as follow:
`\begin{align} H_0 &: \hat{p_1} = \hat{p_2} \\ H_a &: \hat{p_1} \neq \hat{p_2} \end{align}`
with `\(\hat{p_i}\)` being the proportion in group `\(i\)`.

Since Fisher???s method calculate the *exact* p-value, we can solve the
probability as a permutation problem. Therefore, we can calculate the
probability of *each* event using one of following equations:

`\begin{align} P &= \frac{\binom{a + b}{a} \binom{a + b}{b}} {\binom{n}{a + b}} \tag{1} \\   \\   &= \frac{\binom{c + d}{c} \binom{c + d}{d}} {\binom{n}{c + d}} \tag{2} \\   \\   &= \frac{(a+b)!\ (c+d)!\ (a+c)!\ (b+d)!} {a!\ b!\ c!\ d!\ n!} \tag{3} \\   \\   \\   \\ n &= a + b + c + d \end{align}`

Seeing the mathematical equation may not sound too appealing for some people,
we can also simplify equation 1 into a code, as follow:

``` r
fisher.eq <- function(abcd) { # abcd is a list of 4 elements
    a <- abcd[1]; b <- abcd[2]; c <- abcd[3]; d <- abcd[4]
    choose(a+b, a) * choose(a+b, b) / choose(a+b+c+d, a+b)
}
```

Beware though, as Fisher???s test being an exact approach, we need to calculate
*all* possible extreme events to obtain the p-value. In our case, we have $a, b, c, d$ as an array of `\((30,20,20,30)\)`, so we need to address **all**
extreme events which satisfy `\((a,b,c,d) \in \{(31,19,19,31),...,(50,0,0,50)\}\)`.

|   a |   b |   c |   d | probability |
|----:|----:|----:|----:|------------:|
|  30 |  20 |  20 |  30 |       0.022 |
|  31 |  19 |  19 |  31 |       0.009 |
|  32 |  18 |  18 |  32 |       0.003 |
|  33 |  17 |  17 |  33 |       0.001 |
|  34 |  16 |  16 |  34 |       0.000 |
|  35 |  15 |  15 |  35 |       0.000 |
|  36 |  14 |  14 |  36 |       0.000 |
|  37 |  13 |  13 |  37 |       0.000 |
|  38 |  12 |  12 |  38 |       0.000 |
|  39 |  11 |  11 |  39 |       0.000 |
|  40 |  10 |  10 |  40 |       0.000 |
|  41 |   9 |   9 |  41 |       0.000 |
|  42 |   8 |   8 |  42 |       0.000 |
|  43 |   7 |   7 |  43 |       0.000 |
|  44 |   6 |   6 |  44 |       0.000 |
|  45 |   5 |   5 |  45 |       0.000 |
|  46 |   4 |   4 |  46 |       0.000 |
|  47 |   3 |   3 |  47 |       0.000 |
|  48 |   2 |   2 |  48 |       0.000 |
|  49 |   1 |   1 |  49 |       0.000 |
|  50 |   0 |   0 |  50 |       0.000 |

To calculate p-value in a one-tailed test, we simply sum all the probabilities:

``` r
sum(tbl$probability)
```

    | [1] 0.0357

Having the product of one-tailed p-value multiplied by `\(2\)`, we can get the
two-tailed p-value:

``` r
sum(tbl$probability) * 2
```

    | [1] 0.0713

We can compare our calculation with `R`, which resulted in:

``` r
fisher.test(survey, alternative="greater")$p.value
```

    | [1] 0.0357

``` r
fisher.test(survey, alternative="two.sided")$p.value
```

    | [1] 0.0713

# Chi-square Test of Independence

Previous example demonstrated how Fisher???s test measures exact p-value given a
particular condition. We can imagine, with a larger sample space, the
computation gets more complicated. To provide an approximation, we may use
other measures, such as Chi-square test of independence or G-test. As
Chi-square is more ubiquitous, we will limit our discussion on this approach.
Please be advised though, different method of Chi-square test exists, where we
may choose any based on the assumption on how each variable associates with one
another.

Our demonstration on Fisher???s test depicts how arduous the computation can be.
Pearson???s Chi-square, as one method of approximation, gives a close estimate to
Fisher???s test especially with higher sample size of `\(n: n \to \infty\)`. In
Fisher???s test, we often limit our inference to a `\(2 \times 2\)` contingency
table. Pearson???s Chi-square provides a more generalizable construct where we
can apply `\(m \times n\)` contingency tables into calculation. As its name
suggests, statistics computed using Pearson???s Chi-square follows a Chi-square
distribution, where the degree of freedom `\(k\)` depends on the number of classes
in our variables. Approximating the p-value will requires computing the
Chi-square value, where we define:

`\begin{align} \chi^2 &= \displaystyle \sum_{i, j} \frac{(O_{ij} - E_{ij})^2}{E_{ij}} \\ E_{ij} &= \frac{\sum O_i \cdot \sum O_j}{\sum O_i + O_j} \end{align}`

`\(O:\)` Observed outcome  
`\(E:\)` Expected outcome  
`\(i, j:\)` Elements in the contingency table

Knowing the equation, we may compute the Chi-square value using previously
described contingency table:

|            | Outcome 1 | Outcome 2 |
|------------|:----------|:----------|
| Exposure 1 | a         | b         |
| Exposure 2 | c         | d         |

So we will have our expected outcome as:

`$$E_{11} = \frac{(a + b) \cdot (a + c)}{a + b + c + d}$$` `$$E_{12} = \frac{(a + b) \cdot (b + d)}{a + b + c + d}$$`

`$$E_{21} = \frac{(c + d) \cdot (a + c)}{a + b + c + d}$$`

`$$E_{22} = \frac{(c + d) \cdot (b + d)}{a + b + c + d}$$`

Considering the value in `\((a, b, c, d)\)`:

|          | Indomaret | Alfamart |
|----------|----------:|---------:|
| Suburban |        30 |       20 |
| Urban    |        20 |       30 |

We can calculate the expected outcomes:

`\begin{align} E_{ij} &= \frac{\sum O_i \cdot \sum O_j}{\sum O_i + O_j} \\ \\ E_{11} &= 25 \\ E_{12} &= 25 \\ E_{21} &= 25 \\ E_{22} &= 25 \end{align}`

Then we can compute our Chi-square statistics:

`\begin{align} \chi^2 &= \displaystyle \sum_{i, j} \frac{(O_{ij} - E_{ij})^2}{E_{ij}} \\        &= \frac{(30-25)^2}{25} + \frac{(20-25)^2}{25} + \frac{(30-25)^2}{25} + \frac{(20-25)^2}{25} \\        &= 4 \end{align}`

Great! But it has yet to cough up the p-value. To obtain our p-value, we need
to know where `\(\chi^2=4\)` is located in a Chi-square distribution quantile with
a degree of freedom `\(k=1\)`.

``` r
1 - pchisq(4, df=1)
```

    | [1] 0.0455

How does our finding compare to `R`?

``` r
chisq.test(survey, correct=FALSE)$p.value
```

    | [1] 0.0455

# Test of Independence with Paired Samples

When conducting a research, sometimes we are more inclined to see how the same
individual expresses different measurement, either by measuring in a different
time or using a different measurement scale. In other word, we have *same*
samples and variables, yet *different* measures, i.e.??a **paired sample**. By
mathematical design, Pearson???s Chi-square could not determine differences
happening overtime, as it only depicts proportional differences from a given
contingency table. As a solution, McNemar???s Chi-square provides a more
appropriate estimate of p-value on how the proportion changes by respecting its
temporal order.

We will consider the following scenario: Suppose we continue our market
research, where we ask **exactly same** subjects *three months* later. We
expected no changes in their preferences of chain-store outlets. It turned out,
regardless of their area of residence, 25 people who previously preferred go to
Indomaret now shop in Alfamart. Meanwhile, 20 people who used to visit Alfamart
now prefer Indomaret.

To capture changes overtime, we can formulate our contingency table as follow:

|           | Indomaret | Alfamart |
|-----------|----------:|---------:|
| Indomaret |        25 |       25 |
| Alfamart  |        20 |       30 |

Then, we will set our hypothesis as:

`\begin{align} H_0 &: \hat{p_{t_0}} = \hat{p_{t_1}} \\ H_1 &: \hat{p_{t_0}} \neq \hat{p_{t_1}} \end{align}`

To calculate the Chi-square statistics, McNemar???s proposed following equation:

`$$\chi^2 = \frac{(b-c)^2}{b+c}$$`

In `R`, we can have the p-value by issuing:

``` r
mcnemar.test(survey2)
```

    | 
    |   McNemar's Chi-squared test with continuity correction
    | 
    | data:  survey2
    | McNemar's chi-squared = 0.4, df = 1, p-value = 0.6

# Applying Yates??? Correction

Upon reading the section on exact test and its approximation, we may have
wondered why they provide different results? Is it only depends on the sample
size, or rather, will we have similar results when `\(n \to \infty\)`? The answer
is not quite straightforward, and this section will potentially add more
confusion to the question. When conducting statistical inference, we ought to
satisfy some assumptions on a particular test of our interest. In Fisher???s
test, we need to have a fixed marginal total. Looking at our dummy contingency
table, marginal total is simply the sum of all cells, i.e.??`\(a+b+c+d\)`. It is not
a stringent assumption per se, and I personally have not found a strong
evidence on which violation may affect the test robustness. However, as a rule
of thumb based on convenient convention, we only apply Fisher???s test when we
have a low sample count.

Now, we can ask a *different* question, how low can we consider our data before
applying Fisher???s test? Again, it is a rule of thumb, first we need to have our
contingency table ready for Pearson???s Chi-square test. When we calculate our
expected outcome and have any cell \< 5, Fisher???s is a more appropriate test to
conduct. However, in most cases, we may find our data as unsuitable for
Fisher???s exact test, yet we are not sure whether Pearson???s Chi-square will give
a good approximation.

In such a condition, we will have Yates??? correction to give a better estimate.
Yates??? method provides a better estimates and alleviates bias in a `\(2 \times 2\)`
contingency table. With a larger contingency table, we often do not require
Yates??? correction. If we recall Pearson???s Chi-square equation, we can apply
Yates??? correction in following fashion:

`$$\chi^2 = \displaystyle \sum_{i, j} \frac{(|O_{ij} - E_{ij}| - 0.5)^2}{E_{ij}} \\$$`

# Concluding Remakrs

-   Large sample (\> 10 in each cell) `\(\to\)` use approximation
-   Low sample `\(\to\)` use an exact test
-   `\(2 \times 2\)` contingency with approximation `\(\to\)` apply Yates??? correction
-   Low sample with `\(m \times n\)` contingency table `\(\to\)` split or do simulation
