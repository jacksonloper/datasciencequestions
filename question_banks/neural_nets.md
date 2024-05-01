# Neural nets

## Dense layer

Consider a dense affine layer with $10$ inputs and $20$ outputs. How
many parameters does it have (assume there is a "bias" term)?

:::{note} Solutions
:class: dropdown
$20\times 10=200$ weight parameters and $20$ bias parameters so $220$.
:::

## Convolutional layer

Consider a 2d convolutional layer with filterwidth 2 and filterheight 6,
10 input channels and 20 output channels. We are using the layer for
images with width 50 and height 50. How many parameters does the layer
have?

:::{note} Solutions
:class: dropdown
Image size doesn't matter one whit.

There are $20$ filters, each with $2\times 6 \times 10=120$ parameters.
So $2400$ filter parameters. Then there is a bias term for each output
channel. So $2420$ in total.
:::

## Composing layers

Consider a network architecture formed by composing three simple
families of functions. The final network will involve an affine layer
$$(f(x;\beta))_k = \beta_{k0}+ \sum_j \beta_{kj} x_j,$$ another affine
layer $$(g(z;\gamma))_k = \gamma_{k0}+ \sum_j \gamma_{kj} z_j,$$ and a
nonlinearity $(\mathrm{ReLU}(x))_k = \max(x_k,0)$. Which would be a more
typical way to compose these layers into a final neural network
architecture?

1.  $h(x;\beta,\gamma) = \mathrm{ReLU}(f(g(x;\gamma);\beta))$

2.  $h(x;\beta,\gamma) = f(\mathrm{ReLU}(g(x;\gamma));\beta)$

3.  $h(x;\beta,\gamma) = f(g(\mathrm{ReLU}(x);\gamma);\beta)$

:::{note} Solutions
:class: dropdown
Middle answer is most typical.
:::

## Convolution by hand

Compute the convolution of the input $X=(1,1,2,3,4,5,6,6)$ with the
filter $(1,-2,1)$.

:::{note} Solutions
:class: dropdown
-   First window is $(1,1,2)$, so we're looking at $1-2(1)+2=1$.

-   Next window is $(1,2,3)$, so we're looking at $1-2(2)+3=1-4+3=0$.

-   Next window is $(2,3,4)$, so we're looking at $2-2(3)+4=2-6+4=0$. Ah
    there's a pattern here.

-   Next window is $(3,4,5)$, so we're looking at $3-2(4)+5=3-8+5=0$.
    Yeah these will all be zero.

-   \... $(5,6,6)$ will be different. We'll get
    $5 -2(6)+6 = 5-12+6=-7+6=-1$.

Final answer is $(1,0,0,0,0,-1)$.

Note there is some ambiguity about what we meant by convolution here
(because sometimes people think about flipping the filter before
applying it at each window). Here we avoided that ambiguity by making
the filter symmetric.
:::

## Function families

Consider the following three functions.

-   $g_1(x) = x^2 - \cos x$

-   $g_2(x) = (x+3)^2$

-   $g_3(x) = (x-3)^2 + 20 \cos x$

Devise a two-parameter family of functions that could include all three
of those functions. Your answer should define a layer
$f(x;\beta_1,\beta_2)$ that takes a scalar input $x$ and two parameters
$\beta_1,\beta_2$ and returns a number. The layer should be designed so
that, for each function $g_i$ listed above, we can write
$g_i(x)=f(x;\beta_1,\beta_2)$ by choosing $\beta_1,\beta_2$
appropriately.

:::{note} Solutions
:class: dropdown
$f(x;\beta_1,\beta_2) = (x-\beta_1)^2 + \beta_2 \cos x$ would suffice.
:::

## More fun with function families

Consider the family of linear functions with positive slope:
$F=\{g:  g(x)=\beta_0+ \beta_1x\}_{\beta_1> 0}$. Devise a one-parameter
architecture that includes all these functions, and does not include any
functions not in $F$. That is, write a layer $f(x;\beta)$ that takes a
scalar input and parameter $\beta \in \mathbb{R}^2$ and returns a
number. The layer should be designed so that for any $g \in F$ it is
possible to find $\beta$ so that $f(x;\beta)=g(x)$.

:::{note} Solutions
:class: dropdown
$f(x;\beta) = \beta_0 + \exp(\beta_1) x$ will suffice.
:::

## Gradient descent by hand

Consider the objective $L(\beta) = (\beta-5)^2$. Run one iteration of
gradient descent with $\beta$ initialized at $6$. Use a learning rate of
3. What is the loss at initialization? What is the new value of $\beta$
you get? What is the loss with the new value of $\beta$? What can we say
about the choice of learning rate in this case?

:::{note} Solutions
:class: dropdown
At initialization loss is $(6-5)^2=1$

$L'(\beta) = 2(\beta-5)$, so the gradient at initialization is
$L'(6)=2(6-5)=2$. We then take a descent step,
$\beta \gets \beta - \epsilon L'(\beta)$, which in this case comes to
$\beta \gets 6 - 3\times 2 = 0$.

Now loss is $(0-5)^2=25$. Not good! Things got worse! Learning rate was
way too big.
:::

## Improvements to vanilla gradient descent

Consider the loss
$L(\beta_1,\beta_2) = \cos(\beta_1-10)e^{\beta_1^2} + 500 \cos(\beta_2-3)^2 e^{\beta_2^2}$.
This loss can be difficult to minimize using gradient descent. Of the
following, which would be the most typical method we might employ to
help gradient descent find a local optima more quickly?

1.  Minibatch stochastic gradient descent

2.  Gradient descent with momentum

3.  Using GPUs (allowing us to quickly compute a large number of
    iterations of SGD with very small step-sizes)

4.  Ridge regularization on $\beta$

:::{note} Solutions
:class: dropdown
Second option. Momentum can help with the unequal magnitude of curvature
(function is much curvier in $\beta_2$ parameter).

Minibatch stochastic gradient descent makes no sense because we can very
rapidly compute the gradient of $L$.

GPUs won't help here because computing the gradient at each step is
pretty fast. GPUs would help if we had to compute many different
quantities all at the same time, but they won't help us compute a
problem like this where the result at each stage depends on the result
from the previous stage.

Ridge regularization could conceivably help, maybe? But I can't actually
think how right now. Would not be typical. Ridge regularization is
usually employed in the context of improving the final estimate, not to
make gradient descent converge faster.
:::

## The chain rule

All autodiff methods rely on the *chain rule* to help them figure out
how to take derivatives of complicated functions. If $h(x)=f(g(x))$, the
chain rule states that\...

1.  $h'(x)=f'(g(x))g'(x)$

2.  $h'(x)=f(g'(x))g(x)$

3.  $h'(x)=f'(g(x))/g'(x)$

4.  $h'(x)=f(g'(x))/g(x)$

:::{note} Solutions
:class: dropdown
First answer is correct.
:::

