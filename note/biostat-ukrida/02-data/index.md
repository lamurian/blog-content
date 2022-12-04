---
author: lam
title: "Data: Type and Distribution"
weight: 2
description: >
  To comprehend our surroundings, we rely on processed sensory inputs and infer
  the best action to respond with. In a more complex (or complicated)
  situation, we often find recorded inputs more reliable than our sole
  perception. Recorded inputs are data, which may or may not follow a certain
  structure. In essence, data is distinguishable based on their properties,
  resulting in different data types. With more collected data, a particular
  pattern emerges, where we can observe a peculiar behaviour from one pattern
  compared to the other. Here we discuss the nature of data, viz. its type
  and distribution.
summary: >
  To comprehend our surroundings, we rely on processed sensory inputs and infer
  the best action to respond with. In a more complex (or complicated)
  situation, we often find recorded inputs more reliable than our sole
  perception. Recorded inputs are data, which may or may not follow a certain
  structure. In essence, data is distinguishable based on their properties,
  resulting in different data types. With more collected data, a particular
  pattern emerges, where we can observe a peculiar behaviour from one pattern
  compared to the other. Here we discuss the nature of data, viz. its type
  and distribution.
date: 2020-09-15
slides: https://lamurian.rbind.io/note/biostat-ukrida/01-intro/slide
categories: ["statistics", "ukrida"]
tags: ["descriptive statistics", "R", "probability theory"]
slug: 02-data
csl: ../harvard.csl
bibliography: ../ref.bib
draft: false
---

