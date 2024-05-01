# Misc

## Picking estimators

Given a dataset and a collection of 10 candidate estimation procedures,
which would be the more typical way to decide which estimation procedure
to use: cross-validation or bootstrap?

:::{note} Solutions
:class: dropdown
Cross-validation.
:::

## Case control adjustment

A team is developing a predictive model to estimate the probability that
an email is spam, based on predictor features $x$. During the training
phase, a model is trained on a dataset where 70% of the emails are
labeled as "not spam" and 30% as "spam." This model produces an estimate
$\hat f(x)$ for the log odds that an email with features $x$ is spam.
However, when the model is deployed in a real-world scenario, the
distribution of emails changes significantly, with the test data
comprising 50% "spam" and 50% "not spam" emails. Assuming that the
class-conditional distributions remain consistent between the training
and test datasets, which of the following would be a suitable estimate
for the log odds that an email is spam in the real-world setting?

1.  $\hat f(x) - \log(.3/.7)$

2.  $\hat f(x)\frac{.3 \times .5}{.7 \times .5}$

3.  $\hat f(x)+\frac{.3 \times .5}{.7 \times .5}$

4.  None of the above

:::{note} Solutions
:class: dropdown
First option is correct. The label-shift adjustment would usually be
written as $\log(.5/.5) - \log (.3/.7)$, but $\log(.5/.5)=\log 1=0$.
:::

## A choice of two datasets

You would like to be able to predict whether a fishing boat will become
inoperative in the next six months. You are considering two different
datasets about boat longevity. The first dataset contains records for
10000 boats, including 2 boats that become inoperative during the six
months for which they were monitored. The second dataset contains
records for 1000 boats, including 20 that become inoperative during the
six months for which they were being monitored. All else equal, if you
had to pick one dataset, which would you prefer to use?

:::{note} Solutions
:class: dropdown
The second one. If you have only two examples where the boat sank, it
will be almost impossible to build a good predictor ("class imbalance").
There is another question you should be asking yourself here, though:
why do the datasets have such different proportions of broken boats?
They are clearly not sampling from the same population! Regardless of
which dataset you use, it may be important to figure out the overall
rate at which the boats you are interested in break, so you can perform
some form of label-shift adjustment.
:::

## Ridge penalties

Let $n$ be the number of observations and $p$ be the dimension of the
predictors. Suppose we estimate the regression coefficients in a linear
regression model by minimizing
$$\sum_{i=1}^n \left( y_i - \beta_0 - \sum_{j=1}^p \beta_j x_{ij} \right)^2 + \lambda \| \beta \|_2^2$$
for a particular value of $\lambda$. For parts (a) through (e), indicate
which of i. through v. is correct.

1.  As we increase $\lambda$ from 0, the training error will:

    1.  Increase initially, and then eventually start decreasing in an
        inverted U shape.

    2.  Decrease initially, and then eventually start increasing in a U
        shape.

    3.  Steadily increase.

    4.  Steadily decrease.

    5.  Remain constant.

2.  Repeat (a) for test error.

3.  Repeat (a) for variance.

4.  Repeat (a) for (squared) bias.

5.  Repeat (a) for the irreducible error.

:::{note} Solutions
:class: dropdown
\(a\) iii; (b) ii; (c) iv; (d) iii; (e) v
:::
