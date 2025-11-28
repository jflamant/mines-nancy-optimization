# Convergence properties for gradient descent

We consider solving the unconstrained optimization problem
$$\begin{array}{ll}
      \minimize &\quad f(x)\\
      \st& \quad x \in \mathbb{R}^n
  \end{array}
$$
through gradient descent with **constant step size** $\alpha > 0$
$$  x^{(k+1)} =  x^{(k)} -\alpha \nabla f(x^{(k)}), \quad k=0, 1, 2, \ldots$$

**Motivations**
- simplest case, yet already illuminating
- common strategy for large scale applications (e.g. machine learning)
- results can be extended to backtracking / optimal step size without too much trouble.


## Smooth gradient descent

### First results
:::{important} Assumptions
- The objective $f$ is [$L$-smooth](#def:lsmooth)
- a solution $x^\star$ exists
:::
Let us exploit [the upper bound property](#prop_quad_upper_bound_smooth) of smooth functions. Setting $y =  x^{(k+1)}$ and $x =  x^{(k)}$, one gets the inequality
$$
f( x^{(k+1)}) \leq f( x^{(k)}) + \nabla f( x^{(k)})^\top ( x^{(k+1)}- x^{(k)}) + \frac{L}{2}\Vert  x^{(k+1)}- x^{(k)}\Vert_2^2
$$
Using the gradient descent update, this simplifies as
$$
f( x^{(k+1)}) \leq f( x^{(k)}) + \alpha \left(\alpha\frac{L}{2}-1\right)\Vert \nabla f( x^{(k)})\Vert_2^2
$$
The RHS is minimized for $\alpha = 1/L$. For this choice of stepsize, one gets
$$
f( x^{(k+1)})  = f( x^{(k)} - (1/L)\nabla f(x^{(k)}))\leq f( x^{(k)}) - \frac{1}{2L} \Vert \nabla f( x^{(k)})\Vert_2^2
$$
in particular  $f( x^{(k+1)})  \leq  f( x^{(k)})$ for this choice of stepsize.
Now, summing the previous inequalities from $k=0$ to $k+1=K$ one gets
$$
  f(x^{(K)}) \leq f(x^{(0)}) - \frac{1}{2L}\sum_{k=0}^{K-1}  \Vert \nabla f( x^{(k)})\Vert_2^2
$$
Since $f(x^{(K)}) \geq f(x^\star)$,
$$
    \sum_{k=0}^{K-1} \Vert \nabla f( x^{(k)})\Vert_2^2 \leq 2L \left[f(x^{(0)}) - f(x^\star)\right]
$$
This shows that the quantity $\sum_{k=0}^{K-1} \Vert \nabla f( x^{(k)})\Vert_2^2$ is upper bounded by a positive constant $2L \left[f(x^{(0)}) - f(x^\star)\right]$, for any value of $K$. Therefore, one has necessarily
$$\lim_{K\rightarrow \infty} \Vert \nabla f( x^{(K)})\Vert_2^2 = 0$$

Hence, **gradient descent converges to a stationary point** of $f$.

In addition, we get that
$$
  \min_{0 \leq k \leq K-1} \Vert \nabla f( x^{(k)})\Vert_2 \leq \sqrt{\frac{2L[f(x^{(0)})-f(x^{(K)})]}{K}} \leq \sqrt{\frac{2L[f(x^{(0)})-f(x^\star)]}{K}}
$$
After $K$ steps of gradient descent, we can find a point $x$ such that
$$
  \Vert\nabla f(x)\Vert_2 \leq  \sqrt{\frac{2L[f(x^{(0)})-f(x^\star)]}{K}}
$$

:::{important} Recap: Gradient descent for smooth functions
- *convergence* can only be guaranteed on the norm of the gradient of iterates
- the *convergence rate* is very slow, in $K^{-1/2}$
- we cannot say much more without additional assumptions on $f$.
:::
### Convex case
:::{important} Assumptions
- The objective $f$ is [$L$-smooth](#def:lsmooth)
- The objective $f$ is [convex](#def:convex_function)
- a solution $x^\star$ exists
:::

With the addition of the convex assumption on $f$, we can establish the following theorem.

:::{prf:theorem}
:label:thm:cv_smooth_convex_gd
Under the above assumptions, the gradient descent method with constant stepsize $\alpha_k = 1/L$ generates a sequence $\lbrace x^{(k)}\rbrace$ such that, after $K$ iterations,
$$
f(x^{(K)}) - f(x^\star) \leq \frac{L}{2K}\Vert x^{(0)} - x^\star\Vert_2^2
$$
:::

**Comments**
- the convergence is rate has improved, in $K^{-1}$
- the theorem establishes convergence in cost values, but we can only show boundedness of iterates, i.e., : $\Vert x^{(k)} - x^\star\Vert \leq \Vert x^{(0)} - x^\star\Vert$
- a simple proof for quadratic functions exist

Before proving [](#thm:cv_smooth_convex_gd), we detail the main ideas of the proof in the special case of quadratic $f$.

:::{hint} The special case of quadratic functions
Let $f: x \mapsto \frac{1}{2} x^\top Qx -   p^\top x$ with $Q \succeq 0$, hence $f$ is convex. Recall that $\nabla f(x) = Qx - p$; moreover over $f$ is $L$-smooth with $L = \Vert Q\Vert_2 = \max_{\Vert x \Vert = 1} \Vert Q x\Vert_2 = \max_{i} \vert\lambda_i(Q)\vert$.

Since $Q\succeq 0$ a solution always exists (but not necessarily unique), given by $x^\star = Q^\dagger  p$ where $Q^\dagger$ is the pseudo inverse of $Q$ ($Q^\dagger = Q^{-1}$ when $Q\succ 0$).

Iterates read
$$
x^{(k+1)} =  x^{(k)} - \frac{1}{L}(Qx^{(k)} -   p) =  x^{(k)} - \frac{1}{L}Q(x^{(k)} -x^\star)
$$
Therefore
$$
x^{(k+1)}  -   x^\star = \left[I_n - \frac{1}{L}Q\right](x^{(k)} -x^\star) = \left[I_n - \frac{1}{L}Q\right]^{k+1}(x^{(0)} -x^\star)
$$
Since $Q \succeq 0$, eigenvalues of $I_n - \frac{1}{L}Q$ are in $[0, 1]$. Therefore $\Vert I_n - \frac{1}{L}Q \Vert_2 \leq 1$ and thus we have boundedness of the iterates
$$
\Vert x^{(k+1)}  -   x^\star  \Vert_2 \leq  \Vert x^{(0)}  -   x^\star\Vert
$$

:::

:::{prf:proof} Proof of [](#thm:cv_smooth_convex_gd)

By convexity of $f$, we have, for any iterate $x^{(k)}$
$$
f(x^\star) \geq f(x^{(k)})+ \nabla f(x^{(k)})^\top (x^\star-x^{(k)})
$$
Recall that when $f$ is $L$-smooth, gradient descent with constant stepsize $\alpha = 1/L$ satisfies (see above for the proof)
$$
f(x^{(k+1)}) \leq f(x^{(k)}) - \frac{1}{2L}\Vert \nabla f(x^{(k)})\Vert^2
$$
By substituting the two equations into one another, one obtains
\begin{align*}
    f(x^{(k+1)}) & \leq  f(x^\star) + \nabla f(x^{(k)})^\top (x^{(k)}-x^\star)- \frac{1}{2L}\Vert \nabla f(x^{(k)})\Vert^2& \text{(convexity)}\\
    &= f(x^\star) + \frac{L}{2}\left( \Vert x^{(k)}-x^\star\Vert^2 -  \Vert x^{(k)}-x^\star-(1/L)\nabla f(x^{(k)})\Vert^2\right)&\text{(completing the squares)}\\
    &= f(x^\star) + \frac{L}{2}\left( \Vert x^{(k)}-x^\star\Vert^2 -  \Vert x^{(k+1)}-x^\star\Vert^2\right)&\text{(GD update definition)}
\end{align*}
Summing over $k=0, 1, \ldots, K-1$ we get
\begin{align*}
    \sum_{k=0}^{K-1}  \left(f(x^{(k+1)}) -  f(x^\star)\right) &\leq \frac{L}{2}\sum_{k=0}^{K-1}\left( \Vert x^{(k)}-x^\star\Vert^2 -  \Vert x^{(k+1)}-x^\star\Vert^2\right)\\
    &= \frac{L}{2}\left( \Vert x^{(0)}-x^\star\Vert^2 -  \Vert x^{(K)}-x^\star\Vert^2\right)\\
    &\leq \frac{L}{2}\Vert x^{(0)}-x^\star\Vert^2
\end{align*}
Recall that by $L$-smoothness and this choice of stepsize, $\lbrace
f(x^{(k+1)})\rbrace$ is non-increasing. Thus
$$
f(x^{(K)}) - f(x^\star) \leq \frac{1}{K} \sum_{k=0}^{K-1}  \left(f(x^{(k+1)}) -  f(x^\star)\right) \leq \frac{L}{2K} \Vert x^{(0)}-x^\star\Vert^2
$$
as expected.
:::