[Slide](https://lamurian.rbind.io/note/biostat-ukrida/02-data/slide)

To comprehend our surroundings, we rely on processed sensory inputs and infer
the best action to respond with. In a more complex (or complicated) situation, we
often find recorded inputs more reliable than our sole perception. Recorded
inputs are data, which may or may not follow a certain structure. In essence,
data is distinguishable based on their properties, resulting in different data
types. With more collected data, a particular pattern emerges, where we can
observe a peculiar behaviour from one pattern compared to the other. Here we
discuss the nature of data, *viz.* its type and distribution.

# Type of Data

Exist are numerous conventions in describing data. However, we shall keep in
mind that understanding the nature behind categorical and numeric data is more
important. We may further divide categorical data into nominal and ordinal,
while numeric into discrete and continuous. Looking at continuous data, it is
also of interest to distinguish interval and ratio data.

## Categorical - Nominal

Nominal data are observable through its distinctive characteristics.
Differences seen in nominal data is not directly comparable, where it subsists
only to help identifying one object from another. The example of nominal data
includes genders, brand of a certain product, animal species, type of tea, and
the list goes further. It is important to notice that nominal data does not
only come in two classes, but also multiple classes.

<img src="https://www.incimages.com/uploaded_files/image/970x450/male-female-sign-1940x900_35330.jpg" width="100%">

## Categorical - Ordinal

The defining attribute of ordinal data is existing stratification among its
member, enable a direct comparison. There are numerous examples on ordinal
data, where spiciness level, education background, malignancy and severity are
to name but a few. As we shall imagine, we can directly compare spicy foods to
less spicy ones. We could not confidently say the same with nominal data, as we
cannot determine which brand is more than another. For instances, we cannot say
Anchor as being more butter compared to Wijsman, that does not make any sense.
But we can say cayenne peppers as being spicier compared to bird’s eye chili
peppers.

<img src="https://static.vecteezy.com/system/resources/previews/000/680/216/original/spicy-level-of-red-hot-pepper.jpg" width="100%">

## Numeric - Discrete

A discrete data is countable, indicating its availability to a subset of
arithmetical operations, including addition and subtraction. Any frequency
measure is a type of discrete numeric data. Previously, we have seen the
difference between ordinal and nominal data. However, any class from ordinal or
nominal is a subject to frequency measure. It means that we can count the
number of female students in a university, also the how many bird’s eye chilli
peppers we have in a box. In such a situation, both female student and a
particular type of chilli pepper still belongs categorical data, respectively.
But we shall see that the count is a discrete numeric data. So, in essence,
discrete data is the result of frequency counting. In other words,
**countable** defines the discrete numeric data. Other examples of discrete
data includes the number of apples in a store, jugs arranged in a shelf,
plastic bottle wastes on the shore, and so on.

<img src="https://nypost.com/wp-content/uploads/sites/2/2018/03/180315-water-bottles-feature-image.jpg?quality=90&strip=all&w=1200" width="100%">

## Numeric - Continuous

If being countable belongs to discrete data, then **measurable** is the main
characteristics of continuous numeric data. Continuous data assumes the
availability of its subjects to a certain measurement instrument. In measuring
an object, we may stumble upon a problem of whether we are looking at its
interval or the actual ratio. What is the difference?

<img src="https://livelaughloveandlose.files.wordpress.com/2016/05/weighing-scales.jpg" width="100%">

### Interval

Interval data has no absolute zero, but it presents with a **fixed distance**.
Some arithmetical operations are applicable, including addition and
subtraction. Examples of this data type includes temperature in scales other
than Kelvin. To make sense interval data, imagine it this way: you are
measuring water temperature using a thermometer. Suppose you have two cups of
water, one filled with hot and another with cold water. You measured both cups,
where the hot one is about 40<sup>o</sup>C, and the cold one is 20<sup>o</sup>C. We can say the
hot cup as being warmer, but we cannot state 40<sup>o</sup>C being twice as hot as
20<sup>o</sup>C because Celcius is a relative measure. We shall see how Kelvin differ in
that respect.

### Ratio

In ratio data, we do not assume a fixed distance, as the measure may take an
infinitesimal values. However, ratio has an absolute zero. Meaning that any
measurement of `\(x\)` has elements of positive real number `\(\mathbb{R}\)`, with zero being
the lower bound, or simply `\(x \in \mathbb{R}: x \geqslant 0\)`. Examples on ratio
data includes weight, volume and temperature in Kelvin. Kelvin approximates an
entropy within a system, meaning that it standardizes the amount of heat
dissipated or taken by a particular object. So in case we have two cups with
different temperature measured in Kelvin, we can argue one being a few times
hotter than the other.

# A Glimpse Into Probability

Being able to collect data means we can process them to produce an inference of
our surrounding. I may overuse (or even tend to abuse) the word “inference” in
this post, but please bear with it. We want to make sense of our world using
the data we have. For example, suppose we are ordering food which came with
cayenne chilli peppers. We knew from previous experience that out of ten times
consuming cayenne peppers, three resulted in having a diarrhoea. Now, we would
like to know the possibility of having diarrhoea if we proceed to eat this chilli
pepper. In essence, we are facing a probability problem, i.e. what is the
chance of having a diarrhoea after consuming cayenne chilli pepper?

In less sophisticated manner, we may regard probability as a relative
frequency. It means that we are estimating the parameter proportion using
statistical approach. If out of ten times we got three occurrences, we are
more inclined to say that our chance is roughly 30%. In measuring probability,
we need to know the event and its sample space. An event `\(E\)` is an expected result,
whereas the sample space `\(S\)` is all possible outcomes. Therefore, we can
calculate the probability of a particular event as follow:

`$$P(E=e) = \frac{E}{S}$$`

Now suppose we have a fair coin and we intend to flip it 10 times, where `H`
indicates the head and `T` the tail. In such a case, our sample space is:

``` r
set.seed(1)
S <- sample(c("H", "T"), 10, replace=TRUE, prob=rep(1/2, 2)) %T>% print()
```

    ##  [1] "T" "T" "H" "H" "T" "H" "H" "H" "H" "T"

Let the head be our expected outcomes, so we can list our event as:

``` r
E <- S[which(S == "H")] %T>% print()
```

    ## [1] "H" "H" "H" "H" "H" "H"

Thus, we can regard the probability of having a desired outcome as a relative
frequency of events in a given sample space:

``` r
length(E) / length(S)
```

    ## [1] 0.6

So, ten flips using a fair coin resulted in a
60% chance of having heads. The method we use so
far to determine the probability is an enumeration, i.e. listing all desired
events and dividing it to the sample space. However, higher sample space makes
it harder to solve, and it is more apparent with a sequential problem. In a
sequential problem, we have to calculate the probability from multiple
instances, e.g.: having two heads consecutively.

Other methods in estimating the probability is the tree diagram and resampling
method. The tree diagram is available to solve a more complex probability
problem. Suppose we have an urn evenly filled with identical balls, which only
difference is their colours. Let the color be red and blue, which our urn
contains 50 and 30 balls of each red and blue, respectively. If we take three
balls without replacement, how high is the chance of getting three blue balls?
This is the urn problem, a classic from statistics and probability theory. If
we were to build a tree diagram, it will look as follow (`B` stands for blue
and `R` for red):

                            / B (28/78)
                   B (29/79)
                 /          \ R (50/78)
        B (30/80)
       /         \
      /            R (50/79)
    80
      \
       \          
        R (50/80)

Please note it is an incomplete tree diagram. Of course, you may fill out the
rest, but we are only interested with the probability of having three blue
balls. If we take only the upper branches, representing three consecutive draws
of the blue ball, we get the probability of
0.0494. For this type of problem, tree diagram is
practically easier than listing all probable outcomes and desired events.

To understand the resampling method, we will conduct a quick experiment, where
we roll a dice numerous times. Since we do not physically own a dice –even
if we do, I would not suggest doing this experiment physically– we will make a
simple function to do the dice roll for us. I will call this function `dice`.
Very creative, huh? ;)

