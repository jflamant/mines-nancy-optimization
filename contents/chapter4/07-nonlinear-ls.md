# Algorithms for nonlinear least-squares problems

An important special case of (unconstrained) optimization problems are **non-linear least squares**:

:::{math}
:label:prob:nonlinear_ls
\begin{array}{ll}
\minimize &\quad f_0(x):= \dfrac{1}{2} \sum_{j=1}^m r_j(x)^2\\
\st & \quad x \in \mathbb{R}^n
\end{array}
:::
where $r_j: \mathbb{R}^n\to \mathbb{R}$ are *residual* functions. Equivalently, define
$$R(x)= \left[r_1(x),r_2(x), \ldots, r_m(x)\right]^\top,$$
such that the optimization problem [](#prob:nonlinear_ls) can be written as
:::{math}
:label:prob:nonlinear_ls_alt
\begin{array}{ll}
\minimize &\quad f_0(x):= \Vert R(x)\Vert_2^2, \quad R: \mathbb{R}^n \to \mathbb{R}^m\\
\st & \quad x \in \mathbb{R}^n
\end{array}
:::

:::{note} Remark
The term **nonlinear least-squares** comes from the fact that $R$ may be an arbitrary, non-linear function of $x$; if $R$ is linear (or affine), it can be written as $R(x) = Ax - b$ and ones recovers a standard [(linear) least square problem](#def:linear_LS); see [Chapter 3](../chapter3/01-intro.md) .
:::

## Exploiting the structure of the problem

Problems [](#prob:nonlinear_ls) and [](prob:nonlinear_ls_alt) have a specific structure that can be exploited to devise efficient algorithms.

:::{important} Key tool: the Jacobian matrix
:label:def:jacobian
Let $R: \mathbb{R}^n \to \mathbb{R}^m$. The Jacobian matrix of $R$ at point $x$ is defined as
$$ J(x) = \begin{bmatrix}
      \dfrac{\partial r_1}{\partial x_1} & \ldots & \dfrac{\partial r_1}{\partial x_n}\\
      \dfrac{\partial r_2}{\partial x_1} & \ldots & \dfrac{\partial r_2}{\partial x_n}\\
      \vdots & & \vdots\\
      \dfrac{\partial r_m}{\partial x_1} & \ldots & \dfrac{\partial r_m}{\partial x_n}\\
    \end{bmatrix} =
    \begin{bmatrix}
      \nabla r_1(x)^\top\\
      \nabla r_2(x)^\top\\
      \vdots\\
      \nabla r_m(x)^\top\\
    \end{bmatrix}
    \in \mathbb{R}^{m\times n}
    $$
:::

Using the Jacobian matrix allows for a simple expression of the gradient and Hessian of $f_0(x) = \frac{1}{2}\Vert R(x)\Vert^2_2$.
Let us compute the gradient and Hessian of $f_0$:
$$
\nabla f_0(x) & = \sum_{j=1}^m r_j(x)\nabla r_j(x) = J(x)^\top R(x)\\
\nabla^2 f_0(x) & = \sum_{j=1}^m \nabla r_j(x)\nabla r_j(x)^\top + \sum_{j=1}^m r_j(x)\nabla^2 r_j(x) \\
& =  J(x)^\top J(x) + \sum_{j=1}^m r_j(x)\nabla^2 r_j(x)
$$

**Why are these expression interesting?** Because, in many applications:

- Computing the Jacobian $J(x)$ is inexpensive (or easy to obtain)
- The gradient is directly obtained from $ \nabla f_0(x) = J(x)^\top R(x)$
- Often, the second term in the Hessian can be neglected (especially near the solution), so that
  $$
  \nabla^2 f_0(x) \approx J(x)^\top J(x)
  $$
  meaning the Hessian can be well-approximated without second derivatives.

We exploit these properties to construct dedicated nonlinear least squares algorithms, known as the *Gauss-Newton method* and the *Levenberg-Marquardt method*, respectively.

## Algorithms for nonlinear least squares

The Gauss-Newton method can be seen as a quasi-Newton method with matrix $B_k = J(x^{(k)})^\top J(x^{(k)})$ and constant step-size $\alpha_k = 1$.

:::{prf:algorithm} Gauss-Newton Method

Given initial point $x^{(0)}$, iterate
$$
x^{(k+1)} =  x^{(k)} - \left[J(x^{(k)})^\top J(x^{(k)})\right]^{-1} \nabla f_0(x^{(k)})
$$
until a [suitable stopping criterion](./03-general-principles-descent-methods.md#how-to-define-a-stopping-criterion) is met.
:::

The Levenberg-Marquardt method can also be interpreted as a quasi-Newton method with constant step-size $\alpha_k = 1$. The method introduces a regularization parameter $\lambda_k$, which is tuned throughout iterations.

:::{prf:algorithm} Levenberg-Marquardt Method
Given initial point $x^{(0)}$, the $k$-th iteration reads
1. set $C_k = J(x^{(k)})^\top J(x^{(k)})$
2. pick $\lambda_k$ (see discussion below)
3. compute
$$
x^{(k+1)} =  x^{(k)} - \left[C_k + \lambda_k \operatorname{diag}(C_k) \right]^{-1} \nabla f_0(x^{(k)})
$$
where $\diag(C_k)$ extracts the main diagonal of the matrix $C_k$.


Repeat steps 1-2-3 until a [suitable stopping criterion](./03-general-principles-descent-methods.md#how-to-define-a-stopping-criterion) is met.
:::
The Levenberg-Marquardt method is one the standard algorithms for non-linear least squares problems. It is implemented in many libraries, such as ``scipy.optimize.least_squares`` (use option ``method='lm'``).


:::{hint} Choosing Levenberg-Marquardt regularization parameter $\lambda_k$
This is critical (but very wide question) in itself.
Here are some useful guidelines:

- **Start safe**: initialize with a moderately large $\lambda_0$
- **Adapt dynamically**:
  - If objective decreases → decrease $\lambda_k$ (move toward Gauss–Newton)
  - If objective increases → increase $\lambda_k$ (move toward gradient descent)
- **Balance**:
  - Small $\lambda_k$ → faster but less stable
  - Large $\lambda_k$ → slower but robust
- **Safeguard bounds**: keep within  $\lambda_{\min} \leq \lambda_k \leq \lambda_{\max}$

For more details, one can consult [@nocedal2006numerical, Chapters 10-11].
:::
