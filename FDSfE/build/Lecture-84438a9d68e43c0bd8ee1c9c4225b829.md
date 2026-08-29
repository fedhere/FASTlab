# Learning Questions

-   What is a distribution?

-   What is a 'moment' of a distribution?

-   What can the quantiles of a distribution tell us?

-   What is the Law of Large Numbers?

-   What is the Central Limit Theorem?

# Introduction

Data in the real world (, data not generated for the purpose of
learning) emerge as observations drawn from underlying *distributions.*
Lesson 2 introduced probability as a framework for mathematical
reasoning with random processes, and this lesson will teach the
essential machinery of statistical distributions. Understanding these
distributions, and their parameters and properties is necessary for most
analyses.

We will discuss two important distributions: the Poisson and the
Gaussian. The former is defined for discrete data, while the latter is
defined for continuous data. I present these two distributions
*together* because, in a sense, they are cousins. In certain regimes,
one can approximate the other. I present these two distributions *first*
because they enable us to understand the two most important ideas in
statistics: The **Law of Large Numbers** and **The Central Limit
Theorem.**

# The Poisson Distribution

The best way to form an intuitive understanding of any distribution is
to examine an example that resonates with you. Since I don't know who
*you* are, I will choose an example that resonates with me.

Consider a leaky faucet. Every so often, a drop of water will form and
drip from the faucet. As a statistician, you measure the rate of
dripping over a long time and find that an average of two drops form
every minute. However, you note that the number of drops per minute is a
random variable; sometimes there are more drops, and sometimes there are
fewer. You then might wonder: what is the probability that 4 drops form
in one minute? The Poisson distribution comes to your rescue!

The Poisson distribution models the number of events occurring in a
fixed interval of time (or space), given that events occur independently
at a constant average rate (a *stationary process*). We can define its
**probability mass function** (PMF) as:

$$P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}, \quad k = 0, 1, 2, \ldots$$

The random variable --- the *event* --- is $X$ and represents the number
of drops that occur in a particular minute. We don't know what $X$ is,
but we know that it is distributed according to a Poisson distribution
with an average value, $\lambda$, of 2. The values that any distribution
is defined on ( all the possible values that the phenomenon could take)
are known as the **support**, and in the case of the Poisson
distribution, we like to call it $k$. Since in this case $k$ measures a
number of events, it cannot take negative or fractional values: the
support of a Poisson distribution is non-negative integers, or
$\mathbb{N}_0$.

With this meagre amount of knowledge of distributions, we can already
answer our question about the leaky faucet. We know that
$\lambda=2 \text{ drops per minute}$, and we want to know the
probability of $k=4 \text{ drops}$ forming in one minute. Using the PMF:
$$P(X=4) = \frac{2^4 e^{-2}}{4!} = 0.09$$

There's a $\approx9\%$ chance of observing four drops in one minute from
the leaky faucet. Wow! We can formalize the answer to our question with
the statement: "The probability of measuring $k$ drops in the time
interval is $P(X=k)$." We can even state it more generally: "The
probability that the random variable $X$ takes on the value $k$ is
$P(k)$."

The Poisson distribution is a *discrete* distribution; notice how $k$
can only be zero or a positive integer. You can only have a discrete
number of drops after all. What if you had a random variable that takes
on any real value?

![A plot of the probability mass functions for the Poisson distribution
at three different values of $\lambda$. Image credit: Skbkekas
(Wikipedia)](../figures_pedagogy/Poisson_pmf.svg.png)

# The Gaussian Distribution

Consider a class of 100 students who take a test. To be accurate with
our example, we must also imagine that the score of the test can take
any real value between $0\%$ and $100\%$. The average result of their
scores turns out to be $85\%$. Nice! Assuming that the scores are
distributed according to a Gaussian (or Normal) distribution, what is
the probability that a student scored between $80\%$ and $90\%$?

The Gaussian distribution is defined on a continuous support: all real
numbers. Its **probability density function** (PDF) is:

$$f(x;\mu, \sigma) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{1}{2}\left(\frac{x - \mu}{\sigma}\right)^2}, \quad x \in \mathbb{R}$$

