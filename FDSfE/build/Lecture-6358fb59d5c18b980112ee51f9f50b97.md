# Learning Questions

-   What is multiple linear regression?

-   What is logistic regression?

-   Why is logistic regression "classification?"

# Introduction

In the previous lesson, we covered machine learning as a whole, and we
talked a lot about the linear model $y=mx+b$. If we have some feature
$x$ and we have some target $y$, and we believe there is a linear
relationship between them, then we can use the linear model to predict
$y$ based on $x$. This is called **simple linear regression** because
there is one feature, $x$. When there is more than one feature that we
want to use in our model, but we still want the model to be linear ( we
never exponentiate $x$), that is called **multiple linear regression**
(also sometimes called *multilinear* regression).

# Regression: Multiple Linear Regression

This lesson will formalize the simple linear regression we talked about
last time with some new notation that we can more easily expand the
concept to multiple linear regression. This lesson will make use of some
simple vector and matrix arithmetic. To start with, we will now refer to
our parameters as a vector
$\boldsymbol{\beta}=[\beta_0,\beta_1,...,\beta_p]$. We will also refer
to our set of observations --- our features and targets --- like so:
$\{\mathbf{x}_i,y_i\}_{i=1}^n$, where the vector
$\mathbf{x}_i=[x_{i1},x_{i2},...,x_{ip}]$ represents the observation of
$p$ features for observation $i$.[^10] We can also write our vector of
targets as $\mathbf{y}=[y_1,y_2,...,y_n]$.

That's a lot of math. Let's give that a quick example because this is
important and confusing. Imagine that we have a dataset of daily
temperature, humidity and rainfall. Temperature and humidity are our
features, and rainfall is our target.[^11] We have two features, so
$p=2$. Let's say that this data is recorded for three days, so $n=3$.
For one observation, we have three numbers: temperature $T$, humidity
$H$, and rainfall $R$. Using the same notation as before, let's write
out our observations $\mathbf{x}_1$, $\mathbf{x}_2$ and $\mathbf{x}_3$.

$$\begin{aligned}
    \mathbf{x}_1&=[T_1,H_1] \\
    \mathbf{x}_2&=[T_2,H_2] \\
    \mathbf{x}_3&=[T_3,H_3]
\end{aligned}$$

We can compare this to our earlier notation to see that, when $i=1$,
$x_{i1}=T_1$ and $x_{i2}=H_1$, and so on. Something useful we can do
with this is rewrite our linear model in terms of vectors:

$$\begin{aligned}
    y &= 
    \begin{bmatrix}
        1 & x
    \end{bmatrix}
    \cdot
    \begin{bmatrix}
        b \\
        m \\
    \end{bmatrix} \\
    y &= (1 \cdot b) + (x \cdot m) \\
    y &= b + mx
\end{aligned}$$

All I did was rewrite $y=mx+b$ in terms of vectors and then simplify the
expression to get back to $y=mx+b$ to prove to you they are identical.
Now let's apply this same thing to our general notation. For each $y_i$,
we can write our own linear equation with vectors:

$$\begin{aligned}
    y_i &= 
    \begin{bmatrix}
        1 & T_i & H_i
    \end{bmatrix}
    \cdot
    \begin{bmatrix}
        \beta_0 \\
        \beta_1 \\
        \beta_2
    \end{bmatrix} \\
    y_i &= (1 \cdot \beta_0) +  (T_i \cdot \beta_1) + (H_i\cdot \beta_2)
\end{aligned}$$

What we end up with is a simple equation. Do you notice how $b$ and
$\beta_0$ aren't being multiplied by any feature? Just like $b$,
$\beta_0$ is the *intercept* of the equation, and you can think of
$\beta_1$ as the *slope for Temperature*, and $\beta_2$ as the *slope
for humidity*.

We can take all of this one step further and write down an equation not
just for one target $y_i$, but for every target $\mathbf{y}$. To do this
we need to define $\mathbf{X}$, the "design matrix," "model matrix" or
"regressor matrix."

