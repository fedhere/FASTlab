# Learning Questions

-   What is clustering?

-   What is the $k$-means algorithm?

-   Why is feature scaling important for clustering?

-   What is DBSCAN?

-   What is hierarchical clustering?

-   What is a dendrogram?

-   What are the different types of clustering good at?

# Introduction

Imagine that you are a school administrator. You have data on students:
their attendance percentage and their homework scores. You want to find
out if there are ways to group these students based on their attendance
and homework scores. You don't have anything to predict, no labels or
features to regress; you just want to know if there is some structure in
your data. This is the problem of **clustering.** Clustering is an
unsupervised learning method. Unlike classification or regression, where
we have information that we want to learn how to predict, clustering
finds structure within unlabeled data. It partitions data points into
groups (clusters) such that points within the same cluster are similar
to each other, and points in different clusters are dissimilar.

In this lesson, we will cover three major clustering methods: $k$-means,
DBSCAN and agglomerative clustering. Before we begin, recall the four
types of data: NOIR. Clustering requires a definition of similarity or
distance between data points. As a result, clustering is most
appropriate for interval and ratio data, where distances are meaningful.
Clustering on nominal or ordinal data requires special care ( a more
abstract definition of distance).

# $k$-means Clustering

$k$-means is the most intuitive clustering algorithm. The goal is to
partition our data into $k$ clusters. Each cluster is represented by its
**centroid** --- the mean of all points in the cluster. The algorithm
assigns each point to the cluster whose centroid is closest. In our
student example, we have two features: attendance and homework score.
Each student is a point in this 2D feature space, each student can be
represented by a point $(a,b)$ where $a$ is their participation and $b$
is their homework score (the same way a place is identified by two
coordinates, longitude and latitude). Let's say we suspect the students'
attendance and scores are grouped based on who their teacher is. Three
teachers teach a different section of physics, so we're going to try to
group our data into three clusters, $k=3$. The $k$-means algorithm will
find three centroids and assign each student to the nearest one. The
result is that each student can now be represented by three points:
$(a,b,c)$ where $c$ is one of the three clusters. We guessed that the
students have different attendances and homework scores based on their
teacher, so we would expect to see all students from one class belong to
the same cluster. Having just taken random guesses for the coordinates
($\mu_a,\mu_b$) of each center, we are likely not there yet.

$K$-means minimizes the **inertia**.
$$\sum_{i=1}^{k} \sum_{\mathbf{x} \in C_i} (\mathbf{x} - \boldsymbol{\mu}_i)^2$$
where $\mathbf{x}$ is a point in the feature space (in our example it's
a 2D vector), $C_i$ is the set of points in cluster $i$, and
$\boldsymbol{\mu}_i$ is the centroid of cluster $i$ (also a 2D vector).
The expression $(\mathbf{x} - \boldsymbol{\mu}_i)^2$ is the square of
the Euclidean distance between two points, and in 2D space it can be
expanded to $(a-a_{\mu_i})^2 + (b-b_{\mu_i})^2$.

In words: for each cluster, we compute the distance from each point to
the cluster centroid. We square that distance, and sum all of them up.
Then we sum them for each cluster. The goal of the algorithm is to
minimize the inertia by choosing $\mu_i$'s appropriately.

## The Algorithm

The $k$-means algorithm has four steps.

1.  Initialize the locations of the $k$ centroids randomly in the
    feature space.

2.  Assign each point to the nearest centroid.

3.  Update each centroid to the mean of its assigned points.

4.  Repeat the last two steps until the location of the centroids stops
    changing.[^16]

It may be that the initial locations of the centroids were not optimal,
resulting in centroids that aren't well placed. This is the risk we take
with algorithms that have random elements. You can initialize the
centers yourself if you like, but it's also common to run the algorithm
many times with different starting points, and choose the result with
the lowest inertia.

## The Elbow Method

How do we choose $k$? In our example, we chose it based on our knowledge
of the context of the data. This is always the best way to pick any
hyperparameter like $k$. But what if our data is too abstract, or very
high dimensional, and there is no real way to understand the context?
The **elbow method** is one way to choose $k$.

