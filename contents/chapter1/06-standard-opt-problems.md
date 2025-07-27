# Standard optimization problems

Several optimization problems are so frequently encountered  that they are given specific names.
Recall the most general form of optimization problems encountered in this course: 
:::{math}
\begin{array}{lll}
\text{minimize}     & f_0(x) & \\
\text{subject to}   & f_i(x) \leq 0 & i = 1, \ldots, m\\
& h_i(x)  = 0 & i = 1, \ldots, p
\end{array}
:::

For specific choices of objective and constraint functions, one obtains different classes of optimization problems. 

:::{important}
Being able to recognize to which class a given problem belongs is important, as efficient resolution methods are usually problem dependent.
:::

Some important examples are:
- **Linear programming**,
- **Quadratic programming**,
- **Convex programming**,
- **Multi-objective programming**, etc.

For each of these cases, we will review the standard form and give some examples.

## Linear programming

When both the objective function and constraints are **affine**, one obtains a Linear Program (LP).

### LP general form
:::{math}
:label: def:LP
\begin{split}
\minimize\quad & c^\top x + d \\
\text{subject to}\quad& Ax= b,\\ &Gx\leq h
\end{split}
:::

### Comments
- The constant $d$ term does not affect the solution to the optimization problem.
- The **feasible set** is a polyhedron.
- LPs are frequently encountered in *operation research*.
- Solving LPs requires specific algorithms (outside of the scope of this course), such as Dantzig's simplex algorithm.

### Example: diet problem
This example is reproduced from @boyd2004convex [Section 4.3.1.].

A healthy diet contains $m$ different nutrients in quantities at least equal to $b_1, \ldots, b_m$. We can compose such a diet by choosing quantities of $n$ different foods. One unit of food $j$ contains $a_{ij}$ of nutrient $i$ and has a cost of $c_j$.

**Question:** Write the optimization problem corresponding to choosing the cheapest healthy diet.

:::{tip} Solution
:class:dropdown
The corresponding optimization problem is
$$
\begin{split}
\minimize\quad & c^\top x \\
\text{subject to}\quad& Ax\geq b,\\ &x\geq 0
\end{split}
$$
where $x\in \mathbb{R}^n$ contains the (non-negative) quantities of each food.
:::

## Quadratic programming
When the objective function is quadratic and the constraints are affine, we obtain a Quadratic Program (QP).

### QP general form 
:::{math}
:label: def:QP
\begin{split}
\minimize\quad & \frac{1}{2}x^\top P x + q^\top x + r \\
\text{subject to}\quad& Ax= b,\\ &Gx\leq h
\end{split}
:::
where $P$ is a symmetric matrix (but not necessarily positive semidefinite).

### Comments
- Like in LPs, the feasible set is a polyhedron.
- QPs are fundamental for many fields, ranging from signal processing, parameter estimation, regression, machine learning, econometrics, etc.
- Have a direct connection with [least squares problems](../chapter3/01-intro.md) that we will consider later on. 

### Example: Gaussian maximum likelihood estimation

We want to estimate the mean ${\theta} \in \mathbb{R}^n$ of a Gaussian random variable $\mathcal{N}({\theta}, {\Sigma})$, with known covariance matrix ${\Sigma}$, from $m$ i.i.d. measurements $y_1, \ldots, y_m \sim \mathcal{N}({\theta}, {\Sigma})$. 

Recall that
:::{math}
p(y_i; {\theta}) = \frac{1}{(2\pi)^{n/2}\vert {\Sigma}\vert^{1/2}}\exp\left(-\frac{1}{2}(y_i - {\theta})^\top {\Sigma}^{-1}(y_i - {\theta})\right)
:::

**Maximum likelihood (ML) principle**

:::{math}
\text{find } {\theta}^\star \in \argmax_{{\theta}\in \mathbb{R}^n} \mathcal{L}({{\theta}}; y_1, \ldots, y_m):= p(y_1, \ldots, y_m ; {\theta}) 
:::

The function $\mathcal{L}({\theta}; y_1, \ldots, y_m)$ is called the *likelihood function*. The goal of ML estimation is to find the parameter ${\theta}$ for which the data $y_1, \ldots, y_m$ has the highest joint probability.

**Question:** what is the corresponding optimization problem?

::::{tip} Solution
:class:dropdown
First, since observations are i.i.d., $p(y_1, \ldots, y_m ; {\theta}) = \prod_{i=1}^{m} p(y_i; {\theta})$.

Second, observe that the ML principle is equivalent to
:::{math}
\text{find } {\theta}^\star \in \argmax_{{\theta}\in \mathbb{R}^n} \log \mathcal{L}({{\theta}}; y_1, \ldots, y_m)
:::
since the likelihood is positive (by definition) and $\log$ is monotone. Thus, the goal is to find $\theta^\star$ that solves the following optimization problem
:::{math}
\maximize -\frac{1}{2}\sum_{i=1}^m (y_i - {\theta})^\top {\Sigma}^{-1}(y_i - {\theta})
:::
which can be rewritten as the (unconstrained) QP
:::{math}
\minimize\quad \frac{1}{2} {\theta}^\top {\Sigma}^{-1} {\theta} - \frac{1}{m}\left(\sum_{i=1}^m y_i\right)^\top{\Sigma}^{-1} {\theta}
:::
::::


## Convex programming

LPs and QPs (under some conditions) are instances of **convex optimization problems**. 

In the next chapter, we will precisely define this notion and see why it plays such an important role in optimization problems.

### General form of a convex problem
:::{math}
:label: def:CP
\begin{split}
\minimize\quad & f_0(x) \\
\text{subject to}\quad& f_i(x) \leq 0, \quad i=1, \ldots, m\\ &a_i^\top x= b_i, \quad i=1, \ldots, p
\end{split}
:::
where $f_0$ and $f_1, \ldots, f_m$ are *convex* functions.

### Comments
- in convex problems, equality constraints $h_i(x) = 0$ must be affine functions, i.e., of the form $h_i(x) = a_i^\top x - b_i$. **Proof in exercise!**
- CPs have the important property that any local optima is also a global optima.
- Many practical problems in engineering, economics, and data science can be formulated as convex programs.
- The theory of convex optimization provides strong guarantees on solution existence, uniqueness, and stability.

In this course, we will mostly focus on **convex optimization** problems.