# Partitional clustering

## Clustering by hand

Consider this dataset.

```
     Sample     $X$  
  ------------ ----- 
       #1        1   
       #2        2   
       #3        5   
       #4        6  
``` 

Run one iteration of K-means with two clusters initialized with
centroids $\mu_1=1$ and $\mu_2=6$. Report the two new centroid values.

:::{note} Solutions
:class: dropdown
Points #1 and #2 get assigned to cluster 1. Points #3 and #4 get
assigned to cluster 2. So the new means are $\mu_1=1.5$ and $\mu_2=5.5$.
:::

## WSS, BSS, TSS, part I

Consider a dataset $\mathcal{D}=\{X_1,\ldots X_n\}$ that has been
clustered into 3 groups. Let $Y_i \in \{1,2,3\}$ denote the cluster
assignments. Recall that the TSS, BSS, and WSS are defined by the
following series of formulae. $$\begin{aligned}
    \bar \mu &= \sum_{i=1}^n X_i / n\\
    \mu_k &= \sum_{i:\ Y_i=k} X_i / |\{i:\ Y_i=k\}| \qquad k \in \{1,2,3\} \\
    \mathrm{WSS} &= \sum_i \Vert X_i - \mu_{Y_i}\Vert^2\\
    \mathrm{BSS} &= \sum_{k=1}^3 |\{i:\ Y_i=k\}| \Vert \mu_k - \bar\mu\Vert^2\\
    \mathrm{TSS} &= \sum_i \Vert X_i - \mu \Vert^2\\
\end{aligned}$$

Which of the following gives the relationship between WSS, BSS, and TSS?

1.  TSS = WSS + BSS

2.  WSS = TSS + BSS

3.  BSS = TSS + WSS

:::{note} Solutions
:class: dropdown
First answer is correct.
:::

## WSS, BSS, TSS, part II

At each iteration of K-means clustering, which of the following occurs?

1.  TSS won't increase (and might decrease)

2.  WSS won't increase (and might decrease)

3.  BSS won't increase (and might decrease)

:::{note} Solutions
:class: dropdown
Second option. BSS won't decrease (and might increase). TSS will stay
the same (doesn't depend on cluster labels).
:::

## What does Kmeans do?

Which is the most accurate description of what Kmeans clustering
algorithm does?

1.  Kmeans attempts to find clusters so that all points in the same
    cluster are close to each other.

2.  Kmeans finds an optimal clustering, in the sense that it identifies
    a clustering so that the sum of distances between points within each
    cluster is as small as possible.

3.  Kmeans attempts to find clusters so that all points in the same
    cluster are far from each other.

4.  Kmeans finds an optimal clustering, in the sense that it identifies
    a clustering so that the sum of distances between points within each
    cluster is as large as possible.

:::{note} Solutions
:class: dropdown
First option is correct. Kmeans is greedy, may not find anything
optimal. Moreover, "sum of distances between points within each cluster"
is a bit vague. It is true that Kmeans minimizes the WSS, and this can
be related to a quantity involving the distances between all pairs of
points in each cluster. Specifically,
$$\mathrm{WSS} = \sum_i \Vert X_i -\mu_{Y_i}\Vert^2 = \frac{1}{2} \sum_k \sum_{i,j:\ Y_i=k} \Vert X_i -X_j \Vert^2 / |\{i:\ Y_i=k\}|$$
:::

## Silhouette

Consider a dataset $\mathcal{D}=\{X_1,\ldots X_n\}$ that has been
clustered into 3 groups. Let $Y_i \in \{1,2,3\}$ denote the cluster
assignments. Let $C_k = \{i:\ Y_i=k\}$. Recall that the Silhouette
coefficient $s_i$ for each point is defined by the following series of
formulae.

$$\begin{aligned}
    a_i &= \sum_{j \in C_{Y_i} \backslash i} \Vert X_i - X_j \Vert / |C_{Y_i}-1| \\
    b_i &= \min_{y \neq Y_i} \sum_{j \in C_{y}} \Vert X_i - X_j \Vert / |C_y| \\
    s_i &= (b_i-a_i) / \max(a_i,b_i)
\end{aligned}$$

Which of the following statements best describes $a_i,b_i,s_i$?

a.  $-a_i$ is a measure of cohesion, $b_i$ is a measure of separation,
    and large values of $s_i$ indicate the clustering is pretty good.

b.  $a_i$ is a measure of separation, $-b_i$ is a measure of cohesion,
    and large values of $s_i$ indicate the clustering is pretty good.

c.  $-a_i$ is a measure of cohesion, $b_i$ is a measure of separation,
    and large values of $s_i$ indicate the clustering is pretty bad.

d.  $a_i$ is a measure of separation, $-b_i$ is a measure of cohesion,
    and large values of $s_i$ indicate the clustering is pretty bad.

:::{note} Solutions
:class: dropdown
\(a\) is correct. Higher values of $a_i$ mean less cohesion (so higher
values of $-a_i$ mean more cohesion). Higher values of $b_i$ mean more
separation. We want lots of cohesion and lots of separation, so we want
$b_i-a_i$ to be large and positive.
:::

