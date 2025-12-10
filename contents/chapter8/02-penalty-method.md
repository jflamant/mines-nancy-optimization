# Penalty method

## Principle
Consider the optimization problem
:::{math}
:label:prob_penalty_original
  \begin{array}{ll}
      \minimize &\quad f_0(x)\\
      \st &\quad x \in \Omega
  \end{array}
:::
where $f_0$ is the objective and $\Omega \subset \mathbb{R}^n$ is the constraint set.

**Key idea** : replace [](#prob_penalty_original) with the unconstrained optimization problem
\begin{equation}
\begin{array}{ll}
    \minimize&\quad f_0(x) + \rho p(x)\\
    \st & \quad x \in \mathbb{R}^n
\end{array}
,\quad \rho > 0
\end{equation}
where $p:\mathbb{R}^n \rightarrow \mathbb{R}$ is the **penalty function** such that
- $p$ is continuous
- $p(x) > 0$ if $x \notin \Omega$
- $p(x)  = 0$ if $x \in \Omega$.

The **penalty parameter** $\rho$ controls the cost of violating the constraints.

## Choosing penalty functions

Recall $\Omega$ is often parameterized by equality and inequality constraints.
:::{math}
:label:eq:Omega_penalty
\Omega = \left\lbrace x \in \mathbb{R}^n \:\middle\vert \begin{array}{rl}
    f_i(x) \leq 0&, \quad i = 1, \ldots, m\\
    h_i(x) = 0&, \quad i = 1, \ldots, p
\end{array}\right\rbrace
:::

Typical choices of penalty functions: use \alert{quadratic functions}
- **inequality constraint** $f_i(x) \leq 0$: take
    $$
    p(x) = \frac{1}{2}\left[\max(0, f_i(x))\right]^2 := \frac{1}{2}\left[f_i^+(x)\right]^2
    $$
- **equality constraint** $h_i(x) = 0$: take
    $$
    p(x) = \frac{1}{2} (h_i(x))^2
    $$
For multiple constraints, we simply sum all individual penalties. For $\Omega$ defined in [](#eq:Omega_penalty) one would get
$$p(x) = \frac{1}{2}\sum_{i=1}^m (f_i^+(x))^2 + \frac{1}{2}\sum_{i=1}^p (h_i(x))^2$$

Note that besides quadratic functions, one can consider also non-smooth penalties such as $\ell_1$-norm with very interesting properties (outside the scope of this lecture).

## Penalty method algorithm

We thus replace solving [](#prob_penalty_original) by solving a succession of unconstrained penalized problems
:::{math}
:label:prob_rho_k
    \begin{array}{ll}
    \minimize&\quad f_0(x) + \rho_k p(x)\\
    \st & \quad x \in \mathbb{R}^n
\end{array},\quad \rho_k > 0
:::
with increasing penalties $\rho_1 < \rho_2 < \ldots < \rho_k \rightarrow +\infty$.

:::{prf:theorem} Convergence of the penalty method
  Let $f_0$ and $p$ be continuous.
  Let $x^{(k)}$ be a solution of [](#prob_rho_k). Suppose the sequence $\lbrace \rho_k\rbrace$ is strictly increasing with $\rho_k \rightarrow \infty$ as $k\to \infty$. Then $x^{(k)} \to x^\star$, where $x^\star$ is a solution of [](#prob_penalty_original).
:::

This results suggests the following algorithm.

:::{prf:algorithm} Penalty algorithm
:label:alg:penalty_method
**input** penalty function $p(x)$, increase parameter $s$, initial penalty $\rho_0$. Stopping criterion.

1. set $k=0$
2. **while** stopping criterion not satified **do**
    1.  $x^{(k+1)} \in \operatorname{arg\,min} \left(f_0(x) + \rho_k p(x)\right)$  (possibly using $x^{(k)}$)
    2. $k:=k+1$
    3. $\rho_{k+1}:= s\rho_k$

**return** (approximate) solution $x^{(k)}$
:::

## How-to compute solutions of penalized problems?
**Consider a problem with one inequality constraint** $f_1(x) \le 0$.
The quadratic penalty is
$$
p(x)=\tfrac12\left[\max(0,f_1(x))\right]^2
      =\tfrac12 \left[f_1^+(x)\right]^2 .
$$

### **How to compute the gradient of the penalty function?**

A potential concern is that $f_1^+$ is not differentiable at points where  $f_1(x)=0$. However, the penalty can be written as a composition
$$
p(x)=\gamma(f_1(x)), \qquad \gamma(y)=\tfrac12 \left[\max(0,y)\right]^2 .
$$

Although the map $y\mapsto \max(0,y)$ is not differentiable at 0, the **quadratic penalty $\gamma$ *is* differentiable everywhere**, because
$$
\gamma'(y)=\max(0,y),
$$
and in particular $\gamma'(0)=0$.  Therefore, if $f_1$ is differentiable, the chain rule applies.

Using the chain rule,
$$ \nabla p(x) = \gamma'(f_1(x))\, \nabla f_1(x) = f_1^+(x)\,\nabla f_1(x) =
\begin{cases}
f_1(x)\,\nabla f_1(x), & f_1(x)>0, \\[4pt]
0, & f_1(x)\le 0 .
\end{cases}
$$

Thus the quadratic penalty is differentiable (but not twice differentiable) even at points where the constraint is active, and its gradient is given by the expression above.

## Examples

### A simple problem
Consider the (very) simple problem
$$
    \begin{split}
      \text{minimize}&\quad x \\
      \text{subject to}&\quad x\geq 1
    \end{split}
$$
and solve it using the [penalty method](#alg:penalty_method).
:::{hint} Solution
:class:dropdown
Write the penalized problem:
$$
\operatorname{minimize} \quad x+ \frac{\rho}{2}(\max(0, 1-x))^2:=f_\rho(x),\qquad \rho >0
$$
The objective is convex, with gradient $f'_\rho(x)= 1 - \rho \max(0, 1-x)$.

Solving $f'_\rho(x) = 0$ gives $x=1-1/\rho$, which converges to $x=1$ as $\rho \to \infty$.
:::

### Minimum-norm solution of linear equations
Using the penalty method, solve the following problem:
\begin{equation*}
\begin{split}
    \text{minimize}&\quad \frac{1}{2} \Vert x\Vert_2^2 \\
    \text{subject to}&\quad Ax = b
\end{split}
\end{equation*}

:::{hint} Solution
:class:dropdown
Write the penalized problem:
$$
\operatorname{minimize} \quad \frac{1}{2}\Vert x\Vert^2 +\frac{\rho}{2}\Vert Ax-b\Vert_2^2,\qquad \rho >0
$$
The objective is convex, with gradient $\nabla f_\rho(x)= x + \rho A^\top (Ax-b)$. Solving $\nabla f_\rho(x) = 0$ gives $x_\rho = \rho(I_n + \rho A^\top A)^{-1}A^\top b$.

Tedious calculations (e.g., using the SVD) show that, in the limit $\rho\to \infty$, $x_\rho \to x^\star$ with $x^\star = A^\top(AA^\top)^{-1}b$.
:::