$$\label{eq:X}
    \mathbf{X} = 
    \begin{bmatrix}
        1 & T_1 & H_1 \\
        1 & T_2 & H_2 \\
        1 & T_3 & H_3
    \end{bmatrix}$$

We can put all this together now in one beautiful equation:

$$\begin{aligned}
    \label{eq:linearmodel1}
    \mathbf{y} &= \mathbf{X} \boldsymbol{\beta} \\
    \label{eq:linearmodel2}
    \begin{bmatrix}
        y_1 \\
        y_2 \\
        y_3
    \end{bmatrix} &=
    \begin{bmatrix}
        1 & T_1 & H_1 \\
        1 & T_2 & H_2 \\
        1 & T_3 & H_3
    \end{bmatrix} \cdot
    \begin{bmatrix}
        \beta_0 \\
        \beta_1 \\
        \beta_2
    \end{bmatrix}
\end{aligned}$$

With great pleasure, allow me to introduce to you the **linear model**
[\[eq:linearmodel1\]](#eq:linearmodel1). With this equation, we can now express a
linear model for any number of features! This is the key to multiple
linear regression. If you aren't familiar with linear algebra, it may
not yet be clear how this is useful to us. I'm sure you agree that the
equation is beautiful, but the whole point of machine learning is that
we need to find those parameters, $\boldsymbol{\beta}$!

Let's rewrite the sum of squared errors (SSE) with our new notation.
$$\begin{aligned}
    SSE_i &= \sum_{i=1}^n (y_i - \hat{y}_i)^2 \\
    SSE &= ||\mathbf{y}-\mathbf{X}\boldsymbol{\beta}||^2
\end{aligned}$$ where the
$\hat{y}_i=\mathbf{x_i}\cdot\boldsymbol{\beta}$ is the **estimator** for
$y_i$. In other words, $y_i$ is the $i$-th observation of our target and
$\hat{y}_i$ is what our model predicts for $y_i$.

There is an analytic solution for the best parameters
$\boldsymbol{\beta}$, that minimize $SSE$. Just like the last lesson,
this solution is called the ordinary least squares solution:

$$\label{eq:ols}
    \boldsymbol{\beta} = (\mathbf{X}^\intercal \mathbf{X})^{-1} \mathbf{X}^\intercal \mathbf{y}$$

We know $\mathbf{X}$, it's just the design matrix which is just our
features from our data. We know $\mathbf{y}$, it's just the targets in
our dataset. If you can construct $\mathbf{X}$ and $\mathbf{y}$, you can
do multiple linear regression. Congratulations!

## Notation

Notice how I denote a vector with a bold lowercase letter (
$\mathbf{x}_i$, $\mathbf{y}$, $\boldsymbol{\beta}$), I delimit vectors
with brackets ( $\boldsymbol{\beta}=[\beta_0,\beta_1,...,\beta_p]$,
$\mathbf{x}_i=[x_{i1},x_{i2},...,x_{ip}]$,
$\mathbf{y}=[y_1,y_2,...,y_n]$), I delimit sets with braces (
$\{\mathbf{x}_i,y_i\}_{i=1}^n$), and I denote matrices as bold capital
letters ( $\mathbf{X}$ in [\[eq:X\]](#eq:X)). Something else to note is that $p=n+1$ always. In
words: the number of parameters (for a linear model) is always equal to
the number of features, plus one.

# Classification: Logistic Regression

That's a rather curious section title, isn't it? Is it classification or
is it regression? Well, it's both! Let's return to our example data with
temperature $T$, humidity $H$, and rainfall $R$. As it stands,
temperature is interval data, humidity is measured as a fraction between
0 and 1 so it's ratio data, and rainfall is measured in some unit of
length like inches so it's also ratio data. Let's change rainfall from
ratio data to nominal data: did it rain or did it not rain? Our rainfall
feature is now binary. How do you predict a binary variable?

Linear regression won't help us here. If you encode your data as zeros
and ones, it is indeed possible to perform linear regression, but the
model won't be very helpful. Most of the time it's going to end up
predicting $0.5$ which is neither a $1$ or a $0$. We need another model.

**Logistic regression** is all about the logistic function in the same
way that linear regression was all about the linear function. The
logistic function ([1.7](#fig:logistic)) has quite an unusual form, both
mathematically and graphically:

$$\label{eq:logistic}
    \sigma(x)=\frac{1}{1+e^{-x}}$$

![The logistic
function.](../figures_pedagogy/Logistic-curve.svg.png)

The logistic function takes any input and always produces an output
between 0 and 1. Which, you may notice, is quite handy if you are trying
to predict a binary variable.
[\[eq:logistic\]](#eq:logistic) depicts the simplest version of the logistic
function, but we can create a logistic model by introducing some
parameters:

$$p(x)=\frac{1}{1+e^{-(\beta_0 + \beta_1 \cdot x + \ldots)}}$$ where the
parameter vector $\boldsymbol{\beta}$ and the design matrix $\mathbf{X}$
makes a return.

We need an objective function for this new model so we can figure out
what the best parameters are. We also need to choose a threshold for
classification. Did you notice how in
[1.7](#fig:logistic), the logistic function can output numbers
between 0 and 1? This is not a problem: if the logistic model predicts a
number $\geq0.5$, we count it as a 1, otherwise it's a 0.[^12] We are
treating the output of the logistic model as a probabilistic
classification.

You may wonder, how is this any different from the linear model if it
also doesn't have a binary output? The linear model produces an output
between $-\infty$ and $\infty$, whereas the logistic function produces
an output between 0 and 1 no matter the input. Also, the graph of the
logistic function as it transitions from outputting 0 to outputting 1 is
a quick s-curve, whereas the linear model produces a straight line.
These two things make it impossible to consider the output of the linear
model a probabilistic classification, and this is why logistic
*regression* is actually classification.

## The Logistic Loss Function

The objective function we'll use is called the **logistic loss** or
simply log loss.[^13]

$$\label{eq:logloss}
    \ell = \sum_{i=1}^n (y_i \ln(p_i) + (1-y_i)\ln(1-p_i))$$

where $\ell$ is the log loss, $n$ is the number of observations, $y_i$
is the $i$-th target (0 or 1), and $p_i$ is the probabilistic
classification from the logistic model (between 0 and 1).

Unfortunately, there is no analytic solution like there is for the
linear model; I cannot write an equation that starts with
$\boldsymbol{\beta}=$, which is spiritually devastating. Must we
determine $\boldsymbol{\beta}$ by "brute force"? Checking every
combination of our parameters until we find a good result? In the last
lesson we discussed optimization schemes, so no. But for now, yes, we
will find our parameters by brute force.[^14]

## Nominal Features

What if you have a feature in your dataset that is nominal? Imagine that
you have a dataset and one of the features is titled "Blood Type." You
look through the dataset, and you find that there are only four
different entries in this feature: A, B, AB, and O. This is certainly
nominal data, but most people would call this a categorical variable.
How do we turn categorical variables into something that we can do math
with?

The answer is a process called **one-hot encoding.** We expand the
"Blood Type" feature into four different features: "Blood Type: A,"
"Blood Type: B," "Blood Type: AB," and "Blood Type: O." In each of these
four new features, we input a 1 in the corresponding column.
[1.3](#tab:onehot)
shows what this would look like in a tabular dataset.

::: {#tab:onehot}
   Blood Type   Blood Type: A   Blood Type: B   Blood Type: AB   Blood Type: O
  ------------ --------------- --------------- ---------------- ---------------
       A              1               0               0                0
       B              0               1               0                0
       AB             0               0               1                0
       O              0               0               0                1

  : An example of one-hot encoding.
:::

## Min-Max Normalization

Min-max normalization is a simple process that scales all numeric
features to the range $[0, 1]$:

$$x_{\text{scaled}} = \frac{x - x_{\text{min}}}{x_{\text{max}} - x_{\text{min}}}$$

Why is this necessary? Consider two features: income (ranging from \$0
to \$200,000) and age (ranging from 0 to 100). Income has a much larger
scale. Without normalization, income would dominate the model and age
would contribute almost nothing. Normalization puts all features on
equal footing.

# More on the Split: Training, Validation and Test

In the last lesson we touched on why you want to split your data into
three non-overlapping subsets: the training set, the validation set and
the test set. But I did not provide any guidance on how much you should
allocate for each set. After all, you only have one dataset. It's not
easy to get more data; we work with what we have. If you have $100$
observations, you need to figure out how many go into each set.

The training set is where the model learns its parameters. You want this
set to be as large as possible. A larger training set gives the model
more examples to learn from, which usually leads to better
generalization. Every data point you hold back for validation or testing
is one the model cannot learn from.

The validation set is where you monitor performance during training. You
use it to tune hyperparameters, compare different models, and detect
overfitting. You want this set to be as large as possible too. A larger
validation set gives you a more reliable estimate of how the model is
performing. If your validation set is too small, random noise can
mislead you. A dip in validation accuracy might just be bad luck, not
overfitting. A spike might be good luck, not genuine improvement.

The test set is where you prove that your model works. You use it
exactly once, at the very end of your project, to report final
performance. You want this set to be as large as possible as well. A
larger test set gives you a more trustworthy final evaluation. If your
testing set is too small, you cannot be confident that your model will
perform well on new data.

So if we want all three sets to be as large as possible, what do we do?
Allocate a third of the data to each set? There is no perfect ratio that
works for every situation. A massive dataset with millions of rows can
afford to hold out $20\%$ for validation and $20\%$ for testing while
still leaving plenty for training. A tiny dataset with only 100 rows
cannot afford to hold out anything.

The most important thing is not the specific ratio you choose, it's that
**each set is representative of the population your data comes from.**

Let's consider an example. Imagine you are building a model to predict
housing prices. Your data contains houses from five different cities. If
you put all the houses from City A into the training set and all the
houses from City B into the test set, your evaluation will be
meaningless. The model will learn the patterns specific to City A, and
you will test it on a completely different distribution of houses. Your
test set performance will tell you nothing about how the model would
perform on a new house from City A or C.

The splits must preserve the underlying structure of your data. If your
data has categories, each split should contain a representative
proportion of those categories. If your data comes from different time
periods, each split should span the same range of time. In practice, you
achieve this by using `train_test_split` from the `sci-kit learn`
package with appropriate arguments: `stratify` for categorical targets,
`shuffle` to randomize order, and `random_state` to make your results
reproducible.

So, maybe you are reading this section because you just want to know
what size your sets should be. Unfortunately, I truly can't tell you.
There is no single correct answer. You must consider your total sample
size, the complexity of your problem, the noise in your data and the
cost of making a mistake. If you were to read this section and conclude
that the best *starting place* is a one-third split for each set, that
is fine. But never forget: **the representativeness of your splits
matters more than the exact ratios.**



[^10]: I'm using the same notation that the Wikipedia page for "Ordinary
    least squares" uses, and I encourage you to look at that page for
    --- if nothing else --- an alternative wording of the same math
    which may help aid in your understanding.

[^11]: Note that rainfall is also a feature, but we are choosing this
    feature to be the one that we predict based on the others.

[^12]: You don't have to choose a threshold of $0.5$.

[^13]: The log loss is equivalent (up to a constant) to the
    cross-entropy loss, which is used in neural networks.

[^14]: Strange as it is, brute force is the *technical* term for
    computing all possible solutions (in a range) to choose the best
    one.