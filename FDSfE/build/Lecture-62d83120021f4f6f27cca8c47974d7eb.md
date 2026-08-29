# Learning Questions

-   What is a decision tree?

-   What is Gini impurity?

-   What are the hyperparameters of tree models?

-   What are ensemble methods?

-   How does regression with tree models work?

# Introduction

Decision trees are among the most intuitive and interpretable types of
machine learning models. They mimic the way humans make decisions: by
asking a series of questions, each of which narrows down the
possibilities until a conclusion is reached. But despite their
simplicity, decision trees are also among the most successful types of
models, seeing widespread use in all disciplines.

# The Decision Tree

A decision tree is a kind of flowchart, where each internal node
represents a test --- a question --- on a feature. Each branch of the
tree represents the outcome of the test, and each leaf represents the
output of the model (a classification or a regression). To make a
prediction, you start with your observation you want to predict on,
$\mathbf{x}_i$, and you traverse the tree by answering questions about
your observation until you reach a leaf.

[1.8](#fig:dtc) provides
an example of a decision tree classifier. The dataset has three
features: outlook (sunny, rain), humidity ($0-100\%$), and windy (yes,
no). The target variable is whether or not a sports game will be allowed
to be played (play, don't play). We read the decision tree from the root
node at the top (it's an inverted tree). The first node asks "What is
the outlook?" Outlook is a categorical variable with two categories. A
branch spawns for each possible answer to the question: one if the
outlook is 'sunny,' and one if it's 'rain.' Within the node, you'll
notice that, among all of the observations that could traverse the tree
and arrive at that node, the amount of observations with the target
variable 'play,' or 'don't play,' is written.

![An example decision
tree.](../figures_pedagogy/Decision_tree_model.png)

Along the bottom layer, we actually see that all four of the leaf nodes
(remember that a node with no branches is called a leaf) have either
only 'play' or only 'don't play.' We call this a pure node. Compare this
to the node at the end of the 'sunny' branch, which is not pure.

You may be wondering: how does the model know which question to ask at
each node? The goal of the model is to ask a question that leads to the
purest nodes. To do this, we need a way to quantify node purity.

# Gini Impurity

The **Gini impurity** is a common metric for measuring the purity of a
decision in a tree. If the dataset has $J$ class labels ($J=2$, 'play'
and 'don't play'), the Gini impurity can be written as:

$$\begin{aligned}
    G &= 1 - \sum_{i=1}^{J} p_i^2 \\
    J=2 \quad G &= 1 - (p_1^2 + p_2^2)
\end{aligned}$$

where $p_1$ is the relative frequency of class label 1 ('play') and
$p_2$ is the relative frequency of class label 2 ('don't play') (they
are denoted as $p$ because, as we discussed in
[1.2](#sec:pedagogy:stats), frequencies *can* be interpreted as
probabilities, unless you work in a Bayesian framework).

Let's calculate the impurity of some of the nodes in
[1.8](#fig:dtc).
Consider the root node, where the total number of samples is
$N=N_1+N_2=9+5=14$. The relative frequency of 'play' is $N_1/N=64\%$,
and for 'don't play' it's $N_2/N=36\%$. To calculate the Gini impurity:

$$\begin{aligned}
    G &= 1 - (p_1^2 + p_2^2) \\
    G &= 1 - \left( \left(\frac{N_1}{N}\right)^2 + \left(\frac{N_2}{N}\right)^2 \right) \\
    G &= 1 - \left( 0.64^2 + 0.36^2 \right) \\
    G &= 46\%
\end{aligned}$$

Now that we have some practice calculating the Gini impurity on the root
node, let's do it for each of the three nodes after the root node.

$$\begin{aligned}
    \text{sunny} \quad G &= 1 - \left( \left(\frac{2}{5}\right)^2 + \left(\frac{3}{5}\right)^2 \right) = 48\% \\
    \text{rain} \quad G &= 1 - \left( \left(\frac{3}{5}\right)^2 + \left(\frac{2}{5}\right)^2 \right) = 48\% \\
\end{aligned}$$

If we look at all of the four leaf nodes on the bottom, they will all
have a Gini impurity of $0\%$. They are minimally impure, or in other
words, they are maximally pure!

As a final test, let's imagine we have a new sample ( a day to schedule
a game) in our dataset and we want to use our model to predict the
target. The new sample has the following feature values: outlook rain,
humidity $80\%$, windy yes. The first node we travel down is the 'rain'
branch to the 'windy' node. Then we travel down the 'True' branch to the
final leaf node. The final prediction is therefore the class label most
represented in the leaf node; in this case, the final prediction is
'don't play'.

What happens if the target variable remains mixed at the leaf node?
There is no guarantee that you can achieve purity. It is possible that
under the same conditions, you get two different outcomes. If the leaf
node is not pure, the prediction can be the most common label in that
leaf, or it can be interpreted as a probability: if a leaf node
contained 3 'don't play' and one 'play' labels the prediction for a day
that falls in that leaf node would be a 75% "probability" of no game (we
put "probability" in quotes because it is not a probability in a
Bayesian sense, which is what we most often want to achieve with machine
learn models).

## Decision Trees and Data Types

Decision trees can handle both categorical and numerical features. For
categorical or numerical features alike, a question at a node *always*
splits the data into two branches. In
[1.8](#fig:dtc), one
node question is whether the humidity is $\leq70\%$ or $>70\%$. This
threshold is chosen to minimize the Gini impurity.

## Decision Tree Hyperparameters

Decision trees have several important hyperparameters that govern their
growth and complexity. Here I refer specifically to the `scikit-learn`
implementation of decision trees.

`max_depth`: The maximum depth of the tree. This isn't how many branches
can be made; it's how many rounds of decisions can be made. In
[1.8](#fig:dtc), the
total depth was two. Deeper trees can capture more complex patterns but
are more prone to overfitting. With an unlimited number of trees, the
model *will* overfit.

`min_samples_split`: The minimum number of samples required to split an
internal node. Higher values prevent the tree from making splits on very
small subsets, which, again, reduces overfitting.

`min_samples_leaf`: The minimum number of samples required to be at a
leaf node. This ensures that leaf nodes always represent a meaningful
number of observations.

`max_features`: The number of features to consider when looking for the
best split. Restricting the number of features can introduce randomness,
which is beneficial for ensemble methods (which we will discuss later in
this lesson).

`criterion`: There are different ways to measure the quality of a split
apart from Gini impurity. Entropy is the other common criterion you will
see.

## Pruning

Decision trees are notorious for overfitting. A tree that is allowed to
grow until every leaf is pure will perfectly classify the training data,
but perform poorly on unseen data. The model will have learned every
single intricacy of the training set, including the noise, but its
generalizability will be very poor.

Overfitting occurs because the tree can continue splitting until each
leaf contains samples of only one class. This is particularly
problematic when the dataset is small relative to the number of
features, like in [1.8](#fig:dtc). Several techniques can prevent overfitting in
decision trees. The simplest is **early stopping:** halting the growth
of the tree when certain criteria are met, such as reaching a maximum
depth or a minimum number of samples per leaf. **Pruning** is another
approach where the tree is first grown to its full size. Nodes that do
not provide significant improvement in predictive power are then
removed.

# Ensemble Methods

While a single decision tree is easy to visualize and understand
(something we call "interpretability"), its predictive power is often
limited. Ensemble methods address this by combining multiple trees to
produce a more robust prediction. The underlying principle is that a
collection of not-so-good decision trees (called "weak learners") that
are only slightly better than a random guess can form a good decision
tree ("strong learner") when combined appropriately.

## Random Forests

A **random forest** is an ensemble of decision trees, each trained on a
random bootstrap sample[^15] of the data. This means that each tree sees
a slightly different set of training examples. Additionally, at each
split, the tree only considers a random subset of the features. In our
previous example, this would mean that one of the weak learners ignores
humidity while the other ignores outlook. These two quirks help ensure
that the trees are not correlated with each other.

When making a prediction, each tree in the figurative forest "votes" for
a class (the majority class in that leaf node for that tree), and the
class with the most votes is provided by the model as the official
prediction. The philosophy is that, while some trees may be misled by
noise in their training data, the majority vote of many trees will
converge on the correct answer. Random forests are more robust to
overfitting than single decision trees.

## Gradient Boosting

**Gradient boosting** is another ensemble tree method. Instead of
building trees in parallel to each other, gradient boosting builds trees
sequentially. Each new tree is trained to correct the errors made by the
previous trees. The algorithm fits a tree to the residuals (the
difference between the true class and the predicted class), then adds
this tree to the ensemble.

Gradient boosting is particularly powerful for complex classification
problems. The `XGBoost` library is an optimized implementation of
gradient boosting that also includes other features like regularization,
which we will discuss more thoroughly in the lesson on neural networks.

## Boosting and Bagging

Two terms you hear often in machine learning spaces are **boosting** and
**bagging.** Bagging (bootstrap aggregating) is used in random forests.
Trees are built independently, trained on bootstrap samples, and the
final prediction is an average of all of the trees. Boosting is used in
gradient boosting; trees are built sequentially. Each tree focuses on
the mistakes of the previous ones. Bagging helps to curtail overfitting,
while boosting helps to reduce model bias. Both types of models are
useful. In fact, you may want to try both on the same task to see which
is best.

# Regression

The decision tree algorithm seems naturally inclined towards
classification tasks; however, it is equally capable of regression. A
**regression tree** predicts a continuous variable instead of a class.
Instead of Gini impurity, regression trees use a splitting criterion
based on the reduction in MSE of the target variable.

# Interpretability

Decision trees are remarkably interpretable. A trained tree can be
visualized and understood by anyone. This interpretability is a major
advantage in contexts where understanding the relationship between the
features and the target is as important as making accurate predictions.
However, decision trees are prone to instability. Small changes in the
training data can lead to vastly different splits and thus vastly
different trees. The order of the splits matters. Ensemble methods
mitigate this instability.

# Feature importance

Tree models provide a measure of **feature importance.** By tracking how
much each feature reduces impurity across all splits in a tree, we can
determine which features are the most important for prediction. In a
Random Forest, we can also obtain a measure of the variance of the
importance by looking at the variance of its impact for all splits
across different trees.

[^15]: A sample drawn with replacement from the dataset. That is: select
    a subset of size $N$ out of a dataset of size $M$, where $N < M$; do
    that several times, each time selecting $N$ from the full $M$
    objects, and treat each subset separately.