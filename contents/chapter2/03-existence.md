# Existence of solutions

Let $f_0: \mathbb{R}^n\longrightarrow \mathbb{R}$ and consider the following optimization problem
$$\label{eq:prob}
\begin{array}{ll}
\minimize& f_0({x})\\
\st & x \in \Omega
\end{array}
$$
where $\Omega \subseteq \mathbb{R}^n$ is the feasible set. (Recall we assume that the domain of [](#eq:prob) is $\mathcal{D} = \mathbb{R}^n$, so the notion of feasible and constraint set coincide.) 
Under which conditions can we guarantee existence of a solution to the problem [](#eq:prob)? 
Recall that we say that $x$ is a solution of [](#eq:prob) if the optimal value $p^\star$ is finite, and attained at $x$, i.e., $f_0(x) = p^\star$. (see [](../chapter1/04-general-form.md/#optimal-value-and-optimal-points)).

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
The set $\mathbb{R}^n$ is both open and closed. Therefore, according to [](#thm:existence), for unconstrained optimization problems, i.e., problems of the form
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
For the sets $\Omega$ and objective functions below, characterize the existence of solutions to this optimization problem:
- $\Omega = [-1, 1]$ and $f_0(x) = x^3 + x^2 + 1$
- $\Omega = \mathbb{R}$ and $f_0(x) = x^3 + x^2 + 1$
- $\Omega = \mathbb{R}$ and $f_0(x) = x^4 + 3 x^2 -3$
:::

**Existence** does not mean the solution to a given optimization problem is unique. However, in the important case of *convex* optimization problems, it is possible to guarantee uniqueness of solutions, as explained in the next section. 

::::{hint} Coercivity and quadratic functions
Consider the quadratic optimization problem 
$$
\begin{array}{ll}
\minimize& f_0(x):=\frac{1}{2}x^\top Q x - p^\top x + c\\
\st & x \in \mathbb{R}^n
\end{array}
$$
where $Q \in \mathbb{R}^{n\times n}$ is symmetric and where $p\in \mathbb{R}^n$ is arbitrary. We have the following result:
$$ f_0(x) \text{ is coercive } \Leftrightarrow Q \succ 0 \text{ (positive definite)}$$


:::{prf:proof}
:nonumber:
$\Leftarrow$. Suppose $Q \succ 0$. Equivalently, by [this proposition](prop_PSD_eigenvalues), its eigenvalues $\lambda_1, \ldots, \lambda_n$ are all strictly positive. Write $Q = U \Lambda U^\top$ where $U$ is orthogonal and $\Lambda = \diag(\lambda_1, \ldots, \lambda_n)$ where eigenvalues are ordered in ascending order.
Then, 
$$x^\top Q x = x^\top U \Lambda \underbrace{U^\top x}_{=w} = \sum_{i=1}^n\lambda_i w_i^2 &\geq \lambda_1 \sum_{i=1}^n w_i^2 \\
&= \lambda_1 \Vert w\Vert^2_2 = \lambda_1 \Vert U^\top x\Vert_2^2 = \lambda_1 \Vert x \Vert_2^2$$
where the last equality uses the fact that $U$ is orthogonal and thus preserves norms. 
Moreover, by the Cauchy-Schwarz theorem, $-p^\top x \geq - \Vert x\Vert\, \Vert p\Vert$. Therefore, 
$$f_0(x) \geq \lambda_1 \Vert x \Vert_2^2 - \Vert x\Vert\, \Vert p\Vert + c$$
where the lower bound goes to $+\infty$ as $\Vert x \Vert \to +\infty$. As as a consequence $f_0({x})\xrightarrow[\Vert {x} \Vert \to +\infty]{} +\infty$ as well and $f_0$ is coercive. 

$\Rightarrow$. Suppose that $f_0$ is coercive and assume that $Q$ is not positive definite. Let $\lambda_1$ be the smallest eigenvalue of $Q$. We have to distinguish between two cases: either $\lambda_1 < 0$ or $\lambda_1 = 0$. 

- Case $\lambda_1 < 0$. Let $u_1$ be an (normed) eigenvector of $Q$ associated to $\lambda_1$. Then, for $t \in \mathbb{R}$, 
$$f_0(t u_1) = t^2 u_1^\top Q u_1 - t p^\top u_1 + c = \lambda_1 t^2 - tp^\top u_1 + c \xrightarrow[t \to \infty]{} -\infty$$
Hence $f_0$ is not coercive, a contradiction.

- Case $\lambda_1 = 0$. Using the same argument, we get $f_0(t u_1) = - t p^\top u_1 + c$. If $p^\top u_1 = 0$ then $f_0(t u_1)\xrightarrow[t \to \infty]{} c$; if $p^\top u_1 > 0$ then $f_0(t u_1)\xrightarrow[t \to +\infty]{} -\infty$; similarly if $p^\top u_1 < 0$ then $f_0(t u_1)\xrightarrow[t \to -\infty]{} -\infty$. In all of these three cases, the function $f_0$ is never coercive, a contradiction. 

From all cases, we deduce that if $f_0$ is coercive, then $Q \succ 0$. 
:::
::::