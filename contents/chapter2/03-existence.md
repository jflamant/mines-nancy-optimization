# Existence of solutions

Let $f:\Omega \subset \mathbb{R}^n\longrightarrow \mathbb{R}$ and consider the following optimization problem

$$
\label{eq:prob}
\min_{{x} \in \Omega} f({x})
$$

Under which conditions can we guarantee existence of a solution to the problem [](#eq:prob) ? 

Recall that we work in finite dimensions only. The theorem below provide two sufficient conditions.

:::{prf:theorem} Existence in finite dimensions
:label: thm:existence
Suppose that $f$ is continuous and that $\Omega\subset \mathbb{R}^n$. If one of the following conditions is satisfied:
- $\Omega$ is **compact** (or equivalently, **closed and bounded** since we are in finite dimension);
- $\Omega$ is **closed** and $f$ is **coercive** (i.e., such that $f({x})\xrightarrow[\Vert {x} \Vert \to +\infty]{} +\infty$),

then the problem [](#eq:prob) admits (at least) one solution.
:::

:::{prf:remark}
The $\mathbb{R}^n$ is both open and closed. Therefore, according to [](#thm:existence), for optimization problems of the form 
$$\min_{x \in \mathbb{R}^N} f({x})$$ 
it suffices to have $f$ *continuous and coercive* to show that the problem admits at least one solution. 
:::


:::{exercise}
:::