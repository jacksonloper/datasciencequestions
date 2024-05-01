# Classification decision rules (aka hard classification)

## Estimating metrics from data

A binary classifier $\hat{y}$ was applied to test data and its
predictions were compared with the true responses. The following was
found.

1.  There were 20 cases where the classifier predicted + and the truth
    was +.

2.  There were 30 cases where the classifier predicted - and the truth
    was +.

3.  There were 10 cases where the classifier predicted + and the truth
    was -.

4.  There were 40 cases where the classifier predicted - and the truth
    was -.

Given these numbers, estimate $\mathbb{P}\left(Y=+|\hat{y}(X)=+\right)$
and $\mathbb{P}\left(\hat{y}(X)=+|Y=+\right)$

:::{note} Solutions
:class: dropdown
This question asks two things.

1.  First, there were 30 cases where the prediction was positive. Among
    those 30 cases, there were 20 cases where the truth was positive.
    Thus we have reason to suppose that
    $$\mathbb{P}\left(Y=+|\hat{y}(X)=+\right)\approx\frac{2}{3}.$$

2.  Second, there were 50 cases where the truth was positive. Among
    those cases, there were 20 cases where the prediction was positive.
    Thus we have reason to suppose that
    $$\mathbb{P}\left(\hat{y}(X)=+|Y=+\right)\approx\frac{2}{5}.$$
:::

## A better classifier

We would like to design a method that guesses whether a plant is poison
ivy, based on urushiol measurements. Given a measurement $x$, we have a
very accurate estimate of the probability that the plant is poison ivy,
$p(\mathrm{poison}|x)$. Using this estimate we constructed a hard
classifier of the form $$\hat y(x) = \begin{cases}
    \mathrm{poison} & \mathrm{if}\ p(\mathrm{poison}|x)>.2 \\
    \mathrm{poison} & \mathrm{otherwise}
\end{cases}$$ In tests, we found the classifier had a false positive
rate of 30%
($\mathbb{P}(\hat y(X)=\mathrm{poison}|Y\neq \mathrm{poison})=0.3$) and
a true positive rate of 70%
($\mathbb{P}(\hat y(X)=\mathrm{poison}|Y=\mathrm{poison})=0.7$). True or
false: in typical situations, it will be very difficult to find a new
hard classifier that achieves the same false positive rate (30%) while
attaining a substantially higher true positive rate (say, 75%).

:::{note} Solutions
:class: dropdown
True. We built the hard classifier by thresholding on a high-quality
estimate of $p$, so there's little reason to hope we can get something
that has the same FPR yet higher TPR. (Here's the same sentiment but
with a bit more rigor: under mild conditions Neyman Pearson lemma tells
us that if we in fact have a perfect estimate of $p$ then there will be
no way to get a higher TPR will keeping the FPR the same). Of course,
there might be a classifier that has slightly higher FPR (say, 31%) and
much higher TPR (e.g., 100%).

In general, no one hard classifier can ever be said to be the "best." It
depends on what matters to you. Is higher TPR (good) worth the higher
FPR that may come with it (bad)?

If you're viewing things from a precision/recall point of view, a
similar tradeoff appears. Is higher recall (good) worth the lower
precision that might come with it (bad)? It just depends on what matters
to you.
:::

## Decision boundaries

Let $\hat y(x)$ denote a hard classifier. Which is true about the
decision boundary of this classifier?

1.  It is not defined; the idea of a "decision boundary" is only
    well-defined for estimates of the conditional probability.

2.  If $x$ is on the decision boundary, then there are two points
    $x_1,x_2$ that are very close to $x$ such that
    $\hat y(x_1)\neq \hat y(x_2)$.

3.  If $x$ is on the decision boundary, then there is some $\epsilon>0$
    such that $\hat y(\tilde x)=\hat y(x)$ for all $\tilde x$ such that
    $\Vert x -\tilde x\Vert <\epsilon$.

:::{note} Solutions
:class: dropdown
Second option.
:::

## ROC

Let $\hat y(x)$ denote a hard classifier. Given a test dataset, how
should we plot an ROC curve for this classifier?

1.  It cannot be plotted from a hard classifier.

2.  Considering all possible thresholds $t$, compute
    $\mathbb{P}(\hat y(X)=+|Y=+)/t$ and $\mathbb{P}(\hat y(X)=+|Y=-)/t$.
    Plot the values you obtain.

3.  Considering all possible thresholds, compute
    $\mathbb{P}(\hat y(X)=+|Y=+)/t$ and $\mathbb{P}(Y=+|\hat y(X)=+)/t$.
    Plot the values you obtain.

:::{note} Solutions
:class: dropdown
First option takes it. To make an ROC curve, you need some sort of soft
classifier (e.g., an estimate of the log odds).
:::

