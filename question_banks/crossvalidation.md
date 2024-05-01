# Cross-validation

## Picking estimators

You are given a dataset with $n=10$ points. Using this data, you hope to
learn how to guess a binary response $Y$ from input features $X$. You
have been studying machine learning in-depth and have learned about $17$
completely different approaches for this kind of classification problem.
You assess the quality of each of these estimators using $5$-fold
cross-validation and learn that the fourth estimator seems to achieve
the best performance. You then gain access to another dataset with 10
more points. You may assume the new dataset is drawn from the same
distribution as the first. You then train each of the 17 estimators on
the first dataset and evaluate it on the new dataset. True or false: in
most cases, the fourth estimator will perform better than all others on
the new held-out data.

:::{note} Solutions
:class: dropdown
False. You have tried too many different estimators. With finite data,
cross-validation will fail to select the best estimator when it is asked
to compare too many different estimators. If $n=1000$ then your approach
might have been reasonable, but with 10 samples, trying 17 different
approaches is asking too much.
:::

## Picking estimators, II

You are given a dataset with $n=1000$ points concerning many predictors
and one numerical response. You consider two estimators:
ridge-regularized linear regression and LASSO-regularized linear
regression. For both methods, assume that the regularization strengths
are chosen by cross-validation. To see which approach is better, you try
5-fold cross-validation (note that this is a form of nested
cross-validation, since the estimators themselves use cross-validation
to pick regularization strength). The results suggest that LASSO is a
bit better. You then decide to try LOOCV instead. With LOOCV it appears
that LASSO is a bit worse. Which would be the more appropriate
interpretation of these findings?

1.  LASSO is a bit worse (LOOCV gives a more reliable assessment, though
    it has the disadvantage of being more computationally intensive)

2.  LASSO is a bit better (LOOCV gives a less reliable assessment,
    though it has the advantage of giving the same result every time
    because it does not require random resampling)

3.  Can't tell.

:::{note} Solutions
:class: dropdown
Third answer is correct. Honestly in most situations there's no strong
reason to favor either approach for estimating the quality of an
estimate. They both perform okay in different scenarios. K-fold is the
standard guidance, with $K=5$ or $K=10$, but that shouldn't be taken as
a sign that LOOCV is really less reliable. If LOOCV and K-fold are
giving you different answers there's a good chance that you just don't
have enough signal to really know which estimator is really better.
:::

## Picking regularization strength

You are given a dataset with $n=15$ points. Using this data, you hope to
learn how to guess a binary response $Y$ from input features $X$. You
know you want to use ridge-regularized logistic regression, but you're
not sure what value of $\lambda$ to use. Your plan is to select a grid
of values for $\lambda$, use cross-validation to assess the quality of
the estimator based on each value, and then select the best. True or
false: from a statistical standpoint, it is a better idea to consider
only relatively few possible values of $\lambda$, e.g.
$\lambda \in \{.01,.1,1.0,10,100\}$. Considering too many choices for
$\lambda$ leads to a kind of over-fitting.

:::{note} Solutions
:class: dropdown
False. Compare this carefully with the problem above. In the problem
above, we were considering 17 completely different kinds of estimators.
Here we are considering only a single estimator, and figuring out how to
tune *a single hyperparameter*. Choosing a denser grid of $\lambda$s in
this case is fine, e.g. considering each value
$\lambda \in \{10^{-2},10^{-1.9},\ldots 10^{1.9},10^2\}$ would be just
fine.
:::

## LOOCV by hand

Consider a dataset concerning a single numerical response
$Y \in \mathbb{R}$. The dataset has no predictors. Here is the dataset.

```
   Sample ID   Response
  ----------- ----------
      #1         2.0
      #2         4.0
      #3         3.0
```

We seek to estimate the regression function. In this degenerate case,
the regression function is simply the expected value of $Y$,
$\mu = \mathbb{E}[Y]$. For our estimator, we will use the sample
average. For example, applying this estimator to the full dataset, we
obtain $\hat \mu =(2+4+3)/3=3$.

In this problem, you will perform LOOCV by hand to estimate
$\mathbb{E}[(Y-\hat \mu)^2]$. For each test-train split, use the sample
average of the training samples to estimate $\mu$ and evaluate the
squared difference between your estimate and the held-out test sample.
Your final answer will be the average of these squared differences. Your
final answer does not need to be written in a simplified form (however,
here as elsewhere your answer must must be highly legible!).

:::{note} Solutions
:class: dropdown
The answer is $1.5$. A less-simplified form, $(1.5^2+1.5^2+0)/3$, would
also be acceptable.

> Leaving out the first sample, we get a training dataset of $\{4,3\}$.
> This leads to the sample average $3.5$. Testing this prediction on the
> held-out point ($2$), we get $\epsilon_1 = (3.5-2)^2=1.5^2$.
>
> Leaving out the second sample, we get a training dataset of $\{2,3\}$.
> This leads to the sample average $2.5$. Testing this prediction on the
> held-out point ($4$), we get $\epsilon_2 = (2.5-4)^2=1.5^2$.
>
> Leaving out the third sample, we get a training dataset of $\{2,4\}$.
> This leads to the sample average $3$. Testing this prediction on the
> held-out point ($3$), we get $\epsilon_3=0$.

Averaging over the three test/train splits, we get a squared error of
$((3/2)^2+(3/2)^2 + 0)/3 = (2/3) (3/2)^2 = 3/2$.
:::

