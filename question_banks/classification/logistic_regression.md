# Logistic Regression

## Log likelihood

Consider a binary classification problem. Let $\hat p(y|x)$ denote an
estimate for the conditional probability that $Y=y$ given that $X=x$.
Let $p^*(y|x)$ denote the true conditional probability. True or false:
$$\mathbb{E}[\log p^*(Y|X)] \geq \mathbb{E}[\log \hat p(Y|X)].$$

:::{note} Solutions
:class: dropdown
True. The expected log likelihood of the truth is always at least as
good as the expected log likelihood for an estimate. That's part of why
methods like logistic regression estimate coefficients to maximize
average log likelihood on training data.
:::

## Consistency of logistic regression

Consider a binary classification problem. Let $p^*(y|x)$ denote the true
conditional probability that $Y=y$ given that $X=x$. Assume that
$$\log \frac{p^*(+|x)}{\hat p^*(-|x)} = \beta_0 + \sum_{j=1}^p \beta_j^* x_j$$
for some coefficients $\beta^*$. True or false: in typical settings,
given enough data, the logistic regression estimate of the log odds will
become arbitrarily close to the truth.

:::{note} Solutions
:class: dropdown
True. If the log odds are linear, logistic regression will do just fine.
:::

## Softmax redundancy

Consider the "softmax" representation for multiclass logistic regression
with one predictor feature,
$$\hat p(y|x) = \frac{\exp(\beta_{y,0} +\beta_{y,1} x)}{\sum_{y'} \exp(\beta_{y',0} +\beta_{y',1} x)}.$$
True or false: this representation is redundant, i.e., one can find at
least two different numerical coefficients for $\beta$ that lead to the
same conditional probabilities.

:::{note} Solutions
:class: dropdown
True. It's not a problem in most cases. But it is important to be aware
of. You can't interpret the coefficients very easily in this case.
:::

## Logistic regression and LASSO/ridge

True or false: penalties such as LASSO and ridge are rarely used when
training logistic regression.

:::{note} Solutions
:class: dropdown
False. In fact, sklearn uses a mild ridge penalty by default.
:::

## Alzheimer's and bilingualism

You are interested to understand the relationship between bilingualism
(being fluent in at least two languages) and Alzheimer's disease. You
obtain a dataset with 50,000 older adults living the United States, of
whom 5,000 have Alzheimer's. For each patient you know whether they are
fluent in more than one language and what their taxable yearly income
is.

1.  You first fit a logistic regression model to predict whether a
    patient has Alzheimer's disease ($Y=0$ if they don't, $Y=1$ if they
    do) given their bilingualism status ($X_1=0$ for only one language,
    $X_1=1$ for at least two). The model suggests that bilingual
    individuals are less likely to get Alzheimer's disease (note we are
    making no causal statements here). You then realize that your
    approach was probably not the right way to go. Instead, given your
    dataset, you should have fit a model with $p=2$ features (including
    both bilingualism status and income in the model). Why?

    :::{note} Solutions
    :class: dropdown
    Two things here. First, roughly speaking, income may be a
    "confound." We won't define that rigorously in this class, but the
    point is that bringing that extra predictor in may lead to a more
    complete picture. Second, you have plenty of data. If you only had
    10 datapoints, it might have been hard to fit a full model using
    both predictors (variance might be too high). However, with
    $50,000$, you can definitely fit a model with two predictors.
    :::

2.  You next fit a logistic regression model to predict whether a
    patient has Alzheimer's disease ($Y=0$ if they don't, $Y=1$ if they
    do) given their bilingualism status ($X_1=0$ for only one language,
    $X_1=1$ for at least two) and their income ($X_2$, in thousands of
    dollars). We estimate log odds using a generalized additive model
    (GAM) with the following form:
    $$\hat f(x) = \hat \beta_0 + \hat f_1(x_2) +\mathbb{I}(x_1=1)\hat f_2(x_2).$$
    Here $\hat f_1$ and $\hat f_2$ are the two component functions of
    the GAM. We fit $\hat \beta_0$, $\hat f_1$, and $\hat f_2$ to data,
    using splines for $\hat f_1$ and $\hat f_2$.

    Under this model, which is the correct expression for
    $$\Delta(x_2)= \log \left(\frac{\mathbb{P}(Y=1|X_1=1,X_2=x_2)}{\mathbb{P}(Y=0|X_1=1,X_2=x_2)}
    /\frac{\mathbb{P}(Y=1|X_1=0,X_2=x_2)}{\mathbb{P}(Y=0|X_1=0,X_2=x_2)}\right)?$$

    1.  $\hat f_2(x_2)$

    2.  $\hat f_2(x_2)-\hat f_1(x_2)$

    3.  $\exp(\hat f_2(x_2))$

    4.  $\exp(\hat f_2(x_2)-\hat f_1(x_2))$

    :::{note} Solutions
    :class: dropdown
    First answer is correct. Note that $\Delta(x_2$) is not not just a
    combination of ratios and logs: it is an important quantity for
    interpretation. It indicates the estimated change in the log odds of
    getting Alzheimer's, among those with some particular income $x_2$.
    If $\Delta(x_2)$ is positive, it means that among those with income
    $x_2$, those who are bilingual are more likely to get Alzheimer's.
    If it is negative, it means that among those with income $x_2$,
    those who are bilingual are less likely to get Alzheimer's. This
    interpretation comes with two caveats. First, we are not making any
    *causal* statements here. Second, all of our statements are only as
    valid as our model, which might have been estimated badly.
    :::

## Data with irrelevant features

You are given a dataset with $n=400$ data points concerning $p=1000$
input features and one binary response. You suspect that most of the
features are completely irrelevant to the response. Of the following,
which would be the most typical estimator to use in this case?

1.  Logistic regression with no penalty

2.  Logistic regression with a ridge penalty

3.  Logistic regression with a LASSO penalty

:::{note} Solutions
:class: dropdown
The last one. If you think most of the features are irrelevant, LASSO
can help you ignore them. Of course if you know which features are
irrelevant you can just delete them, leading to lower variance estimates
in most cases---but lasso is useful if you aren't sure which are
irrelevant.
:::

## Interpreting LASSO

You are given a dataset concerning $p=100$ input features and one binary
response. You estimate log odds using LASSO-regularized logistic
regression (using cross-validation to pick the regularization strength).
You find that the estimated coefficients are zero for all but the first
two input features (i.e. $\beta_3=\beta_4=\ldots=\beta_100=0$). Which of
the following would be the most appropriate interpretation of this
finding?

1.  Cross-validation probably failed to identify a good choice for the
    regularization strength.

2.  In future efforts to predict the response, it may be sensible to
    focus research on features $X_1$ and $X_2$.

3.  The response appears to be independent of the features $X_1,X_2$.

4.  The response appears to be independent of features
    $X_3,X_4\ldots X_100$.

:::{note} Solutions
:class: dropdown
The second one. LASSO doesn't say anything conclusive about independence
(except perhaps in some very special cases). And there's no reason to
suppose cross-validation failed just because you get a bunch of zero
coefficients.
:::

## Logistic regression coefficients under two different models

You are given a dataset with two predictors ($X_1,X_2$) and one binary
response. You first fit logistic regression using only $X_1$ (call that
Model I). Then you fit logistic regression with both predictors (call
that Model II). True or false: if the coefficient for $X_1$ was positive
in Model I, then it must be positive in Model II.

:::{note} Solutions
:class: dropdown
False. When you bring in a new predictor, anything can happen.
:::

