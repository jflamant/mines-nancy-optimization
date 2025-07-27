# Existence of solutions

Let $f_0: \mathbb{R}^n\longrightarrow \mathbb{R}$ and consider the following optimization problem
$$\label{eq:prob}
\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}
$$
where $\Omega \subseteq \mathbb{R}^n$ is the feasible set. 
Under which conditions can we guarantee existence of a solution to the problem [](#eq:prob)? 
Recall that we say that $x$ is a solution of [](#eq:prob) if the optimal value $p^\star$ is finite, and attained at $x$, i.e., $f_0(x) = p^\star$. 

In this course we work in finite dimensions only. The theorem below provides two sufficient conditions, which depend on the nature of the set $\Omega$. 

:::{prf:theorem} Existence in finite dimensions
:label: thm:existence
Suppose that $f_0$ is continuous and that $\Omega \neq \emptyset$. If one of the following conditions is satisfied:
- $\Omega$ is **compact** (or equivalently, **closed and bounded** since we are in finite dimension);
- $\Omega$ is **closed** and $f_0$ is **coercive** (i.e., such that $f_0({x})\xrightarrow[\Vert {x} \Vert \to +\infty]{} +\infty$),

then the problem [](#eq:prob) admits (at least) one solution.
:::

:::{prf:remark}
As expected, if $\Omega = \emptyset$, the problem is not feasible, hence there are no solutions. 
:::

:::{prf:remark}
The set $\mathbb{R}^n$ is both open and closed. Therefore, according to [](#thm:existence), for optimization problems of the form 
$$\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \mathbb{R}^n
\end{array}$$
it suffices to have $f_0$ *continuous and coercive* to show that the problem admits at least one solution. 
:::


:::{exercise}
:class:dropdown
Consider the optimization problem 
$$
\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}
$$
For the sets $\Omega$ and objective functions below, characterize the existence of solutions to this optimization problem
- $\Omega = [-1, 1]$ and $f_0(x) = x^3 + x^2 + 1$
- $\Omega = \mathbb{R}$ and $f_0(x) = x^3 + x^2 + 1$
- $\Omega = \mathbb{R}$ and $f_0(x) = x^4 + 3 x^2 -3$
:::

**Existence** does not mean the solution to a given optimization problem is unique. However, in the important case of *convex* optimization problems, it is possible to guarantee uniqueness of solutions, as explained in the next section. 