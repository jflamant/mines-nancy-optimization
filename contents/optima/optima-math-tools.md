# The mathematical toolbox
This section reviews essential mathematical concepts used throughout optimization. We focus on differentiability, gradients, Hessians, and related tools that form the foundation for analyzing and solving optimization problems.
## Differentiability 

Let $f: U \to \mathbb{R}$ be a function defined on an open subset $U$ of $\mathbb{R}^N$.

:::{prf:definition} Differentiability
The function $f$ is said to be differentiable at $a \in U$ if there exists a linear map $df_{a}: \mathbb{R}^N \rightarrow \mathbb{R}$ such that
$$
f(\mathbf{a}+\mathbf{h}) - f(\mathbf{a}) = df_{\mathbf{a}}(\mathbf{h}) + o(\Vert \mathbf{h} \Vert).
$$
:::
If $f$ is differentiable at $\mathbf{a} \in U$, the *differential* $df_{\mathbf{a}}$ can be expressed in terms of *partial derivatives* of $f$ at $\mathbf{a}$:
$$
df_{\mathbf{a}}(\mathbf{h}) = \sum_{i=1}^N  h_i \frac{\partial f}{\partial x_i}(\mathbf{a})
$$
and as the limit, for any $\mathbf{h} \in \mathbb{R}^N$,
$$
df_{\mathbf{a}}(\mathbf{h}) = \lim_{\varepsilon \rightarrow 0} \frac{f(\mathbf{a}+\varepsilon \mathbf{h}) - f(\mathbf{a})}{\varepsilon}
$$

:::{prf:definition}
The function $f$ is said to be of class $C^k$ on $U$ if all its partial derivatives up to order $k$ exist and are continuous on $U$.
:::

If $f \in C^1$, we say it is *continuously differentiable*. If $f\in C^2$, we say it is *twice continuously differentiable*. 

:::{prf:theorem}  
If $f$ admits partial derivatives at every point in a neighborhood of $\mathbf{a}$, and if the functions $\frac{\partial f}{\partial x_i}$ are continuous at $\mathbf{a}$, then $f$ is differentiable at $\mathbf{a}$.
:::
Keep in mind that the converse is not true, as shown by the following example.

:::{prf:example}
The function $f(x_1, x_2) = |x_1 x_2|$ is differentiable at $(0, 0)$ but its partial derivatives do not exist everywhere around the origin.
:::


## Gradient and Hessian
Probably two of the most important tools for this lecture! 

:::{prf:definition} Gradient
Assume $f:\mathbb{R}^N \rightarrow \mathbb{R}$ is at least of class $C^1$.
For $\mathbf{a} \in \mathbb{R}^N$, the vector of partial derivatives
$$
\nabla f (\mathbf{a}) = \begin{bmatrix}
    \dfrac{\partial f}{\partial x_1}(\mathbf{a})\\
    \dfrac{\partial f}{\partial x_2}(\mathbf{a})\\
    \vdots\\
    \dfrac{\partial f}{\partial x_N}(\mathbf{a})
\end{bmatrix}
\in \mathbb{R}^N
$$
is called the **gradient of $f$ at point $\mathbf{a}$**.
::: 

:::{prf:remark} 
The differential of $f$ at $a$ reads $df_{\mathbf{a}}(\mathbf{h}) = \nabla f(\mathbf{a})^\top \mathbf{h}$.
:::


