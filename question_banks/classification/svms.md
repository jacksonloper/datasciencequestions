# Support vector machines

## Slacks, part I

Consider a classification task with one predictor $X \in \mathbb{R}$ and
a binary response $Y \in \{-1,+1\}$. After fitting an SVM, you find the
following decision function: $$f(x) = 4 - 2x.$$ How would the point
$x=0$ be classified? As positive or negative?

:::{note} Solutions
:class: dropdown
$f(0)=4>0$, so it would be classified as positive.
:::

## Slacks, part II

There are different conventions for how the decision function is
written, and what it says about the margin. But let us say that the
decision function is $f(x) = 4 - 2x$ and the margin is equal to $1/2$.
What would be the slack for the following points?

```
     Sample     $X$   $Y$
  ------------ ----- -----
       #1       1.5   +1
       #2       1.5   -1
       #3        2    +1
       #4        2    -1
       #5        3    +1
       #6        3    -1
```

:::{note} Solutions
:class: dropdown
Let's sketch out what's going on here with the decision function. The
key things to track are (1) what's the decision boundary, (2) how big is
the margin radius, and (3) what is the orientation of the decision
boundary (i.e. which side classifies as negative and which side
classifies as positive). We can sketch all that out here.

```
                            margin radius is 1/2
                              |------|------|
        0.0----0.5----1.0----1.5----2.0----2.5----3.0
         |      |      |      |      |      |      |
                              |   decision  |
          f(x)>0 here         |   boundary  |    f(x)<0 here
                              |    is here  |
                              |             |
                           margin         margin boundary
                           boundary       for points
                          for points      with y=-
                           with y=+
```

For the samples with $y=+$ we compute distance to the margin boundary
$1.5$, divided by margin radius (and assign zero slack for points on the
correctly classified side of the margin boundary).

```
    If y=-, slacks will be...

    slack   5      4      3      2      1      0      0     
      x    0.0----0.5----1.0----1.5----2.0----2.5----3.0
            |      |      |      |      |      |      |
                                     decision  
          f(x)>0 here                boundary        f(x)<0 here
                                     is here  
```                                        

For the samples with $y=+$ we compute distance to the margin boundary
$2.5$, divided by margin radius (and assign zero slack for points on the
correctly classified side of the margin boundary).

```
    If y=+, slacks will be...

    slack   0      0      0      0      1      2      3     
      x    0.0----0.5----1.0----1.5----2.0----2.5----3.0
            |      |      |      |      |      |      |
                                     decision  
          f(x)>0 here                boundary        f(x)<0 here
                                      is here  
```                                            

This is one way to get the slacks. The other way is to use the formula.
Let $\Delta(x) = (4-2x)/2 = 2-x$ denote the *signed* distance to the
decision boundary. Let $m=1/2$ denote the margin. Then the slack for the
point $(x,y)$ is given by $$\xi = \max(0, 1 - y \Delta(x)/m)$$ So, for
example, if $x=3$ and $y=+$ then the signed distance is
$\Delta(3)=2-3=-1$ and so the slack will be $$\begin{aligned}
\max(0,1-y\Delta(x)/m) &= \max(0,1-(+1)\Delta(3)/(1/2)) \\
&= \max(0,1-2(-1)) = \max(0,3)=3 \\
\end{aligned}$$

In this problem I followed the convention that the margin is defined as
the inverse of the square root of the sum of the squares of the
non-intercept coefficients of the decision function, i.e., in this case,
$1/\sqrt{2^2}=1/2$. When the margin is defined this way, there is a
cancellation, and the formula for the slack takes on a slightly simpler
form, namely $\xi = \max(0,1-y f(x))$. In my exams I will always adopt
this convention. In any case, the formula $\xi = \max(0,1-y\Delta(x)/m)$
should always hold, even if you're not taking one of my exams.

The final answers are

```
     Sample     $X$   $Y$   slack  
  ------------ ----- ----- ------- --
       #1       1.5   +1      0    
       #2       1.5   -1      2    
       #3        2    +1      1    
       #4        2    -1      1    
       #5        3    +1      3    
       #6        3    -1      0   
``` 
:::

## More slacks

Now let's fit another SVM. This time we get $$f(x) = 2 + x/5.$$ Assume
the margin is $m=5$. What is the slack for the point $X=-6,Y=+$?

:::{note} Solutions
:class: dropdown
Decision boundary $f(x)=0$ occurs when $x=-10$. Points are classified as
positive when $x>-10$. Picture is like this:

                            margin radius is 5
                              |------|------|
                           -15.0  -10.0   -5.0    0.0
         |      |      |      |      |      |      |
                              |   decision  |
          f(x)<0 here         |   boundary  |    f(x)>0 here
                              |    is here  |
                              |             |
                           margin         margin boundary
                           boundary       for points
                          for points      with y=+
                           with y=-

The point $(X,Y)=(-6,+)$ is correctly classified (it is on the correct
side of the decision boundary). But it isn't on the right side of the
margin line. $f(-6)=2+(-6)/5 = 0.8$. Slack is given by
$$\max(0,1-yf(x)) = \max(0, 1 - (+1)f(-6)) = \max(0, 1-.8) = .2$$
:::

