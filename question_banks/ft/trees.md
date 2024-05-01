# Trees

## Sunlight cafe, part I

Imagine you run a small cafe, and you're trying to predict the daily
revenue ($Y$) based on the number of hours of sunlight for the day
($X$). You believe that more sunlight encourages more people to go out,
thereby potentially increasing your revenue.

You'd like to approximate $f^*(x)=\mathbb{E}[Y|X=x]$ and you're going to
do so by fitting a regression stump (i.e. a regression tree with one
internal node). Which of the following describes the class of estimators
that you could obtain?

a.  $f(x;\beta) = \beta_0 + \beta_1 x$, with $\beta \in \mathbb{R}^2$

b.  $f(x;\beta,s) = \beta_0 + \beta_1 x + \mathbb{I}(x\geq s)\beta_2$,
    with $\beta \in \mathbb{R}^3$

c.  $f(x;a,b,s) = a + \mathbb{I}(x\geq s)b$ for $a,b,s \in \mathbb{R}$,

d.  $f(x;\beta,\gamma,s) = (\beta_0 + \beta_1 x)\mathbb{I}(x<s) +  (\gamma_0 + \gamma_1 x)\mathbb{I}(x\geq s)$,
    with $\beta,\gamma \in \mathbb{R}^2$ and $s \in \mathbb{R}$

:::{note} Solutions
:class: dropdown
c\. Though it is a bit of an unconventional way of writing it. We would
more often think of it as being defined through $\mu_1$ (mean in left
child) $\mu_2$ (mean in right child) and $s$, split. Family of possible
functions would then be
$$f(x;\mu_1,\mu_2,s) = \mathbb{I}(x< s)\mu_1 + \mathbb{I}(x\geq s)\mu_2.$$
However, the family of functions you can represent this way is the same
as the family you can represent with
$f(x;a,b,s) = a + \mathbb{I}(x\geq s)b$.
:::

## Sunlight cafe, part II

Consider the dataset

```
   **Sample**   **Hours of Sunlight**   **Revenue**
  ------------ ----------------------- -------------
       #1                 3                 4.5
       #2                 7                 5.0
       #3                 8                 7.0
```

Fit a regression stump to this dataset to predict revenue from hours of
sunlight. What is the estimate of $\mathbb{E}[Y|X=x]$?

:::{note} Solutions
:class: dropdown
It would be $$\hat f(x) = \begin{cases}
    4.75 & x<7.5 \\
    7 & x\geq 7.5.
\end{cases}$$ This solution has the smallest possible MSPE among all
possible decision stumps (because it groups the two samples together
that are closer in response).

Note that other choices such as $$f(x) = \begin{cases}
    4.75 & x<7.4 \\
    7 & x\geq 7.4
\end{cases}$$ would also be just as good. The choice to use $7.5$
(midpoint between observed datapoints) is conventional.
:::

## Woodchuck behavior, part I

Imagine you run a small woodchuck sanctuary, and you're trying to
predict whether a woodchuck will come out
($Y \in \{\mathrm{no},\mathrm{yes}\}$) based on the number of hours of
sunlight for the day ($X$). You believe that more sunlight encourages
woodchucks to come out more.

You'd like to estimate $p(y|x)=\mathbb{P}(Y=\mathrm{yes}|X=x)$ and
you're going to estimate it by fitting a decision tree.

In fitting this tree to a set of training data
$\mathcal{D}=\{(X_1,Y_1),\ldots(X_n,Y_n)\}$, we must search among
different trees in search of one that best fits the training data. In
this search, which of the following would be a typical *loss*? That is,
which of the following would be the most typical way to characterizes
how *poorly* a given estimate $\hat p(y|x)$ fits the data?

1.  $L = \sum_i \hat p(y_i|x_i)$

2.  $L = -\sum_i \hat p(y_i|x_i)$

3.  $L = \sum_i \log \hat p(y_i|x_i)$

4.  $L = -\sum_i \log \hat p(y_i|x_i)$

:::{note} Solutions
:class: dropdown
Last answer, negative log likelihood, is correct.

There are actually other forms of loss that you'll see (e.g., involving
Gini coefficients), but of the listed answers only (d) is commonly used.
:::

## Woodchuck behavior, part II

Consider the dataset

```
   **Sample**   **Hours of Sunlight**   **Woodchuck appears**
  ------------ ----------------------- -----------------------
       #1                 3                      No
       #2                 7                      No
       #3                 8                      Yes
```

If we fit a classification stump to this dataset to predict revenue from
hours of sunlight, what would the final estimate be?

:::{note} Solutions
:class: dropdown
We would end up with $$p(\mathrm{yes}|x) = \begin{cases}
    0 & x<7.5 \\
    1 & x\geq 7.5.
\end{cases}$$ Note that this answer is probably not ideal. Given that we
only have three training samples, it is probably pretty unwise to
declare $p(\mathrm{yes}|3)=0$!
:::

## A tree: test and train

Say you fit a decision tree to predict a numerical response variable $Y$
from predictor variables $X$, based on a dataset $\mathcal{D}^{(tr)}$.
You fit the tree with the "max leaves" parameter set to $9$. You also
have a held-out dataset $\mathcal{D}^{(te)}$. Which will typically be
true?

a.  Among trees with 9 leaves, your estimate will have the smallest mean
    squared predictive error on the training data (but not the testing
    data).

b.  Among trees with 9 leaves, your estimate will have the smallest mean
    squared predictive error on the training data and the testing data.

