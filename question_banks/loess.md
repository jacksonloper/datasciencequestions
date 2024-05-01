# LOESS

## LOESS locality

True or false: say you obtain an estimate of the regression function,
$\hat{f}(x)$, using LOESS applied to a dataset
$\left(\left(X_{1},Y_{1}\right),\ldots,\left(X_{n},Y_{n}\right)\right)$.
Then, due to the locality property, for any point $(X_{i},Y_{i})$ from
the training data itself, we have that $\hat{f}(X_{i})=Y_{i}$.

:::{note} Solutions
:class: dropdown
Nope that's nonsense.
:::

## LOESS data gremlin

Say you collect a dataset with one predictor variable, one response
variable, and 100 samples. Assume the predictor values are unique among
the samples (i.e. there are no two samples that have the exact same
value for their input). Then you use loess with a span of .8 to predict
the response on a test point $x_{0}$, greater than the predictor value
of any train point. Then you realize you made a mistake in data
collection and the response for the point in your data with the very
lowest predictor value must be changed. Finally, you recalculate the
loess prediction on the test point $x_{0}$. True or false: the
prediction is unchanged.

:::{note} Solutions
:class: dropdown
True. For each test point we want to evaluate, we have to fit a least
squares problem based on the 80% of points closest to the test query
(the span is $0.8$). We set this problem up so that the test query was
far away from the altered training point, and the local model we fit
wouldn't involve the altered point.
:::

## LOESS data gremlin, II

Say you collect a dataset with one predictor variable, one response
variable, and 100 samples. Assume the predictor values are unique among
the samples (i.e. there are no two samples that have the exact same
value for their input). Then you use loess with a span of .8 to predict
the response on a test point $x_{0}$, greater than the predictor value
of any train point. Then you realize you made a mistake in data
collection and the response for the point in your data with the median
predictor value must be changed. Finally, you recalculate the loess
prediction on the test point $x_{0}$. True or false: the two predictions
must be the same.

:::{note} Solutions
:class: dropdown
False. For each test point we want to evaluate, we have to fit a least
squares problem based on the 80% of points closest to the test query
(the span is $0.8$). We set this problem up so that the test query was
closer to the altered training point, and the local model we fit would
involve the altered point.
:::

