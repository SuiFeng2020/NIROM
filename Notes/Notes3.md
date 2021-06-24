# Shifted boundary finite element method

# Abstract

We propose a model order reduction technique integrating the Shifted Boundary Method (SBM) with a POD-Galerkin strategy. This approach allows to deal with complex parametrized domains in an efficient and straightforward way. The impact of the proposed approach is threefold. 

First, problems involving parametrizations of complex geometrical shapes and/or large domain deformations can be efficiently solved at full-order by means of the SBM. This unfitted boundary method permits to avoid remeshing and the tedious handling of cut cells by introducing an approximate surrogate boundary.
Second, the computational effort is reduced by the development of a Reduced Order Model (ROM) technique based on a POD-Galerkin approach.
Third, the SBM provides a smooth mapping from the true to the surrogate domain, and for this reason, the stability and performance of the reduced order basis are enhanced. This feature is the net result of the combination of the proposed ROM approach and the SBM. Similarly, the combination of the SBM with a projection-based ROM gives the great advantage of an easy and fast to implement algorithm considering geometrical parametrization with large deformations. The transformation of each geometry to a reference geometry (morphing) is in fact not required.
These combined advantages will allow the solution of PDE problems more efficiently. We illustrate the performance of this approach on a number of two-dimensional Stokes flow problems.

## Full-order parametrized Shifted Boundary Method formulation

![](D:\Data\GitHub\NIROM\Notes\figure\7.png)

|         notation         |                           meaning                            |
| :----------------------: | :----------------------------------------------------------: |
|      $\mathcal{D}$       |           the original(true) computational domain            |
|  $\tilde{\mathcal{D}}$   |                     the surrogate domain                     |
|         $\Gamma$         |                      the true boundary                       |
|     $\tilde{\Gamma}$     |                    the surrogate boundary                    |
|     $\boldsymbol{n}$     |       the outward-pointing normal of  $\tilde{\Gamma}$       |
| $\tilde{\boldsymbol{n}}$ | the unit outward-pointing normal to the surrogate boundary $\tilde{\Gamma}$ |

The closest-point projection, in spite of the segmented/faceted nature of the surrogate boundary $\tilde{\Gamma}$, is actually a continuous, piecewise-smooth mapping $\boldsymbol{M}$ from points in $\tilde{\Gamma}$ to points in $\Gamma:$
$$
\boldsymbol{M}: \tilde{\boldsymbol{x}} \in \tilde{\Gamma} \rightarrow \boldsymbol{x} \in \Gamma .
$$
The mapping $\boldsymbol{M}$ defines a distance vector function
$$
\boldsymbol{d} \equiv d_{M}(\boldsymbol{x})=\boldsymbol{x}-\tilde{\boldsymbol{x}}=[M-I](\tilde{\boldsymbol{x}})
$$
where the distance vector $\boldsymbol{d}=\|\boldsymbol{d}\| \boldsymbol{n}$ is aligned along $\boldsymbol{n}$, due to the closest point projection properties, see Fig. 1 Since the distance vector $\boldsymbol{d}$ is aligned with the normal to the true surface $\Gamma$ and the true surface is smooth between edges and corners, it is legitimate to assume that $\boldsymbol{M}$ is continuous, and piecewise smooth. The unit normal vector $\boldsymbol{n}$ and the unit tangential vectors $\boldsymbol{\tau}_{i}(1<i<d-1)$ to the boundary $\Gamma$ can be easily extended to the boundary $\tilde{\Gamma}$ since $\overline{\boldsymbol{n}}(\tilde{\boldsymbol{x}}) \equiv \boldsymbol{n}(\boldsymbol{M}(\tilde{\boldsymbol{x}})), \overline{\boldsymbol{\tau}}_{i}(\boldsymbol{x}) \equiv \boldsymbol{\tau}_{i}(\boldsymbol{M}(\tilde{\boldsymbol{x}})) .$ In the following, whenever we write $\boldsymbol{n}(\tilde{\boldsymbol{x}})$ we actually mean $\overline{\boldsymbol{n}}(\tilde{\boldsymbol{x}})$ at a point $\tilde{\boldsymbol{x}} \in \tilde{\Gamma}$, and similarly for $\boldsymbol{\tau}_{i}(\tilde{\boldsymbol{x}})$ and $\overline{\boldsymbol{\tau}}_{i}(\tilde{\boldsymbol{x}})$. We can also introduce the derivative of a function $\boldsymbol{g}$ along the direction $\overline{\boldsymbol{\tau}}_{\boldsymbol{i}}$ at a point $\tilde{\boldsymbol{x}} \in \tilde{\Gamma}$ as $\nabla_{\overline{\boldsymbol{\tau}}_{i}} \overline{\boldsymbol{g}}=\nabla \overline{\boldsymbol{g}}(\tilde{\boldsymbol{x}}) \cdot \overline{\boldsymbol{\tau}}_{i}(\tilde{\boldsymbol{x}})$. The above constructions are the key ingredients in the extension of the boundary conditions on $\Gamma$ to the boundary $\tilde{\Gamma}$ of the surrogate domain.

These equations yield the following algebraic system of equations:
$$
\left[\begin{array}{cc}
A(\mu) & \boldsymbol{B}^{T}(\mu) \\
\boldsymbol{B}(\mu)+\hat{\boldsymbol{B}}(\mu) & \boldsymbol{C}(\mu)
\end{array}\right]\left[\begin{array}{c}
\boldsymbol{u}(\mu) \\
p(\mu)
\end{array}\right]=\left[\begin{array}{c}
\boldsymbol{F}_{g}(\mu) \\
\boldsymbol{F}_{q}(\mu)
\end{array}\right]
$$
In the above system of equations it is important to highlight several factors that will play a crucial role in reduced order model with a POD-Galerkin method . First of all, the discretized differential operators $A, B$ and $\hat{B}$ are parameter dependent. Secondly, in the typical saddle point structure of the Stokes problem, the incompressibility equation is partially relaxed adding a stabilization term $\boldsymbol{C}$. This stabilization term, at full-order level, permits to circumvent the fulfillment of the "inf-sup" condition and the use of otherwise unstable pair of finite elements, such as $\mathbb{P}_{1}-\mathbb{P}_{1}$, and, as reported in Section 3 , it helps to partially preserve the stability of the reduced order model. In principle, the SBM method can also be combined with"inf-sup"-stable elements, such as the Taylor-Hood element: the $\mathbb{P}_{1}-\mathbb{P}_{1}$ was chosen for its simplicity.

The presented formulation is used to solve the full-order problem during an offline stage and to produce the snapshots necessary for the construction of the ROM.

## References

Karatzas E N, Stabile G, Nouveau L, et al. A reduced basis approach for PDEs on parametrized geometries based on the shifted boundary finite element method and application to a Stokes flow[J]. Computer Methods in Applied Mechanics and Engineering, 2019, 347: 568-587.