# Learning Questions

-   What does it mean to 'explore' data?

-   What is correlation?

-   What is probability?

-   How can `pandas` help to inspect data?

# Introduction

Effective decision-making requires data. Before any sophisticated
modeling or inference can begin, you must thoroughly understand your
data. Thus, before we begin a project in earnest, we must undertake an
adventure into our dataset! We must examine the variables, understand
their distributions and relationships and visualize the data. This is
**data exploration**.

Viewing raw data directly, such as examining the raw content of a `.csv`
file, can be illuminating and should be one of your very first steps of
data exploration. The data may be corrupted or have missing segments,
and sometimes this can be easy to see in raw form. However, viewing raw
data can hardly be more informative than a cursory glance. We must
*inspect*, *investigate*, *explore* the data; we must become friends
with the data!

Our first inspection tools will be scatter plots and Pearson's
correlation coefficient. Python's `pandas` package will help us greatly
in this endeavor.

# Pearson's Correlation Coefficient

Pearson's correlation coefficient, denoted as $r_{xy}$, measures the
strength and direction of a *linear* relationship between two variables.
It ranges from -1 to +1:

-   $r_{xy} > 0$ indicates positive correlation (as one variable
    increases, the other tends to increase)

-   $r_{xy} < 0$ indicates negative correlation (or anti-correlation)

-   $r_{xy} = 0$ indicates no linear relationship

Take care to notice that $r_{xy} = 1$ means the data are identical,
except for a possible constant offset. The coefficient is defined as:

$$r_{xy} = \frac{1}{n-1} \sum_{i=1}^{n} \left( \frac{x_i - \bar{x}}{s_x} \right) \left( \frac{y_i - \bar{y}}{s_y} \right)$$

where $\bar{x}$ and $\bar{y}$ are the sample means, $s_x$ and $s_y$ are
the sample standard deviations, and $n$ is the number of data points.

