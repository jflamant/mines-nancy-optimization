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


## Smooth gradient descent: first results

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
## Smooth convex case
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
- the theorem establishes convergence in cost values, but we can only show boundedness of iterates, i.e., : $\Vert x^{(k)} - x^\star\Vert \leq \Vert x^{(0)} - x^\star\Vert$; see below for an illustration in the case of quadratic convex functions.


:::{prf:proof} Proof of [](#thm:cv_smooth_convex_gd)
:class:dropdown
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

To illustrate why we can only boundedness of iterates, let us consider the special case (but important) of convex quadratic functions.

:::{hint} Understanding boundedness of iterates: the special case of quadratic functions
Let $f: x \mapsto \frac{1}{2} x^\top Qx -   p^\top x$ with $Q \succeq 0$, hence $f$ is convex. Recall that $\nabla f(x) = Qx - p$; moreover over $f$ is $L$-smooth with $L = \Vert Q\Vert_2 = \max_{\Vert x \Vert = 1} \Vert Q x\Vert_2 = \max_{i} \vert\lambda_i(Q)\vert$.

Since $Q\succeq 0$ a solution always exists (but not necessarily unique), given by $x^\star = Q^\dagger  p$ where $Q^\dagger$ is the pseudo inverse of $Q$ ($Q^\dagger = Q^{-1}$ when $Q\succ 0$).

<!-- Interestingly, for convex quadratic functions we have that $p = Qx^\star$ (because $\nabla f(x^\star) = 0)$. This permits to write the suboptimality in a simple way:
$$ f(x^{(k)}) - f(x^\star) &= \frac{1}{2}x^{(k)\top} Q x^{(k)} - p^\top x^{(k)} - \frac{1}{2}(x^\star)^\top Q x^\star + p^\top x^\star\\
&=  \frac{1}{2}x^{(k)\top} Q x^{(k)} - (x^\star)^\top  x^{(k)} - \frac{1}{2}(x^\star)^\top Q x^\star + (x^\star)^\top Qx^\star\\
&= \frac{1}{2}x^{(k)\top} Q x^{(k)} - \frac{1}{2}((x^\star)^\top Q x^{(k)} + x^{(k)\top }Q x^\star) + \frac{1}{2}(x^\star)^\top Q x^\star\\
&= \frac{1}{2}\left[x^{(k)} - x^\star\right]^\top Q \left[x^{(k)} - x^\star\right].$$ -->

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
Indeed, if $Q$ has a zero eigenvalue, $\Vert I - Q/L\Vert_2 = 1$ and the algorithm will get "stuck" in directions corresponding to that eigenvalue.
<!-- Moreover, we have
$$f(x^{(k+1)}) - f(x^\star) &= \frac{1}{2}\left[x^{(k+1)} - x^\star\right]^\top Q \left[x^{(k+1)} - x^\star\right]\\
& =(x^{(k)} -x^\star)^\top \left[I_n - \frac{1}{L}Q\right]^{\top} Q\left[I_n - \frac{1}{L}Q\right](x^{(k)} -x^\star)\\
&\leq  \underbrace{\left\Vert\left[I_n - \frac{1}{L}Q\right]\right\Vert_2^2}_{\leq 1} \underbrace{\Vert Q\Vert_2}_{=L}\Vert x^{(0)} -x^\star\Vert^2_2$$ -->
:::

## Smooth and strongly convex case
:::{important} Assumptions
- The objective $f$ is [$L$-smooth](#def:lsmooth)
- The objective $f$ is [$m$-strongly convex](#def:strong_cvx)
- a solution $x^\star$ exists
:::
Strong convexity permits to refine the previous [](#thm:cv_smooth_convex_gd) with a better rate.

:::{prf:theorem}
:label:thm:cv_smooth_strong_convex_gd
 Under the above assumptions, the gradient descent method with **$\alpha_k = 1/L$** generates a sequence $\lbrace x^{(k)}\rbrace$ such that, after $K$ iterations,
\begin{align*}
    f(x^{(K)}) - f(x^\star) &\leq \left(1-\frac{m}{L}\right)^K\left(f(x^{(0)}) - f(x^\star) \right)\\
    \Vert x^{(K)} - x^\star \Vert_2^2&\leq \left(1-\frac{m}{L}\right)^K \Vert x^{(0)} - x^\star \Vert_2^2
\end{align*}
:::

**Comments**
- convergence in objective and iterates;
- exponential rate (also known as *linear convergence*)
- rate depends on the ratio $m/L$  (yet not the best in this case, see further below)

Before giving the proof of [](#thm:cv_smooth_strong_convex_gd), we can give a simple proof of this result in the case of convex quadratics.

:::{hint} Strongly convex quadratic case
 Let $f(x) = \frac{1}{2} x^\top Qx -   p^\top x$ with $Q \succ 0$. Note $m = \lambda_{\min} (Q)$ and $L = \lambda_{\max} (Q)$. Hence $f$ is $m-$strongly convex and $L-$smooth.

Then
\begin{align*}
x^{(k+1)} - x^* &= x^{(k)} - \frac{1}{L}(Qx^{(k)} -   p) -x^*\\
&= x^{(k)}-x^* -\frac{1}{L}Q(x^{(k)}-x^*)\\
&=(I_n -\frac{1}{L}Q)(x^{(k)}-x^*)
\end{align*}
Since $Q \succ 0$, the eigenvalues of $I_n -\frac{1}{L}Q$ are in $[0, 1-(m/L)]$.

Therefore, by recursion
$$
   \Vert x^{(K)} - x^*\Vert_2^2 \leq \left(1-\frac{m}{L}\right)^K \Vert x^{(0)} - x^*\Vert_2^2.
$$
(Similar proof for objective values).
:::


:::{prf:proof} Proof of [](#thm:cv_smooth_strong_convex_gd), convergence in objective values
:class:dropdown
Suppose that $f$ is $L$-smooth and $m$-strongly convex. From the [definition of strong convexity](#def:strong_cvx), we have
$$
f(z) \geq f(x) + \nabla f(x)^\top (z-x) + \frac{m}{2}\Vert z -x \Vert^2\text{ for any } z, x \in \mathbb{R}^n
$$
Let us minimize this inequality with respect to $z$.
The LHS is clearly minimized for $z = x^\star$. On the other hand, the RHS is minimized for $z= x - (1/m)\nabla f(x)$. Thus,
$$
f(x^\star) \geq f(x) + \nabla f(x)^\top  \frac{1}{m}\nabla f(x) + \frac{m}{2}\Vert \frac{1}{m}\nabla f(x)\Vert^2 = f(x) - \frac{1}{2m}\Vert \nabla f(x)\Vert^2
$$
Rearranging terms, we obtain the famous Polyak-Lojasiewicz (PL) inequality
$$
\Vert \nabla f(x)\Vert^2 \geq 2m \left[f(x) - f(x^\star)\right]\text{ for all } x \in \mathbb{R}^n\tag{PL}
$$
Now, recall that for $L$-smooth functions, we have shown that (see above)
$$
f(x^{(k+1)}) \leq f(x^{(k)}) - \frac{1}{2L}\Vert \nabla f(x^{(k)})\Vert^2
$$
Bounding the gradient norm through PL inequality, we get
$$
f(x^{(k+1)}) \leq f(x^{(k)})  - \frac{2m}{2L} \left[f(x^{(k)}) - f(x^\star)\right]
$$
substracting $f(x^\star)$ from both sides gives
$$
f(x^{(k+1)}) - f(x^\star)\leq \left(1  - \frac{m}{L}\right) \left[f(x^{(k)}) - f(x^\star)\right]
$$
showing linear convergence.
After $K$ iterations, we get the result
$$
f(x^{(K)}) - f(x^\star) \leq \left(1  - \frac{m}{L}\right)^K \left[f(x^{(0)}) - f(x^\star)\right].
$$
:::

:::{prf:proof} Proof of [](#thm:cv_smooth_strong_convex_gd), convergence in iterates
:class:dropdown
Let $\alpha = 1/L$.
Computing the distance between the current iterate and the optimum, we get
\begin{align*}
\Vert x^{(k+1)} - x^\star\Vert_2^2 &=  \Vert x^{(k)} - \alpha\nabla f(x^{(k)}) - x^\star\Vert_2^2 & \text{ (GD)}\\
&= \Vert x^{(k)} - x^\star \Vert^2 +\alpha^2 \Vert \nabla f(x^{(k)})\Vert^2 - 2\alpha\nabla f(x^{(k)})^\top (x^{(k)} - x^\star)& \\
&= \Vert x^{(k)} - x^\star \Vert^2 +\alpha^2 \Vert \nabla f(x^{(k)})\Vert^2 + 2\alpha\nabla f(x^{(k)})^\top (x^\star-x^{(k)} )& \\
&\leq \Vert x^{(k)} - x^\star \Vert^2 + \alpha^2 \Vert \nabla f(x^{(k)})\Vert^2 + 2\alpha(f(x^\star) - f(x^{(k)})) - \alpha m \Vert x^{(k)} - x^\star\Vert & \text{(strong convexity)}\\
&= (1-\alpha m)\Vert x^{(k)} - x^\star \Vert^2 + \alpha^2 \Vert \nabla f(x^{(k)})\Vert^2 + 2\alpha(f(x^\star) - f(x^{(k)})) &\\
&\leq (1-\alpha m)\Vert x^{(k)} - x^\star \Vert^2 + \alpha^2 \Vert \nabla f(x^{(k)})\Vert^2 - \frac{\alpha}{L}\Vert \nabla f(x^{(k)})\Vert^2 & (\text{$L$-smooth property})\\
&= (1-\alpha m)\Vert x^{(k)} - x^\star \Vert^2 + \alpha(\alpha - \frac{1}{L}) \Vert \nabla f(x^{(k)})\Vert^2 &\\
&\leq (1-\alpha m)\Vert x^{(k)} - x^\star \Vert^2  & (\alpha(\alpha-\frac{1}{L})\leq 0\text{ for } \alpha \leq 1/L)
\end{align*}

As a consequence we have that, for $\alpha = 1/L$
$$
\Vert x^{(K)} - x^\star\Vert_2^2 \leq (1-\frac{m}{L})^K  \Vert x^{(0)} - x^\star\Vert_2^2
$$
:::

### Refining the rate

It is possible to refine the rate in [](#thm:cv_smooth_strong_convex_gd) using a property of strongly convex functions, known as the **coercivity of the gradient**.

:::{prf:property} Coercivity of the gradient
:label:prop:coercivity_gradient
Let $f$ be $L$-smooth and $m$-strongly convex on $\mathbb{R}^n$. Then for all $x, y \in \mathbb{R}^n$, one has
$$
    (\nabla f(x) - \nabla f(y))^\top(x-y) \geq \frac{m L}{m+L}\Vert x- y \Vert^2 + \frac{1}{L+m}\Vert \nabla f(x) - \nabla f(y)\Vert^2
$$
:::
See for instance  @bubeck2015convex [Lemma 3.11] or @nesterov2013introductory [Theorem 2.1.12] for a proof of this property.

:::{prf:theorem}
:label:thm:cv_smooth_strong_convex_gd_refined
Under the above assumptions, the gradient descent method with $\alpha_k = 2/(m+L)$ generates a sequence $\lbrace x^{(k)}\rbrace$ such that, after $K$ iterations,
\begin{align*}
    f(x^{(K)}) - f(x^\star) &\leq \frac{L}{2}\left(\frac{L-m}{L+m}\right)^{{2K}}\Vert x^{(0)} - x^\star \Vert_2^2\\
    \Vert x^{(K)} - x^\star \Vert_2^2 &\leq {\left(\frac{L-m}{L+m}\right)^{{2K}}} \Vert x^{(0)} - x^\star \Vert_2^2
\end{align*}
:::
**Comments**
- improved rate: still linear, but better constant than in [](#thm:cv_smooth_strong_convex_gd)
- stepsize incorporates our knowledge of strong convexity (compare with the stepsize $\alpha = 1/L$ use in previous theorems)

:::{prf:proof}
The proof is adapted from @bubeck2015convex [Theorem 3.12].
It exploits directly [the coercivity of the gradient property](#prop:coercivity_gradient).
Fix $x = x^{(k)}$ and $y = x^\star$ in [](#prop:coercivity_gradient) we get the bound
$$
    (\nabla f(x^{(k)}))^\top(x^{(k)}-x^\star) \geq \frac{m L}{m+L}\Vert x^{{k}} - x^\star \Vert^2 + \frac{1}{L+m}\Vert \nabla f(x^{(k)})\Vert^2
$$
Let us consider the case of arbitrary stepsize $\alpha$.
Computing the distance between the current iterate and the optimum, we get
\begin{align*}
    \Vert x^{(k+1)} - x^\star\Vert_2^2 &=  \Vert x^{(k)} - \alpha\nabla f(x^{(k)}) - x^\star\Vert_2^2 & \text{ (GD)}\\
    &= \Vert x^{(k)} - x^\star \Vert^2 +\alpha^2 \Vert \nabla f(x^{(k)})\Vert^2 - 2\alpha\nabla f(x^{(k)})^\top (x^{(k)} - x^\star)& \\
    &\leq \left(1-\frac{2\alpha m L}{m+L}\right) \Vert x^{(k)} - x^\star \Vert^2 + \alpha \left(\alpha-\frac{2}{L+m}\right)\Vert \nabla f(x^{(k)})\Vert^2 & \text{(coercivity of the gradient)}
\end{align*}
The last term is negative if and only if $\alpha \leq 2/(L+m)$. Hence, for $0 < \alpha \leq 2/(L+m)$, one has
$$
    \Vert x^{(K)} - x^\star\Vert_2^2 \leq \left(1-\frac{2\alpha m L}{m+L}\right)^K \Vert x^{(0)} - x^\star \Vert^2
$$
In particular for $\alpha = 2/(L+m)$, we get
$$
    \Vert x^{(K)} - x^\star\Vert_2^2 \leq \left(\frac{L-m}{L+m}\right)^{2K} \Vert x^{(0)} - x^\star \Vert^2
$$
which proves the convergence of iterates.

To show that the objective converges as well, use the fact that, by $L$-smoothness of $f$,
$$
    f(x^{(K)}) - f(x^\star)\leq \frac{L}{2}\Vert x^{(K)} - x^\star\Vert_2^2
$$
hence
$$
    f(x^{(K)}) - f(x^\star)\leq \frac{L}{2}\left(\frac{L-m}{L+m}\right)^{2K} \Vert x^{(0)} - x^\star \Vert^2 .
$$
:::

## Summary

For gradient descent with constant stepsize, *convergence properties* depend on *assumptions* on the function $f$.

| assumption on $f$      | stepsize $\alpha$  | rate                                            | convergence of iterates? |
| ----------------------- | ------------------ | ----------------------------------------------- | -------------- |
| smooth                  | $\alpha = \frac{1}{L}$    | $\Vert\nabla f(x^{(K)})\Vert = \mathcal{O}(1/\sqrt{K})$ | –              |
| convex, smooth          | $\alpha = \frac{1}{L}$    | $f(x^{(K)}) - f(x^\star) = \mathcal{O}(1/K)$   | bounded        |
| strongly convex, smooth | $\alpha = \frac{1}{L}$    | $f(x^{(K)}) - f(x^\star) = \mathcal{O}(c^{K})$  | yes            |
| strongly convex, smooth | $\alpha = \frac{2}{L+m}$ | $f(x^{(K)}) - f(x^\star) = \mathcal{O}(d^{2K})$ | yes            |

:::{note} What about other stepsize strategies?
Most of the results for constant step size can be adapted to other stepsize strategies. \bigskip

For instance, let $f$ be $L$-smooth and $m$-strongly convex. We have:
- **Optimal step size** same result as $\alpha = \frac{1}{L}$, i.e.,
$$
f(x^{(K)}) - f(x^\star) \leq \left(1-\frac{m}{L}\right)^K\left(f(x^{(0)}) - f(x^\star) \right)
$$
- **Backtracking line search** similar result, with a different constant
$$
f(x^{(K)}) - f(x^\star) \leq c^K\left(f(x^{(0)}) - f(x^\star) \right)
$$
where $c = 1- \min(2ms, 2\eta s m/L) < 1$, with $(s, \eta)$ backtracking parameters

See @boyd2004convex [Section 9.3.1] for more detail.
:::
