# Bootstrap

## What is bootstrap for?

Bootstrap is most commonly used for which of the following?

1.  Assessing estimator variance.

2.  Assessing estimator bias.

3.  Assessing estimator error.

:::{note} Solutions
:class: dropdown
The first. Bias and error are hard.
:::

## Bootstrap and the Bayes optimal classifier

Let $\hat y$ denote an estimator for the Bayes optimal classifier. Given
a dataset with two predictors, a user creates 10 bootstrap datasets and
fits the estimator to each one, leading to 10 fits in all. The user then
visualizes the decision boundary for each fit. They use this to help
guide their intuition about the estimator's variability due to
randomness in the training dataset. True or false: the user will reach
meaningless conclusions because estimator variance is undefined in this
context.

:::{note} Solutions
:class: dropdown
False. It is actually true that estimator variance is undefined in this
context ($\hat y$ isn't a number). However, the user's procedure may
still be helpful for visualizing how much $\hat y$ varies from dataset
to dataset.
:::

