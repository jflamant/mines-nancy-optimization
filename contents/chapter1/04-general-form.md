--- 
title: "Continuous optimization problems: general form"
kernelspec:
    name: python3
---

As explained before, this course focuses exclusively on **continuous** optimization problems, where the variables belong to continuous spaces (e.g., $\mathbb{X} = \mathbb{R}^n$), and where tools from calculus and convex analysis are applicable. Throughout, we adopt the presentation and terminology of @boyd2004convex, which is widespread in the optimization literature.   

A general continuous optimization problem can be formulated as
:::{math}
:label: eq:general_optim
\begin{array}{lll}
\text{minimize}     & f_0(x) & \\
\text{subject to}   & f_i(x) \leq 0 & i = 1, \ldots, m\\
& h_i(x)  = 0 & i = 1, \ldots, p
\end{array}
:::
This notation describes the problem of finding a vector $x$ that minimizes the objective function $f_0(x)$ among those satisfying $f_i(x) \leq 0$, $i=1, \ldots, $ and $h_i(x) = 0$, $i=1, \ldots, p$. 

## Terminology: objective, constraints, domain and feasible set

- $x \in \mathbb{R}^n$: the **decision variable**, or vector of optimization parameters.
- $f_0: \mathbb{R}^n \to \mathbb{R}$: the **objective function**, which assigns a scalar cost to each possible choice of $x$. 
- $f_i(x) \leq 0, i=1, \ldots, m$: the **inequality constraints**, where each $f_i: \mathbb{R}^n \to \mathbb{R}$ is a function that restricts the feasible region to one side of a surface in $\mathbb{R}^n$.
- $h_i(x) = 0, i=1, \ldots, p$: the **equality constraints**, where each $h_i: \mathbb{R}^n \to \mathbb{R}$ enforces a strict condition that must be satisfied exactly.
- the **domain** $\mathcal{D}$ of the optimization problem [](#eq:general_optim) corresponds to the set of points in $\mathbb{R}^n$ for which the functions (objectives or constraints) are well defined. For instance, Formally, it is the intersection of the domains of all functions involved, i.e., 
$$
\mathcal{D} = \bigcap_{i=0}^m \dom f_i \;\cap\; \bigcap_{j=1}^p \text{dom}\, h_j
$$
For instance, for a function $x \mapsto f(x)=\log(x)$, the domain is $\dom f = \mathbb{R}_{+}^*$. 

- The set of all $x \in \mathcal{D}$ that satisfy the constraints is called the **feasible set**:
:::{math}
:label: eq:feasible_set

\Omega = \{ x \in \mathcal{D} \mid f_i(x) \leq 0, i=1, \ldots, m,\; h_j(x) = 0, j=1, \ldots, p \}.
:::
if $\Omega = \emptyset$, there are no feasible point and we say the problem is **infeasible**. Otherwise, if there is at least one feasbile point, the problem is said to be **feasible**. 

::::{prf:remark} Domain of definition
The domain $\mathcal{D}$ of the objective and constraint functions imposes **implicit constraints** on the optimization problem. Even if not explicitly stated, a candidate solution must lie in $\mathcal{D}$ for the functions to be well-defined. 
However, note that in many settings of the course, functions will be well defined over $\mathbb{R}^n$, i.e., $\mathcal{D}=\mathbb{R}^n$, and the domain will be implicit. 
::::

## Optimal value and optimal points

An optimization problem seeks to find the **optimal value** of the objective function over the feasible set. Formally, the **optimal value** $p^\star$ is defined as:
$$
p^\star = \inf_{x \in \Omega} f_0(x),
$$
where $\Omega$ is the feasible set defined in [](#eq:feasible_set). Depending on the problem, three situations may arise:

- if the problem is **infeasible** (i.e., $\Omega = \emptyset$) we set by convention $p^\star = +\infty$.
- if there exists a sequence of feasible points $x_k$ such that $f_0(x_k) \to -\infty$ as $k\to + \infty$, then $p^\star = -\infty$ and we say the problem is **unbounded below**.
- if $p^\star$ is finite, then we have to distinguish between two situations: 
    - there exists a feasible $x^\star$ such that $f(x^\star) = p^\star$. In this case, we say $x^\star$ is **optimal** and the optimal value is **attained**. We also say the problem [](#eq:general_optim) has a **solution $x^\star$ with optimal value $p^\star$**. Note that they might be more than one $x^\star$ that attains the optimal value!
    - the optimal value $p^\star$ is never attained, i.e., there is no $x^\star$ such solves the problem. 

:::{prf:example}
Consider the **unconstrained** optimization problem in one dimension ($n=1$)
$$\begin{array}{ll}
\minimize& f_0(x)\\
\st & x \in \mathbb{R}\end{array}$$
Consider the domain $\mathcal{D} = \mathbb{R}_+$. Consider several objective functions:
- $f_0(x) = 1-e^{-x}$. We have $p^\star = 0$, uniquenely attained for $x^\star = 0$. 
- $f_0(x) = e^{-x} - 1$.  We have $p^\star = -1$, however it is never attained since $f_0(x) \to -1$ as $x\to +\infty$. 
- $f_0(x) = -x^2$. Clearly, $f_0(x) \to -\infty$ as $x \to +\infty$. Therefore, the problem is unbounded below. 
:::

In the next section, we will see the notion of global and local optima. 

## About notations and terminology

Many names are used in the literature to refer to the field of optimization, including:
- **mathematical programming**
- **mathematical optimization**
- **numerical optimization**

All these terms refer to the study of **optimization problems** and are used interchangeably. 

:::{prf:remark}
The term **programming** refers to the process of **planning or decision-making** rather than writing computer code. Historically, it was used to describe methods for organizing and solving mathematical models to find the best possible outcome under given constraints. In other terms, *programming* means constructing an optimal plan or strategy, not writing software.
:::

### How to write an optimization problem? 

In the literature, you will encounter several equivalent notations for expressing the same optimization problem. In this lecture, we adopt the notation of @boyd2004convex, which uses the **minimize** and **subject to** keywords, for example:

:::{math}
\begin{array}{ll}
\minimize & f_0(x) \\
\st & x \in \Omega
\end{array}
:::

In more mathematical or theoretical contexts, the same problem is often written as:
$$
\inf_{\mathbf{x} \in \Omega} f_0(\mathbf{x}).
$$

Some classical textbooks (e.g., [@nocedal2006numerical]) use the more compact notation:
$$
\min_{\mathbf{x} \in \Omega} f_0(\mathbf{x}),
$$
which is widely accepted, even though technically the minimum may not exist (e.g., if the function is unbounded below).

To emphasize interest in actual **solutions** of the optimization problem, one often writes:
$$
x^\star \in \arg\min_{\mathbf{x} \in \Omega} f_0(\mathbf{x}),
$$
a notation frequently encountered in fields like signal processing.

:::{important}
Despite differences in notation, all of these expressions refer to the **same underlying optimization problem**!
:::

