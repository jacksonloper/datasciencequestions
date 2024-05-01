# Assessing predictive error

## Test error, train error

A dataset was gathered and split into two groups of samples:
$\mathscr{D}_{1}$ with $n_{1}$ points and $\mathscr{D}_{2}$ with $n_{2}$
points. A regression function estimate $\hat{f}_A$ was fit using
$\mathscr{D}_1$. Another regression function estimate $\hat{f}_B$ was
also fit using $\mathscr{D}_1$. The following quantities were computed:
$$\begin{aligned}
\frac{1}{n_{1}}\sum_{(x,y)\in\mathscr{D}_{1}}\left(\hat{f}_A(x)-y\right)^{2} &= 10\\
\frac{1}{n_{2}}\sum_{(x,y)\in\mathscr{D}_{2}}\left(\hat{f}_A(x)-y\right)^{2} &= 40\\
\frac{1}{n_{1}}\sum_{(x,y)\in\mathscr{D}_{1}}\left(\hat{f}_B(x)-y\right)^{2} &= 20\\
\frac{1}{n_{2}}\sum_{(x,y)\in\mathscr{D}_{2}}\left(\hat{f}_B(x)-y\right)^{2} &= 30
\end{aligned}$$ Given this information, which estimate would generally
be preferred, $\hat f_A$ or $\hat f_B$?

:::{note} Solutions
:class: dropdown
$\hat f_B$, which has the lower error on the testing data.
:::

## Test error, train error, II

A dataset was gathered and then split into a train set $\mathscr{D}_{1}$
with $n_{1}$ points and a test set $\mathscr{D}_{2}$ with $n_{2}$
points. A regression function estimate $\hat{f}$ was trained using the
training dataset. Then two forms of error were computed, as follows.
$$\begin{aligned}
\epsilon_{1} & =\frac{1}{n_{1}}\sum_{(x,y)\in\mathscr{D}_{1}}\left(\hat{f}(x)-y\right)^{2}\\
\epsilon_{2} & =\frac{1}{n_{2}}\sum_{(x,y)\in\mathscr{D}_{2}}\left(\hat{f}(x)-y\right)^{2}
\end{aligned}$$ True or false: in typical cases, we expect
$\epsilon_{1}$ to be greater than $\epsilon_{2}$.

:::{note} Solutions
:class: dropdown
False. $\epsilon_{1}$ is the train error. It's usually better (i.e.
lower) than $\epsilon_{2}$.
:::

## Estimator bias

Imagine you are given an estimator and a dataset. Using test/train
splits and/or the bootstrap, we can usually get an accurate assessment
of the estimator's bias on this dataset. True or false?

:::{note} Solutions
:class: dropdown
Unfortunately, no. Getting estimator bias is quite hard.
:::

## Mean squared error

Consider a prediction problem with one categorical predictor
$X \in \{1,2,\ldots M\}$ and one continuous response $Y\in \mathbb{R}$.
You obtain an estimate $\hat f$ for the regression function
$\mathbb{E}[Y|X=x]$. True or false:
$$\mathbb{E}[(Y-\hat f(X))^2] = \frac{1}{M}\sum_{x=1}^M \mathbb{E}[(Y-\hat f(X))^2|X=x].$$

:::{note} Solutions
:class: dropdown
In general, false.

However, let $\epsilon_x=\mathbb{E}[(Y-\hat f(X))^2|X=x]$. If
$\mathbb{P}(X=x)=q_x$, then
$$\mathbb{E}[(Y-\hat f(X))^2] = \mathbb{E}[\epsilon(X)]=\sum_{x=1}^M q_x \epsilon_x.$$
:::

## Mean squared error and heteroskedasticity

Consider a prediction problem with one continuous response
$Y\in \mathbb{R}$. You obtain an estimate $\hat f$ for the regression
function $\mathbb{E}[Y|X=x]$. Let $x_1,x_2$ denote two possible values
for the input. True or false: if there is no heteroskedasticity in the
true relationship between $X$ and $Y$, then we know that
$$\mathbb{E}[(Y-\hat f(X))^2|X=x_1] = \mathbb{E}[(Y-\hat f(X))^2|X=x_2]$$

:::{note} Solutions
:class: dropdown
In general, false! However, if $\hat f(x)$ was a *perfect* estimate of
the regression function, then then this statement would be true. Let
$f^*$ denote the true regression function. Recall that
$$\mathbb{E}[(Y-f^*(X))^2|X=x] = \mathrm{var}(Y|X=x).$$ and
homoscedasticity (i.e., lack of heteroskedasticity) implies that
$\mathrm{var}(Y|X=x_1)=\mathrm{var}(Y|X=x_2)$ for every $x_1,x_2$.
However, in general, $\hat f \neq f^*$, so one may have that
$\mathbb{E}[(Y-\hat f(X))^2|X=x_1] \neq \mathbb{E}[(Y-\hat f(X))^2|X=x_2]$.

This may seem like a bit of a trick question, but it is a trick that
actually tricks real people in the real world. Here's the key thing to
remember: even if the true relationship is basically homoskedastic, your
estimator may just be *more wrong* for some inputs. That means it will
have worse error for some inputs than others.
:::

## Quality of an estimate for the conditional distribution

Let $\hat p(y|x)$ denote an estimate of the conditional distribution of
a binary response $Y$ given an input $X$. Which of the following are
traditional tools for measuring the quality of this estimate?

1.  Log likelihood

2.  Misclassification rate (of the hard classifier
    $\hat y(x) = \arg \min_y \hat p(y|x)$)

3.  Squared error

4.  Mean absolute error

5.  AUROC

:::{note} Solutions
:class: dropdown
Misclassification rate, log likelihood, and AUROC are all traditional.
Squared error and mean absolute error are more traditional on a
regression context.
:::

## Low train error, high test error

If you observe low training error and high validation error for your
model, you might want to adjust your estimator in ways that increase its
bias. True or false?

:::{note} Solutions
:class: dropdown
True. You don't really *want* to increase the bias, but you may choose a
different estimator that has less variance (and more bias).
:::

