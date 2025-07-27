# Optimality conditions for constrained convex problems

We now turn our attention to **convex constrained optimization problems** of the form
```{math}
:label:optim-problem-constrained
\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}
```
where $f_0$ is a convex function and $\Omega$ a convex set (by definition of a convex optimization problem). We also assume that $f_0$ is differentiable (i.e., $f_0 \in C^1$).

One of the key feature of convex optimization problems is that any local optimum is also a global optimum (see [this theorem](#thm:uniqueness_cvx)).


## First-order characterization of optimal points for convex problems
:::{prf:theorem}
:label: thm:first-order-solution-cvx
A point ${x}^\star$ is a solution of [](#optim-problem-constrained) if and only if ${x}^\star \in \Omega$ and
$$
\label{eq:supporting-hyperplane-cdt}
\nabla f_0({x}^\star)^\top ({y}-{x}^\star) \geq 0\qquad \text{ for all }{y} \in \Omega
$$
:::
:::{prf:proof}
:enumerated: false
:class:dropdown
- ${\Leftarrow}$ Let ${x}^\star\in \Omega$ such that the condition is satisfied. Then by convexity of $f_0$, one has for ${y} \in \Omega$, $f_0({y}) \geq f_0({x}^\star) + \nabla f_0({x}^\star)^\top ({y}-{x}^\star) \geq f_0({x}^\star)$. Thus ${x}^\star$ is optimal. 

- ${\Rightarrow}$ Suppose ${x}^\star \in \Omega$ is optimal but the conditions does not hold. There is some ${y} \in \Omega$ for which $\nabla f_0({x}^\star)^\top ({y} - {x}^\star) < 0$.
Consider ${z}(t) = t {y} + (1-t){x}^\star$ for $t \in [0, 1]$. Since $\Omega$ is convex, ${z}(t) \in \Omega$.
Moreover, observe that $f'_0({z}(t)) = \nabla f_0({z}(t))^\top ({y}-{x}^\star)$ and thus for $t=0$ one has $f'_0({z}(t))\vert_{t=0} = \nabla f_0({x}^\star)^\top ({y}-{x}^\star) < 0$.
For small positive $t$, we thus have $f_0({z}(t)) \leq f_0({x}^\star)$ which shows that ${x}^\star$ is not optimal. Hence we have a contradiction.
:::
:::{prf:remark}
[](#thm:first-order-solution-cvx) generalizes the necessary and sufficient condition ($\nabla f_0({x}^\star) = 0 \Leftrightarrow {x}^\star$ is solution) encountered for unconstrained convex problems.

:::
## Geometric interpretation
If $\nabla f_0({x}^\star) \neq 0$, the vector $-\nabla f_0({x}^\star)$ defines a **supporting hyperplane** to $\Omega$ at ${x}$.

:::{hint} Supporting hyperplane (see @boyd2004convex [Section 2.5.2])
Let $\mathcal{C} \subseteq \mathbb{R}^n$ and $x_0$ a point at the boundary of $\mathcal{C}$. Consider a vector $a$ such that $a^\top x \leq a^\top x_0$ for all $x \in \mathcal{C}$. In this case, the hyperplane $\lbrace x \vert a^\top x = a^\top x_0\rbrace$ is a supporting hyperplane to $\mathcal{C}$ at $x_0$. 
:::
In the context of [](#thm:first-order-solution-cvx), letting $a = -\nabla f_0(x^\star)$ the condition [](#eq:supporting-hyperplane-cdt) rewrites as ${a}^\top {y} \leq {a}^\top {x}^\star$ for all $y \in \Omega$. Therefore, 
$\lbrace {y} \mid {a}^\top {y} = {a}^\top {x}^\star \rbrace$ is a supporting hyperplane to $\Omega$ at point $x^\star$.

```{figure} figures/optimality-condition-convex.png
:width: 75%
:alt: Optimality condition illustration
A geometric illustration of the optimality condition for convex problems.
```

:::{important}
The condition of [](#thm:first-order-solution-cvx) is not always easy to check and is limited to convex optimization problems. 
In a few chapters, we'll see how further conditions can be derived with the help of **Lagrange duality theory**. 
:::