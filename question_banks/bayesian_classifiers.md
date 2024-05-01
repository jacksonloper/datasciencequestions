# Bayesian classifiers

## Gaussian posteriors vs logistic regression

In a two-class classification problem when the class conditional
distributions $f(X|Y=0)$ and $f(X|Y=1)$ are modelled as Gaussians with
equal covariance matrices, the posterior probabilities always turn out
to be the same as those predicted by the logistic regression. True or
false?

:::{note} Solutions
:class: dropdown
False. The problem essentially describes LDA. The final predictions of
LDA have the same parametric form as that found by logistic
regression---but LDA is not equivalent to logistic regression! The
parameters get estimated differently by the two methods. LDA will work
better if the class-conditionals actually are Gaussians with common
covariance. Otherwise, hard to say.
:::

## LDA vs logistic regression

You fit LDA on a classification dataset with one predictor, $X$. Assume
the response $Y$ takes on two possible values, $Y \in \{-1,+1\}$. Let
$\hat p(y|x)$ denote the estimated conditional probability density
function. True or false: there exists $\beta_0,\beta_1$ such that
$$\log \frac{\hat p(+|x)}{\hat p(-|x)} = \beta_0 + \beta_1 x$$

:::{note} Solutions
:class: dropdown
True.
:::

## QDA vs logistic regression with derived features

You fit QDA on a classification dataset with one predictor, $X$. Assume
the response $Y$ takes on two possible, values, $Y \in \{-1,+1\}$. Let
$\hat p(y|x)$ denote the estimated conditional probability density
function. True or false: there exists $\beta_0,\beta_1,\beta_2$ such
that
$$\log \frac{\hat p(+|x)}{\hat p(-|x)} = \beta_0 + \beta_1 x + \beta_2 x^2$$

:::{note} Solutions
:class: dropdown
True.
:::

## QDA vs logistic regression with derived features, II

You fit QDA on a classification dataset with two predictors, $X_1,X_2$.
Assume the response $Y$ takes on two possible, values,
$Y \in \{-1,+1\}$. Let $\hat p(y|x)$ denote the estimated conditional
probability density function. True or false: there must exist
$\beta_0,\beta_1,\beta_2,\beta_3,\beta_4$ such that
$$\log \frac{\hat p(+|x)}{\hat p(-|x)} = \beta_0 + \beta_1 x_1 + \beta_2 x_1^2 + \beta_3 x_2 + \beta_4 x_2^2$$

:::{note} Solutions
:class: dropdown
False! But there do exist $\beta_0,\ldots \beta_5$ such that
$$\log \frac{\hat p(+|x)}{\hat p(-|x)} = \beta_0 + \beta_1 x_1 + \beta_2 x_1^2 + \beta_3 x_2 + \beta_4 x_2^2 + \beta_5 x_1 x_2$$
With $p$ features the log odds will be guaranteed to take the form
$$\beta_0 + \sum_{i=1}^p x_i \beta_i + \sum_{i=1}^p \sum_{j=1}^p x_i x_j D_{i,j}$$
for some $\beta \in \mathbb{R}^{p+1}$ and some symmetric matrix
$D \in \mathbb{R}^{p \times p}$.
:::

## LDA vs logistic regression with derived features

You fit LDA on a classification dataset with one predictor, $X$. Assume
the response $Y$ takes on two possible, values, $Y \in \{-1,+1\}$. Let
$\hat p(y|x)$ denote the estimated conditional probability density
function. True or false: there exists $\beta_0,\beta_1,\beta_2$ such
that
$$\log \frac{\hat p(+|x)}{\hat p(-|x)} = \beta_0 + \beta_1 x + \beta_2 x^2$$

:::{note} Solutions
:class: dropdown
True. But this is sort of a trick question. You can indeed find
$\beta_0,\beta_1,\beta_2$. But it will always be the case that
$\beta_2=0$.
:::

## Consistent classification with quadratic decision rule

Consider a binary classification problem in which it is possible to
perfectly predict the response from the predictors. In particular,
assume there are two predictors and $$y = \begin{cases}
    1&\mathrm{if}\ x_1^2 + x_2^2 > 10 \\
    0&\mathrm{otherwise}
\end{cases}$$ Which of these approaches can make the misclassification
rate arbitrarily close to zero on held-out test data, if we assume that
the training dataset can be as large as needed?

1.  LDA

2.  QDA

3.  KNN (with $K$ chosen by 5-fold cross-validation)

4.  Logistic regression

:::{note} Solutions
:class: dropdown
LDA and logistic regression will fail badly. QDA and KNN would both be
fine.

*Aside. You wouldn't actually have to choose $K$ by cross-validation due
to some special properties of this problem. In this very special case,
because the relationship between $X$ and $Y$ is deterministic, you could
still get a consistent estimator by setting $K=1$. However, this fact is
beyond the scope of what you will be expected to consider for the
midterm.*
:::

## Consistent classification with quadratic decision rule and derived features

