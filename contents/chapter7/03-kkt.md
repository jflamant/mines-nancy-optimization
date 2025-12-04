# Karush-Kuhn-Tucker conditions

## Motivation

In this section, we look for necessary and/or sufficient conditions for optimality of the constrained problem
$$
  \begin{split}
    \minimize&\quad f_0(x)\\
    \text{subject to}&\quad f_i(x) \leq 0, \quad i = 1, \ldots, m\\
    &\quad h_i(x) = 0, \quad i = 1, \ldots, p
    \end{split}
$$
with Lagrange dual problem
$$
  \begin{split}
    \maximize&\quad g(\lambda, \nu) := \inf_{x\in \mathcal{D}} \left(f_0(x) + \sum_{i=1}^m \lambda_i f_i(x) + \sum_{i=1}^p\nu_i h_i(x)\right)\\
    \text{subject to}&\quad \lambda \geq 0
    \end{split}
$$
Recall that, for unconstrained problems, such necessary and / or sufficient conditions are discussed in [Chapter 2](../chapter2/05-optimality-conditions.md).

## Complementary Slackness

:::{important} Assumptions
:label:assumption_KKT
- the primal and dual value are attained, i.e. there exist $x^\star \in \mathcal{F}$ such that $p^\star = f_0(x^\star)$ and there exist $(\lambda^\star, \nu^\star)\in \mathbb{R}^m_+ \times \mathbb{R}^p$ such that $g(\lambda^\star, \nu^\star) = d^\star$.
- strong duality holds, i.e., $d^\star = p^\star$.
:::

Let $x^\star$ and $(\lambda^\star, \nu^\star)$ be primal and dual optimal, respectively. Then
\begin{align*}
  f_0(x^\star) &= g(\lambda^\star, \nu^\star)&\text{(strong duality)}\\
  &=\inf_{x} \left(f_0(x)+ \sum_{i=1}^m \lambda_i^\star f_i(x) + \sum_{i=1}^p \nu_i^\star h_i(x)\right)&\text{(def of $g$)}\\
  &\leq f_0(x^\star)+ \sum_{i=1}^m \lambda_i^\star f_i(x^\star) + \sum_{i=1}^p \nu_i^\star h_i(x^\star)&\text{($\inf$ property)}\\
  &\leq f_0(x^\star)&\text{($x^\star, \lambda^\star$ are feasible)}
\end{align*}
Since by definition, $\sum_{i=1}^m \lambda_i^\star f_i(x^\star) \leq 0$, we conclude that $\sum_{i=1}^m \lambda_i^\star f_i(x^\star) = 0$ and since each term in the sum is negative, one has the **complementary slackness property**
$$
\lambda_i^\star f_i(x^\star) = 0, \quad i=1, \ldots, m
$$
or in other terms,
$$\lambda_i^\star > 0 \Rightarrow f_i(x^\star) = 0 \Longleftrightarrow f_i(x^\star) < 0 \Rightarrow \lambda_i^\star = 0$$
This means that the $i$-th optimal Lagrange multiplier $\lambda_i^\star$ is zero as long as the associated inequality constraint is **inactive** ($f_i(x^\star) < 0$). Conversely, if $\lambda_i^\star > 0$, this means the constraint is **active** ($f_i(x^\star) = 0$).

## KKT conditions