Where a Poisson distribution was defined by one number, the mean
$\lambda$, the Gaussian is defined by two numbers: the mean $\mu$ and
the standard deviation $\sigma$. A mathematical introduction to the
standard deviation of a distribution will come in the next section, but
for now, it is enough to say that the standard deviation defines how
likely you are to measure the random variable far away from the mean. A
small standard deviation means that the random variable is likely to be
measured close to the mean, while a large standard deviation implies the
opposite. Our class had a mean of $\mu = 85\%$ and a standard deviation
of $\sigma = 5\%$. That means that not only did the students do well on
average, but also the students mostly scored around $85\%$.

![Two examples of a Gaussian probability distribution function with
different means and standard
deviations.](../figures_pedagogy/gaussian_pdf.pdf)

There are a few things to note about the Gaussian distribution and our
particular choice of example. First, our example has limited support.
That is, $x$ can only take on values between $0\%$ and $100\%$. Gaussian
distributions are defined on an infinite support ( $x$ can be any value
between negative infinity and infinity; the mathematical way to say that
is $x \in \mathbb{R}$), so technically what we have here is called a
**truncated normal distribution.** For the purposes of this example,
though, the differences aren't relevant (especially because with a small
$\sigma$ the probability of getting values $<0\%$ or $>100\%$ is very
low in our example, $<0.3\%$) .

Also note how we asked different types of questions in the leaky faucet
example and the test example. In the former, we asked, "What is the
probability of observing *exactly* four drops in one minute?" And in the
latter we asked, "What is the probability that a student scored
*between* an $80\%$ and a $90\%$?" For a discrete distribution, its
probability *mass* function (PMF) $P(k)$ gives the direct probability of
observing exactly $k$.

But if the support is continuous, as in the case of a Gaussian
distribution, there are infinitely many values the function could take
in any given interval of that support. So we can only define the
probability $P(X \pm dx)$ of a continuous distribution as the
probability the variable will take a value within a range $dx$, which
can be as small or large as we want. We can, for example (and this will
be important in the next lecture), ask what is $P(X \geq 0.05)$ or
$P(X \leq 0.05)$.

It's also important to discuss special cases of the Gaussian/Normal
distribution where $\mu=0$ and $\sigma=1$. We give this distribution a
specific name: the **Standard** Normal Distribution, and a specific
symbol for its PMF: $P_\mathcal{N}$. In this special case,
$$P_\mathcal{N}(X \geq -1)  + P_\mathcal{N}(X \leq 1) = 68.2\%$$ and
$$P_\mathcal{N}(X \geq -2) + P_\mathcal{N}(X \leq 2) = 95.4\%$$ In the
next section we will see why, and in the next lecture, we will leverage
this fact to test a hypotheses.

# Moments of a Distribution

Let's define something called a **moment** of some function, $f(x)$.

$$\mu_n = \int^{\infty}_{-\infty} (x-c)^n\ f(x)\ dx$$

where $f(x)$ is the function we are taking a moment of and $n$ is the
ordinality of the moment ( first moment, second moment, third moment,
etc.). In this instance, we would say "$\mu_n$ is the $n$-th moment of
$f(x)$ *with respect to* $c$." When $c=0$, the moment is said to be a
**raw moment.** The first raw moment of a distribution is the mean!

$$\mu \equiv \mu_1 = \int^{\infty}_{-\infty} x\ f(x)\ dx$$

When $c=\mu$ the moment is said to be a **central moment.** The first
central moment is 0 by definition (it's not immediately obvious, but
plugging in $c=\mu$ and $n=1$ to the moment equation and then
simplifying will eventually yield 0), but the second central moment is
something we call the **variance.** And it turns out that the variance
is the square of what we called the standard deviation, $\sigma$. Isn't
that neat!

$$\sigma^2 \equiv \int^{\infty}_{-\infty} (x-\mu)^2\ f(x)\ dx$$

The variance (and by extension the standard deviation) quantifies the
*spread* of the distribution, as we discussed before. There are two more
moments worth discussing, the third and the fourth central moments.