``` r
dice <- function(n) {
    sample(1:6, n, replace=TRUE, prob=rep(1/6, 6))
}
```

We want to know whether our function work:

``` r
dice(1)
```

    ## [1] 3

It does! Now, we can safely proceed and roll the dice ten times. Let 4 be our
outcome of interest. How high is the probability of having the event within 10
trials?

``` r
set.seed(1)
roll <- dice(10) %T>% print()
```

    ##  [1] 3 4 5 1 3 1 1 5 5 2

Turns out, the probability of getting 4 is
1/10. As we have a fair dice, why is
the probability not 1/6? The clue is a sample and the population. We only have
a subset of its population. Ideally, we will need to roll the dice `\(n\)` number
of time, with `\(n \to \infty\)`. The more sample we have, the closer it is to
represent the population. What will we get with different number of rolls?

``` r
roll <- c(10, 100, 1000, 10000, 100000, 1000000, 10000000)
prob <- sapply(roll, function(n) {
    set.seed(1); roll <- dice(n)
    sum(roll==4) / length(roll)
})
```

Theoretically, the error of estimated probability is inversely proportional to
the number of trial, where we can compute as:

`$$\epsilon = \sqrt{\frac{\hat{p} (1-\hat{p})}{N}},\ where:$$`

`\(\epsilon\)`: Error  
`\(\hat{p}\)`: Estimated probability (current trial)  
`\(N\)`: Number of resampling

To compute the error in our experiment, we will make a function as follow:

``` r
epsilon <- function(p.hat, n) {
    sqrt({p.hat * (1-p.hat)}/n)
}
```

``` r
df <- data.frame(list("roll"=roll, "prob"=prob))
```

``` r
df$error <- mapply(function(p.hat, n) {
    epsilon(p.hat, n)
}, p.hat=df$prob, n=df$roll) %T>% print()
```

    ## [1] 0.0948683 0.0433013 0.0126491 0.0037773 0.0011769 0.0003724 0.0001178

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.trial-1.png" width="100%" />

With more trials, we get closer to the expected probability in a fair dice,
which is 1/6, or equivalently 0.1667.

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.error-1.png" width="100%" />

Following previous theory, the error is decreasing proportionally to the number
of sample.

# Random Variables

We often heard, or at least will often hear, about independent and identically
distributed (I.I.D) random variables. I.I.D simply states all sampled random
variables should be independent from one another. Each sampling instance should
also follow an identical procedure. With I.I.D, we can do a better probability
approximation using:

-   Probability Mass Function (PMF) in discrete variable
-   Probability Density Function (PDF) in continuous variable

The function explained in this section is arbitrary, where we will
take a deeper look on most used ones. Mathematically, we can have any
probability function as long as it satisfies following rules:

`\begin{align} P(E=e) &= f(e) > 0: E \in S \tag{1} \\ \displaystyle \sum_{e \in S} f(e) &= 1 \tag{2} \\ P(E \in A) &= \displaystyle \sum_{e \in A} f(x) \tag{3}: A \subset S \end{align}`