Under [the assumptions above](#assumption_KKT), we can formulate two KKT conditions for nonconvex and convex problems, respectively.


:::{prf:theorem} KKT conditions for nonconvex problems
Let $x^\star$ and $(\lambda^\star, \nu^\star)$ be primal and dual optimal with zero duality gap.
Then one has
- Primal feasibility:
$$f_i(x^\star) \leq 0, &\quad i=1, \ldots, m \\
h_i(x^\star) = 0, &\quad i=1, \ldots, p$$
- Dual feasibility:
$$\lambda_i^\star \geq 0, \quad i=1, \ldots, m$$

- Complementary slackness
$$\lambda_i^\star f_i(x^\star) = 0, \quad i=1, \ldots, m$$
- Stationarity of the Lagrangian with respect to $x$
$$
\nabla_{x} L(x^\star, \lambda^\star, \nu^\star) = 0 \Longleftrightarrow \nabla f_0(x^\star) + \sum_{i=1}^m \lambda_i^\star \nabla f_i({x^\star}) +\sum_{i=1}^p \nu_i^\star \nabla h_i(x^\star) = 0
$$
These five conditions are known as Karush-Kuhn-Tucker conditions.
:::

Hence, for any (differentiable) optimization problem where **strong duality holds**, KKT conditions are necessary conditions for optimality of the triplet $(x^\star, \lambda^\star, \nu^\star)$.

For convex problems, the KKT conditions become sufficient. Recall that a general convex problem can be put into the form
:::{math}
:label:cvx_pb_kkt
  \begin{split}
    \minimize&\quad f_0(x)\\
    \text{subject to}&\quad f_i(x) \leq 0, \quad i = 1, \ldots, m\\
    &\quad Ax = b
    \end{split}
:::

:::{prf:theorem} KKT conditions for convex problems
Consider the convex problem [](#cvx_pb_kkt) and let $\tilde{x}$ and $(\tilde\lambda, \tilde\nu)$ satisfy the KKT conditions
- Primal feasibility:
$$f_i(\tilde x) \leq 0, &\quad i=1, \ldots, m \\
A\tilde x = b$$
- Dual feasibility:
$$\tilde\lambda_i \geq 0, \quad i=1, \ldots, m$$

- Complementary slackness
$$\tilde\lambda_i f_i(\tilde x) = 0, \quad i=1, \ldots, m$$
- Stationarity of the Lagrangian with respect to $x$
$$
\nabla_{x} L(\tilde x, \tilde\lambda, \tilde\nu) = 0 \Longleftrightarrow \nabla f_0(\tilde x) + \sum_{i=1}^m \lambda_i \nabla f_i(\tilde{x}) + A^\top \tilde\nu = 0
$$
Then $\tilde x$ is primal optimal, $(\tilde \lambda, \tilde \nu)$ are dual optimal, with zero duality gap.
:::
In other words, for a convex problem, if the triplet $(\tilde{x}, \tilde\lambda, \tilde\nu)$ satisfies KKT conditions then we have found optimal points and strong duality holds.

Understanding why KKT works:
- the first two condition ensures that $\tilde{x}$ is primal feasible and $(\tilde{\lambda}, \tilde{\nu})$ are dual feasible
- complementary slackness implies that $f_0(\tilde{x}) = L(\tilde{x}, \tilde{\lambda}, \tilde{\nu})$
- stationarity of the Lagrangian with the fact that $L(\cdot, \tilde{\lambda}, \tilde{\nu})$ is convex show that $\tilde{x}$ minimizes the Lagrangian over $x$. Thus by definition $g(\tilde{\lambda}, \tilde{\nu}) = L(\tilde{x},\tilde{\lambda}, \tilde{\nu})$.
- hence $f_0(\tilde{x}) = g(\tilde{\lambda}, \tilde{\nu})$. Strong duality follows and $(\tilde{x}, \tilde{\lambda}, \tilde{\nu})$ are optimal.

In general, for convex problems KKT conditions are only **sufficient**. However, provided that strong duality holds, they become **necessary and sufficient**.

:::{prf:proposition} KKT conditions for convex problems: necessary and sufficient case.
:label:prop:KKT_sufficient_necessary
For a convex problem, if [Slater's condition](#prop:slater) is satisfied then  $\tilde x$ is primal optimal and $(\tilde \lambda, \tilde \nu)$ are dual optimal **if and only if** they satisfy the KKT conditions.
:::
In particular, Slater's condition implies strong duality and that the dual optimum is attained.

[](#prop:KKT_sufficient_necessary) generalizes the necessary and sufficient condition of unconstrained convex problems ($\nabla f_0(x) = 0$) to the general constrained convex case.

:::{exercise}
Solve the following optimization problem using KKT conditions:
$$
  \begin{split}
    \minimize&\quad \frac{1}{2}x^\top Q x - p^\top x+r\\
    \text{subject to}&\quad Ax = b
    \end{split}
$$
where $Q \succeq 0, A\in \mathbb{R}^{p\times n}, b\in \mathbb{R}^{p}$ and $\operatorname{rank} A = p < n$.
:::

## Summary

:::{important} Key notions of the chapter
- Lagrangian, Lagrange dual function and Lagrange dual problem
- Lagrange dual problem is always convex, even if the primal problem is not
- Dual problem provides a lower bound on the optimal value of the primal problem, both are equal when strong duality holds
- For convex problems, KKT conditions give sufficient optimality conditions (and necessary and sufficient when Slater's condition holds)
:::

**Comments**
- strong duality (almost always) holds for convex problems
- the dual problem may help to solve the primal problem very efficiently
- *duality* and *dual methods* are abundant in optimization and play a very important role in modern optimization; this course is just a rough sketch of the tip of the iceberg!
