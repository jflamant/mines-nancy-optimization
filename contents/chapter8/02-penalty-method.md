---
kernelspec:
    name: python3
---
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

**How to compute the gradient of the penalty function?**

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

### A nonconvex 1D example

We consider the following nonconvex optimization problem
:::{math}
\begin{array}{ll}
\minimize & \quad (x_1-1)^2 + (x_2+1)^2\\
\st&\quad x_1^2+x_2^2 - \sin(3x_1)\cos(2x_2) -2\leq 0
\end{array}
:::

The feasible set of this problem is nonconvex, as shown by the plot below (green area).
```{code-cell} python
:tags:[hide-input]
import numpy as np
import matplotlib.pyplot as plt

# define objective and constraints
def f(x, y):
    return (x - 2)**2 + (y + 1)**2

def g(x, y):
    """Nonlinear inequality constraint g(x,y) <= 0"""
    return -(2- x**2-y**2 +  np.sin(3*x) * np.cos(2*y))

# Grid for contour plots
xx = np.linspace(-2.5, 3, 400)
yy = np.linspace(-2.5, 2.5, 400)
X, Y = np.meshgrid(xx, yy)

F = f(X, Y)
G = g(X, Y)

plt.figure(figsize=(10, 8))

# Objective contours
CS1 = plt.contour(X, Y, F, levels=20, cmap='viridis', alpha=0.6)
plt.clabel(CS1, inline=1, fontsize=8)

# Constraint boundary (g(x,y)=0)
CS2 = plt.contour(X, Y, G, levels=[0], colors='red', linewidths=2)
plt.clabel(CS2, fmt={0: '$f_1(x) = 0$'}, fontsize=10)

# Feasible region shading
plt.contourf(X, Y, G, levels=[-10, 0], colors=['#d0ffd0'], alpha=0.3)
plt.xlabel("$x_1$")
plt.ylabel("$x_2$")
plt.scatter(2, -1, color='black', marker='*', s=150, label='Unconstrained solution')
plt.legend()
```
Let us display how the penalized cost function will look for different values of $\rho$.

```{code-cell} python
:tags:[hide-input]
rhos = [0, .1, .5, 1]
Gplus = np.maximum(0, G)
fig, ax = plt.subplots(ncols=len(rhos), figsize=(12, 4), sharex=True, sharey=True)
for i, rho in enumerate(rhos):

    ax[i].contour(X, Y, F+rho*(Gplus)**2)
    ax[i].set_title(r'$\rho = '+str(rho)+'$')
```
We can iteratively minimize penalized problems with increasing penalty parameter $\rho$. Let's do this numerically and plot the results.

```{code-cell} python
:tags:[hide-input]
# redefine function to be used with scipy minimize
from scipy.optimize import minimize

def f(xy):
    x, y = xy
    return (x - 2)**2 + (y + 1)**2

def g(xy):
    x, y = xy
    return -(2- x**2-y**2 +  np.sin(3*x) * np.cos(2*y))

def g_plus(xy):
    return max(0.0, g(xy))

# penalized objective
def P_mu(xy, rho):
    return f(xy) + 0.5 * rho * g_plus(xy)**2


rhos = [1, 2, 5, 10, 100]
solutions = []

x0 = np.array([2, -1])   # initial guess

for rho in rhos:

    # Wrap for minimize
    fun = lambda xy: P_mu(xy, rho)

    # Run optimizer
    res = minimize(fun, x0, method='BFGS') #options={'disp': True} to show details

    sol = res.x
    solutions.append(sol)


    # warm start next iteration
    x0 = sol

print("\nSolutions for increasing rho:")
for rho, sol in zip(rhos, solutions):
    print(f"rho={rho:4d}:  x={sol},  f_1(x)={g(sol)}")

## plots
xx = np.linspace(-2.5, 3, 400)
yy = np.linspace(-2.5, 2.5, 400)
X, Y = np.meshgrid(xx, yy)

F = (X - 2)**2 + (Y + 1)**2
G = X**2+Y**2 -  np.sin(3*X) * np.cos(2*Y)-2

plt.figure(figsize=(10, 8))

# Objective contours
CS1 = plt.contour(X, Y, F, levels=20, cmap='viridis', alpha=0.6)
plt.clabel(CS1, inline=1, fontsize=8)

# Constraint boundary
CS2 = plt.contour(X, Y, G, levels=[0], colors='red', linewidths=2)
plt.clabel(CS2, fmt={0: "$f_1(x) = 0$"}, fontsize=10)

# Feasible region shading
plt.contourf(X, Y, G, levels=[-10, 0], colors=['#d0ffd0'], alpha=0.3)

# Plot solutions for each mu
colors = ['blue', 'green', 'orange', 'purple', 'red']
for sol, rho, col in zip(solutions, rhos, colors):
    plt.scatter(sol[0], sol[1], color=col, s=80, label=f"ρ={rho}")


plt.title("Penalty Method in 2D")
plt.xlabel("$x_1$")
plt.ylabel("$x_2$")
plt.scatter(2, -1, color='black', marker='*', s=150, label='Unconstrained solution')
plt.legend()
plt.grid(alpha=0.3)
plt.show()
```

In the limit of large $\rho$, we would get the (unique) solution to the initial problem. Stopping at a given $\rho$ value gives an approximation of that point, characterized by the value of $f_1(x)$, which quantifies "how much" we are violating the constraint.
