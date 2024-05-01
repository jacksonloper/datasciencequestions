# Featurization

## Estimator variance with polynomial features

Consider the following distribution on $\left(X,Y\right)$
$$\begin{aligned}
X & \sim\mathrm{Uniform}\left([-10,-8]\cup[8,10]\right)\\
\left[Y|X=x\right] & \sim\mathcal{N}\left(x^{2},1\right).
\end{aligned}$$ Imagine we gathered a dataset $\mathscr{D}$ sampled from
this distribution and used it to produce an estimate
$\hat{f}(x;\mathscr{D})\approx\mathbb{E}\left[Y|X=x\right]$. Assume we
used least squares with polynomial features of degree 9. True or false:
$$\mathrm{var}\left(\hat{f}(0;\mathscr{D})\right)>\mathrm{var}\left(\hat{f}(9;\mathscr{D})\right)$$

:::{note} Solutions
:class: dropdown
True! We will never see points where $X\approx0$ is close to zero. The
result is that our estimates of $\mathbb{E}\left[Y|X=0\right]$ will be
quite bad.
:::

## Estimator variance with polynomial features, II

Consider a regression problem with one predictor $X$ and one numerical
response $Y$. Assume the predictor $X$ is distributed as
$X \sim \mathcal{N}(0,1)$. Let $\hat f_A(x)$ denote the least squares
estimator for the regression function, based on natural cubic spline
features. Let $\hat f_B(x)$ denote the least squares estimator for the
regression function, based on some cubic spline features (including
features that are not natural cubic splines). Of the following, which is
the best explanation for why $f_A$ might be preferred?

1.  $\hat f_A(x)$ will have less variance than $\hat f_B(x)$, especially
    when $|x|$ large.

2.  $\hat f_A(x)$ will have less variance than $\hat f_B(x)$, especially
    when $|x|$ is small.

3.  $\hat f_A(x)$ will have less bias than $\hat f_B(x)$, especially
    when $|x|$ is large.

4.  $\hat f_A(x)$ will have less bias than $\hat f_B(x)$, especially
    when $|x|$ small.

:::{note} Solutions
:class: dropdown
First answer. Natural cubic splines and unconstrained cubic splines
generally give same answers near the "middle" of the data. But for
values $x$ that are far away from observed data, unconstrained cubic
spline features (e.g. B-spline features) lead to very large variance. So
we expect
$$\mathrm{var}(f_A(1000;\mathscr{D})) \ll \mathrm{var}(f_B(1000;\mathscr{D})).$$
:::

## Estimator variance with derived features

In typical cases, does using additional derived features lead to more
variance or less variance?

:::{note} Solutions
:class: dropdown
More.
:::

## 8th order polynomials vs splines with 8 knots

True or false: in the context of linear regression, using polynomial
features of degree 8 always leads to the same predictions as using cubic
spline features with 8 evenly spaced knots.

:::{note} Solutions
:class: dropdown
False. If you use polynomial features of 8th degree you'll get estimated
regression functions with nonzero eighth derivative. Predictions made
using cubic spline features have zero eighth derivative (away from the
knots). Indeed the cubic spline is made of cubic pieces, and so we get
zero for for fourth derivatives, fifth derivatives, etc.
:::

## Splines vs polynomials

True or false: splines are typically much better than polynomials,
especially if the predictor is uniformly distributed throughout its
range.

:::{note} Solutions
:class: dropdown
False. They're often pretty comparable in this regime.
:::

## Splines vs polynomials, part II

True or false: spline features with carefully placed knots can often
outperform polynomial features.

:::{note} Solutions
:class: dropdown
True.
:::

## Placing knots by eye

If you want to place knots by looking at the data and choosing them
yourself, while still getting a good assessment of how your method will
perform on future test data, which of the following would be preferred?

1.  Use all the data to place knots, to ensure best possible placement.
    Then make a test/train split, train the spline model on the training
    data, and assess its performance on the test data.

2.  Make a test/train split. Use the training data to decide where to
    place the knots, and the same training data to fit the model. Then
    assess performance on the test data.

:::{note} Solutions
:class: dropdown
Second option is better. If you use all of the data to pick the knots,
you are "cheating" to some extent, which means you may mis-assess how
well your estimate will perform on future test data.
:::

## Picking knots

Among the following choices, which is the most typical way to pick the
knots for spline features?

1.  Uniformly spaced inside the range of the observed features.

2.  Uniformly at random inside the range of the observed features

3.  Place a knot at each data point (and use lasso regularization to
    control curvature)

