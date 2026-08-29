# Learning Questions

-   What is data?

-   What is machine learning?

-   What is a model?

-   What is the difference between a parameter and a hyperparameter?

-   What is the difference between supervised and unsupervised learning?

-   What is an objective function?

-   What is Ordinary Least Squares (OLS) regression?

-   How do we evaluate model performance?

-   Why do we split data into training, validation, and test sets?

-   What is overfitting?

# Introduction

Imagine you are trying to teach a computer to recognize handwritten
digits. You could try to write explicit rules: "A seven has a horizontal
line across the top and a vertical line on the right." But what about
different handwriting styles? What about smudges? What about a seven
written with a serif? You would spend forever writing rules, and your
program would still fail on the first handwritten note you gave it.
There must be a better way.

And there is! Instead of programming the rules explicitly, you can show
the computer many examples of handwritten digits and let it discover the
rules itself. This is **supervised machine learning.**

# Part 1: What Is Data?

Before we can build models, we must understand the types of data we work
with. The NOIR taxonomy classifies data into four levels of measurement.
Each level supports different mathematical operations, and using the
wrong operation on the wrong type of data leads to nonsense.

::: {#tab:noir}
              Order   Distance   Mean   Median   Mode   Absolute Zero
  ---------- ------- ---------- ------ -------- ------ ---------------
   Nominal      x                         x            
   Ordinal      x                         x       x    
   Interval     x        x        x       x       x    
    Ratio       x        x        x       x       x           x

  : NOIR data. The four types of data and their features.
:::

**Nominal data** consist of unordered categories. Colors are nominal.
Bus route numbers are nominal. You can't say one color is "more" than
another; you cannot calculate the average of
$\{\text{blue}, \text{green}, \text{orange}\}$. The mode --- the most
frequently occurring category --- is the only appropriate measure of
central tendency.

**Ordinal data** have order but not equal spacing. Movie ratings (1--5
stars) are ordinal. You know that 5 stars is better than 1 star, but you
cannot claim that a 4-star movie is "twice as good" as a 2-star movie.
The difference between 4 stars and 5 stars may not mean the same thing
as the difference between 1 star and 2 stars. The median and mode are
appropriate, but the mean is not.

**Interval data** have equal spacing but no true zero. Temperature in
Celsius or Fahrenheit is interval. Twenty degrees Celsius is hotter than
ten degrees Celsius, and the ten-degree difference means the same thing
anywhere on the scale. However, twenty degrees Celsius is *not* twice as
hot as ten degrees Celsius because zero degrees Celsius doesn't
represent an absence of temperature. The mean, median, and mode are all
meaningful.

**Ratio data** have equal spacing *and* a true zero. Temperature in
Kelvins is ratio. Twenty Kelvins *is* twice as hot as ten Kelvins. Zero
Kelvins represents absolute zero, the complete absence of thermal
energy.[^4] All arithmetic operations, including ratios, are valid.

**NB:** Many introductory data science projects treat all numeric data
as ratio data. This is a mistake. If your data lacks a true zero,
reporting that "the average temperature doubled" is mathematically
incorrect.

# Part 2: What Is Machine Learning?

One time in 1959, some important guy[^5] said something really
insightful about machine learning. "\[Machine learning is\] the field of
study that gives computers the ability to learn without being explicitly
programmed." This\... is not a very helpful definition for us. But it's
poetic! And it will serve us for now. Let's go over some important
terms.

A **model** is a low-dimensional representation of a higher-dimensional
dataset. Consider a scatter plot of 100 points that roughly follow a
straight line. The raw data contains 100 x-values and 100 y-values;
that's 200 numbers. But a linear fit can be described by just two
numbers: the slope and the intercept. That is a model: it is a
simplification that captures the essential pattern while discarding the
noise.

**Parameters** are learned from the data. In the linear model
$y = mx + b$, the slope $m$ and intercept $b$ are parameters. The
learning algorithm adjusts them to fit the training data.

**Hyperparameters** are set by you before training begins. They
represent a decision that you have made about how you want your model to
be. They are not learned from the data. You must choose them based on
your own knowledge.

# Machine Learning Paradigms

Machine learning can be done in all sorts of different ways. To explain
them, we first need to understand the concept of a **feature.** If you
are recording the name, height and age of your friends, then 'name,'
'height' and 'age' are the features of your dataset. They're also often
called 'columns' because data is often stored in a 2D, tabular format
where each row represents one **object** (one of your friends) and each
column contains one feature ( name, height and age).

Sometimes we use machine learning to predict one feature based on other
features. This is called **supervised learning.** For example, you could
try to predict someone's age based on their name and height. Or you
could try to predict their name based on their height and age. Both of
those sound like practically impossible tasks, but you could try it! The
two main types of supervised machine learning are **classification,**
where the goal is to predict (classify) a nominal or ordinal feature,
and **regression** where the goal is to predict (regress) an interval or
ratio feature.

Supervised learning is probably the most common type of machine
learning, but **unsupervised learning** is almost as ubiquitous. In this
paradigm, you aren't trying to predict one of your features. Rather, you
are looking for structure within the features. The commonest
applications are: **clustering,** partitioning data into groups of
similar points; **anomaly detection,** identifying unusual observations
that do not fit the pattern; **dimensionality reduction,** compressing
data while preserving important information.

All of the tasks we will cover in this course will be supervised and
unsupervised learning, but there are other paradigms worth mentioning.
**Semi-supervised learning** combines a small amount of labeled data
with a large amount of unlabeled data. This is common when labeling is
expensive, but unlabeled data is abundant. **Active learning** allows a
model to interactively query the user for labels on particularly
informative data points. The model asks: "What is *this* one?" and you
tell it. With **reinforcement learning,** the model (agent) learns to
make sequential decisions by maximizing a cumulative reward signal
through interactions with an environment.[^6]

# Part 3: Model Fitting and Objective Functions

For now, we will focus on supervised machine learning where our ultimate
goal is to use our features to predict some target by fitting a model.
Fitting a model involves two steps.

**Step 1:** Choose a mathematical form. Do you have a reason to believe
that there is an underlying linear relationship between your feature and
your target? You could visualize your data and decide for yourself what
model to use too, but keep in mind that this introduces biases. It's
best to pick a model based on your theoretical understanding of the
data. But, for a linear relationship, you might choose the model
$y = mx + b$.

![A collection of data points spread out in a rough line is approximated
by a linear model. In this case, the parameters of the model are $m=1$
and $b=0$.](../figures_pedagogy/linearmodel.pdf)

**Step 2:** Optimize an **objective function** (also called a loss
function) that quantifies how well the model fits the data. Consider the
machine learning task posed in
[1.6](#fig:linearmodel) where we have a collection of data points,
and we want to fit a linear model to it. We don't know what the
parameters of the model are yet (that's what machine learning does), so
we're going to guess the parameters first and then see how good that
guess was. In order to evaluate how good the guess is, we will use this
objective function that compares the predictions of the model with our
guessed parameters to the true target values. Then, the objective
function returns one number which lets us know if the parameters were
good or not.

The **sum of absolute errors** (SAE), also called the L1, is one such
objective function:

$$L_1 = \sum_{i=1}^n |y_i - \hat{y}_i|$$

where $n$ is the number of samples/objects/rows in your data, $y_i$ is
the $i$-th target, and $\hat{y}_i$ is the model's prediction of $y_i$.
Thus the "error" that is being summed is $|y_i - \hat{y}_i|$. Notice
that when $y_i$ and $\hat{y}_i$ are close to each other (when the
prediction of the model is good and errors are low), then $L_1$ is
small.

The **sum of squared errors** (SSE), or L2, is another objective
function:

$$L_2 = \sum_{i=1}^n (y_i - \hat{y}_i)^2$$

The difference here is that the L2 squares the error rather than summing
them together. What this means practically is that when the errors are
large ( $|y_i-\hat{y}_i|>1$) then $L_2$ is always larger than $L_1$.

These two objective functions are the most primitive and most useful for
teaching and learning, but they're certainly not the best. In fact,
there is no "best" objective function. You always want to choose the
objective function that is best for your machine learning task; in this
way, the choice of the objective function is a hyperparameter.

## Ordinary Least Squares (OLS) Regression

Depending on your choice of objective function, you may be able to
analytically solve for the best parameters ( do some clever math rather
than guessing and checking). If you have some data, and you want to
model it with a line, and you want to use the L2 objective function,
then there happens to be such an analytic solution for the parameters
$m$ and $b$ based on the data. Neat!

$$\begin{aligned}
    m &= \frac{\sum_{i=1}^{N} (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{N} (x_i - \bar{x})^2} \\
    b &= \bar{y} - m\bar{x}
\end{aligned}$$

where $\bar{x}$ and $\bar{y}$ are the average values of $x$ and $y$ in
your data.[^7]

# Part 4: Model Performance Metrics

We already discussed how the objective function quantifies how good of a
model you have, but we can generalize that idea further. A **metric** is
any function that measures how good a model is. The distinction between
the metric and the objective function is that the objective function may
not mean anything to a human. If you calculated the L2 for a dataset and
a model and found $L_2 = 1.5$, what would you do with that information?
You could try to wrap your head around it, but for the most part,
objective functions are only useful in comparison. If you changed the
model parameters and found $L_2=1.4$, then you could say that the model
improved because the L2 decreased. A metric is *generally* somewhat
easier to understand.

$$\begin{aligned}
    \epsilon_i &= y_i - \hat{y}_i \quad \text{(Error)} \\
    SAE &= \sum_i^n |\epsilon_i| \quad \text{(L1 or Sum of Absolute Errors)} \\
    SSE &= \sum_i^n \epsilon_i^2 \quad \text{(L2 or Sum of Squared Errors)} \\
    MAE &= \frac{1}{n} \sum_i^n |\epsilon_i| \quad \text{(Mean Absolute Error)} \\
    MSE &= \frac{1}{n} \sum_i^n \epsilon_i^2 \quad \text{(Mean Squared Error)} \\
    RMSE &= \sqrt{MSE} \quad \text{(Root Mean Square Error)}
\end{aligned}$$

I've introduced three new metrics (they can also be used as objective
functions): the **mean absolute error,** the **mean squared error** and
the **root mean square error.** The first two are simply the L1/SAE and
L2/SSE divided by the number of samples. This makes the quantity a bit
simpler to understand. All of these are commonly used, but there is one
more that is ubiquitous in linear modeling: the **coefficient of
determination** or simply the "R-squared."

$$\begin{aligned}
    RSS &= \sum_i^n (y_i - \hat{y}_i)^2 \quad \text{(L1 or SAE or ``Residual Sum of Squares'')} \\
    TSS &= \sum_i^n (y_i - \bar{y})^2 \quad \text{(``Total Sum of Squares'')} \\
    R^2 &= 1 - \frac{RSS}{TSS} \quad \text{(Coefficient of Determination)}
\end{aligned}$$

Unfortunately it's true that the **residual sum of squares** (RSS), L1
and SAE are all the same thing, but I've re-written it here for clarity
because I've also introduced the **total sum of squares** (TSS) where
the sum is not over the errors but over the difference between $y_i$ and
the $\bar{y}$, the mean. The $R^2$ is always between 0 and 1, where a
"perfect" model is a 1, and a model that always predicts $\bar{y}$ would
be a 0. What the $R^2$ actually measures is the proportion of variance
in $y$ that is *explained* by the model. A model with $R^2=0$ predicts
only the mean, and it doesn't account for any of the variance from the
mean within the data. A model with $R^2=1$ perfectly predicts every
single variation from the mean.

For classification problems, the metrics and objective functions are
entirely different. The objective functions are necessarily less simple
because math with discrete variables is unusual for most of us. We will
just cover four common classification metrics for now.

Let's say we are predicting a target based on some data. The target we
are predicting has two possibilities: positive or negative. Whenever we
make a prediction, there are four possibilities: we predict positive and
the target was positive (a **true positive,** TP); we predict negative
and the target was negative (a **true negative,** TN); we predict
positive and the target was negative (a **false positive,** FP); we
predict negative and the target was positive (a **false negative,** FN).
Table[1.2](#tab:confusion_table) represents these four outcomes in
**confusion matrix.**

::: {#tab:confusion_table}
                   Prediction is $+$   Prediction is $-$
  --------------- ------------------- -------------------
   Target is $+$          TP                  FN
   Target is $-$          FP                  TN

  : An example "confusion matrix" demonstrating the four possible
  outcomes of binary classification.
:::

We can now define our four metrics based on those four outcomes:

$$\begin{aligned}
    \text{Accuracy} &= \frac{TP + TN}{TP + FP + TN + FN} \\
    \text{Precision} &= \frac{TP}{TP + FP} \\
    \text{Recall} &= \frac{TP}{TP + FN} \\
    F_1 &= \frac{2TP}{2TP + FP + FN}
\end{aligned}$$

**Accuracy** is simply the proportion of correct predictions.
**Precision**[^8] is the fraction of true positives to all of the
positives that the model predicted. Precision answers the question: "How
many retrieved items are relevant?" **Recall**[^9] is the fraction of
true positives among all positives. Recall answers the question "How
many relevant items are retrieved?" Finally, we have the **F1-score**,
which is actually the harmonic mean of Precision and Recall, therefore
representing both equally within one metric. All four of these metrics
can take on values between 0 and 1, and can therefore be represented by
percentages.

# The Split: Training, Validation, and Test

Here is a trap that ensnares many students: You fit a model to your data
and you evaluate its performance on that data. The performance looks
great, you find $R^2 = 0.95$ (regression) or $F_1=99\%$
(classification). Then you come across some new data, and you try your
model on it and find the metrics have plummeted. What happened?

Your model learned the data it trained on, including its noise and
quirks. It did not learn the underlying pattern. This is called
**overfitting;** the model was not generalizable. To honestly assess
generalizability, you must split your dataset into three non-overlapping
subsets. The **training set**, **validation set** and **test set.**

The training set is where the model learns its parameters. For example,
if you were to use the OLS method to find the parameters of a linear
model, you would calculate $\bar{y}$ and $\bar{x}$ on this training set.
The validation set is used to tune hyperparameters. You evaluate
performance here repeatedly, though you don't train on it. Perhaps you
want to know what the effect of changing the objective function would
be? You would compare the two models' performance on the validation set.
Lastly, the test set is used *only once,* ever, at the very end of your
project. When you write your research paper on your cool new model, in
the results z you will include the performance of the model on this test
set. You never use the test set to inform any model parameters or
hyperparameters.

## Overfitting and Underfitting

When your model is too simple to capture the underlying pattern in the
data, we say the model is underfitting. Imagine fitting a straight line
to data that clearly follows a curve. Underfitting produces poor
performance on both the training set and validation set. You could
improve the model by changing hyperparameters ( going from a linear
model to a quadratic model). Overfitting occurs when your model is too
complex relative to your amount of data. The model learns noise and
idiosyncrasies specific to the training set, and the fundamental pattern
or physical system that created your data is lost. Overfitting produces
excellent performance on the training set but poor performance on the
validation set. The dataset split is your primary defense against
overfitting.

While we learn the parameters from the data in the training set, we
change the hyperparameters based on the performance of the validation
set. We may choose a quadratic model instead of a linear one if we see
the model underfitting on the validation set. Or we might choose a
linear model instead of a quadratic one if we see overfitting. This is
why splitting the data into two sets is not sufficient. We need a third
test set to make sure the hyperparameters were not tuned specifically to
the validation data. The theme here is that we want our models to
generalize, and the only way to ensure that is to hide parts of the
dataset from our model and then surprise it at the end.

# Optimization

We discussed objective functions, formulae that quantify how poorly or
how well a model fits data. The L1 objective sums absolute errors. The
L2 objective sums squared errors. Both return a single number: the
error. The smaller the number, the better the model. But knowing the
error is only half the problem. How do we find the model parameters that
minimize the error? This is the task of **optimization.**

The simplest optimization method is brute force. You try every possible
combination of parameters and compute the objective function for each,
picking the parameters that lead to the smallest value. If your model
has only one continuous parameter, that is actually *infinite* values
you have to calculate the loss for. You cannot do that, so where do you
start? What is the first value you test? Perhaps you will choose 0 to be
your starting point. Now what is the next point? 0.1? 0.01? The smaller
this number, the more calculations you have to make and the more likely
you "step over" the minimum. The issue with the brute force method is
that there is no limit to how many parameters you'll need to test. We
will use **gradient descent** instead.

Imagine you are standing in a mountainous landscape, blindfolded. Your
goal is to find the lowest point. You can feel the slope beneath your
feet, so if the ground slopes downhill to your left, you take a step to
the left. If it then slopes downhill to your right, you take a step to
the right. You could repeat this process until you can no longer feel
any downward slope.

Optimization algorithms work the same way. The "mountainous landscape"
is the **loss landscape**: a surface that maps model parameters to the
value of the objective function. The "lowest point" in the loss
landscape is the set of parameters that minimizes the objective
function. The "slope" is the gradient of the loss landscape.

The gradient is like your feet: it can tell you what direction the
landscape is sloping. Mathematically, it is the derivative of the loss
at that value of the parameter vector. The gradient is a vector that
points in the direction in which the loss landscape is steepest. We want
to traverse down the loss landscape, so we calculate the gradient of the
objective function and move in the opposite direction of the gradient.
And when I say "move" I mean "try a new set of parameters." In practice,
we will try a set of parameters, calculate the objective function,
calculate the gradient of the objective function at that point, and then
change the parameters in the opposite direction ( increase or decrease
each parameter) of the gradient.

Consider a simple objective function: a parabola, $f(x)=x^2$, where x is
the model parameter. The minimum value that $f(x)$ takes on is at $x=0$,
$f(0)=0$. But let's say we didn't know that (because in general,
objective functions are more complicated), and start at $x=2$. The
gradient (or just the derivative, for functions of one parameter) is
$f'(x)=2x;f'(2)=4$. The gradient at $x=2$ is $4$; it's positive. Thus,
we should decrease our parameter to move in the opposite direction of
the gradient. Let's try $x=-2$. The gradient is $f'(x=-2)=-4$, negative,
which means we should increase our parameter.

Once we have the gradient, we know whether to increase or decrease our
parameters, but by how much? This **step size** is controlled by the
**learning rate** hyperparameter. A small learning rate means you make
small changes to your parameter in proportion to the gradient, and a big
learning rate means you make big changes to the parameter in proportion
to the gradient. Small learning rates mean your optimizer will be
cautious and slow. Large learning rates mean your optimizer will be
risky (you may overshoot the minimum), but fast. Choosing the right
learning rate requires you to balance your desire for speed and
algorithm stability.

Gradient descent works well, but it requires you to calculate the
objective function on the entire training set at each step. **Stochastic
gradient descent** (SGD) computes the gradient using only a single,
randomly chosen (hence "stochastic"), data point. The objective function
evaluated on one data point may be wildly different compared to another,
but this is actually a strength of SGD. The gradient is noisy and
imprecise, but this helps with avoiding local minima.

A local minimum is a valley that is not the lowest point in the entire
landscape. Standing in a local minimum, it would feel like you're at the
lowest point, but perhaps over the nearby hill is an even deeper valley.
You would never know, just by measuring the slope at your location. The
noise in the calculated gradient can help kick the algorithm out of a
local minimum.

There is also **mini-batch gradient descent** where, instead of using
the entire dataset (gradient descent) or a single point (SGD),
mini-batch gradient descent uses a small random subset of the data to
compute the gradient. This balances the accuracy of the gradient
estimate (which improves with more points) with the ability to avoid
local minima. Local minima are a significant challenge in optimization,
especially for models with many parameters, so mini-batch gradient
descent, or some variant of it, is very common.


[^4]: Any system at $0\text{ K}$ would still have some zero-point
    energy, so this statement isn't completely true.

[^5]: Arthur Samuel, a pioneer in machine learning.

[^6]: Reinforcement learning is really cool! Maybe I will add a lesson
    on it one day.

[^7]: You may have encountered this before under a different name. When
    I was in high school, I learned about the "line of best fit," which
    was almost certainly the OLS method.

[^8]: also called positive predictive value (PPV)

[^9]: also called sensitivity