# Discrete Distribution

In previous subsection explaining probability theory, we have done coin
flipping to see how probability works. Coin flipping is a situation where we
expect two possible outcomes, in which we desire a certain event to happen. In
essence, we have conducted a Bernoulli trial, which is:

-   An experiment with only two outcomes
-   All trials being independent from each other
-   All instances have an identical probability

We shall see some probability functions describing series of Bernoulli event.

## Binomial Distribution

Binomial is a type of discrete distribution with an identical iteration over
`\(n\)` times of trial. Each iteration is an independent instances corresponding to
a Bernoulli trial. Binomial distribution uses the following PMF, which explain
the probability of having desired outcome (an event) within a certain amount of
time given `\(n\)` times of trial.

`\begin{align} f(x) &= \binom{n}{x} p^x (1-p)^{n-x} \tag{1} \\ \binom{n}{x} &= \frac{n!}{x! (n-x)!} \end{align}`

Or simply denoted as: `\(X \sim B(n, p)\)`

Knowing the PMF and its parameters, we can calculate the descriptive
statistics. From this point onward, this post will only consider explaining
mean and standard deviation. We will find their use in comprehending the
Central Limit Theorem (CLT).

`\begin{align} \mu &= n \cdot p \\ \sigma &= \sqrt{\mu \cdot (1-p)} \end{align}`

By changing the parameters, we can see the behaviour of binomial distribution
as depicted by following figures:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.binom.n-1.png" width="100%" />

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.binom.p-1.png" width="100%" />

## Geometric Distribution

Suppose we are conducting Bernoulli trials, then we are interested to see the
number of trial to conduct before getting an event. In other words, we are
looking for a way to calculate the probability of having a particular event
after `\(k\)`-th number of failures. Geometric distribution is a derivation of
binomial distribution, where we use `\(x=1\)` in `\(n\)` number of trials. So we can
formulate geometric distribution as:

`$$f(n) = P(X=n) = p (1-p)^{n-1}, with:$$`

`\(n\)`: Number of trials to get an event  
`\(p\)`: The probability of getting an event  
Or simply denoted as `\(X \sim G(p)\)`

`\begin{align} \mu &= \frac{1}{p} \\ \sigma &= \sqrt{\frac{1-p}{p^2}} \end{align}`

Using different value of `\(p\)` as its parameter, we can obtain following
probability distribution:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.geom-1.png" width="100%" />

## Poisson Distribution

Suppose we know the rate of certain outcomes, then Poisson distribution may
define the probability of an outcome happening `\(x\)` times. The time unit in
Poisson distribution is discrete, as we only limit our observation within a
particular time frame (observation period). Poisson distribution uses following
PMF:

`$$f(x) = \frac{e^{-\lambda}\lambda^x}{x!},\ with:$$`

`\(x\)`: The number of expected events  
`\(e\)`: Euler’s number  
`\(\lambda\)`: Average number of events in one time frame

Or simply denoted as `\(X \sim P(\lambda)\)`

`\begin{align} \mu &= \lambda \\ \sigma &= \sqrt{\lambda} \end{align}`

Altering the `\(\lambda\)` in its parameter, we can see how Poisson distribution
behave:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.pois-1.png" width="100%" />

# Continuous Distribution

So far, we have seen three commonly used distributions in explaining discrete
variables, which often came from a Bernoulli trial. Understanding the essence
behind probability distribution has tempted us to find a plausible explanation
on continuous distributions. However, since we consider continuity, it makes no
sense to use PMF, because PMF defines a probability in a given discrete
measure. We will look into a probability density function (PDF) and use
histogram with density plot to depict probabilities in continuous measure.

## Uniform Distribution

As its name suggests, the uniform distribution is a type of continuous
distribution which describe (drum roll intensifies) uniform probabilities. We
may not find its potential uses in common statistical procedure, but the
uniform distribution is a capable tool to generate random numbers. This is
useful when you need a properly randomized situation. For example, when you
randomly assign patient groups in a clinical trials.

`$$f(x) = \frac{1}{b-a}$$`

Or simply denoted as `\(X \sim U(a,b)\)`

`\begin{align} \mu &= \frac{b+a}{2} \\ \sigma &= \frac{(b-a)^2}{12} \end{align}`

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.unif-1.png" width="100%" />

