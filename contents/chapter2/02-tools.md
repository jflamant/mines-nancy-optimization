# The mathematical toolbox
This section reviews essential mathematical concepts used throughout optimization. We focus on differentiability, gradients, Hessians, and related tools that form the foundation for analyzing and solving optimization problems.
## Differentiability 

Let $f: U \to \mathbb{R}$ be a function defined on an open subset $U$ of $\mathbb{R}^n$.

:::{prf:definition} Differentiability
The function $f$ is said to be differentiable at $a \in U$ if there exists a linear map $df_{a}: \mathbb{R}^n \rightarrow \mathbb{R}$ such that
$$
f(a+h) - f(a) = df_{a}(h) + o(\Vert h \Vert).
$$
:::
If $f$ is differentiable at $a \in U$, the *differential* $df_{a}$ can be expressed in terms of *partial derivatives* of $f$ at $a$:
$$
df_{a}(h) = \sum_{i=1}^n  h_i \frac{\partial f}{\partial x_i}(a)
$$
and as the limit, for any $h \in \mathbb{R}^n$,
$$
df_{a}(h) = \lim_{\varepsilon \rightarrow 0} \frac{f(a+\varepsilon h) - f(a)}{\varepsilon}
$$

:::{prf:definition}
The function $f$ is said to be of class $C^k$ on $U$ if all its partial derivatives up to order $k$ exist and are continuous on $U$.
:::

If $f \in C^1$, we say it is *continuously differentiable*. If $f\in C^2$, we say it is *twice continuously differentiable*. 

:::{prf:theorem}  
If $f$ admits partial derivatives at every point in a neighborhood of $a$, and if the functions $\frac{\partial f}{\partial x_i}$ are continuous at $a$, then $f$ is differentiable at $a$.
:::
Keep in mind that the converse is not true, as shown by the following example.

:::{prf:example}
The function $f(x_1, x_2) = |x_1 x_2|$ is differentiable at $(0, 0)$ but its partial derivatives do not exist everywhere around the origin. (Prove it!)
:::


## Gradient and Hessian
Probably two of the most important tools for this lecture! 

:::{prf:definition} Gradient
Assume $f:\mathbb{R}^n \rightarrow \mathbb{R}$ is at least of class $C^1$.
For $a \in \mathbb{R}^n$, the vector of partial derivatives
$$
\nabla f (a) = \begin{bmatrix}
    \dfrac{\partial f}{\partial x_1}(a)\\
    \dfrac{\partial f}{\partial x_2}(a)\\
    \vdots\\
    \dfrac{\partial f}{\partial x_N}(a)
\end{bmatrix}
\in \mathbb{R}^n
$$
is called the **gradient of $f$ at point $a$**.
::: 

:::{prf:remark} 
The differential of $f$ at $a$ reads $df_{a}(h) = \nabla f(a)^\top h$.
:::


:::{prf:definition} Hessian
Let $f: \mathbb{R}^n \rightarrow \mathbb{R}$ be of class $C^2$.
For $a \in \mathbb{R}^n$, the matrix
$$
\nabla^2 f(a) = \begin{bmatrix}
    \frac{\partial^2 f}{{\partial x_1}^2}(a) & \frac{\partial^2 f}{\partial x_1\partial x_2}(a) & \cdots & \frac{\partial^2 f}{\partial x_1\partial x_N}(a) \\
    \frac{\partial^2 f}{\partial x_2\partial x_1}(a) & \frac{\partial^2 f}{{\partial x_2}^2}(a) & \cdots & \frac{\partial^2 f}{\partial x_2\partial x_N}(a) \\
    \vdots & \vdots & \ddots & \vdots \\
    \frac{\partial^2 f}{\partial x_N\partial x_1}(a) & \frac{\partial^2 f}{\partial x_N\partial x_2}(a) & \cdots & \frac{\partial^2 f}{{\partial x_N}^2}(a)
\end{bmatrix} \in \mathbb{R}^{N\times N}
$$
is called the **Hessian matrix of $f$ at point $a$**. Sometimes the notation $\mathsf{Hess}\,f (a)$ is also used. 
:::

:::{prf:remark} Important!
Since $f$ is of class $C^2$ on $\mathbb{R}^n$, Schwarz's theorem ensures that for every $a \in \mathbb{R}^n$:
$$
\frac{\partial^2 f}{\partial x_i \partial x_j}(a) = \frac{\partial^2 f}{\partial x_j \partial x_i}(a) \quad \text{for all } (i, j).
$$
Hence the Hessian $\nabla^2 f(a)$ is a symmetric matrix.
:::

## Positive definiteness

Let $A \in \mathbb{R}^{n\times n}$ be a symmetric matrix ($A^\top = A$).

:::{prf:definition}
- $A$ is *positive semi-definite (PSD)* if for every $x \in \mathbb{R}^n$, $x^\top A x \geq 0$.
- $A$ is *positive definite (PD)* if for every $x \in \mathbb{R}^n \setminus \lbrace 0\rbrace$, $x^\top A x > 0$.
:::
We write $A \succeq 0$ when it is PSD, and $A \succ 0$ when it is PD.


:::{prf:proposition} Characterization of PSD / PD matrices
- $A \succeq 0$ if and only if all its eigenvalues are non-negative.
- $A \succ 0$ if and only if all its eigenvalues are strictly positive.
:::
:::{prf:proof}
:nonumber:
:class:dropdown
  Let $A \in \mathbb{R}^{n\times n}$ be symmetric. By the spectral theorem, $A$ is diagonizable by a orthogonal matrix $Q\in \mathbb{R}^{n\times n}$ such that $A = Q D  Q^\top$, where $D = \diag(\lambda_1, \lambda_2, \ldots, \lambda_N)$ is a real diagonal matrix.
  Then let $v \in \mathbb{R}^n$ be arbitrary and denote by $w = Q^T v$ its projection into the orthonormal basis defined by $Q$. Compute
  $$
  v^\top A v = v^T Q D Q^T v = w^\top D w = \sum_{i=1}^n \lambda_i w_i^2
  $$
  Clearly, this expression is non-negative for all $v$ if and only if $\lambda_i \geq 0$ for  every $i=1, \ldots, n$, and strictly positive if and only $\lambda_i > 0$ for every $i=1, \ldots, n$. 
:::
As we shall see, the positive (semi)-definiteness of the Hessian matrix is crucial to characterize solutions of optimization problems.


## Taylor's theorem

Among different versions of Taylor's theorem, the following will be useful for proofs later on.

:::{prf:theorem} see e.g.,  @nocedal2006numerical [Theorem 2.1]
:label: thm:taylor
Suppose $f:\mathbb{R}^n\rightarrow \mathbb{R}$ is continuously differentiable and $p\in \mathbb{R}^n$. Then for some $t\in (0,1)$
$$
f(x+p) = f(x) + \nabla f(x + t p)^\top p
$$
Moreover, if $f$ is twice continuously differentiable,
$$
\nabla f(x+ p) = \nabla f(x) + \int_0^1 \nabla^2 f(x + t p) p \,dt
$$
and
$$
f(x+p) = f(x) + \nabla f(x)^\top p + \frac{1}{2} p^\top \nabla^2 f(x+t p)p
$$
for some $t \in (0, 1)$.
:::