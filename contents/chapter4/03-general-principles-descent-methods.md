# Descent methods: general principles


## Principle of descent algorithms
Consider the unconstrained optimization problem
:::{math}
:label:prob_unconstrained_descent
\begin{array}{ll}
\minimize &\quad f_0(x)\\
\st & \quad x \in \mathbb{R}^n
\end{array}
:::

To solve [](#prob_unconstrained_descent), we construct a sequence $x^{(0)}, x^{(1)}, \ldots$ such that the $k+1$-th iteration reads
:::{math}
:label:eq:descent_direction_generic
x^{(k+1)} = x^{(k)} + \alpha_k d_k
:::
where $\alpha_k \geq 0$ is called **the step size** and $d_k$ is a **direction** at iteration $k$.
The goal is to construct a sequence $\lbrace x^{(k)} \rbrace$ that describes a **descent** algorithm, i.e.,
:::{math}
:label:eq:decrease_function_cdt
f_0(x^{(0)}) > f_0(x^{(1)}) > \ldots > f_0(x^{(k)}) > f_0(x^{(k+1)}) > \ldots
:::
with the goal that, in the limit of $k\to \infty$, $x^{(k)} \rightarrow x^\star$ with $ x^\star$ being (at least) a local minimizer of $f_0$.


:::{important} How to choose the direction $d_k$?
Recall that the gradient $\nabla f_0(x^{(k)})$ gives, by definition, the direction of steepest ascent at point $x^{(k)}$. Therefore, in order to satisfy the decrease condition [](#eq:decrease_function_cdt), one must take $d_k$ in a opposite direction, i.e.,
$$
\nabla  f_0(x^{(k)})^\top d_k < 0
$$
In other terms, $d_k$ must be taken as a **descent direction**.
:::

## A zoo of descent algorithms

Simply put, strategies for choosing the direction $d_k$ define different categories of optimization algorithms.
Generally speaking, $d_k$ is chosen as
$$
d_k = -B_k^{-1}\nabla f_0(x^{(k)})
$$
where $B_k$ is symmetric and invertible. In the next sections, we will see several classical choices for the matrix $B_k$:
- $B_k = I_n$ (identity matrix). This is known as the **Gradient Descent (GD) method**.
- $B_k = \nabla^2 f_0(x^{(k)})$ (Hessian of $f$). This is known as the **Newton's method**.
- $B_k \approx \nabla^2 f_0(x^{(k)})$ (approx. Hessian). This corresponds to **quasi-Newton** methods, where many strategies for choosing $B_k$ exist.

## How to define a stopping criterion

Before going over these methods in more detail, it is important to address the following question: how to check that an algorithm has *converged*? In other terms, when do we decide to stop the sequence [](#eq:descent_direction_generic)?

This is a wide topic, but the main idea is stop the algorithm whenever at the current iteration, a given **criterion** becomes smaller than a pre-defined (small) tolerance $\varepsilon$ (e.g., $\varepsilon = 10^{-6}$).
:::{note} classical stopping criterions
- Change in objective function: $\vert f_0(x^{(k+1)}) - f_0(x^{(k)})\vert \leq \varepsilon$
- Gradient norm: $\Vert \nabla f_0(x^{(k)}) \Vert \leq \varepsilon$
- Change in solution: $\Vert x^{(k+1)} - x^{(k)}\Vert \leq \varepsilon$
:::

To avoid scaling issues, it is common to use a criterion calculated in *relative* terms, for example:
$$
\frac{\vert f_0(x^{(k+1)}) - f_0(x^{(k)})\vert}{\vert f_0(x^{(k)}) \vert} \leq \varepsilon
\quad \text{or} \quad
\frac{\Vert x^{(k+1)} - x^{(k)}\Vert}{\Vert x^{(k)}\Vert} \leq \varepsilon
$$

```{admonition}
:class: tip
In addition to the above criterion, a condition on the maximal number of iterations is often used to control the computational burden.
```

Now, let us turn our attention to gradient descent.
