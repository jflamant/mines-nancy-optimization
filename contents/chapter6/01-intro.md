---
title: About convergence
---

# Introduction

In this chapter, we will focus on studying **convergence properties** of **unconstrained optimization algorithms**. Specifically, we focus on problems of the form
$$\begin{array}{ll}
      \minimize &\quad f(x)\\
      \st& \quad x \in \mathbb{R}^n
  \end{array}
$$
(Note that we will use $f$ to denote the objective function *instead of $f_0$* to simplify the notation in the chapter). We also make two assumptions:
- the function $f$ is (at least) continuously differentiable
- the problem admits at least one solution, i.e., there exists a (global) optima $x^\star$ and we denote by $p^\star = f(x^\star) < \infty$ the optimal value.

Recall from [Chapter 4](../chapter4/01-intro.md) the principle of descent methods: starting from an initial point $x^{(0)}$, iteratively compute
$$
   x^{(k+1)} =  x^{(k)} + \alpha_k d_k
$$
where $\alpha_k$ is the *stepsize* and where $d_k$ is a *descent direction*, i.e., such that  $d_k^\top \nabla f(x^{(k)}) < 0$.

**Two important examples**
- [Gradient descent](../chapter4/04-gradient-descent.md): first-order update rule $x^{(k+1)} =  x^{(k)} - \alpha_k \nabla f(x^{(k)})$
- [Newton's method](../chapter4/05-newton-method.md): second-order update rule $x^{(k+1)} =  x^{(k)} - \alpha_k \left[\nabla^2f(x^{(k)})\right]^{-1} \nabla f(x^{(k)})$

In this chapter, we will study **convergence properties** of such algorithms. We will see how properties of the objective function $f$ impact what can be said about the sequence $\lbrace x^{(k)}\rbrace$, in terms of **type and rate of convergence**.


:::{important} Key notions of the chapter
- **review of important mathematical tools**: smoothness, strong convexity, and related properties
- **convergence analysis of fixed-step size gradient descent**: understanding how properties of the objective impact the rate
- **convergence analysis of Newton's method**
:::

```{hint} References
This chapter compiles material from different sources, indicated as precisely as possible. This includes: @boyd2004convex, @nocedal2006numerical, @wright2022optimization, @bubeck2015convex
and @nesterov2013introductory, among others.
```
