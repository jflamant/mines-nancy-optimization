---
title: "Introducing the vocabulary" 
---

As illustrated by the [introductory example](./02-introductory-example.md), an optimization problem is typically formulated as the **minimization** of an **objective function** with respect to a set of **decision variables**, subject to a set of **constraints**. More generally, an arbitrary optimization problem can be written in abstract form as:

$$
\begin{array}{ll}
\text{minimize}     & f_0(x) \\
\text{subject to}   & x \in \Omega \subseteq \mathbb{X}^n
\end{array}
$$

This formulation is intentionally general and not yet operational, but it suffices to introduce the core vocabulary and nomenclature of optimization.

## Vocabulary

- $x = [x_1, x_2, \ldots, x_n]^\top \in \mathbb{X}^n$: the vector of **decision variables** (or **parameters**) over which the optimization is performed.
- $n$: the number of decision variables; also referred to as the **dimension** of the problem.
- $f_0: \mathbb{X}^n \rightarrow \mathbb{R}$: the **objective function**, also known as the **cost function** or **criterion**, which assigns a scalar value to each candidate solution.
- $\Omega \subseteq \mathbb{X}^n$: the **constraint set**, which contains all values of $x$ that satisfy the problem's constraints. In practice, $\Omega$ is often implicitly defined through **equality** and **inequality constraints**.

## Nomenclature

- The space $\mathbb{X}$ can be:
  - **Finite or discrete**: leads to *discrete* or *combinatorial optimization*
  - **Continuous**, e.g., $\mathbb{X} = \mathbb{R}$: leads to *continuous optimization*

- If there are no constraints (i.e., $\Omega = \mathbb{X}^n$), the problem is called **unconstrained optimization**.

- If constraints are present (i.e., $\Omega \subset \mathbb{X}^n$), the problem is called **constrained optimization**.

- The nature of the objective function $f_0$ defines specific problem classes:
  - $f_0$ is **linear**: *linear optimization*
  - $f_0$ is **quadratic**: *quadratic optimization*
  - $f_0$ is **convex**, and constraints are convex: *convex optimization*
  - $f_0$ may be **nonconvex**, **non-differentiable**, etc.: general nonconvex nonsmooth optimization

:::{note}
Each category of optimization problems has specialized solution methods and algorithms.
:::

## A word about discrete optimization (not covered in this course)

In **discrete optimization** (also called *combinatorial optimization*), the decision variables take values from a finite or countable set, such as $\mathbb{X} = \{0,1\}^n$ or a list of permutations. These problems arise in areas such as scheduling, routing, assignment, and network design, and often involve integer or binary variables.

Discrete optimization problems are generally **non-convex**, **combinatorially complex**, and may require specialized algorithms such as:
- branch-and-bound,
- dynamic programming,
- greedy methods, etc.

This course focuses exclusively on **continuous optimization**, where the variables belong to continuous spaces (e.g., $\mathbb{X} = \mathbb{R}^n$), and where tools from calculus and convex analysis are applicable.