![A visualization of the "elbow" method to choose the number of clusters
in $k$-means clustering. From <https://harisnazir.github.io>.](../figures_pedagogy/elbowMethod.png)

We run $k$-means with different values of $k$. As $k$ increases, inertia
always decreases, but at some point the decrease becomes smaller (see
[1.9](#fig:elbow)). If
you were to plot the inertia as a function of $k$, you would find a kind
of "elbow," visually, at this point where the decrease becomes smaller.
The elbow is a good choice for $k$. The implication with the elbow
method is that, once you have the "correct" number of clusters for your
dataset, adding more clusters doesn't do much to decrease the total sum
of distances between centroids.

This method is subjective. Sometimes the elbow is clear, and sometimes
it's not. In my experience, it's mostly not very clear. Ultimately, you
will have to decide how many clusters to use.

## Feature Scaling

$k$-means is sensitive to the scale of features. Consider our student
example. We're measuring attendance as a fraction (between 0 and 1), and
we're measuring homework score as a percentage (between 0 and 100). The
distance between each point will be dominated by the homework scores. A
big change in attendance (between 0.2 and 0.8) barely matters compared
to a modest change in homework score (between 65 and 72). Why? Because
we calculate the distance with the Euclidean formula:
$d=\sqrt{(0.8-0.2)^2 + (72-65)^2} = 7.02$. Compare this to the distance
if the students had the same attendance: $d=7$. Hardly different! The
attendance has minimal impact on the result.

We could convert attendance to a percentage too, so both features are
between 0 and 100. A better solution is to standardize each feature to
have mean 0 and a standard deviation of 1. This ensures no feature will
dominate the other(s), and each feature has the same spread (standard
deviation), so a change in one feature represents the same amount of
change as in another feature. All features will contribute equally to
the inertia. Feature scaling is essential for $k$-means. Without it, the
algorithm will produce meaningless clusters.

# DBSCAN

**Density-Based Spatial Clustering of Applications with Noise** (DBSCAN)
is a clustering method that identifies clusters based on point density.
Unlike $k$-means, DBSCAN does not require specifying the number of
clusters in advance. It can also find clusters of arbitrary shape
($k$-means can only find circular clusters. Why? Because of the
Euclidean distance. To convince yourself of this, think about the
equation of a circumference). Perhaps most importantly, since in this
algorithm we define a concept of "density", we also have a concept of
"isolation" such that this method can also identify *outliers* (or
*anomalies*) that don't belong to any cluster.

DBSCAN classifies points into three categories: **core points**, points
that have at least `min_samples` points within $\epsilon$ distance;
**border points**, points that are within $\epsilon$ of a core point but
are not core points themselves; **noise points**, points that are
neither core nor border points.

We can see that DBSCAN has two parameters: epsilon, $\epsilon$, and
`min_samples`. Epsilon is the radius of a neighborhood around a point,
and `min_samples` is the minimum number of points to form a dense
region. Clusters are formed by connecting core points and their border
points. Noise points are left unassigned. We call them outliers.
Choosing epsilon and `min_samples` is not trivial. There are techniques
to help, but they are beyond the scope of this lesson. DBSCAN is
powerful because it can find clusters of any shape and is robust to
outliers, but it struggles when clusters have different densities, and
it is very sensitive to the choice of epsilon and `min_samples`.

# Agglomerative Clustering

**Agglomerative clustering** starts with each point as its own cluster.
It then repeatedly merges the two closest clusters until all points are
in a single cluster. The result is a tree-like structure called a
**dendrogram.** [1.10](#fig:dendrogram) shows such a dendrogram where each cluster
at the top (each cluster represents only one point at the top) is
successively combined with the nearest clusters until there is only one
cluster.

![An example dendrogram. Image credit: Mhbrugman
(Wikipedia).](../figures_pedagogy/Hierarchical_clustering_simple_diagram.svg.png)

This type of clustering is very computationally expensive. You can
expect it to take a long time to run on large datasets. But the method
is very useful when you don't know how many clusters the data should
have; making a dendrogram can help you visualize how many clusters there
should be.

[^16]: In practice, this would be a very risky stopping condition
    because there may be unstable configurations where points swap back
    and forth from the clusters. What we actually use is "repeat until
    the inertia does not change by more than a chosen threshold".