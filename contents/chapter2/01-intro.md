---
title: Characterizing solutions
---

# Introduction

In this chapter, we consider optimization problems of the form 
$$\label{eq:optim}
\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}
$$
where $f_0 : \mathbb{R}^n \to \mathbb{R}$ is the objective function. For simplicity, we assume (unless otherwise stated) that the domain of [](#eq:optim) is $\mathbb{R}^n$. 

Recall from the previous chapter that when $\Omega = \mathbb{R}^n$ the problem [](#eq:optim) the problem is said to be unconstrained; otherwise, if $\Omega \subset \mathbb{R}^n$, we say that [](#eq:optim) is a constrained optimization problem. 

:::{important} Key notions of the chapter
- Review of elementary results from multivariable calculus and algebra
- Notions of **gradient**, **Hessian**, and **convexity**
- **Existence** and **uniqueness** results for solutions
- For **unconstrained problems**, necessary and sufficient **optimality conditions** for a point to be a solution.
:::