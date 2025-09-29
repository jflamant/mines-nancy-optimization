# Back to least squares: existence, uniqueness, and expression of solutions

Given $y \in \mathbb{R}^m$ and $A\in \mathbb{R}^{m\times n}$ a general linear least squares problem is written as
:::{math}
\begin{array}{ll}
\minimize&  \Vert y - A x\Vert_2^2\\
\st & x \in \mathbb{R}^n
\end{array}
:::
[As we already saw](03-general-form-LS.md), this is an unconstrained quadratic program with $Q = A^\top A$ and $p = A^\top y$.


Setting the gradient to zero yields the so-called **normal equations**:
$$A^\top Ax =  A^\top y$$

## Existence of solutions
A few observations are in order:
- we have $Q = A^\top A$, which shows that $Q \succeq 0$. 
- we have in addition $p = A^\top y \in \operatorname{Im} Q$. To see this, observe that $\operatorname{Im} A^T A = \operatorname{Im} A^\top$, since $\ker A= \ker A^\top A$ and $\ker A = (\operatorname{Im}A^\top)^\perp$. 


Comparing with all possible cases for QPs, we only have two choices:
- either $Q \succ 0$, and there is a unique solution; 
- either $Q \succeq 0$ and $\lambda_{\min}(Q) = 0$. Since $p\in \operatorname{Im}Q$, there is an infinite number of solutions. 

:::{important} 
Linear least square problems admit at least one solution.
:::
## Typology of solutions

The rank of $A$ determines the number of solutions:
- If $\rank A = n$. This means that $A$ is full column rank ($n$ linearly independent columns). 
Then $\rank A^\top A = n$ and thus $Q = A^\top A$ is invertible. 
The solution is unique and reads
$$\hat{x} = Q^{-1}p =  \left(A^\top A\right)^{-1}A^\top y$$
This case is also referred to as the **overdetermined** case ($m \geq n$).
- If $\rank A < n$, the matrix $A^\top A$ is singular and there are infinitely many solutions of the form
$$\hat{x} = x_0 + \mathbf{n}, \quad \text{where } Ax_0 = y,\, \mathbf{n} \in \ker A$$
This case is referred to as the **underdetermined** case.

## About the Moore-Penrose pseudo-inverse
Consider a linear system of equations $y = Ax$. Often the solution to this system is denoted by
$$\hat{x} = A^\dagger y$$
where $A^\dagger$ is **the Moore-Penrose pseudo-inverse** of $A$. While it relates to least squares problems, they are some key subtleties to keep in mind:
- When $\rank A = n$ (and thus $m \geq n$), the pseudo-inverse is defined as
$$A^\dagger = \left(A^\top A\right)^{-1} A^\top$$
which matches the solution to the least squares problem with objective $\Vert y - Ax\Vert_2^2$.
- When $\operatorname{rank}(A) = m < n$ (underdetermined), $A^\dagger$ gives the minimum-norm solution to the system of equations $y = Ax$. Its expression differs and will be discussed in later lectures.

:::{prf:property} Properties of the pseudo-inverse when $\operatorname{rank}(A) = n$
- $A^\dagger$ is a left inverse: $A^\dagger A = I_n$
- If $m = n$ and $A$ is invertible, then $A^\dagger = A^{-1}$
:::