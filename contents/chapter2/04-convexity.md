# Convexity and uniqueness of solutions

Convexity is a key tool to study **uniqueness** of solutions in optimization.

## Convex sets

:::{prf:definition} Convex set
A set $\mathcal{C}$ is convex if for every ${x}_1, {x}_2 \in \mathcal{C}$ and for every $\theta \in [0, 1]$, one has
$$\label{eq:convex_set}\theta {x}_1 +(1-\theta){x}_2 \in \mathcal{C}$$
:::
Equation [](#eq:convex_set) states that, for every ${x}_1, {x}_2 \in \mathcal{C}$, any point on the segment joining ${x}_1$ and ${x}_2$ must be also in $\mathcal{C}$. 

::::{grid} 1 2 2 2
:::{card}
:header: convex set 
:::{figure} figures/convex_set.png
:::

:::{card}
:header: non-convex set
:::{figure} figures/nonconvex_set.png
:::

::::

## Convex functions
:::{prf:definition} Convex function
:label:def:convex_function
Let $\Omega \subseteq \mathbb{R}^n$ be a convex set. 
The function $f:\Omega \rightarrow \mathbb{R}$ is said to be convex (on $\Omega$) if for every ${x}_1, {x}_2 \in \Omega$ and for every $\theta \in [0, 1]$, one has
$$f(\theta{x}_1 + (1-\theta){x}_2) \leq \theta f({x}_1) + (1-\theta)f({x}_2)$$
and strictly convex if the inequality is strict.
:::

:::::{prf:example} Examples of convex and non-convex functions (1-D)
::::{grid} 1 2 2 2
:::{card}
:header: strictly convex function
:::{figure} figures/convexExample0.png
:::

:::{card}
:header: strictly convex function
:::{figure} figures/convexExample1.png
:::

:::{card}
:header: convex function
:::{figure} figures/convexExample2.png
:::

:::{card}
:header: non-convex function
:::{figure} figures/convexExample3.png
:::

::::
:::::

## Useful characterization results for convex functions
In the following we give two practical theorem to characterize the convexity of functions $f:\Omega \to \mathbb{R}$, where $\Omega \subseteq \mathbb{R}^n$ is a convex set. We assume sufficient smoothness (e.g., $f \in C^1$ or $f\in C^2$). To simplify we state results for $\Omega = \mathbb{R}^n$, but the extension to arbitrary convext sets is direct. 
:::{prf:theorem} First order
:label:thm:first_order_convexity
Let $f:\mathbb{R}^n \rightarrow\mathbb{R}$ be differentiable. These statements are equivalent:
1. $f$ is convex on $\mathbb{R}^n$
2. For all ${x}, {y} \in \mathbb{R}^n$, $f({y}) \geq f({x}) + \nabla f({x})^\top ({y}-{x})$
3. For all ${x}, {y} \in \mathbb{R}^n$, $\left(\nabla f({y}) - \nabla f({x})\right)^\top \left({y}-{x}\right) \geq 0$

The equivalence is preserved for strict convexity, with ${x} \neq {y}$ and strict inequalities.
:::

:::{prf:theorem} Second order
Let $f:\mathbb{R}^n \rightarrow\mathbb{R}$ be twice differentiable. We have equivalence between
1. $f$ is convex on $\mathbb{R}^n$
2. For all ${x} \in \mathbb{R}^n$, $\nabla^2 f({x}) \succeq 0$, i.e. the Hessian is positive semidefinite.
:::

:::{warning}
The equivalence is not preserved in the strict case: if for all $x \in \mathbb{R}^n$, $\nabla^2 f(x) \succ 0$ then $f$ is strictly convex, but the converse is not true.
:::

A classical counter-example is $f:\mathbb{R} \to \mathbb{R}$ such that $f(x) = x^4$. The function is strictly convex (this can be checked by applying [](#def:convex_function) or [](#thm:first_order_convexity)), but its second derivative is $f''(x) = 12x^2$ which cancels for $x = 0$. 


## Uniqueness of solutions in optimization 

Consider the following optimization problem 
$$
\label{eq:prob_cvx_uniquness}
\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}
$$

The following theorem is very useful. 
:::{prf:theorem}
:label: thm:uniqueness_cvx
Suppose that $f_0$ is a convex function and that $\Omega\subseteq \mathbb{R}^n$ is a convex set. Then for the problem [](#eq:prob_cvx_uniquness), 
- any local optima is a global optima
- if $f_0$ is strictly convex, there is at most one (global) optima.
:::
:::{prf:proof}
:enumerated:false
:class:dropdown
**Exercise!** (hint: by contradiction)
:::

:::{danger}
[](#thm:uniqueness_cvx) does not say anything about existence of solutions! In fact, a convex optimization problem can admit no solution ( e.g., infeasible, unbounded below, or no point achieving $p^\star$). For instance, the optimization problem 
$$
\begin{array}{ll}
\minimize& e^x\\
\st & x \in \mathbb{R}
\end{array}
$$
admits no solution ($p^\star = 0$, but never attained). Yet it is convex optimization problem with $f_0$ strictly convex. 
:::

## More comments on convex functions
- By definition, $f$ is (strictly) concave iff $-f$ is  (strictly) convex
-  Optimization problems of the form 
$$\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}$$
    where $f_0$ is a convex function and $\Omega\subseteq \mathbb{R}^n$ is a convex set are called **convex optimization problems**. 
    It is one of the most successful field in numerical optimization. 
- the special (and important!) case of **quadratic functions**
   $$
    f(x) = x^\top {Q}x + {p}^\top x + {r} \qquad (\text{with }{Q} = {Q}^\top)
    $$

    :::{exercise}
    Show that $f$ is (resp. strictly) convex iff ${Q} \succeq 0$ (resp. ${Q}\succ 0$). What can we say about the solutions of the problem $\minimize f(x) \st x \in \mathbb{R}^n$?
:::