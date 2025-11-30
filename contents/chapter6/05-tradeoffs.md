# Assessing tradeoffs: complexity vs precision

 It is useful to express convergence results as *complexity* bounds, i.e., given a precision $\varepsilon$, provide a bound on the number of iterations $K$ needed to reach that bound, in the worst case scenario.

:::{prf:example}
  Consider gradient descent in the smooth convex case.
  We have from the [theorem](#thm:cv_smooth_convex_gd);
$$
f(x^{(K)}) - f(x^\star) \leq \frac{L}{2K}\Vert x^{(0)} - x^\star\Vert_2^2
$$
Let $\varepsilon > 0$ be the desired precision after $K$ iterations. Then, if one wants $f(x^{(K)}) - f(x^\star) \leq \varepsilon$, this can be satisfied if
that
$$
\frac{L}{2K}\Vert x^{(0)} - x^\star\Vert_2^2\leq \varepsilon
$$
and thus whenever
$$
K \geq\frac{L}{2\varepsilon}\Vert x^{(0)} - x^\star\Vert_2^2
$$
Hence, the number of iterations is a linear function of the inverse precision $\frac{1}{\varepsilon}$.

:::{exercise}
Let $\tau^{(k)} = f(x^{(k)})-f(x^\star)$ and note $c = 1-m/L$.

For $f$ smooth and strongly convex, show that the number of iterations of gradient descent with $\alpha = 1/L$ scales with $\varepsilon$ as
$$
  K \geq c_1 \log \frac{1}{\varepsilon} + c_2
$$
where $c_1\geq 0, c_2 \in \mathbb{R}$ are constants to be determined.
:::

:::{exercise}
For Newton's method, show that the number of iterations $K$ that guarantees $\tau^{(K)} = \Vert x^{(K)} - x^\star\Vert_2 \leq \varepsilon$ is such that
$$
K \geq c_1 \log \log \frac{1}{\varepsilon} + c_2
$$
where $c_1, c_2$ are constants to be determined.

(Hint): use the fact that the condition that $x^{(0)}$ must be sufficiently close from $x^\star$ can be formulated as $L'\tau^{(0)} < 1$.
:::

We conclude this chapter with a few important takeaways.

:::{important} Chapter summary
- convergence results can be stated in terms of **objective values** or in terms of convergence of **iterates** (sometimes, both)
- convergence rates correspond to worst-case scenarios: they do not tell about the exact rate of algorithms for a given problem
- this means that gradient descent, **under the right assumptions**, converges **at least with a linear rate**
- this means that Newton's method, **under the right assumptions**, converges **at least with a quadratic rate**
\item Statements on number of iterations such as
$$
K \geq c_1 \log \frac{1}{\varepsilon} + c_2 \text{ or }  \qquad K \geq c_1 \log \log \frac{1}{\varepsilon} + c_2
$$
are "at most" statements.
:::
