# Constrained optimization algorithms

We again consider the general, possibly nonconvex optimization problem
$$
\begin{array}{lll}
\text{minimize}     & f_0(x) & \\
\text{subject to}   & x\in \Omega
\end{array}
$$
where the constraint set $\Omega$ is parameterized by equality and inequality constraints
$$\Omega = \left\lbrace x \in \mathbb{R}^n \mid f_i(x) \leq 0, \:  i = 1, \ldots, m, \quad
h_i(x)  = 0, \: i = 1, \ldots,p\right\rbrace$$

In this chapter, we introduce several algorithmic strategies to solve such optimization problem.

:::{important} Key notions of the chapter
- **penalty method**: choice of penalty functions, computation and convergence
- **projected gradient descent**: principle, projection operators, algorithm
- **Uzawa's algorithm**: principle and algorithm
- **Disciplined convex programming**: introduction to CVXPy
:::
