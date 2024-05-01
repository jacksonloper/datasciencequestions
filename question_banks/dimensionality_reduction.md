# Dimensionality reduction

## Fitting an autoencoder by hand

Let's say you have the following dataset, $\mathcal{D}=\{x_1,x_2\}$,
shown below.

   **Sample**   Height   Weight
  ------------ -------- --------
       #1         1        2
       #2         1        4
       #3         1        6
       #4         1        13

Say you use this data and find an optimal linear autoencoder with one
latent dimension. You get two functions,
$f_e:\ \mathbb{R}^2\rightarrow \mathbb{R}$, and
$f_d:\ \mathbb{R}\rightarrow \mathbb{R}^2$.

What will be the value of
$$\sum_{i=1}^6\Vert f_d(f_e(x_i))-x_i\Vert^2?$$ *(Hint: it may be
helpful to try to explicitly find suitable functions $f_e,f_d$.)*

:::{note} Solutions
:class: dropdown
Zero. Here is a suitable autoencoder. $$\begin{aligned}
        f_e(x_{i1},x_{i2}) &= x_{i2} \\ 
        f_d(z) &= (1,x_{i2}) \
    
\end{aligned}$$ The dataset has *no diversity* in the Height variable,
so, as far as this dataset is concerned, we can summarize everything
about each sample by looking at Weight alone.
:::

## Reconstruction error

You have been given an autoencoder: $$\begin{aligned}
        f_e(x_1,x_2,x_3) &= (0.5*x_1+0.5*x_2,x_3) \\ 
        f_d(z_1,z_2) &= (z_1,z_1,z_2) \
    
\end{aligned}$$

What is the squared reconstruction error for the point $x=(2,4,6)$?

:::{note} Solutions
:class: dropdown
$f_e(2,4,6)=(3,6)$ and $f_d(f_e(2,4,6))=(3,3,6)$. So the squared
reconstruction error is $(2-3)^2+(4-3)^2+(6-6)^2=2$.
:::

## PCA and scaling

You have a dataset $\mathcal{D}$ with $n=100$ samples and $p=30$
features per sample. You run PCA to get a 2d latent representations
(summaries) for each point. You store these summaries in a matrix,
$U \in \mathbb{R}^{100 \times 2}$. Then you standardize your dataset
with the standard scaler, run PCA on the standardized data, and get a
different matrix of summaries $\tilde U \in \mathbb{R}^{100 \times 2}$.
Which is true?

1.  $U=\tilde U$.

2.  There exists a $2\times 2$ matrix $A$ such that $UA=\tilde U$.

3.  None of the above.

:::{note} Solutions
:class: dropdown
None of the above is correct. Running PCA on standardized data will give
you fundamentally different latent representations. One way to see why
is to consider that PCA minimizes the mean squared reconstruction
error---and that reconstruction error is measured in terms of euclidean
distance in the feature space. If you rescale the feature space, you're
changing what distance means, and you're changing what is considered a
"large" error.
:::

## Variance explained

Let $X \in \mathbb{R}^p$ be a random variable. Let $\mu=\mathbb{E}[X]$
and $b=\mathbb{E}[\Vert X - \mu \Vert^2]$. Say you have fit an
autoencoder $f_e,f_d$. Say the mean squared reconstruction error is
given by $\epsilon = \mathbb{E}[\Vert f_d(f_e(X))-X\Vert^2]$. What is
the formula for the proportion of variance explained by the autoencoder?

:::{note} Solutions
:class: dropdown
$1 - (\epsilon/b)$.
:::

