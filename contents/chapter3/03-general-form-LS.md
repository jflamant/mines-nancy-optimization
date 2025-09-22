# General form of (linear) least squares problems

The main hypothesis is that there exists a **linear** relationship between data $y \in \mathbb{R}^m$ and variables $x \in \mathbb{R}^n$. 

```{figure} ./figures/ls-general-form.svg
---
height: 300px
name: ls-general-form
---
illustration of the linear model $y = Ax + b$ in the overdetermined case ($m \geq n$)
```
## Definitions and Notations
- The vector $y \in \mathbb{R}^m$ contains the data, also named observations or measurements, which we want to model;
- The matrix $A \in \mathbb{R}^{m\times n}$ encodes the model (assumed known).
- The vector $x\in \mathbb{R}^n$ represents the unknowns.
- The vector $b \in \mathbb{R}^m$ expresses errors (model, noise, etc.).

::::{prf:definition} Linear least squares
Given data $y \in \mathbb{R}^m$ and model matrix $A \in \mathbb{R}^{m\times n}$, the linear least squares problem is the optimization problem 
:::{math}
:label:prob:linear_LS
\begin{array}{ll}
\minimize&  \Vert y - A x\Vert_2^2\\
\st & x \in \mathbb{R}^n
\end{array}
:::
::::
For simplicity, we assume that observations $y_1, \ldots y_n$ are linearly independent. This is equivalent to saying that observations are not redundant. The problem [](#prob:linear_LS) is said to be
- overdetermined when $m \geq n$ (more observations than unknowns)
- underdetermined when $m \leq n$ (strictly less observations than unknowns)

The over/under determination property play an important role regarding the uniqueness of solutions to the least squares problem, as we will see later on. 

## Interpretation as an unconstrained quadratic program (QP)



## Examples