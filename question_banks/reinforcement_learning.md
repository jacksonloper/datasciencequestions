# Reinforcement learning

## Pick your preferences

Consider a MAB problem with two possible actions, $a \in \{1,2\}$. Let
the distribution of rewards for action $a=1$ be given by
$\rho_1 = \mathcal{N}(5,1)$. Let the distribution of rewards for action
$a=2$ be given by $\rho_2 = \mathcal{N}(10,1)$.

Assuming the policy $\pi$ is computed as the softmax of a preference
vector $H \in \mathbb{R}^2$, which preference vector would lead to the
highest expected reward for an actor following policy $\pi$?

1.  $H = (0,1)$

2.  $H = (-5,1)$

3.  $H = (0,0)$

4.  $H = (-5,0)$

:::{note} Solutions
:class: dropdown
$H = (-5,1)$ is best, though it will be pretty close to $H = (-5,0)$.
Basically the only thing that matters is $H_2-H_1$, which gives the log
odds of picking action 2.
:::

## Stochastic gradient descent

Stochastic gradient descent is an iterative method for minimizing a loss
function $L(\beta)$. It is performed iteratively. At each step, we have
parameters $\beta$, we produce an estimate $g \approx \nabla L(\beta)$,
and update $\beta$ according to $g$.

If the estimate $g$ is awful, this won't work. Which of the following is
a typical condition that we seek to satisfy in designing an SGD
algorithm?

1.  $\mathbb{E}[\Vert g\Vert] / \mathrm{var}(\Vert g\Vert) > \Vert \nabla L(\beta) \Vert / \mathrm{var}(\Vert g\Vert)$

2.  $\mathbb{P}(\Vert g- \nabla L(\beta) \Vert  > \Vert \nabla L(\beta) \Vert) < \frac{1}{2}$

3.  $\mathbb{E}[g]= \nabla L(\beta)$

4.  $\Vert \mathbb{E}[g]\Vert = \Vert \nabla L(\beta)\Vert$

:::{note} Solutions
:class: dropdown
Third answer is correct: unbiasedness.
:::

## Expected reward

Consider a MAB problem with two possible actions, $a \in \{1,2\}$. Let
the distribution of rewards for action $a=1$ be given by
$\rho_1 = \mathcal{N}(5,1)$. Let the distribution of rewards for action
$a=2$ be given by $\rho_2 = \mathcal{N}(10,1)$. What is the expected
reward for a policy $\pi=(0.2,0.8)$ that selects $a=1$ with probability
$0.2$ and selects $a=2$ with probability $0.8$?

:::{note} Solutions
:class: dropdown
$.2\times 5 + .8\times 10 = 1+8=9$.
:::

## What kind of RL problem is this?

Consider the following three problems.

a.  You are training a robot to balance a carrot on its nose. In each
    episode you place a carrot on the robot's nose. Every 10
    milliseconds, you estimate how balanced the carrot is and give the
    robot a reward for being balanced. The robot then calculates how it
    is going to move over the next 10 milliseconds. You continue the
    episode for 3 minutes. The goal is for the robot to keep the carrot
    well-balanced for all 3 minutes. The episode can also end early if
    the robot drops the carrot. You train the robot over many episodes.

b.  You have 10 bouncy icosahedrons and you want to know which one
    bounces highest on average. How high the balls bounce is somewhat
    random (it depends on the angle they land at). You can perform
    experiments where you drop a ball from a one-story balcony and then
    observe how high it bounces. Unfortunately, you only have time to
    perform 50 experiments.

c.  You have a machine that can throw balls, and you want to learn how
    to make the machine throw different kinds of balls very far. For any
    given ball, you can find out how heavy it is and what diameter it
    is, program the settings of the machine, and ask the machine to
    perform the throw. You want to find out how to pick the best
    settings based on the properties of the ball.

For each problem, indicate what kind of problem it is: MAB, Contextual
MAB, or POMDP.

:::{note} Solutions
:class: dropdown
\(a\) POMDP, (b) MAB, (c) contextual MAB
:::

## Expectations

Consider a MAB problem with $K=4$ possible actions ("arms"). Let
$\mu_k = \mathbb{E}_{\rho_k}[R]$ denote the average reward for taking
action $k$. Consider a policy $\pi=(.2,.3,.1,.4)$ on the four possible
actions. Assume $\mu=(5,10,20,30)$. Assuming $A \sim \pi$ and
$[R|A]\sim \rho_A$, compute $$\mathbb{E}[\mu_A - R]$$

:::{note} Solutions
:class: dropdown
The answer is zero, because $\mathbb{E}[R|A=k]=\mu_k$.
:::

## More expectations

Consider a MAB problem with $K=4$ possible actions ("arms"). Assume that
the reward is a deterministic function of the action,
$$R(A) = \begin{cases}
2 & \mathrm{if}\ A\in \{1,2,3\} \\
10 & \mathrm{if}\ A=4\\
\end{cases}$$ Consider a policy $\pi=(.2,.3,.1,.4)$ on the four possible
actions. Assuming $A \sim \pi$, calculate the value of

$$\mathbb{E}[R(A)\times\mathbb{I}(A=3)]$$

:::{note} Solutions
:class: dropdown
The expectation can be expanded as a sum
$$\sum_{a=1}^4 \pi_a R(a)\mathbb{I}(a=3).$$ The indicator function is
zero unless $a=3$. So the only nonzero term is
$\pi_3 R(3) = .1 \times 2 = .2$.
:::