:::{prf:definition} Hessian
Let $f: \mathbb{R}^N \rightarrow \mathbb{R}$ be of class $C^2$.
For $\mathbf{a} \in \mathbb{R}^N$, the matrix
$$
\nabla^2 f(\mathbf{a}) = \begin{bmatrix}
    \frac{\partial^2 f}{{\partial x_1}^2}(\mathbf{a}) & \frac{\partial^2 f}{\partial x_1\partial x_2}(\mathbf{a}) & \cdots & \frac{\partial^2 f}{\partial x_1\partial x_N}(\mathbf{a}) \\
    \frac{\partial^2 f}{\partial x_2\partial x_1}(\mathbf{a}) & \frac{\partial^2 f}{{\partial x_2}^2}(\mathbf{a}) & \cdots & \frac{\partial^2 f}{\partial x_2\partial x_N}(\mathbf{a}) \\
    \vdots & \vdots & \ddots & \vdots \\
    \frac{\partial^2 f}{\partial x_N\partial x_1}(\mathbf{a}) & \frac{\partial^2 f}{\partial x_N\partial x_2}(\mathbf{a}) & \cdots & \frac{\partial^2 f}{{\partial x_N}^2}(\mathbf{a})
\end{bmatrix} \in \mathbb{R}^{N\times N}
$$
is called the **Hessian matrix of $f$ at point $\mathbf{a}$**. Sometimes the notation $\mathsf{Hess}\,f (\mathbf{a})$ is also used. 
:::

:::{prf:remark} Important!
Since $f$ is of class $C^2$ on $\mathbb{R}^N$, Schwarz's theorem ensures that for every $\mathbf{a} \in \mathbb{R}^N$:
$$
\frac{\partial^2 f}{\partial x_i \partial x_j}(\mathbf{a}) = \frac{\partial^2 f}{\partial x_j \partial x_i}(\mathbf{a}) \quad \text{for all } (i, j).
$$
Hence the Hessian $\nabla^2 f(\mathbf{a})$ is a symmetric matrix.
:::

## Positive definiteness

Let $\mathbf{A} \in \mathbb{R}^{N\times N}$ be a symmetric matrix ($\mathbf{A}^\top = \mathbf{A}$).

:::{prf:definition}
- $\mathbf{A}$ is *positive semi-definite (PSD)* if for every $\mathbf{x} \in \mathbb{R}^N$, $\mathbf{x}^\top \mathbf{A}\mathbf{x} \geq 0$.
- $\mathbf{A}$ is *positive definite (PD)* if for every $\mathbf{x} \in \mathbb{R}^N \setminus \lbrace \mathbf{0}\rbrace$, $\mathbf{x}^\top \mathbf{A}\mathbf{x} > 0$.
:::
We write $\mathbf{A} \succeq 0$ when it is PSD, and $\mathbf{A} \succ 0$ when it is PD.


:::{prf:remark} Characterization of PSD / PD matrices
- $\mathbf{A} \succeq 0$ if and only if all its eigenvalues are non-negative.
- $\mathbf{A} \succ 0$ if and only if all its eigenvalues are strictly positive.
:::
As we shall see, the positive (semi)-definiteness of the Hessian matrix is crucial to characterize solutions of optimization problems.


## Taylor's theorem

Among different versions of Taylor's theorem, the following will be useful for proofs later on.

:::{prf:theorem} see e.g., @nocedal1999numerical  
Suppose $f:\mathbb{R}^N\rightarrow \mathbb{R}$ is continuously differentiable and $\mathbf{p}\in \mathbb{R}^N$. Then for some $t\in (0,1)$
$$
f(\mathbf{x}+\mathbf{p}) = f(\mathbf{x}) + \nabla f(\mathbf{x} + t\mathbf{p})^\top \mathbf{p}
$$
Moreover, if $f$ is twice continuously differentiable,
$$
\nabla f(\mathbf{x}+ \mathbf{p}) = \nabla f(\mathbf{x}) + \int_0^1 \nabla^2 f(\mathbf{x} + t\mathbf{p}) \mathbf{p} \,dt
$$
and
$$
f(\mathbf{x}+\mathbf{p}) = f(\mathbf{x}) + \nabla f(\mathbf{x})^\top \mathbf{p} + \frac{1}{2} \mathbf{p}^\top \nabla^2 f(\mathbf{x}+t\mathbf{p})\mathbf{p}
$$
for some $t \in (0, 1)$.
:::