A common pitfall is treating $r_{xy} = 0$ as evidence of no correlation
whatsoever when, in fact, it only means there is no *linear*
correlation. A parabolic or sinusoidal relationship obviously has
correlation ( the x and y variables are clearly related) but
$r_{xy} = 0$. **Pearson's coefficient detects only linear
relationships**. The best thing you can do to avoid missing obvious
correlations in your data is to visualize it with a scatter plot like in
[1.1](#fig:pedagogy:linear_corr).

![Some standard and some curious examples of measuring linear
correlation between the horizontal and vertical
axes.](../figures_pedagogy/linear_corr.png)

[1.1](#fig:pedagogy:linear_corr) demonstrates the limitations of
the Pearson's correlation coefficient. The top row and center row show
data with the same $r_{xy}$, but note how the noisier data in the top
row can have identical $r_{xy}$ but look very different. The bottom row
shows data with obvious correlation. That is, each data point is clearly
related to each other. However, these datasets all have $r_{xy} = 0$

# Probability

Probability admits two primary interpretations, each with distinct
philosophical and practical implications. **Frequentist probability**
defines the probability of an event as its long-run relative frequency
of occurrence:

$$P(E) = \lim_{n \to \infty} \frac{\text{number of times E occurs}}{n}$$

Under this view, after 101 coin flips yielding 51 heads,
$P(\text{heads}) = 51/101 \approx 0.505$.

**Bayesian probability** treats probability as a degree of belief that
can incorporate prior information. A Bayesian statistician who observes
ten heads in a row but holds a strong prior belief that the coin is fair
will still expect approximately $50\%$ heads in the long run.
Conversely, if you were in the streets of Vegas and someone invited you
to gamble on the outcome of a coin flip *with their own coin*, you would
not be likely to accept the bet, even if they flipped the coin a couple
of times and got heads and tails once each. This flexibility makes
Bayesian methods particularly powerful when data are scarce or when
prior knowledge exists (for example, from theory, or from historical
data about the same phenomenon).

For most introductory data exploration tasks, the frequentist
interpretation suffices. However, many popular machine learning methods
(Gaussian processes, MCMC, Bayesian neural networks) explicitly rely on
Bayesian reasoning.

# Probability Arithmetic

Probability obeys several fundamental rules. *The* most fundamental rule
being: probability is always represented by a number between 0 and 1
(which can alternately be represented by a number between $0\%$ and
$100\%$). Furthermore, statisticians like to consider the concept of an
'event,' and we like to give these events simple labels like 'Event $A$'
and 'Event $B$.' Event $A$ may represent a coin landing heads up, and
Event $B$ may represent a dice landing on a six. Mathematically, we
represent the probability of such an event like so: $P(A)$ or $P(B)$.
With this notation, we can now represent our first rule of probability
like so: $0 \leq P(A) \leq 1$.

If Event $A$ is a coin landing heads up, we may also consider Event
$\bar{A}$ where the coin lands tails up. $\bar{A}$ is said to be the
*complement* of $A$ and ― assuming the coin cannot land in any other
state besides heads up or tails up ― $P(A) + P(\bar{A}) = 1$.

## Independent (Disjoint) Events

When two events are independent --- the two events do not influence each
other whatsoever ― there exists a relationship between the probabilities
of the two events. The mathematical way to state this is:
$P(A) \cap P(B) = 0$ (where $\cap$ means 'intersection'). The
probability of $A$ *or* $B$ occurring, $P (A \cup B)$ (where $\cup$
means 'union'), is the sum of their probabilities. The probability of
both $A$ *and* $B$ occurring, $P (A \cap B)$, is the product of their
probabilities. The **conditional probability** of $A$ given $B$,
$P(A|B)$, is the probability of Event $A$ happening given that Event $B$
has happened. For disjoint events, this is equal to $P(A)$ since $A$ and
$B$ are independent.

The arithmetic rules for events with independent probabilities are:

$$\begin{aligned}
    \textrm{If }P(A) \cap P(B) = 0 \textrm{ then:}\\
    P(A \cup B) &= P(A) + P(B) \\
    P(A \cap B) &= P(A) * P(B) \\
    P(A | B) &= P(A)
\end{aligned}$$

## Dependent Events

When events are dependent upon each other in some way, the preceding
rules no longer hold, and new relationships emerge. We may state a
dependent relationship like so: $P(A) \cap P(B) > 0$. Consider the
conditional probability $P(A|B)$ again. This quantity will now always be
less than $P(A)$. Two more, less intuitive relationships emerge, which
are stated below.

$$\begin{aligned}
    \textrm{If }P(A) \cap P(B) > 0 \textrm{ then:}\\
    P(A | B) &< P(A) \\
    P(A | B) &= \frac{P(A \cap B)}{P(B)} \\
    P(A \cap B) &= P(A) P(B|A)
\end{aligned}$$

# Practical Data Exploration with Pandas

In Python, the `pandas` library provides the `DataFrame` object, the
workhorse for tabular data exploration. We will use the function
`pd.read_csv` to ingest a `.csv` file and load it as a `DataFrame`
object. The `DataFrame` object, often called `df`, has many attributes
and methods that are valuable for data exploration.

-   `df.shape`: Returns (rows, columns). **Knowing the dimensionality of
    your data is paramount.**

-   `df.head(n)` and `df.tail(n)`: Display first and last `n` rows. This
    can reveal data entry errors, inconsistencies, or unexpected
    formatting.

-   `df.columns`: Lists all column names. This is the spine of your
    data. All analysis flows from knowing your columns (*aka*
    'features').

-   `df.info()`: Provides column names, non-null counts, and data types.
    Missing data often appears here first, but only if the missing data
    is represented by null values (`None` or `nan`).

-   `df.describe()`: Generates summary statistics (count, mean, standard
    deviation, min, quartiles, max) for numeric columns. Pay particular
    attention to min/max values as they often expose outliers or coding
    errors.
