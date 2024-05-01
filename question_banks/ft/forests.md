# Forests

## How big a forest?

In using bagging forests, you have to pick the number of trees. True or
false: in the limit as the number of trees goes to infinity, the
training error will always converge to zero.

:::{note} Solutions
:class: dropdown
False. Training error will not typically converge to zero (unless
training error was already zero for a single tree because you used way
too many leaves).
:::

## Feature importance

Many packages allow you compute feature importance based on an ensemble
of trees. In the context of estimating a regression function, each
feature importance values indicate the extent to which each feature was
helpful in reducing the mean squared predictive error on the training
data. More specifically\...

1.  The importance value for each feature is proportional to the total
    MSPE improvement due to splitting with that feature.

2.  The importance values are computed by first taking the proportion of
    total MSPE improvement due to splitting with each feature. The
    importance value for each feature is then computed as the negative
    log of the corresponding proportion.

3.  The importance value for each feature is equal to the exponentiated
    total MSPE improvement due to splitting with that feature.

:::{note} Solutions
:class: dropdown
First answer is correct. For each feature $j$, we sum over all trees and
all nodes that split using feature $j$, compute the level of MSPE
improvement that occurred when that node was created, and add it all up.
:::

## Random forests vs boosting forests

Which is true?

1.  Boosting forests are built one at a time, each tree built using
    information from all the trees that have already been built. By
    contrast, each tree in a random forest is built independently.

2.  Random forests are built one at a time, each tree built using
    information from all the trees that have already been built. By
    contrast, each tree in a boosting forest is built independently.

:::{note} Solutions
:class: dropdown
First one is true.
:::

## Boosting by hand

You have a dataset with three samples. For each sample you have one
predictor $X$ and one response $Y$. You also have already used a tree to
fit an estimate, $\hat f(x)\approx \mathbb{E}[Y|X=x]$. The dataset and
the current estimate $\hat f(x)$ are shown below.

```
     Sample      X     \hat f(X)     Y 
  ------------ ----- ------------- -----
       #1        3        4.5       4.5
       #2        7         6        5.0
       #3        8         6        7.0
```

Now you would like to use boosting. You want to fit a second tree that
improves on the results of the first. You decide your new tree will have
only two leaves. Let $\hat g(x)$ denote the function associated with the
new tree. What is the function?

:::{note} Solutions
:class: dropdown
It's helpful to compute the residuals.

```
     Sample      X     \hat f(X)     Y    residual
  ------------ ----- ------------- ----- ----------
       #1        3        4.5       4.5      0
       #2        7         6        5.0      -1
       #3        8         6        7.0      1
```

We can either split around $X=5$ (between sample #1 and samples #2,#3)
or split around $X=7.5$ (between samples #1,#2 and sample #3). Splitting
around $X=5$ won't lead to any reduction in error. Splitting at $X=7.5$
can help:

$$\hat g(x) = \begin{cases}
    -.5 & \mathrm{if}\ X<7.5 \\
    1 & \mathrm{if}\ X<7.5.
\end{cases}$$

With a learning rate of 1, we would produce a final prediction of
$\hat f + \hat g$, leading to\...

```
     Sample      X     \hat f(X)+ \hat g(X)     Y 
  ------------ ----- ------------------------ -----
       #1        3             4.0             4.5
       #2        7             5.5             5.0
       #3        8             7.0             7.0
```

This final prediction is actually a bit worse for sample #1, but overall
it is better.
:::

