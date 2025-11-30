# Convergence properties for Newton's method

We consider solving the unconstrained optimization problem
$$\begin{array}{ll}
      \minimize &\quad f(x)\\
      \st& \quad x \in \mathbb{R}^n
  \end{array}
$$
through Newton's method with **unit step size**
$$  x^{(k+1)} =  x^{(k)} - \left[\nabla^2 f(x^{(k)})\right]^{-1} \nabla f(x^{(k)}), \quad k=0, 1, 2, \ldots$$

**basic assumptions**
- $f$ is twice differentiable
- a solution $x^\star$ exists, and it satisfies the [second-order sufficient conditions](#thm:second_order_sufficient_conditions) such that $\nabla f(x^\star) = 0$ and $\nabla^2 f(x^\star) \succ 0$.
- The Hessian $\nabla^2 f$ is Lipschitz-continuous in a neighbourhood of $x^\star$.

The content of this section has been adapted from @nocedal2006numerical.

## Main result

:::{prf:theorem} @nocedal2006numerical [Theorem 3.5]
Under these assumptions, Newton's method with unit step size satisfies
- for $x^{(0)}$ close enough to $x^\star$, the sequence $\lbrace x^{(k)}\rbrace$ converges to $x^\star$;
- the rate of convergence of $\lbrace x^{(k)}\rbrace$ is quadratic
- the sequence of gradient norms $\lbrace \Vert \nabla f(x^{(k)})\Vert_2\rbrace$ converges quadratically to zero
:::

**Comments**
- Convergence guarantees are local, i.e., apply near the minimizer $x^\star$
- Convergence is quadratic, i.e., $$
  \Vert x^{(k+1)} - x^\star \Vert_2 \leq c \Vert x^{(k)} - x^\star \Vert_2^{{2}}$$
(Compare with the linear rate of gradient descent)

:::{prf:proof}
 Start by evaluating the distance and exploit the fact that $\nabla f(x)^\star = 0$
  \begin{align*}
    x^{(k+1)} - x^\star & =  x^{(k)} - x^\star - \left[\nabla^2f(x^{(k)})\right]^{-1}\nabla f(x^{(k)}) \\
    & = \left[\nabla^2f(x^{(k)})\right]^{-1}\left[\nabla^2 f(x^{(k)})(x^{(k)} - x^\star) - (\nabla f(x^{(k)})  - \nabla f(x^{\star}) )\right]
  \end{align*}
  Recall [Taylor's theorem](#thm:taylor) (Chapter 2) applied to gradients:
  $$
  \nabla f(x^{(k)})- \nabla f(x^\star) = \int_0^1 \nabla^2 f(x^{(k)} + t(x^\star - x^{(k)}))(x^{(k)}-x^\star)\mathrm{d}t
  $$
  We are now ready to bound the second parenthesis in the RHS above. Then
 \begin{align*}
  &\left\Vert\nabla^2 f(x^{(k)})(x^{(k)} - x^\star) - (\nabla f(x^{(k)})  - \nabla f(x^{\star}) )\right\Vert_2\\
  &=\left\Vert\int_0^1\left[\nabla^2 f(x^{(k)}) - \nabla^2 f(x^{(k)} + t(x^\star - x^{(k)}))\right](x^{(k)} - x^\star)\mathrm{d}t\right\Vert_2\\
  &\leq \Vert x^{(k)} - x^\star\Vert_2 \int_0^1\left\Vert\nabla^2 f(x^{(k)}) - \nabla^2 f(x^{(k)} + t(x^\star - x^{(k)}))\right\Vert\mathrm{d}t\\
  &\leq  \Vert x^{(k)} - x^\star\Vert_2^2 \int_0^1 L t\mathrm{d}t \quad \text{(Lipschitz-continuity of the Hessian near $x^\star$)}\\
  &=\frac{1}{2}L \Vert x^{(k)} - x^\star\Vert_2^2
 \end{align*}
 Moreover, $\nabla^2 f(x^\star)$ is non-singular and continuous. Thus there is a radius $r > 0$ such that $\Vert\nabla^2 f(x^{(k)})^{-1}\Vert \leq 2\Vert \nabla^2 f(x^\star)^{-1}\Vert$ for every $x^{(k)}$ s.t. $\Vert x^{(k)} - x^\star\Vert \leq r$.
 By substitution, we get
\begin{align*}
  \Vert  x^{(k+1)} - x^\star \Vert_2 \leq \frac{L}{2}\Vert \nabla^2 f(x^{(k)})^{-1}\Vert \Vert x^{(k)} - x^\star\Vert_2^2 \leq \underbrace{L\Vert \nabla^2 f(x^\star)^{-1}\Vert}_{=L'} \Vert x^{(k)} - x^\star\Vert_2^2
\end{align*}
If we choose $x^{(0)}$ such that $\Vert x^{(0)} - x^\star \Vert \leq \min(r, 1/(2L'))$ we obtain quadratic convergence to $x^\star$ as
\begin{align*}
  \Vert  x^{(k+1)} - x^\star \Vert_2 &\leq  L' \Vert  x^{(k)} - x^\star \Vert_2^2\\
  &\leq L' \cdot (L')^2 \Vert  x^{(k-1)} - x^\star \Vert_2^4\\
  & \leq L'(L')^{2}\cdots (L')^{2^{k+1}} \Vert x^{(0)} - x^\star \Vert_2^{2^{k+1}}\\
  & = (L')^{2^{k+1}-1}\Vert x^{(0)} - x^\star \Vert_2^{2^{k+1}}
\end{align*}

 Regarding the gradients, note $  p^{(k)} = - \left[\nabla^2f(x^{(k)})\right]^{-1} \nabla f(x^{(k)})$ and let us observe that
  \begin{align*}
    &x^{(k+1)} - x^{(k)} =   p^{(k)}\\
    \text{and }& \nabla f(x^{(k)}) + \nabla^2f(x^{(k)})  p^{(k)} = 0
  \end{align*}
  Then,
  \begin{align*}
    \Vert \nabla f(x^{(k+1)})\Vert_2 &= \Vert \nabla f (x^{(k+1)})- \nabla f(x^{(k)}) - \nabla^2f(x^{(k)})  p^{(k)}\Vert_2\\
    &= \left\Vert\int_0^1 \nabla^2f(x^{(k)} + t  p^{(k)})  p^{(k)}\mathrm{d}t- \nabla^2f(x^{(k)})  p^{(k)}\right\Vert_2\\
    &\leq \frac{1}{2}L\Vert   p^{(k)}\Vert_2^2 \\
    & \leq L' \Vert \nabla f(x^{(k)})\Vert_2^2
  \end{align*}
  which shows that gradient norms converge quadratically.
:::
