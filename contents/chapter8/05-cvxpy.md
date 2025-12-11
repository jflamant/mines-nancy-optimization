---
kernelspec:
    name: python3
---

# Introduction to Disciplined Convex Programming with CVXPy

## What is it?
A set of rules, a **language** that permits to formulate and implement efficiently convex optimization problems.

This is the underlying language of numerical solvers, such as CVXPY (Python) or CVX (Matlab).

In this course we look at `CVXPy`, a  Python-based solver using Discplined Convex Programming (DCP)
- see [https://www.cvxpy.org/]({https://www.cvxpy.org/}) for tutorial and documentation
- see also the [lectures slides](https://web.stanford.edu/class/ee364a/lectures/cvx_lecture_slides.pdf) on Stanford's convex optimization lecture
- you can also check the JMLR paper:
[CVXPY: A Python-Embedded Modeling Language for Convex Optimization](https://www.jmlr.org/papers/volume17/15-408/15-408.pdf) by S. Diamond and S. Boyd

The key idea is that by properly formulating your problem using DCP, CVXPy will 'pick up' the appropriate solver to efficiently return a solution.
## Example from CVX tutorial
We consider solving the following problem
$$\begin{array}{ll}
\minimize& \quad (x-y)^2\\
\st & \quad x+y =1\\
&\quad x-1 \geq 1
\end{array}
$$
In CVXPy, the code reads:

```{code-cell} python

import cvxpy as cp

# Create two scalar optimization variables.
x = cp.Variable()
y = cp.Variable()

# Create two constraints.
constraints = [x + y == 1,
                x - y >= 1]

# Form objective.
obj = cp.Minimize((x - y)**2)

# Form and solve problem.
prob = cp.Problem(obj, constraints)
prob.solve(verbose=True)  # Returns the optimal value.
print("status:", prob.status)
print("optimal value", prob.value)
print("optimal var", x.value, y.value)
```

:::{exercise} another example
Solve the following problem using CVXPy
$$
\begin{split}
  \operatorname{minimize}&\quad (x_1-2)^2+(x_2-1)^2\\
  \text{subject to}&\quad x_1^2 - x_2 \leq 0\\
  &\quad x_1+x_2\leq 2
 \end{split}
$$
:::
