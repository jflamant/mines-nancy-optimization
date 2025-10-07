---
kernelspec:
    name: python3
---
# First example: gradient-free optimization and the Nelder-Mead algorithm

A first example that relies only on being able to evaluate $f_0(x)$ is the Nelder-Mead algorithm.


## General idea
The general idea of the algorithm is summarized below.

:::{prf:algorithm} Nelder-Mead
For an objective function $f_0: \mathbb{R}^n \to \mathbb{R}$
1. Randomly select $n+1$ points $x_1, x_2, \ldots, x_{n+1}$ in $\mathbb{R}^n$.
2. Construct the associated simplex (a convex polytope with $N+1$ vertices: a triangle in 2D, a tetrahedron in 3D, etc.).
3. Evaluate $f$ at each vertex.
4. Transform the simplex to move it toward a (local) minimizer using operations such as reflection, expansion, contraction, and shrinkage.
5. Repeat steps 3-4 until the simplex becomes sufficiently small.
:::
The [Wikipedia page](https://en.wikipedia.org/wiki/Nelder%E2%80%93Mead_method) describes in more details the operations performed on the simplex and in which situation they occur.


## Examples
The following python code implements and demonstrate the Nelder mead algorithm on the [Himmelblau test function](https://en.wikipedia.org/wiki/Himmelblau%27s_function) has been adapted from [Wikimedia](https://commons.wikimedia.org/wiki/File:Nelder-Mead_Himmelblau.gif
) [^1]

[^1]: Nicoguaro, CC BY 4.0 <https://creativecommons.org/licenses/by/4.0>, via Wikimedia Commons


:::{code-cell} python
:tags:[hide-input]
"""
Animation of the Nelder-Mead method for the Himmelblau function.
"""
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from IPython.display import HTML


def himmel(x):
    if len(x.shape) == 1:
        x1, x2, = x
    else:
        x1 = x[:, 0]
        x2 = x[:, 1]
    return (x1**2 + x2 - 11)**2 + (x1 + x2**2 - 7)**2

def nelder_mead_step(fun, verts, alpha=1, gamma=2, rho=0.5,
                     sigma=0.5):
    """Nelder-Mead iteration according to Wikipedia _[1]


    References
    ----------
     .. [1] Wikipedia contributors. "Nelder–Mead method." Wikipedia,
         The Free Encyclopedia. Wikipedia, The Free Encyclopedia,
         1 Sep. 2016. Web. 20 Sep. 2016.
    """
    nverts, _ = verts.shape
    f = fun(verts)
    # 1. Order
    order = np.argsort(f)
    verts = verts[order, :]
    f = f[order]
    # 2. Calculate xo, the centroid"
    xo = verts[:-1, :].mean(axis=0)
    # 3. Reflection
    xr = xo + alpha*(xo - verts[-1, :])
    fr = fun(xr)
    if f[0]<=fr and fr<f[1]:
        new_verts = np.vstack((verts[:-1, :], xr))
    # 4. Expansion
    elif fr<f[0]:
        xe = xo + gamma*(xr - xo)
        fe = fun(xe)
        if fe < fr:
            new_verts = np.vstack((verts[:-1, :], xe))
        else:
            new_verts = np.vstack((verts[:-1, :], xe))
    # 5. Contraction
    else:
        xc = xo + rho*(verts[-1, :] - xo)
        fc = fun(xc)
        if fc < f[-1]:
            new_verts = np.vstack((verts[:-1, :], xc))
    # 6. Shrink
        else:
            new_verts = np.zeros_like(verts)
            new_verts[0, :] = verts[0, :]
            for k in range(1, nverts):
                new_verts[k, :] = sigma*(verts[k,:] - verts[0,:])

    return new_verts

# Contour data
npts = 201
x, y = np.mgrid[-6:6:npts*1j, -6:6:npts*1j]
x.shape = (npts**2)
y.shape = (npts**2)
z = himmel(np.column_stack((x, y)))
x.shape = (npts, npts)
y.shape = (npts, npts)
z.shape = (npts, npts)

# Nelder-Mead animation setup

fig, ax = plt.subplots(figsize=(6,6))
levels = np.logspace(0.35, 3.2, 20)

ax.contour(x, y, z, levels=levels, cmap='viridis')
ax.set_title("Nelder-Mead on Himmelblau's function")
ax.set_xlabel('$x_1$')
ax.set_ylabel('$x_2$')

# Initial simplex
verts = np.array([[3, 6], [2, 2], [3, 1]])

simplex_lines, = ax.plot([], [], 'ro-', lw=2)
history = []

def data_gen(frame):
    global verts
    history.append(verts.copy())
    verts = nelder_mead_step(himmel, verts)
    simplex = np.vstack([verts, verts[0]])
    simplex_lines.set_data(simplex[:,0], simplex[:,1])
    return simplex_lines,

ani = animation.FuncAnimation(fig, data_gen, 20, blit=False)
plt.close()
HTML(ani.to_jshtml())
:::
For this choice of initial points, the Nelder-Mead algorithm converges to one of the four stationary points of the Himmelblau function.

Let us choose different initial points.

:::{code-cell} python
:tags:[hide-input]
# Contour data
npts = 201
x, y = np.mgrid[-6:6:npts*1j, -6:6:npts*1j]
x.shape = (npts**2)
y.shape = (npts**2)
z = himmel(np.column_stack((x, y)))
x.shape = (npts, npts)
y.shape = (npts, npts)
z.shape = (npts, npts)

# Nelder-Mead animation setup

fig, ax = plt.subplots(figsize=(6,6))
levels = np.logspace(0.35, 3.2, 20)

ax.contour(x, y, z, levels=levels, cmap='viridis')
ax.set_title("Nelder-Mead on Himmelblau's function")
ax.set_xlabel('$x_1$')
ax.set_ylabel('$x_2$')

# Initial simplex
verts = np.array([[-3, -4], [-2, -2], [-3, -1]])

simplex_lines, = ax.plot([], [], 'ro-', lw=2)
history = []

def data_gen(frame):
    global verts
    history.append(verts.copy())
    verts = nelder_mead_step(himmel, verts)
    simplex = np.vstack([verts, verts[0]])
    simplex_lines.set_data(simplex[:,0], simplex[:,1])
    return simplex_lines,

ani = animation.FuncAnimation(fig, data_gen, 20, blit=False)
plt.close()
HTML(ani.to_jshtml())
:::

The algorithm now converges to a different local optima! Last example:

:::{code-cell}
:tags:[hide-input]
# Contour data
npts = 201
x, y = np.mgrid[-6:6:npts*1j, -6:6:npts*1j]
x.shape = (npts**2)
y.shape = (npts**2)
z = himmel(np.column_stack((x, y)))
x.shape = (npts, npts)
y.shape = (npts, npts)
z.shape = (npts, npts)

# Nelder-Mead animation setup

fig, ax = plt.subplots(figsize=(6,6))
levels = np.logspace(0.35, 3.2, 20)

ax.contour(x, y, z, levels=levels, cmap='viridis')
ax.set_title("Nelder-Mead on Himmelblau's function")
ax.set_xlabel('$x_1$')
ax.set_ylabel('$x_2$')

# Initial simplex
verts = np.array([[-3, -4], [2, 2], [3, 1]])

simplex_lines, = ax.plot([], [], 'ro-', lw=2)
history = []

def data_gen(frame):
    global verts
    history.append(verts.copy())
    verts = nelder_mead_step(himmel, verts)
    simplex = np.vstack([verts, verts[0]])
    simplex_lines.set_data(simplex[:,0], simplex[:,1])
    return simplex_lines,

ani = animation.FuncAnimation(fig, data_gen, 20, blit=False)
plt.close()
HTML(ani.to_jshtml())
:::

In this last case, the Nelder-Mead algorithm gets trapped in the valley surrounding local optima, and does not converge. This is one of the limitations of the algorithm: despite its simplicity, little can be said about its convergence properties (in the sense that it is not possible to guarantee that it will be able to find a local optima of the minimisation problem).

In the reminder of the chapter, we'll turn our attention to **descent methods**, i.e., methods that exploit first-order (gradient of $f_0$) and second-order properties (Hessian of $f_0$) to construct sequences that iteratively decrease the value of the cost function $f_0$.