The third central moment is called **skewness** and is a measure of the
asymmetry of the distribution. The Gaussian is symmetric and thus it has
0 skewness. I encourage you to substitute the Gaussian into the moment
equation with $c=\mu$ and $n=3$ to see this for yourself.

The fourth central moment is called **kurtosis** and is a measure of the
*tailedness* of the distribution. A distribution with positive kurtosis
will appear to "lean" to the right, and a distribution with negative
kurtosis will appear to "lean" to the left. Once again, you will find
that the kurtosis of a Gaussian is 0. Overall, we can say that the
moments of a distribution quantify its shape.

# Quantiles and the Empirical Rule

Quantiles divide a distribution into equal-sized intervals. For the
Gaussian distribution, quantiles corresponding to integer multiples of
the standard deviation are particularly important.
[1.4](#fig:stddevs)
shows a standard normal distribution where each standard deviation from
the mean is marked. The area under the curve in each of the regions is
written above.

![A standard normal distribution ($\mu=0$, $\sigma=1$). Each standard
deviation is marked with vertical dashed lines and the percentage of the
area under the curve in each region is
annotated.](../figures_pedagogy/stddevs.pdf)

Remember our test example? We said that $\mu=85\%$ and $\sigma=5\%$.
Using this chart --- *and assuming that the test scores are indeed
distributed according to a Gaussian* --- we can say that $\approx 68\%$
of students will have scored within one standard deviation of the mean
(between $80\%$ and $90\%$) and $\approx 95\%$ of students will have
scored within two standard deviations (between $75\%$ and $95\%$).

**NB:** This concept will become exceptionally relevant in the next
lecture on Null Hypothesis Rejection Testing.

# The Law of Large Numbers

Let's first define some terms: in statistics, we refer to a finite group
of examples of a given phenomenon that you can collect data on ( a
finite number of people you have access to for which you can measure,
say, the height) as a **sample**. The entirety of the instances of that
phenomenon in existence (the heights of everyone in the world, in this
case) is a **population**.

Imagine you were trying to measure the average human height. At first
you might have access to only two friends: your **sample size,** $n$,
would be $2$ and your **sample mean,** $\bar{X}_n$ would be fairly
inaccurate as a representation of the average human height since it only
includes two samples. If you expanded your sample size then your sample
mean would become more accurate --- it would become closer to the *true*
average human height. We would call this true average human height the
**population mean,** $\mu$.

**The Law of Large Numbers** states that as the sample size increases,
the sample mean converges to the population mean. This convergence
explains the intuitive knowledge that larger samples will yield more
stable estimates.

# The Central Limit Theorem

The Central Limit Theorem (CLT) is arguably the most important result in
introductory statistics. It's important to internalize --- this will
come with time and practice --- but it's most important for its
implication for scientific measurements. To understand the CLT, consider
the following scenario. Let the set $\{X_1, X_2, ..., X_N\}$ be a random
sample of size $N$ from some population. And let the distribution (it
can be any[^3] distribution) of that population have mean $\mu$ and
variance $\sigma^2$. Let the mean of the sample be $\bar{X}_N$. The CLT
then states that $\bar{X}_N$ is a random variable described by a
Gaussian distribution with mean $\mu$ and variance $\sigma^2/N$.

**Read that again.** It's ok, I'll wait.

The implications of this should not be immediately obvious to you, but
consider this scenario: a scientist is attempting to measure some value.
It doesn't matter what the value is; maybe it's the average rate of the
dripping faucet from before. If the scientist were to take $N$ different
measurements of the average rate of the dripping faucet (this would be
$\{X_1, X_2, ..., X_N\}$ representing the number of drops that fell in
time intervals $\{t_1, t_2, ..., t_N\}$), then the CLT tells the
scientist the average value of those measurements will be distributed
according to a Gaussian. And not just any Gaussian! A Gaussian whose
mean *is* the true value of the rate of the dripping faucet. Not to
mention, the CLT also tells us about the variance --- the shape --- of
the underlying distribution.

Measuring the rate of a dripping faucet may not be very interesting, but
many sciences boil down to the measurement of variables. The CLT gives
us the confidence that we can measure these variables and place limits
on how well we know them.

[^3]: It cannot be *any* distribution but you'll learn the exceptions as
    they come up.