Consider a binary classification problem in which it is possible to
perfectly predict the response from the predictors. In particular,
assume there are two predictors and $$y = \begin{cases}
    1&\mathrm{if}\ x_1^2 + x_2^2 > 10 \\
    0&\mathrm{otherwise}
\end{cases}$$ To obtain flexible models, a data scientist augments the
feature space with a derived feature $x_3 = x_1^2 + x_2^2$. Using the
new data (with three features), which of these approaches can make the
misclassification rate arbitrarily close to zero on held-out test data,
if we assume that the training dataset can be as large as needed?

1.  LDA

2.  QDA

3.  KNN (with $K$ chosen by 5-fold cross-validation)

4.  Logistic regression

:::{note} Solutions
:class: dropdown
All of them could be fine. KNN with derived features is slightly
unusual, but it will still work.
:::

## LDA by hand

Suppose that we wish to predict whether a given stock will issue a
dividend this year ("Yes" or "No") based on X, last year's percent
profit. We examine a large number of companies and discover that the
mean value of $X$ for companies that issued a dividend was
$\bar{X} = 20$, while the mean for those that didn't was $\bar{X} = 10$.
In addition, the variance of X for each of these two sets of companies
was $\sigma^2 = 25$. Finally, 75% of companies issued dividends.
Assuming that X follows a normal distribution, predict the probability
that a company will issue a dividend this year given that its percentage
profit was $X = 14$ last year.\
\
Assume $\phi(x;\mu,\sigma^{2})$ denotes the Gaussian density for a value
$x$, with mean $\mu$ and variance $\sigma^{2}$. That is,
$$\phi(x; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi \sigma^2}} \exp\left\{-\frac{(x-\mu)^2}{2\sigma^2}\right\}.$$
You may write your answer using $\phi$ (answer does not need to be
reduced to a number).

:::{note} Solutions
:class: dropdown
$$\begin{aligned}
    P(Y = \textrm{Yes} \mid X = 14) &= \frac{P(X = 14 \mid Y = \textrm{Yes}) P(Y = \textrm{Yes})}{P(X = 14)} \\
             &= \frac{\phi(14; 20, 25) \times 0.75}{f(14; 10, 25) \times 0.25 + \phi(14; 20, 25) \times 0.75}
\end{aligned}$$
:::

## LDA by hand, II

In the fictional region of Plantasia, agronomists are working to
distinguish between two species of magical herbs, Gillyweed and
Silverleaf, based on the level of a magical essence, Enchantol, found in
their stems. They've decided to employ Linear Discriminant Analysis
(LDA) for this task, leveraging the fact that Enchantol levels follow a
normal distribution in both herb species. The concentration of Enchantol
is recorded in enchantment units (EU). The researchers have documented
the following details:

1.  The prior probability of encountering Gillyweed is 0.65, and
    Silverleaf is 0.35.

2.  The mean Enchantol concentration in Gillyweed is 15 EU, and in
    Silverleaf, it is 30 EU.

3.  The variance of Enchantol concentration is consistently 20 EU$^{2}$
    for both species.

What is the LDA estimate of the Bayes optimal decision rule? Assume
$\phi(x;\mu,\sigma^{2})$ denotes the Gaussian density for a
concentration $x$, with mean $\mu$ and variance $\sigma^{2}$.

1.  Classify as Gillyweed if
    $\phi(x;15,20)\times0.65>\phi(x;30,20)\times0.35$

2.  Classify as Gillyweed if
    $\log(\phi(x;15,20)+0.65)>\log(\phi(x;30,20)+0.35)$

3.  Classify as Gillyweed if
    $\log(\phi(x;15,20))\times\log(0.65)>\log(\phi(x;30,20))\times\log(0.35)$

4.  Classify as Gillyweed if
    $\phi(x;15,20)\times\log(0.65)>\phi(x;30,20)\times\log(0.35)$

:::{note} Solutions
:class: dropdown
First option.
:::

## Bayes rule

Imagine machine learning researchers have developed a new binary
classification method called Octonian Discriminant Analysis (ODA). The
method first estimates a class-conditional probability density function
$f_{y}(x)$ and a prior probability $\pi_{y}$ for positive and negative
classes, $y\in\{0,1\}$. Unlike in LDA or QDA, the class-conditional
densities are not Gaussian. ODA uses Bayes rule to predict the
conditional distribution of $Y$ given $X$. Which is true about the ODA
estimate, $\hat{p}$?

1.  $\hat{p}(1|x)=\frac{\pi_{1}f_{1}(x)}{\pi_{0}f_{0}(x)}$

2.  $\hat{p}(1|x)=\pi_{1}f_{1}(x)$

3.  $\log\frac{\hat{p}(1|x)}{\hat{p}(0|x)}=\log\pi_{1}+\log f_{1}(x)-\log\pi_{0}-\log f_{0}(x)$

4.  $\frac{\hat{p}(1|x)}{\hat{p}(0|x)}=\pi_{1}f_{1}(x)-\pi_{0}f_{0}(x)$

5.  None of the above

:::{note} Solutions
:class: dropdown
Third option.

