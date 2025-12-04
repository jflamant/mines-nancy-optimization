---
kernelspec:
  name: python3
  display_name: 'Python 3'
---
# Lagrangian duality

## The Lagrangian

Let us consider the general constrained optimization problem
:::{math}
:label:prob_general_lagrangian
\begin{split}
\minimize&\quad f_0(x)\\
\text{subject to}&\quad f_i(x) \leq 0, \quad i = 1, \ldots, m\\
&\quad h_i(x) =  0, \quad i = 1, \ldots, p
\end{split}
:::


:::{prf:definition} Lagrangian of a constrained optimization problem
:label:def:lagrangian
The **Lagrangian** of the problem [](#prob_general_lagrangian) is defined as the function $L: \mathcal{D}\times \mathbb{R}^m\times \mathbb{R}^p \to \mathbb{R}$ such that
$$
L(x, \lambda, \nu) = f_0(x) + \sum_{i=1}^m \lambda_i f_i(x) + \sum_{i=1}^p\nu_i h_i(x)
$$
where $\lambda \in \mathbb{R}^m$ and $\nu \in \mathbb{R}^P$ are Lagrange multiplier vectors.
:::
**Comments**
- $\lambda_i$ is the Lagrange multiplier associated to the $i-$th inequality constraint $f_i(x) \leq 0$;
- $\nu_i$ is the Lagrange multiplier associated to the $i-$th equality constraint $h_i(x) = 0$
- the Lagrangian is defined for **unfeasible** points: it penalizes the violation of constraints through Lagrange multipliers.

## The Lagrange dual function
By minimizing the [Lagrangian](#def:lagrangian) over all $x$ in the domain $\mathcal{D}$, we get a very useful function, called the **(Lagrange) dual function**.

:::{prf:definition} Lagrange dual function
:label:def:lagrange_dual_function
The Lagrange dual function of the problem [](#prob_general_lagrangian) is defined as the function $g: \mathbb{R}^m\times \mathbb{R}^p \to \mathbb{R}$ such that
$$  g(\lambda, \nu) = \inf_{x\in \mathcal{D}} L(x, \lambda, \nu) = \inf_{x\in \mathcal{D}}\left(f_0(x) + \sum_{i=1}^m \lambda_i f_i(x) + \sum_{i=1}^p\nu_i h_i(x)\right)$$
:::

**Comments**
- the dual function $g$ is **always concave**, even if the problem [](#prob_general_lagrangian) **is not convex**. Proof below!
- if the Lagrangian $L$ is unbounded below in variable $x$, the dual function $g$ takes on the value $-\infty$
-  the dual function provides a lower bound of the optimal value $p^\star$ of the problem ($\mathcal{P})$.

:::{hint} On the concavity of $g$
To prove that $g$ is a concave function, recall that a function $g: \mathcal{C} \to \mathbb{R}$ is concave if 1) $\mathcal{C}$ is a convex set and 2) $-g$ is a convex function, i.e.,
$$\text{for all }x,y \in \mathcal{C}, \text{ for all } \theta \in [0, 1], g(\theta x + (1-\theta)y ) \geq \theta g(x) + (1-\theta) g(y)$$
Applying this definition to the dual function, we get for $\mathcal{C} =  \mathbb{R}^m\times \mathbb{R}^p$ which is clearly convex. For point 2), note $\xi = (\lambda, \nu)$ for simplicity.
Take $x \in \mathcal{D}$. For all $\xi,\xi'$ and for all $\theta \in [0, 1]$,
$$L(x, \theta \xi + (1-\theta)\xi') = \theta L(x, \xi) + (1-\theta) L(x, \xi')$$
by definition of the Lagrangian. Or, by definition of the Lagrange dual function, we have $L(x, \xi) \geq g(\xi)$. Therefore,
$$L(x, \theta \xi + (1-\theta)\xi' ) \geq \theta g(\xi) + (1-\theta) g(\xi')$$
and since this is true for any $x \in \mathcal{D}$, in particular,
$$g(\theta \xi + (1-\theta)\xi') = \inf_{x \in \mathcal{D}}L(x, \theta \xi + (1-\theta)\xi' ) \geq \theta g(\xi) + (1-\theta) g(\xi')$$
which shows that $g$ is concave.
:::

Another very important property is that $g$ gives a lower bound on the optimal value of the initial problem [](#prob_general_lagrangian).

:::{prf:property} Lower-bound property of the Lagrange dual function
:label:lower_bound_prop_g
For any $\lambda \in \mathbb{R}^m$ such that $\lambda\geq 0$ and any $\nu \in \mathbb{R}^p$, we have
$$
g(\lambda, \nu) \leq p^\star = \inf_{x \in \mathcal{F}} f_0(x)
$$
:::
:::{prf:proof}
:class:dropdown
Suppose that $\tilde{x}$ is feasible and that $\lambda \geq 0$.
Hence $f_i(\tilde{x}) \leq 0$ for $i=1, \ldots, m$ and $h_i(x)= 0$ for $i=1, \ldots, p$.
It results that $\sum_{i=1}^m \lambda_i f_i(\tilde{x}) + \sum_{i=1}^p\nu_i h_i(\tilde{x}) \leq 0$.
Therefore
$$
  L(\tilde{x}, \lambda, \nu) = f_0(\tilde{x})+\sum_{i=1}^m \lambda_i f_i(\tilde{x}) + \sum_{i=1}^p\nu_i h_i(\tilde{x}) \leq f_0(\tilde{x})
$$
  Hence,
$$
  f_0(\tilde{x}) \geq L(\tilde{x}, \lambda, \nu) \geq  \inf_{x\in \mathcal{D}} L({x}, \lambda, \nu) = g(\lambda, \nu)
$$
The inequality holds for all feasible $\tilde{x}$ and in particular for $\tilde{x} = x^\star$, one gets $g(\lambda, \nu) \leq p^\star$.
:::

:::{exercise}
For the two problems below, write the Lagrangian, Lagrange dual function and obtain a lower bound on the optimal value of the problem.
1. minimum norm solution of least-squares equations
$$
  \begin{split}
    \minimize&\quad \Vert x\Vert^2\\
    \text{subject to}&\quad Ax = b, \quad \text{where } A \in \mathbb{R}^{p\times n}, b \in \mathbb{R}^p
    \end{split}
$$
2. linear program in standard form
$$
  \begin{split}
    \minimize&\quad c^\top x\\
    \text{subject to}&\quad Ax = b, \quad \text{where } A \in \mathbb{R}^{p\times n}, b \in \mathbb{R}^p\\
    &\quad x\geq 0
    \end{split}
$$
:::

## The Lagrange dual problem

We can exploit the lower bound property of the dual function to obtain the **best** lower bound possible on the original problem [](#prob_general_lagrangian) (which we call the *primal problem*).

:::{prf:definition} Lagrange dual problem
For the primal problem  [](#prob_general_lagrangian), the Lagrange dual problem is
$$
  \begin{split}
    \maximize&\quad g(\lambda, \nu)\\
    \text{subject to}&\quad \lambda \geq 0
    \end{split}
$$
:::

**Comments**
- a pair $(\lambda, \nu)$ is said to be \alert{dual feasible} if $\lambda \geq 0$ and $g(\lambda, \nu) > -\infty$
- the dual optimal parameters are denoted by $(\lambda^\star, \nu^\star)$
- the dual problem is **always convex** (since $g$ is always concave) even if the primal problem isn't!

## Weak and strong duality

 Let $d^\star = \sup_{\lambda \geq 0} g(\lambda, \nu)$ be the optimal value of the dual Lagrange problem.
By the [lower bound property of $g$](#lower_bound_prop_g), we have the fundamental property of **weak duality**.

:::{prf:property} Weak duality
We have
$$d^\star \leq p^\star$$

:::


**Comments**
-  Weak duality always holds even if the primal problem [](#prob_general_lagrangian) is not convex
- the property works even if $d^\star$ or $p^\star$ are infinite.
    - If the primal problem is unbounded below, $p^\star = -\infty$ and hence $d^\star = -\infty$ (dual is infeasible);
    - if the dual problem is unbounded above, $d=\infty$ and thus $p^\star = \infty$ (primal is infeasible).
- the quantity $p^\star - d^\star \geq 0$ is called the duality gap

:::{prf:definition} Strong duality
When $d^\star = p^\star$, we say that **strong duality** holds.
:::

**Comments**
- strong duality *does not always hold*.
- strong duality is equivalent to having zero duality gap;
- strong duality can be ensured by **constraint qualifications**.

For convex problems, a useful constraint qualification is known as *Slater's criterion*.

:::{prf:proposition} Slater's constraint qualifications
If the primal problem is convex, i.e., it takes the form
$$
    \begin{split}
    \minimize&\quad f_0(x)\\
    \text{subject to}&\quad f_i(x) \leq 0, \quad i = 1, \ldots, m\\
    &\quad Ax = b
    \end{split}
$$
where $f_i$, $i=0, \ldots, m$ are convex functions, and if there exists a feasible $\tilde{x}$ s.t. $f_i(\tilde{x})< 0, i = 1, \ldots, m$ and $A\tilde{x} = b$ (i.e., it is strictly feasible) then strong duality holds.
In addition, when $d^\star > -\infty$, the dual optimum is attained.
:::

Keep in mind that Slater's criterion only applies to **convex problems**; in addition it is a sufficient condition for strong duality. Strong duality can exist even if Slater's criterion is not satisfied.

A few examples to put this notion into practice.

:::{prf:example}
Using Slater's criterion, discuss the strong duality of the following optimization problems
- minimum norm solution of least-squares equations
$$
\begin{split}
    \minimize&\quad \Vert x\Vert^2\\
    \text{subject to}&\quad Ax = b, \quad \text{where } A \in \mathbb{R}^{p\times n}, b \in \mathbb{R}^p
    \end{split}
$$
- quadratic constrained quadratic program
$$
  \begin{split}
    \minimize&\quad \frac{1}{2}x^\top Q_0x - p^\top_0 x\\
    \text{subject to}&\quad \frac{1}{2}x^\top Q_1x - p^\top_1 x + r_1 \leq 0
    \end{split}
$$
where $Q_0 \succ 0$ and $Q_1 \succeq 0$.
:::


## Saddle-point interpretation
Consider, for simplicity, the optimization problem without equality constraints
$$
\begin{split}
\minimize&\quad f_0(x)\\
\text{subject to}&\quad f_i(x) \leq 0, \quad i = 1, \ldots, m
\end{split}
$$
with Lagrangian $L(x, \lambda) =  f_0(x) + \sum_{i=1}^m \lambda_i  f_i(x)$. Observe that
$$
\sup_{\lambda\geq 0}L(x, \lambda)= \sup_{\lambda\geq 0} \left(f_0(x) + \sum_{i=1}^m \lambda_i  f_i(x)\right) = \begin{cases}
  f_0(x) & \text{ if } x \text{ is feasible}\\
  \infty & \text{ otherwise}
\end{cases}
$$
To see this, observe that if $x$ is feasible, then $f_i(x) \leq 0$ for all $i=1, \ldots, m$. Hence the optimal choice of $\lambda=0$. If $x$ is not feasible, then for some $i$, $f_i(x) > 0$. Hence $L(x, \lambda)$ is unbounded above as a function of $\lambda$ and $\sup_{\lambda\geq 0}L(x, \lambda) = \infty$.

:::{hint} Rewriting primal and dual optimal values
$$
p^\star = \inf_{x} \sup_{\lambda\geq 0}L(x, \lambda), \quad   d^\star =  \sup_{\lambda\geq 0}\inf_{x}L(x, \lambda)
$$
:::

When strong duality holds $p^\star = d^\star$ and thus
$$
\inf_{x} \sup_{\lambda\geq 0}L(x, \lambda) = \sup_{\lambda\geq 0}\inf_{x}L(x, \lambda)
$$
i.e., the order of maximization/ minimization can be interchanged without changing the result.

This equality also characterizes the *saddle point property* of the Lagrangian $L:\mathcal{D} \times \mathbb{R}^m_+$; if $x^\star$ and $\lambda^\star$ are primal and dual optimal (and strong duality holds) one has
$$
L(x^\star, \lambda) \leq L(x^\star, \lambda^\star) \leq L(x, \lambda^\star) \text{ for any } x \text{ and } \lambda \geq 0
$$
i.e., $(x^\star, \lambda^\star)$ define a saddle point of the Lagrangian.

Conversely, if $(x, \lambda)$ is a saddle-point of the Lagrangian, then $ x$ is primal optimal, $\lambda$ is dual optimal with zero duality gap.

::::{prf:example} A simple 1D example

Consider the problem
$$
\begin{split}
\minimize&\quad \frac{1}{2}x^2\\
\text{subject to}&\quad 1-x \leq 0
\end{split}
$$
The solution to the primal problem is trivial and is $x^\star = 1$ with $p^\star =\frac{1}{2}$. The Lagrangian is $L(x, \lambda) = \frac{1}{2}x^2 + \lambda (1-x)$. The dual function is $g(\lambda) = \inf_x L(x, \lambda) = \frac{\lambda^2}{2} + (1-\lambda)\lambda = -\frac{\lambda^2}{2} + \lambda$. The function $g$ is maximal for $\lambda^\star = 1$; hence $d^\star = \frac{1}{2}$. Hence strong duality holds.

Let us compute the Hessian of the Lagrangian at point $(x^\star, \lambda^\star) = (1, 1)$. One gets
$$\nabla^2 L(1, 1) = \begin{bmatrix}
1 &-1\\
-1 & 0
\end{bmatrix}
$$
which has eigenvalues of distinct sign. This shows that the point $(1, 1)$ is indeed a saddle point of the Lagrangian.


:::{code-cell} python
import numpy as np
import matplotlib.pyplot as plt

def L(x, lam):
    return 0.5*x**2 + lam*(1-x)

x = np.linspace(-0.5, 2.5, 128)
lam = np.linspace(0, 2, 128)

xx, ll = np.meshgrid(x, lam)
plt.figure()
plt.contourf(x, lam, L(xx, ll))
plt.colorbar()
plt.axhline(1, color='k', linestyle = 'dashed')
plt.axvline(1, color='k', linestyle = 'dashed')
plt.scatter([1], [1], label='saddle point', color='r')
plt.legend()
plt.xlabel('$x$')
plt.ylabel('$\lambda$')

fig, ax = plt.subplots(ncols=2, sharey=True)
ax[0].plot(L(x, 1), label='$L(x, \lambda^\star)$', color='b')
ax[1].plot(L(1, lam),  label='$L(x^\star, \lambda)$', color='r')
ax[0].set_xlabel('$x$')
ax[1].set_xlabel('$\lambda$')
fig.legend()
:::
::::