## Features to kernels

We have a dataset $\mathcal{D}=\{(X_1,Y_1),\ldots,(X_n,Y_n)\}$ and we
would like to learn to predict $Y_i \in \{-1,+1\}$ from
$X \in \mathbb{R}$. To build more expressive prediction models, we will
use the following featurization of our data: $$\phi(x) = \begin{pmatrix}
    x \\
    \cos x\\
    \sin x
\end{pmatrix}.$$ Under this featurization, we now have 3 predictive
features. We will then fit an SVM to this data. Our final decision
function will take the form
$$f(x) = \beta_0 + \beta_1 x + \beta_2 \cos x + \beta_3 \sin x$$ for
some coefficients $\beta \in \mathbb{R}^4$. Due to the kernel trick, it
will also be possible to express the final decision function in a
different form: $$f(x) = \gamma_0 + \sum_{i=1}^n \gamma_i K(x,X_i)$$ for
some coefficients $\gamma \in \mathbb{R}^{n+1}$. What is the formula for
$K$ in this case?

:::{note} Solutions
:class: dropdown
$K$ is the kernel here, and it calculates dot products in the featurized
space. Specifically, $$K(x,y) = xy + \cos(x)\cos(y) + \sin(x)\sin(y).$$
:::

## Kernel trick with linear SVMs

Say you have a dataset about $X \in \mathbb{R}^p$ and $Y \in \{-1,+1\}$.
The dataset has $n$ samples. You'd like to try to learn to predict $Y$
from $X$ using a linear SVM. Which is true?

1.  The kernel trick is not useful in this setting.

2.  The kernel trick could be useful if $p=1,000,000$ and $n=100$.

3.  The kernel trick could be useful if $n=1,000,000$ and $p=100$.

:::{note} Solutions
:class: dropdown
Second answer is correct. Kernel trick allows us to perform optimization
in $n$-dimensional space instead of $p$-dimensional space. That is
advantageous when $p$ is very large. Same trick can work for linear
regression with ridge penalties (`sklearn.kernel_ridge.KernelRidge`) or
logistic regression with ridge penalties (though sklearn doesn't have
software for it).
:::

## Linear SVMs vs polynomial SVMs

You have a dataset with one predictor feature $X$ and one binary
response $X$. You would like to be able to predict the response from the
predictor, and you are considering two options.

Option I is to preprocess the data to include the features $X,X^2$ and
then apply a linear SVM. Option II is to use an SVM with a polynomial
kernel $K(x,y)=(1+xy)^2$. Which is true?

True or false: Option I and option II will yield identical results.

:::{note} Solutions
:class: dropdown
False. The results will be similar, but slightly different. Although
both methods consider the same set of features, the features are scaled
differently.

Option I is a SVM with features $X,X^2$.

Option II is equivalent to a linear SVM with features $1,\sqrt{2}X,X^2$.
Because SVMs always include an offset term, this is really equivalent to
a linear SVM with features $\sqrt{2}X,X^2$.

This problem is pretty hard, wouldn't be on exam in exactly this form.
But an easier version of this problem might be (e.g., one that
explicitly guides you through the process of figuring out what the
featurization is that corresponds to the kernel $K(x,y)=(1+xy)^2$).
:::

## Kernels to features

Consider a classification with one predictor $X$ and one binary response
$Y$. You are thinking about fitting a kernel SVM with the kernel
$K(x,y) = xy+\exp(x+y)$. You realize that this is equivalent to fitting
a linear SVM with a certain featurization. What featurization is it?

:::{note} Solutions
:class: dropdown
One suitable choice is
$$\phi(x)=\begin{pmatrix}x \\ \exp(x)\end{pmatrix}.$$ Under this
featurization, $\phi(x)^\top \phi(y)=xy + \exp(x)\exp(y)=xy+ \exp(x+y)$.
:::

## Positive definite, part I

Consider a classification with one predictor $X$ and one binary response
$Y$. You have decided to preprocess the data with a featurization
$\phi:\ \mathbb{R}\rightarrow \mathbb{R}^{\tilde p}$. Which best
describes the sign of $\phi(x)^\top \phi(x)$ for an input query $x$?

1.  $\phi(x)^\top \phi(x) \geq 0$ for all $x$ (but it may be that
    $\phi(x)^\top \phi(x) =0$ for some $x$)

2.  $\phi(x)^\top \phi(x) > 0$ for all $x$

3.  $\phi(x)^\top \phi(x) \leq 0$ for all $x$ (but it may be that
    $\phi(x)^\top \phi(x) =0$ for some $x$)

4.  $\phi(x)^\top \phi(x) < 0$ for all $x$

:::{note} Solutions
:class: dropdown
First answer is best.
:::

## Positive definite, part II

For any featurization $\phi$, one can show that the kernel defined by
$K(x,y)=\phi(x)^\top \phi(y)$ is positive semi-definite (also called
nonnegative definite). What does that mean?

:::{note} Solutions
:class: dropdown
It means this. Pick any set of $n$ points, $x_1,\ldots x_n$. Construct
the matrix $M_{i,j}=K(x_i,x_j)$. Then all eigenvalues of matrix $M$ are
nonnegative.
:::

