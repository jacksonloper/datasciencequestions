# Splines

## Let $f(x)=\left|x\right|$

Is $f$ piecewise linear?

:::{note} Solutions
:class: dropdown
Yes: it can be written in a piecewise form\... $$f(x)=\begin{cases}
-x & \mathrm{if}\ x\leq0\\
x & \mathrm{if}\ x\geq0
\end{cases}$$ \...such that each of the pieces ($-x$ and $x$) are
linear.
:::

Is $f$ a linear spline? Why or why not?

:::{note} Solutions
:class: dropdown
Yes: it is piecewise linear, with one knot at $x=0$, and at that knot
$\lim_{x\uparrow0}f(x)=\lim_{x\downarrow0}f(x)=0$.
:::

Is $f$ a cubic spline? Why or why not?

:::{note} Solutions
:class: dropdown
No: its derivative at zero is discontinuous; $\lim_{x\uparrow0}f'(x)=-1$
but $\lim_{x\downarrow0}f'(x)=1$.
:::

## Linear splines, and a basis

Suppose $f$ is a linear spline. What conditions does it satisfy at its
knots? What conditions does it satisfy between its knots?

:::{note} Solutions
:class: dropdown
$f$ is continuous at the knots and linear between the knots.
:::

Consider the space of all linear splines with a single knot at
$\xi_{1}$. It is possible to create a three-dimensional *basis* for this
space, i.e. a set of three functions $h_0,h_1,h_2$ such that any
function in the space can be written as
$\beta_0 h_0(x) + \beta_1 h_1(x) + \beta_2 h_2(x)$ for some suitable
choice of coefficients $\beta \in \mathbb{R}^3$. List three functions
that, together, form such a basis.

:::{note} Solutions
:class: dropdown
Here's one option. $h_0(x) = 1$, $h_1(x) = x$, and
$h_2(x) = \mathbb{I}(x > \xi_1)(x-\xi_1)$.
:::

## Let $f(x)=2x+\mathbb{I}(x>0)x^{2}$

1.  Is its derivative continuous?

    :::{note} Solutions
    :class: dropdown
    Yes: it is a piecewise quadratic (every piece is a polynomial of
    degree at most 2) with a knot at $x=0$ and at that knot the limit of
    the derivative from below
    $\lim_{x\uparrow0}f'(x)=\lim_{x\uparrow0}2=2$ agrees with the limit
    of the derivative from above
    $\lim_{x\downarrow0}f'(x)=\lim_{x\downarrow0}2+2x=2$.
    :::

2.  Is it a cubic spline?

    :::{note} Solutions
    :class: dropdown
    No: its second derivative is discontinuous;
    $\lim_{x\uparrow0}f''(x)=0$ but
    $\lim_{x\downarrow0}f''(x)=\lim_{x\downarrow0}2=2$.
    :::

## Let $f(x)=\mathbb{I}(x>0)x^{7}$. Is it a cubic spline?

:::{note} Solutions
:class: dropdown
No: it is piecewise\... $$f(x)=\begin{cases}
0 & \mathrm{if}\ x\leq0\\
x^{7} & \mathrm{if}\ x\geq0
\end{cases}$$ \...but one of its pieces isn't a cubic polynomial. The
function $f$ is piecewise polynomial, but its degree is at most 7.
:::

## Write a linear spline

Give an example of a linear spline with three knots.

:::{note} Solutions
:class: dropdown
Here is a cheeky answer: $$f(x) = \begin{cases}
0 & \mathrm{if}\ x<0\\
0 & \mathrm{if}\ x\in[0,1)\\
0 & \mathrm{if}\ x\in[1,2)\\
0 & \mathrm{if}\ x>3.
\end{cases}$$ This has three knots, sort of, at $x=0,x=1,x=2,x=3$. There
is some ambiguity about what a knot actually is. Does this function
really have three knots? It is identically zero everywhere. Yet, the way
we defined it, in pieces, it is defined piecewise with four pieces, so
in that sense it has three knots. And it is certainly a linear spline
(each piece is linear and it is continuous), albeit a trivial one. I
wouldn't take off points for this answer, though it isn't what we think
of as a linear spline with three knots.

Here would be a more conventional sort of answer.

$$f(x) = \begin{cases}
0 & \mathrm{if}\ x<0\\
x & \mathrm{if}\ x\in[0,1)\\
2-x & \mathrm{if}\ x\in[1,2)\\
0 & \mathrm{if}\ x>2.
\end{cases}$$
:::

## Let $f(x)=\mathbb{I}(x>4)(x-4)^{3}$. Is it a natural cubic spline?

:::{note} Solutions
:class: dropdown
No, its first derivative grows unboundedly on the right,
$\lim_{x\rightarrow\infty}f'(x)=\infty$. So it is a cubic spline, but
not a natural cubic spline.
:::

## Fourth derivatives for cubic splines

Let $f(x)$ be a cubic spline with knots at 0 and 5. What is the fourth
derivative of $f$ evaluated at $3$, $f''''(3)$?

:::{note} Solutions
:class: dropdown
Zero.
:::

## General question type

More generally, given any sort of piecewise-defined function,
$$f(x)=\begin{cases}
a_{1}+b_{1}x+c_{1}x^{2}+d_{1}x^{3} & \mathrm{if}\ x<\xi_{1}\\
a_{2}+b_{2}x+c_{2}x^{2}+d_{2}x^{3} & \mathrm{if}\ \xi_{1}\leq x<\xi_{2}\\
a_{3}+b_{3}x+c_{3}x^{2}+d_{3}x^{3} & \mathrm{if}\ \xi_{2}\leq x
\end{cases}$$ we might ask you questions like

1.  it is a linear spline?

2.  is it a cubic spline?

3.  is it a natural cubic spline?

4.  is it continuous?

5.  is the first derivative of $f$ continuous?

6.  is the second derivative of $f$ continuous?

7.  are the pieces constant (true only if $b_{i}=c_{i}=d_{i}=0$)?

8.  are the pieces linear (true only if $c_{i}=d_{i}=0$)?

9.  are the pieces quadratic (true only if $d_{i}=0$)?

You'll need to be able to calculate the answers to these questions in
terms of the coefficients. Because the pieces are all polynomials, any
derivative of these pieces themselves will be continuous (e.g., if
$f(x)$ is a cubic polynomial then **every** derivative of $f$ is
continuous). So when you're thinking about continuity of derivatives for
piecewise polynomials, you really just have to check the knots.

## Smoothing spline

Consider a dataset with $p=12$ features and one numerical response for
each of $n=2000$ data points. You would like to fit this data using a
GAM, with a smoothing spline for each feature. Which of the following
will be your biggest challenge?

a.   Picking the right regularization strength for each feature.

b.   Choosing the right number of knots for each feature.

:::{note} Solutions
:class: dropdown
The first answer is correct. With smoothing splines you don't have to
pick knots. But picking the regularization strength (that controls the
roughness penalty) can be very challenging.

Generalized additive models (GAMs) make it *possible* to use methods
that penalize "roughness" (integrated squared second derivative) even
when $p$ is large: construct a one-dimensional component function for
each feature and apply the roughness penalty to each component function
separately. However, although GAMs make it possible, it isn't exactly
easy, because best results are often found by giving a different
strength of roughness penalty to each component feature. Picking those
regularization strengths is tricky.
:::

