---
title: Algorithms for unconstrained optimization
---

# Introduction

In this chapter, we will study algorithms for solving the unconstrained optimization problem
:::{math}
\begin{array}{ll}
\minimize &\quad f_0(x)\\
\st & \quad x \in \mathbb{R}^n
\end{array}
:::

In many cases, finding an explicit solution to the problem is not feasible (unlike for least squares problems encountered in the previous chapter, for instance).

**Solution: Use iterative methods (aka algorithms).** We aim to find a minimizer of the problem through an iterative process:
- Start from one initial point $x^{(0)}$.
- At each iteration $k$, compute a new point $x^{(k+1)}$ using an update rule based on the function $f_0$ to be minimized.
- Repeat this process until a stopping criterion is met.


:::{important} Key notions of the chapter
- **General descent methods**: Iterative strategies that ensure each step reduces the objective function.
- **First-order methods**: Use only gradient information, e.g., gradient descent and its variants.
- **Second-order methods**: Employ curvature info, e.g., Newton and quasi-Newton methods, for faster convergence.
- **Stepsize selection strategies**: Fixed, optimal, or adaptive line search to choose effective steps.
:::
