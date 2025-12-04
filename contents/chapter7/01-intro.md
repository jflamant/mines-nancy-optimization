# Theory of constrained optimization

In this chapter, we will introduce tools to characterize (and sometimes, solve) general constrained optimization problems of the form
:::{math}
:label: eq:general_optim_chapter7
\begin{array}{lll}
\text{minimize}     & f_0(x) & \\
\text{subject to}   & f_i(x) \leq 0 & i = 1, \ldots, m\\
& h_i(x)  = 0 & i = 1, \ldots, p
\end{array}
:::

This problem **is not necessarily convex**. However, as we will see, certain tools rely on the assuming that [](#eq:general_optim_chapter7) is a convex problem. This will be clearly stated in the text.

Recall from [Chapter 1](../chapter1/01-intro.md) some important notions. The **constraint set** of  [](#eq:general_optim_chapter7) is given by
$$\Omega = \left\lbrace x \in \mathbb{R}^n \mid f_i(x) \leq 0, \:  i = 1, \ldots, m, \quad
h_i(x)  = 0, \: i = 1, \ldots,p\right\rbrace$$
whereas its **domain** is
$$
\mathcal{D} = \bigcap_{i=0}^m \dom f_i \;\cap\; \bigcap_{j=1}^p \text{dom}\, h_j.
$$
The **feasible set** is then
$$\mathcal{F} = \mathcal{D} \cap \Omega = \{ x \in \mathcal{D} \mid f_i(x) \leq 0, i=1, \ldots, m,\; h_j(x) = 0, j=1, \ldots, p \}.$$
Recall that the **optimal value** of the problem [](#eq:general_optim_chapter7) is given by
$$p^\star = \inf_{x \in \mathcal{F}} f_0(x)$$
and $p^\star = + \infty$ whenever the problem is *unfeasible*, i.e., $\mathcal{F} = \emptyset$. See also [this page in Chapter 1](../chapter1/04-general-form.md) for a review of the vocabulary.


For **convex problems**, the constraint set $\Omega$ is defined by
$$\Omega = \left\lbrace x \in \mathbb{R}^n \mid f_i(x) \leq 0, \:  i = 1, \ldots, m, \quad
a_i^\top x  = b_i, \: i = 1, \ldots,p\right\rbrace$$
where
- the inequality constraints are such that the functions $f_i$, $i=1, \ldots, m$ are **convex functions**
- the equality constraints $h_i(x) = a_i^\top x - b_i$ are **affine functions**.



:::{important} Key notions of the chapter
- Introduction to **Lagrange duality**: Lagrangian, Lagrange dual function, dual problem
- Notion of **weak and strong duality**; Slater's criterion for convex problems
- Optimality conditions: **Karush-Kuhn-Tucker (KKT)** conditions
:::


:::{hint} Main reference
This chapter follows the presentation of @boyd2004convex [Chapter 4 and 5].
:::
