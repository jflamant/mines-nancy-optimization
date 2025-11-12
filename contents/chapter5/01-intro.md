---
title: Conjugate gradient methods
---

# Introduction

In this chapter, we will focus on **conjugate gradient methods**, a popular class of iterative algorithms widely used for solving large-scale optimization and linear algebra problems.

The original motivation comes from the study of (square) **positive definite** linear systems of $n$ equations,
:::{math}
:label:eq:linear_problem_cg_intro
Q x = p
:::
where $x \in \mathbb{R}^n$, $Q \in \mathbb{R}^{n\times n}$ and $p\in \mathbb{R}^n$. **We assume that $Q \succ 0$.**

The solution to [](#eq:linear_problem_cg_intro) is of course $x^\star = Q^{-1}p$. So, why does it matter to study this problem? Because, for large $n$, *direct computation* or even, *memory storage* become too costly to be used in practice.

Instead, we seek **iterative methods** to solve [](#eq:linear_problem_cg_intro): this is known as the **linear conjugate gradient method**, which exploits the specific structure of the problem.  (also closely related to least squares problems, as we will recall next)


:::{important} Key notions of the chapter
- **linear conjugate gradient method**: understand first principles, basic theoretical and numerical properties
- **nonlinear conjugate gradient methods**: principle and two important cases: Fletcher-Reeves and Polak-Ribière methods.
:::

```{hint} Main reference
Most of the material of this section has been adapted from @nocedal2006numerical [Chapter 5].
```
