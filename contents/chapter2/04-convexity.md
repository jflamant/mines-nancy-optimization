# Convexity

Convexity is a key tool to study **uniqueness** of solutions in optimization.

## Convex sets

:::{prf:definition} Convex set
A set $\mathcal{C}$ is convex if for every ${x}_1, {x}_2 \in \mathcal{C}$ and for every $\theta \in [0, 1]$, one has
$$\label{eq:convex_set}\theta {x}_1 +(1-\theta){x}_2 \in \mathcal{C}$$
:::
Equation [](#eq:convex_set) states that, for every ${x}_1, {x}_2 \in \mathcal{C}$, any point on the segment joining ${x}_1$ and ${x}_2$ must be also in $\mathcal{C}$. 

:::{figure} figures/convexSet.png
---
width: 80%
name: convex-set-fig
---
$\mathcal{C}_1$ is convex; $\mathcal{C}_2$ is non-convex.
:::

## Convex functions
:::{prf:definition} Convex function
:label:def:convex_function
Let $\Omega \subset \mathbb{R}^n$ be a convex set. 
The function $f:\Omega \rightarrow \mathbb{R}$ is said to be convex if for every ${x}_1, {x}_2 \in \Omega$ and for every $\theta \in [0, 1]$, one has
$$f(\theta{x}_1 + (1-\theta){x}_2) \leq \theta f({x}_1) + (1-\theta)f({x}_2)$$
and strictly convex if the inequality is strict.
:::

:::{figure} figures/convexExample0.pdf
---
width: 50%
name: convex-function-fig
---
Example of a convex function.
:::

## Examples of convex and non-convex functions (1-D)

:::{figure} figures/convexExample1.pdf
---
width: 48%
align: left
name: strictly-convex-fig
---
Strictly convex
:::

:::{figure} figures/convexExample2.pdf
---
width: 48%
align: right
name: convex-fig
---
Convex
:::

:::{figure} figures/convexExample3.pdf
---
width: 48%
name: non-convex-fig
---
Non-convex
:::

## Useful characterization results for convex functions
In the following we give two practical theorem to characterize the convexity of functions $f:\Omega \to \mathbb{R}$, where $\Omega \subset \mathbb{R}^n$ is a convex set. We assume sufficient smoothness (e.g., $f \in C^1$ or $f\in C^2$). To simplify we state results for $\Omega = \mathbb{R}^n$, but the extension to arbitrary convext sets is direct. 
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
1. $f$ is convex
2. For all ${x} \in \mathbb{R}^n$, $\nabla^2 f({x}) \succeq 0$, i.e. the Hessian is positive semidefinite.
:::

:::{warning}
The equivalence is not preserved in the strict case: if for all $x \in \mathbb{R}^n$, $\nabla^2 f(x) \succ 0$ then $f$ is strictly convex, but the converse is not true.
:::

A classical counter-example is $f:\mathbb{R} \to \mathbb{R}$ such that $f(x) = x^4$. The function is strictly convex (this can be checked by applying [](#def:convex_function) or [](#thm:first_order_convexity)), but its second derivative is $f''(x) = 12x^2$ which cancels for $x = 0$. 