---
kernelspec:
    name: python3
---

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

The objective function $f(x) = \Vert y - Ax\Vert_2^2$ can be rewritten as 
:::{math}
f(x)&= (y - Ax)^\top ( y - Ax)\\
&=y^\top y - x^\top A^\top y - y^\top Ax + x^\top A^\top A x
:::
such that the least-squares optimization problem can be equivalently reformulated as the unconstrained quadratic program 
:::{math}
\begin{array}{ll}
\minimize&  \frac{1}{2}x^\top Q x - p^\top x\\
\st & x \in \mathbb{R}^n
\end{array}
:::
where $Q:= A^\top A \in \mathbb{R}^{n\times n}$ and $p := A^\top y \in \mathbb{R}^n$. 

:::{important}
the interpretation of unconstrained (linear) least squares problems as quadratic programs will be useful later on to study the existence and uniqueness of solutions of least square problems.
:::

## Examples

### Polynomial regression

Assume we are given some data $y_1, \ldots, y_m$ as a function of time $t$, as shown below:

```{code-cell} python
:tags: [hide-input]
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(2025)
def f(x):
    return x**3 + 4*x**2 - x + 1

N = 20
ts = np.linspace(-4., 2., N)

y = f(ts) + np.random.randn(N)
plt.scatter(ts, y, label="data $y$")
plt.xlabel(r"$t$")
plt.ylabel(r"$y$")
plt.legend()
plt.show()
```

The trend clearly looks polynomial. If we assume the degree $d$ of the polynomial is known (here $d=3$), we can write a **linear** model to *fit* the data $y$. 
The approach very classical: hence it is implemented in many libraries, such as [scikit-learn](https://scikit-learn.org/stable/) (see [PolynomialFeatures](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PolynomialFeatures.html) and [LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)).

```{code-cell} python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

degree = 3
poly = PolynomialFeatures(degree)
X_poly = poly.fit_transform(ts.reshape(-1, 1))

model = LinearRegression()
model.fit(X_poly, y)
y_pred = model.predict(X_poly)
plt.scatter(ts, y, label="data $y$")
plt.plot(ts, f(ts), color="green", linestyle="--", label="ground truth")
plt.plot(ts, y_pred, color="red", label=f"degree {degree} fit")
plt.xlabel("$t$")
plt.ylabel("$y$")
plt.legend()
plt.show()
```

In [TP1](), we will see what's under the hood of such tools and implement the solution from scratch. 

::::{note} How to write the polynomial model as a linear model?
**Polynomial model**
$$y_{\text{model}}(t) = x_3 t^3 + x_2 t^2 + x_1 t + x_0$$

**Measurements**
$$y_i = y_{\text{model}}(t_i) + b_i \quad \text{for known $t_1, \ldots, t_m$} $$

**Setting up the equations**
:::{math}
y = \begin{bmatrix}
    y_1\\
    y_2\\
    \vdots\\
    y_M
\end{bmatrix} = \begin{bmatrix}
    1 & t_1 & t_1^2 & t_1^3\\
    1 & t_2 & t_2^2 & t_2^3\\
    \vdots & \vdots & \vdots & \vdots\\
    1 & t_M & t_M^2 & t_M^3
\end{bmatrix}
\begin{bmatrix}
    x_0\\
    x_1\\
    x_2\\
    x_3
\end{bmatrix}
+
\begin{bmatrix}
    b_1\\
    b_2\\
    \vdots\\
    b_M
\end{bmatrix}
= Ax + b
:::
The vector ${\hat{x}} \in \argmin \Vert y - Ax\Vert_2^2$ is the least squares estimate of the polynomial coefficients in $y_m(t)$.
::::

### Deconvolution