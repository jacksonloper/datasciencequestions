# Variance control

I'm not going to pose this section as questions. Instead, here's a
summary of the facts.

-   In general, QDA is higher variance than LDA.

-   In general, QDA has less variance if we assume all covariance
    matrices are diagonal, and even less variance if we assume all
    covariance matrices are a multiple of the identity.

-   In general, LDA has less variance if we assume all covariance
    matrices are diagonal, and even less variance if we assume all
    covariance matrices are a multiple of the identity.

-   In general, adding more derived features increases variance (note
    that using higher-order polynomial features usually entails more
    derived features, and using spline features with more knots usually
    entails more derived features).

-   In general, KNN (whether for regression or classification) is lower
    variance with higher values of K (more neighbors).

-   In general, LOESS is lower variance with higher span values (local
    regression models fit using more data).

-   In general, if you have bad class imbalance problems you may have
    higher variance.

-   In general, if there is a region of input space with very few
    training samples, predictions about that input space may be higher
    variance.

-   In general, if you have more data it is easier to get an estimator
    with lower variance.

-   In general, a lasso-regularized method (whether for linear
    regression or logistic regression) is lower variance when the
    regularization strength is higher.

-   In general, a ridge-regularized method (whether for linear
    regression or logistic regression) is lower variance when the
    regularization strength is higher.

-   In general, a roughness-regularized method such as smoothing splines
    (whether for linear regression or logistic regression) is lower
    variance when the regularization strength is higher.

You can of course memorize all these facts, but it is better to see the
patterns.

-   In general, more parameters means more variance. QDA has more
    parameters than LDA (more variance). Constraining the covariances to
    be diagonal or multiples of the identity means less parameters (less
    variance). Adding more features usually means there are more
    parameters to learn (more variance).

-   In general, more data means less variance. For local methods like
    KNN or LOESS, basing each prediction on more data points means less
    variance. For other methods, more total data usually means lower
    variance (e.g., variance of logistic regression estimator usually
    goes down with more and more total data). If there's class
    imbalance, it means you're doing classification and there is at
    least one class $y$ that doesn't appear in your training data very
    often---so you don't have much data about that particular class. If
    a region of input space has very few samples, you don't have much
    data about inputs in that region, so higher variance (this is
    particularly true in the context of splines or polynomial derived
    features).

-   In general, more regularization penalty (LASSO, ridge, roughness
    penalty) means less variance.

A few remarks.

1.  These things interact. In general, more features means more
    variance---but you can actually have an unbounded number of derived
    features and still be okay as long as you have a suitable penalty
    that controls the variance (this is one way of thinking about what
    smoothing splines do).

2.  Remember: estimator variance is not one thing. For example, in many
    cases, you get a different estimator variance for different inputs,
    i.e., it may be that $\mathrm{var}(\hat f(3))=1$ yet
    $\mathrm{var}(\hat f(4))=100$. Estimator variance depends upon (a)
    the estimator (b) the number of samples (c) the data-generating
    distribution and (d) the input point $x$ being considered.

3.  Final thought: why would you ever want an estimator with more
    variance? All else equal, of course you want low variance. But often
    the higher variance estimators also have less bias, so you tolerate
    the higher variance to get the lower bias. Remember, expected
    squared estimator error is just squared bias plus variance.

