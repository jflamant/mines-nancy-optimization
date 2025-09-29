# Study of unconstrained quadratic programs

As we saw in the [previous section](03-general-form-LS.md#interpretation-as-an-unconstrained-quadratic-program-qp), any (linear) least squares problem can be interpreted as an (unconstrained) QP. 

In this section, we study in detail problems of the form
:::{math}
:label:prob:unconstrained_QP
\begin{array}{ll}
\minimize&  \frac{1}{2}x^\top Q x -p^\top x\\
\st & x \in \mathbb{R}^n
\end{array}
:::
where $Q \in \mathbb{R}^{n\times n}$ is symmetric ($Q^\top = Q$) and where $p \in \mathbb{R}^n$ is arbitrary. 
We address the following questions:
- when does [](#prob:unconstrained_QP) admits solutions? (existence)
- is there a unique solution? 
- how do we express such solutions? 

:::{hint} some useful identities
:class:dropdown
It is useful to keep in mind these identities. In doubt, check the [Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf) or learn how to re-derivate them easily!

```{table}
:name: tab:matrix_derivatives
:widths: auto
:align: left

| $f(x)$                | $\nabla f(x)$         | $\nabla^2 f(x)$ |
|-----------------------|----------------------------------------------|------------------------------------------------------|
| $a^\top x$            | $a$                                          | $0$                                                  |
| $x^\top A x$          | $(A + A^\top)x$                              | $A + A^\top$                                         |

```
:::

## Preliminaries
**Gradient and Hessian of the objective function of QP**

For $f(x) = \frac{1}{2}x^\top Q x -p^\top x$, the gradient and Hessian are easily found as 
:::{math}
\nabla f(x) &= \frac{1}{2}(Q+Q^\top)x -p = Qx-p\\
\nabla^2 f(x) &= Q
:::

**Diagonalization of the Hessian**

Since $Q$ is symmetric, the spectral theorem shows $Q$ is diagonalizable by an orthogonal matrix $U$ (i.e., $U^\top U = UU^\top = I_n$) such that 
$$
Q = U^\top D U, \text{ where } D = \begin{bmatrix}
\lambda_1 & & (0)\\
& \ddots & \\ 
(0) & &\lambda_n
\end{bmatrix}\quad \text{ with }\lambda_1 \leq \lambda_2 \leq \ldots \leq \lambda_n. 
$$
The properties of the eigenvalues of the Hessian permits to analyze the behavior of QPs. 

## Typology of solutions
### Case 1: $\lambda_1 = \lambda_{\min}(Q) < 0$
Let $u_1 \in \mathbb{R}^n$ be the eigenvector associated with $\lambda_1$. For any $z \in \mathbb{R}$,
$$
f(zu_1) = \frac{\lambda_1}{2} z^2 - z\, p^\top u_1 \xrightarrow[|z| \to +\infty]{} -\infty
$$
This shows that $f$ is unbounded below. Hence, the QP [](#prob:unconstrained_QP) has no solutions. 

### Case 2: $\lambda_1 = \lambda_{\min}(Q) = 0$
This case corresponds to $Q \succeq 0$ (positive semidefinite).
- If $p \notin \operatorname{Im} Q$, the equation $\nabla f(x) = 0$ has no solution, so there is no stationary point. Since $f$ is convex (because $\nabla^2 f \succeq 0$), the QP [](#prob:unconstrained_QP) has no solution.
- If $p \in \operatorname{Im} Q$, the equation $\nabla f(x) = 0$ admits infinitely many solutions of the form $x_0 + \mathbf{n}$, where $x_0$ is a particular solution and where $\mathbf{n} \in \ker Q$. They are all solutions of the QP [](#prob:unconstrained_QP).

### Case 3: $\lambda_1 = \lambda_{\min}(Q) > 0$
This case corresponds to $Q \succ 0$ (positive definite).

The matrix $Q$ is invertible since all its eigenvalues are strictly positive. 
Moreover $f$ is strictly convex (because $\nabla^2 f \succ 0$) and therefore there is a unique solution, given by $\hat{x} = Q^{-1}p$


:::{admonition} Recap: Solutions of Unconstrained QP
- If $Q$ is **not positive semidefinite** ($\lambda_{\min}(Q) < 0$): no solution exists, the objective is unbounded below.
- If $Q$ is **positive semidefinite** ($\lambda_{\min}(Q) = 0$):
    - If $p \notin \operatorname{Im} Q$: no solution.
    - If $p \in \operatorname{Im} Q$: infinitely many solutions, forming an affine subspace.
- If $Q$ is **positive definite** ($\lambda_{\min}(Q) > 0$): unique solution $\hat{x} = Q^{-1}p$.
:::