*Aside. It is in fact possible to find very strange distributions where
this sort of formula doesn't work: there are certain distributions which
cannot be expressed as a probability density function and also cannot be
expressed as a probability mass mass function (neither discrete nor
continuous). In such a case, the class-conditional distributions cannot
be expressed by a simple function like $f_+$ or $f_-$, so you have to do
some extra clever stuff. But these sorts of distributions are all
out-of-scope for this class.*
:::

## LDA with binary features

Consider a dataset with eight binary predictor variables and one binary
response. Assume the binary predictor variables are coded using 0s and
1s. Which of the following is most true?

1.  There is reason to hope that LDA will perform well in this context.

2.  We cannot apply LDA in this context, because the covariance of a
    binary random vector is not uniquely defined.

3.  We probably should not apply LDA in this context, because it is
    unlikely that the class-conditional covariances will be exactly the
    same for the two classes.

4.  We probably should not apply LDA in this context, because the
    class-conditional distributions do not admit probability density
    functions.

:::{note} Solutions
:class: dropdown
The last one is correct. The covariance is perfectly well defined
(because we have coded the predictors using 0s and 1s, so all the
features can be interpreted as numbers). The fact that the features are
binary does not preclude the possibility that the class-conditional
covariances would be the same. The strongest argument here is that LDA
models the class-conditionals as Gaussians, with nice smooth probability
density functions. But here we have categorical predictors, and these
are not going to look Gaussian in the least.
:::

## Fisher's discriminant plot

Fisher's discriminant plot gives us a way to visualize our data even if
we have more than two predictor features. The key idea is to project our
feature space down to a lower-dimensional representation that we can
actually look at. For example, given a classification problem with $K=3$
possible values for the response, Fisher's discriminant plot is designed
to identify a two-dimensional representation of our input space such
that\...

1.  Between-class variability appears small while within-class
    variability appears large.

2.  Between-class variability appears large while within-class
    variability appears small.

3.  The class-conditional variance appears isotropic (equal variance in
    all directions).

4.  The between-class variance appears isotropic (equal variance in all
    directions).

:::{note} Solutions
:class: dropdown
Second option.
:::

## Naive Bayes assumption

Consider a classification setting in which one seeks to predict a
response $Y$ from $p$ predictor features $X$. Naive Bayes is a
classification method that builds its predictions using a certain
independence assumption. If this assumption holds, which of the
following is guaranteed to be true?

1.  $\mathbb{E}[\prod_{i=1}^p X_i] = \prod_{i=1}^p \mathbb{E}[X_i]$.

2.  $\mathbb{E}[\prod_{i=1}^p X_i|Y=y] = \prod_{i=1}^p \mathbb{E}[X_i|Y=y]$
    for each $y$.

3.  $\prod_{i=1}^p \mathbb{E}[Y|X_i=x_i] = \mathbb{E}[Y|X=x]$ any $x$.

4.  $\prod_{i=1}^p \mathbb{P}(Y=y|X_i=x_i) = \mathbb{P}(Y=y|X=x)$ any
    $y,x$.

:::{note} Solutions
:class: dropdown
The second answer is correct. The assumption is that the $X$s are
independent after conditioning on upon $Y$. This is also called
"conditional independence": for any $y$ and any functions
$g_1,g_2,\ldots g_p$, we have that
$\mathbb{E}[\prod_{i=1}^p g_i(X_i)|Y=y] = \prod_{i=1}^p \mathbb{E}[g_i(X_i)|Y=y]$.
:::

## Class conditional for categorical predictor

Consider a binary classification task with two categorical predictors.
You would like to apply a naive Bayes estimator. Your first step is to
estimate the class-conditional for each predictor and each class. Let
$$((X_{1,1},X_{1,2},Y_1), (X_{2,1},X_{2,2},Y_2), \ldots (X_{n,1},X_{n,2},Y_n)$$
denote your dataset of $n$ samples. Assume that
$X_{i,1} \in \{1,2,3,\ldots n_1\}$ and
$X_{i,2} \in \{1,2,3,\ldots n_2\}$.

Which would be the most suitable estimator for
$\mathbb{P}(X_{i,1}=x|Y=y)$?

1.  $\sum_i \mathbb{I}(y_i=y\ \mathrm{and}\ x_{i,1}=x) / \sum_i \mathbb{I}(y_i=y)$

2.  $\sum_i \mathbb{I}(y_i=y) / \sum_i \mathbb{I}(y_i=y, x_{i,1}=x)$

3.  $\sum_i \mathbb{I}(y_i=y\ \mathrm{and}\  x_{i,1}=x) / \sum_{i,y'} \mathbb{I}(y_i=y')$

4.  $\sum_i (\mathbb{I}(y_i=y\ \mathrm{and}\  x_{i,1}=x) / \sum_{y'} \mathbb{I}(y_i=y'))$

:::{note} Solutions
:class: dropdown
First option. $\mathbb{P}(X_{i,1}=x|Y=y)$ means this: among cases where
$Y=y$, how often will first feature takes on the discrete value $x$? The
first answer gives an estimate of this, by counting the proportion in
the given dataset.
:::