## Exponential Distribution

Previously, we used Poisson distribution to find the probability of having `\(n\)`
number of events given a particular occurrence rate. If we are interested in
finding the minimum required time to observe an event, we shall re-parameterize
Poisson distribution. Exponential PDF calculate the probability of spending a
certain amount of time to observe an event in a Poisson process. Please be
advised though, as it is not always about the time. Time frame is an intangible
definition, as it could be other continuous measures in a Poisson process such
as mileage, weight, volume, etc.

`$$f(x) = \lambda e^{-x \lambda}, with:$$`

`\(x\)`: Time needed to observe an event  
`\(\lambda\)`: The rate for a certain event

Or simply denoted as `\(X \sim Exponential(\lambda)\)`

`$$\mu = \sigma = \frac{1}{\lambda}$$`

Changing the `\(\lambda\)` parameters may result in following figure:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.exp-1.png" width="100%" />

## Gamma Distribution

Exponential distribution is a good approximate to surmise the probability on
spending a particular amount of time for **one** event to occur. However, when
we are interested to find the probability over **numerous** amount of events,
then Gamma PDF can model a more suitable distribution. Gamma PDF only differs
to the exponential PDF with respect to its shape parameter. When we set the
shape `\(\alpha=1\)` in Gamma PDF, we can get the exponential PDF.

`\begin{align} f(x) &= \frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-x \beta} \\ \Gamma(\alpha) &= \displaystyle \int_0^\infty y^{\alpha -1} e^{-y}\ dy,\ with: \end{align}`

`\(\beta\)`: Rate ( `\(\lambda\)` in exponential PDF)  
`\(\alpha\)`: Shape  
`\(\Gamma\)`: Gamma function  
`\(e\)`: Euler number

Or simply denoted as `\(X \sim \Gamma(\alpha, \beta)\)`

`\begin{align} \mu &= \frac{\alpha}{\beta} \\ \sigma &= \frac{\sqrt{\alpha}}{\beta} \end{align}`

In exponential distribution, we observed how the distribution changes over
different `\(\lambda\)` values. Since the effect of rate `\(\beta\)` parameter is
somewhat similar in Gamma PDF, we will see how the distribution looks over
different values of shape `\(\alpha\)` parameter.

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.gamma-1.png" width="100%" />

## `\(\chi^2\)` Distributions

As special cases of Gamma distribution, `\(\chi^2\)` PDF defines a family of
distributions widely used in statistical inferences. This post will not go in a
full length to describe the `\(\chi^2\)` distributions. Instead, we shall take a
look at the general form and later see its application on upcoming lectures.

`$$f(x) = \frac{1}{\Gamma (k/2) 2^{k/2}} x^{k/2 - 1} e^{-x/2},\ with:$$`

`\(k\)`: Degree of freedom  
The rest are Gamma PDF derivations

Or simply denoted as `\(X \sim \chi^2(k)\)`

`\begin{align} \mu &= k \\ \sigma &= \sqrt{2k} \end{align}`

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.chisq-1.png" width="100%" />

## Normal Distribution

Normal PDF is arguably the most ubiquitous probability function in modelling real-world
data. It is a symmetric distribution where `\(\mu\)` and `\(\sigma\)` completely
describe the probability.

`$$f(x) = \frac{1}{\sigma \sqrt{2\pi}}exp \bigg\{ -\frac12 \bigg( \frac{x-\mu}{\sigma} \bigg)^2 \bigg\},\ with:$$`

`\(x \in \mathbb{R}: -\infty < x < \infty\)`  
`\(\mu \in \mathbb{R}: -\infty < \mu < \infty\)`  
`\(\sigma \in \mathbb{R}: 0 < \sigma < \infty\)`

Or simply denoted as `\(X \sim N(\mu, \sigma)\)`

By alternating the parameter `\(\mu\)`, we may observe following instances:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.norm.mu-1.png" width="100%" />

While changing the parameter `\(\sigma\)` will result as follow:

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.norm.sigma-1.png" width="100%" />

# Goodness of Fit Test