:::{note} Solutions
:class: dropdown
First option is most common. Third option would be plausible except for
that bit about lasso regularization to control curvature. It is perhaps
conceivable that one could control curvature using some form of lasso,
but it is certainly not common practice. Instead, if you want to control
curvature it would be typical to use a penalty on the integral of the
squared second derivative.
:::

## Roughness penalties with quadratic models

Consider the parametric regression model
$Y=\beta_{0}+\beta_{1}x+\beta_{2}x^{2}+ \epsilon$. Suppose we are given
a dataset $\mathscr{D}=\{(x_1,y_1),\ldots,(x_n,y_n)\}$ and we estimate
the parameters $\beta$ by solving
$$\arg \min_{\hat \beta}\sum_{i=1}^n (y_{i}-f(x_i;\hat \beta))^{2}+\int_{-\infty}^{\infty}f''(x;\hat \beta)^{2}dx.$$
True or false: our estimated value of $\beta_{2}$ will be zero.

:::{note} Solutions
:class: dropdown
True. In this case,
$$\int_{-\infty}^{\infty}f''(x;\beta)^{2}dx=\int_{-\infty}^{\infty}(2\beta)^{2}dx=\begin{cases}
\infty & \mathrm{if}\ \beta\neq0\\
0 & \mathrm{otherwise}.
\end{cases}$$ Thus if we take $\beta_{2}\neq0$ we will incur an infinite
penalty. That means the solution that minimizes the objective must have
$\beta_{2}=0$.
:::

## Roughness penalties with quadratic models, II

Let $f(x;\beta)=\beta_{0}+\beta_{1}x+\beta_{2}x^{2}$ and consider
estimating the coefficients via solving the minimization problem
$$\arg \min_{\beta}\sum(y_{i}-f(x_i;\beta))^{2}+\int_{-\infty}^{\infty}\lambda f''(x;\beta)^{2}dx.$$
Let $\hat{\beta}(\lambda)$ denote the coefficients estimated for any
given choice of regularization strength $\lambda$. True or false:
$\lim_{\lambda\downarrow0}\hat{\beta}_{2}(\lambda)=\hat{\beta_{2}}(0).$

:::{note} Solutions
:class: dropdown
False. For **any nonzero $\lambda$,** we will find that $\beta_{2}$ must
be exactly zero. Indeed, the penalty will be of the form
$$\int_{-\infty}^{\infty}\lambda\left(2\beta_{2}\right)^{2}dx=\begin{cases}
\infty & \mathrm{if}\ \lambda\beta_{2}\neq0\\
0 & \mathrm{otherwise}
\end{cases}$$ So if $\lambda\neq0$, the only way to control this penalty
is to set $\beta_{2}=0$. For $\lambda=0$, by contrast, it will be
possible to obtain some nonzero value of $\beta_{2}$.
:::

## Knots vs training error

In cubic regression spline, adding more knots in general will increase
the training error. True or false?

:::{note} Solutions
:class: dropdown
False. More knots usually decreases training error (but may increase
test error).
:::

## Polynomial regression and validation error

You are performing least-squares polynomial regression. As the degree of
your polynomials increases, the validation error is commonly seen to go
down at first but then go up. True or false?

:::{note} Solutions
:class: dropdown
True.
:::

## Preprocessing binary features

You are given a dataset concerning $p$ binary features (coded as zeros
and ones) and one numerical response. True or false: before proceeding
with most estimators, it is essential to preprocess your data into a new
form with $2p$ features (this is referred to as "one-hot encoding" and
the new features are sometimes called "dummy features").

:::{note} Solutions
:class: dropdown
False. You don't need one-hot encoding for binary features. You can just
code them as 0s and 1s.
:::

## Preprocessing binary features, II

You are given a dataset concerning $p$ categorical features and one
numerical response. Assume each categorical feature can take on one of
four unique values (e.g., $X_i \in \{1,2,3,4\}$ for each feature $i$).
Before proceeding with most estimators, it is usually helpful to
preprocess your data into a new form using a one-hot encoding. After
this encoding process, you will have a new dataset that a wide variety
of methods can be directly applied to. How many predictor features will
this new dataset have?

-   Either $5p$ or $4p$ (either is fine; depends on whether you drop one
    of the categories).

-   Either $3p$ or $4p$ (either is fine; depends on whether you drop one
    of the categories).

-   Either $2p$ or $3p$ (either is fine; depends on whether you drop one
    of the categories).

:::{note} Solutions
:class: dropdown
Second solution is correct. You need at least three features for each
raw categorical feature (if that feature takes on four unique values).
You can also use 4. 4 introduces redundancy, but that doesn't usually
matter for most estimators.
:::

