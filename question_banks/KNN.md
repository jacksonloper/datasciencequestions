# KNN

## KNN with one neighbor

Which of these choices best describes the $K$ nearest neighbor
prediction rule, with $K=1$? Why?

1.  low bias, low variance

2.  low bias, high variance

3.  high bias, low variance

4.  high bias, high variance

:::{note} Solutions
:class: dropdown
\(b\)
:::

## KNN and standardization

Say you have a dataset about houses, and you would like to use KNN with
this dataset to predict the price of a house that was not in the
dataset. However, before applying KNN, you recall that it is common
practice to apply a standardization that involves two steps:

1.  subtract the mean from each predictor feature (also known as
    **centering**) and

2.  divide by the standard deviation of each predictor feature (also
    known in this context as **scaling)**.

However, it would be possible to preprocess the data by only performing
one of these steps. We will now consider how different preprocessing
choices might affect your prediction. Let

1.  $\hat{y}_{0}$ denote the KNN prediction if no preprocessing is used,

2.  $\hat{y}_{c}$ denote the KNN prediction if a centering preprocessing
    step is used,

3.  $\hat{y}_{s}$ denote KNN prediction if a scaling preprocessing step,
    and

4.  $\hat{y}_{c+s}$ denote KNN prediction if centering and scaling
    preprocessing steps are used.

In typical cases, only one of the following is true. Which is it?

1.  $\hat{y}_{0}\neq\hat{y}_{c}$

2.  $\hat{y}_{0}\neq\hat{y}_{s}$

3.  $\hat{y}_{s}\neq\hat{y}_{c+s}$

:::{note} Solutions
:class: dropdown
The second answer is correct. KNN is invariant to shifts in feature
space. So centering does nothing. But scaling has a huge impact on
predictions.
:::

## KNN by hand

Say you have this dataset. $$\begin{array}{|c|c|c|c|}
\hline \text{Employee} & \text{Years of Experience} & \text{Number of Projects} & \text{Satisfaction Level}\\
\hline 1 & 3 & 4 & 0.6\\
2 & 4 & 5 & 0.8\\
3 & 2 & 3 & 0.5\\
4 & 5 & 6 & 0.9\\
5 & 3 & 2 & 0.4\\
6 & 4 & 4 & 0.7
\\\hline \end{array}$$ Predict the satisfaction level of an employee
with 3 years of experience and 3 projects using KNN with $k=3$.

:::{note} Solutions
:class: dropdown
The prediction is $0.5$. Here are the distances.

1.  Employee 1: $(3-3)^{2}+(4-3)^{2}=1$

2.  Employee 2: $(4-3)^{2}+(5-3)^{2}=5$

3.  Employee 3: $(2-3)^{2}+(3-3)^{2}=1$

4.  Employee 4: $(5-3)^{2}+(6-3)^{2}=13$

5.  Employee 5: $(3-3)^{2}+(2-3)^{2}=1$

6.  Employee 6: $(4-3)^{2}+(4-3)^{2}=2$

So the three closest train samples are #1, #3, and #5. So the average
satisfaction is ($0.6+0.5+0.4)/3=1.5/3=0.5$.
:::

## KNN by hand, III

Say you have this dataset. $$\begin{array}{|c|c|c|c|}
\hline \text{Employee} & \text{Years of Experience} & \text{Number of Projects} & \text{Turnover}(0:\text{Stay},1:\text{Leave})\\
\hline 1 & 3 & 4 & 0\\
2 & 4 & 5 & 1\\
3 & 2 & 3 & 0\\
4 & 5 & 6 & 1\\
5 & 3 & 2 & 1\\
6 & 4 & 4 & 1
\\\hline \end{array}$$ Predict whether an employee will leave or stay (0
for stay, 1 for leave) based on years of experience and the number of
projects using KNN with $k=3$.

:::{note} Solutions
:class: dropdown
The prediction is **stay.** As per above, the closest train samples are
employees #1, #3, and #5. Two out of three of those employees stayed.
:::

## Misclassification of 1NN with infinite data

As the number of data points approaches $\infty$, the misclassification
error rate of a 1-nearest-neightbor classifier must approaches 0. True
or false?

:::{note} Solutions
:class: dropdown
Nope. In most real-world scenarios, the 1NN estimator always has
variance, even with infinite data.
:::

## Curse of dimensionality

True or false: the curse of dimensionality (as applied to regression
problems) states that one can only obtain good estimates for
$\mathbb{E}[Y|X=x]$ when the number of predictor features is less than
the number of training samples.

:::{note} Solutions
:class: dropdown
False. The curse of dimensionality can't be pinned down so neatly. The
curse of dimensionality, in general terms, just says that prediction
gets harder with more features. But there's no hard-and-fast rule. It is
often possible to get good answers from LASSO with more predictor
features than training samples.
:::