To determine whether our data follow a particular distribution, we ought to
conduct a statistical analysis called goodness of fit test. Numerous methods
exist, but we will dig into more popular ones. Given correct parameters, some
methods can fully describe the data. Since we consider doing a statistical
analysis, it is always a good practice to clearly state the hypothesis:

-   `\(H_0\)`: Given data follow a certain distribution
-   `\(H_1\)`: Given data does not follow a certain distribution

## Binomial Test

As a goodness of fit measure, binomial test is an adaptation of binomial PMF.
It determines whether acquired relative frequency is following after Bernoulli trial’s
probability. First we need to compute the probability using given parameters:

`$$Pr(X=k) = \binom{n}{k}p^k (1-p)^{n-k}$$`

Remember we previously tossed a coin 10 times? We will use that example to
perform a binomial test.

``` r
set.seed(1)
S <- sample(c("H", "T"), 10, replace=TRUE, prob=rep(1/2, 2)) %T>% print()
```

    ##  [1] "T" "T" "H" "H" "T" "H" "H" "H" "H" "T"

``` r
length(E) / length(S)
```

    ## [1] 0.6

If it corresponds with a Bernoulli trial, it should satisfy `\(P(X=6)\)` in such a way
that we cannot reject the `\(H_0\)` when calculating its probability:

`$$P(X=6) = \binom{10}{6}0.5^6(1-0.5)^4$$`

Luckily, we do not need to compute it by hand (yet). We can use following
function in `R`.

``` r
binom.test(x=6, n=10, p=0.5)
```

    ## 
    ##  Exact binomial test
    ## 
    ## data:  6 and 10
    ## number of successes = 6, number of trials = 10, p-value = 0.8
    ## alternative hypothesis: true probability of success is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.2624 0.8784
    ## sample estimates:
    ## probability of success 
    ##                    0.6

Interpreting the p-value, we cannot reject the `\(H_0\)`, so our coin toss followed
a Bernoulli trial’s probability after all.

## Kolmogorov-Smirnov Test

Often referred as KS test, it is available to determine various distributions
using a non-parametric method. Power-wise, it is pretty much robust and
comparable to the Anderson-Darling test and Shapiro-Wilk on normal
distribution. In following example, we will demonstrate how to conduct a KS
test over an exponential distribution.

Let `\(X \sim Exponential(2): n = 100\)`

``` r
set.seed(1)
X <- rexp(n=100, rate=2)
```

By imputing `\(\lambda\)` variable, Kolmogorov-Smirnov can compute its goodness of fit

``` r
ks.test(X, pexp, rate=2)
```

    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  X
    ## D = 0.084, p-value = 0.5
    ## alternative hypothesis: two-sided

With p-value \> 0.05, we can confidently keep the `\(H_0\)`, that our data follow a
certain distribution. In this case, `\(X \sim Exponential(2)\)`.

## Visual Examination

Conducting a numerical analysis gives us various advantage when reporting our
results. However, in a large sample, even a small deviation will result in
`\(H_0\)` rejection. Which mean, previously mentioned tests are of no use! Still,
we can rely on some visual cues to determine the distribution. For this
demonstration we will again use the previous object of `\(X\)`.

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.gof.hist-1.png" width="100%" />

That is a good start! Sadly, this does not give a clear indication on which
distribution it follows.

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.gof.qq-1.png" width="100%" />

Quantile-Quantile Plot (QQ Plot) can give a better visual cue :)

# Test of Normality

Normality test is practically a subset of goodness of fit test, as it only
describes the normal distribution. Some tests are more appropriate under
certain circumstances. Following sections will look at widely used normality
tests in statistics. As always, we will begin by stating the hypothesis:

-   `\(H_0\)`: Sample follows the normal distribution
-   `\(H_0\)`: Sample does not follow the normal distribution

For demonstration purpose, we will populate normally-distributed data into an
object called `\(X\)`, where we let `\(X \sim N(0, 1): n=100\)`.

``` r
set.seed(1)
X <- rnorm(n=100, mean=0, sd=1)
```

## Shapiro-Wilk Test

Shapiro-Wilk is a well-established normality test to assess (*ahem*) normality.
It can tolerate skewness to a certain degree. Its implementation in `R` has a
constraint, where the sample size of `\(n\)` should be $3 \leqslant n \leqslant 5000$.

