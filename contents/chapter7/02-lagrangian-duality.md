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


## Weak and strong duality

## Saddle-point interpretation