c.  Among trees with 9 leaves, your estimate will have the smallest mean
    squared predictive error on the testing data (but not the training
    data).

d.  None of the above.

:::{note} Solutions
:class: dropdown
d, none of the above. Trees are fit greedily to *try* to minimize mean
squared predictive error on training data---but the fitting procedure
doesn't typically find the tree that achieves the minimum error on the
training data.
:::

## Tree augmentation

You are considering a regression problem with $10$ binary predictor
variables. You have been given a decision tree with $20$ leaf nodes and
a dataset $\mathcal{D}$ with $300$ samples. You would like to augment
the tree to make it a little bit bigger and reduce the mean squared
predictive error on the dataset.

To perform the augmentation, you will search among a set of possible
trees--each a bit larger than the tree you started with. How many trees
will you consider?

:::{note} Solutions
:class: dropdown
$200$ -- for each of the $10$ predictors and each of the $20$ leaf
nodes, you'll consider a new tree formed by splitting that leaf node
using that predictor. $201$ would also be an acceptable answer (to
include the original tree in the list of trees you consider).
:::

## Max depth

In fitting a decision tree, the "max depth" hyperparameter sets the
maximum possible depth for the tree you fit. Which is true about how
this hyperparameter for fitting decision trees affects the quality of
the estimator?

1.  More depth typically leads to higher bias and higher variance.

2.  More depth typically leads to higher bias and lower variance.

3.  More depth typically leads to lower bias and higher variance.

4.  More depth typically leads to lower bias and lower variance.

:::{note} Solutions
:class: dropdown
Lower bias, higher variance. More depth means more flexibility (and more
parameters!).
:::

## Min leaf samples

The "min leaf sample" is a hyperparameter sometimes used in fitting
decision trees. If set, it ensures that we only consider trees with a
particular relationship with the training data. Specifically, if the
parameter value is set to $\tilde n$, it requires that for each leaf
there are at least $\tilde n$ training points inside the region
associated with that leaf. Which is true about how this hyperparameter
for fitting decision trees affects the quality of the estimator?

1.  Higher $\tilde n$ typically leads to higher bias and higher
    variance.

2.  Higher $\tilde n$ typically leads to higher bias and lower variance.

3.  Higher $\tilde n$ typically leads to lower bias and higher variance.

4.  Higher $\tilde n$ typically leads to lower bias and lower variance.

:::{note} Solutions
:class: dropdown
Higher bias, lower variance. Higher $\tilde n$ means there are more
samples in each leaf, so we get relatively low variance estimates for
what's going on in each leaf. But it also means that we cannot have too
many leaves (indeed, we can have at most $n/\tilde n$). And fewer leaves
means less flexibility.
:::

## Trees versus logistic regression

Compared with the logistic regression estimator, which best
characterizes decision tree classification estimators?

1.  Trees are higher bias and higher variance

2.  Trees are higher bias and lower variance

3.  Trees are lower bias and higher variance

4.  Trees are lower bias and lower variance

5.  None of the above

:::{note} Solutions
:class: dropdown
None of the above. Depending on how you set hyperparameters, trees can
be very high variance or very low variance. Amount of bias also depends
a lot on the true underlying distribution you are trying to estimate. A
tree estimator typically has a lot of bias bias if the real relationship
between the predictors and the response is linear. On the other hand,
trees are capable of being very low bias if you allow enough leaves.
:::

## Leaf nodes

Consider a decision tree for a problem with $p$ numerical predictors.
Each leaf node $i$ in a decision tree is associated with a subset
$S_i \subset \mathbb{R}^p$. Assume the tree has $k$ leaf nodes. Which is
true?

1.  $\bigcup_{i=1}^k S_i = \mathbb{R}^p$

2.  $\bigcup_{i=1}^k S_i \neq \mathbb{R}^p$, but
    $\bigcup_{i=1}^k S_i \subset \mathbb{R}^n$

3.  $\bigcup_{i=1}^k S_i \neq \mathbb{R}^p$, but
    $\bigcup_{i=1}^k S_i \supset \mathbb{R}^n$

:::{note} Solutions
:class: dropdown
First answer is correct. There is a unique leaf associated with every
possible value of $X\in \mathbb{R}^p$.
:::

## An internal node

Each internal node of a decision tree is associated with a rule. The
rule is used to decide whether a given query point $x$ should go to the
left child or the right child. In the context of $p$ numerical
predictors, which of the following best describes the set of possible
rules that might be associated to an internal node?

1.  LEFT if $\beta_0 + \sum_{i=1}^p x_i \beta_i$ and RIGHT otherwise
    (for some $\beta \in \mathbb{R}^{p+1}$)

2.  LEFT if $x_i<s$ and RIGHT otherwise (for some feature
    $i\in \{1,\ldots,p\}$ and some $s \in \mathbb{R}$)

3.  LEFT if $x_i > x_j + s$ and RIGHT otherwise (for some features
    $i,j\in \{1,\ldots,p\}$ and some $s \in \mathbb{R}$)

:::{note} Solutions
:class: dropdown
Middle answer is correct.

There are some tree-fitting packages with native support for categorical
predictors that can look at other kinds of rules (e.g., if
$x_i \in \{1,2,3,4,5\}$ they can consider rules like LEFT if
$x_i \in \mathrm\{1,4,5\}$ and RIGHT otherwise). But for numerical
predictors this is pretty much all you will see.
:::