``` r
shapiro.test(X)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  X
    ## W = 1, p-value = 1

## Anderson-Darling Test

Compared to Shapiro-Wilk and Kolmogorov-Smirnov, Anderson-Darling test is less
well known. However, we ought to know this test because Anderson-Darling gives
more weight to the tails when testing for normality. It means, some data with
higher frequencies on its tails, may receive better justification using
Anderson-Darling test. Anderson-Darling implementation in `R` requires the
package `nortest`, and it needs at minimum a sample size `\(n\)` of 7.

``` r
nortest::ad.test(X)
```

    ## 
    ##  Anderson-Darling normality test
    ## 
    ## data:  X
    ## A = 0.16, p-value = 0.9

## Visual Examination

<img src="{{< blogdown/postref >}}index_files/figure-html/nortest.demo4-1.png" width="100%" />

## `\(\chi^2\)` and Normal Distribution

We previously mentioned `\(\chi^2\)` distributions as special cases of Gamma
distribution. It turned out, if we raise a normally-distributed data to the
power of 2, it behaves as a `\(\chi^2\)` distribution with 1 degree of freedom.
The following example on `\(X \sim N(0,1): n=100\)` will demonstrate how $X^2 \sim \chi^2(1)$. We previously tested `\(X\)` against Shapiro-Wilk and Anderson-Darling
test to indicate normality.

``` r
set.seed(1)
X <- rnorm(n=100, mean=0, sd=1)
```

First, we raised `\(X\)` to the power of two.

``` r
X2 <- X^2
```

``` r
shapiro.test(X2)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  X2
    ## W = 0.7, p-value = 5e-13

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.chisq.demo1-1.png" width="100%" />

Shapiro-Wilk and visual examination suggested `\(X^2\)` does not follow a normal
distribution. But, does it follow a `\(\chi^2\)` distribution?

``` r
ks.test(X2, pchisq, df=1)
```

    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  X2
    ## D = 0.1, p-value = 0.2
    ## alternative hypothesis: two-sided

<img src="{{< blogdown/postref >}}index_files/figure-html/plt.chisq.demo2-1.png" width="100%" />

So we can safely conclude that raising a normal distribution to the power of
two resulted in a `\(\chi^2\)` distribution with a parameter of `\(k=1\)`.

# Central Limit Theorem

So far, we have learnt sampling distributions, where we see different
implementation on probability functions in discrete and continuous variables.
We are also able to compute the mean and standard deviation based on their
parameters. It just happened that the sample mean follows a normal
distribution ;) Central Limit Theorem (CLT) delineates such an occurrence,
applicable to both discrete and continuous distributions.

`$$\bar{X} \xrightarrow{d} N \bigg(\mu, \frac{\sigma}{\sqrt{n}} \bigg) as\ n \to \infty$$`

The equation above basically tells us that the sample mean distribution
`\(\bar{X}\)` have a convergence upon random variables to follow a normal
distribution with mean of `\(\mu\)` and standard deviation of `\(\frac{\sigma^2}{n}\)`.
Each `\(\mu\)` and `\(\sigma\)` corresponds to computed values acquired from the
sampled distribution.

The caveat (or the perk) with CLT is we need a sufficiently large sample number
of `\(n\)`. The number of sample depends on data skewness, where more skewed data
requires a higher value of `\(n\)`. We can determine `\(n\)` using a simple simulation,
using following steps:

1.  Choose any distribution
2.  Generate `\(n\)` random numbers using specified parameters
3.  Compute the mean and variance based on previous parameters `\(\to\)` Use it to generate a normal distribution
4.  Reuse the parameters to re-iterate step 2
5.  Conduct the simulation for an arbitrary number of times (e.g. for convenience, 1000)
6.  Calculate mean from all generated data `\(\to\)` Make a histogram and compare it with step 3
7.  Does not fit normal distribution? `\(\to\)` Increase `\(n\)`

Why should we care about CLT if we can do statistical inferences without it? In
a research settings, we may find a differing average values despite following
the exact procedure. It could be frustrating! Knowing CLT, we can prove the
difference is indeed within expectation. As a final excerpt, CLT describes a
tendency of a mean to follow normal distribution, but it requires a sufficient
number of `\(n\)`.
