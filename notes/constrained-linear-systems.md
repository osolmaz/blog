---
layout: page
title:  "Solving Constrained Linear Systems"
date: 2016-04-20
---

{% include shortsym.html %}
{% include lazyeqn.html %}

Constrained linear systems arise when Dirichlet boundary conditions are imposed
on a variational formulation

> Find $u\in U$ such that
>
> $$\begin{equation}
>   a(u, v) = b(v)
> \end{equation}$$
>
> for all $v \in V$, where
>
> $$\begin{equation}
>   \begin{aligned}
>     U &= \{u\mid u\in H^1(\Omega), u=\bar{u} \text{ on } \del\Omega_u\} \\
>     V &= \{u\mid u\in H^1(\Omega), u=0 \text{ on } \del\Omega_u\} \\
>   \end{aligned}
> \end{equation}$$
>
> where $\bar{u}$ is the Dirichlet condition.


We additively decompose the solution into known and unknown parts:

$$\begin{equation}
  u = \bar{u} + w
\end{equation}$$

and substitute into our variational formulation

$$\begin{equation}
  a(\bar{u}+w, v) = b(v)
\end{equation}$$

We can take advantage of the linearity condition, and reformulate the
variational formulation:

> Find $w\in V$ such that
>
> $$\begin{equation}
>   a(w, v) = b(v) - a(\bar{u}, v)
> \end{equation}$$
>
> for all $v \in V$.


The algorithmic analogue of this formulation will be developed in the following
section Direct Modification Approach.

# Static Condensation Approach

For a linear system

$$\begin{equation}
  \BA \Bu = \Bb\,,
  \label{eq:system1}
\end{equation}$$

of size $N\times N$, we constrain the values of the solution or right-hand side
at certain degrees of freedom.
We sort the system so that these degrees of freedom are grouped together after the
unconstrained degrees of freedom. The resulting system is,

$$\begin{equation}
  \label{eq:bcsystem1}
  \left[
    \begin{array}{ccc|ccc}
      A_{1,1} & \cdots & A_{1,M} & A_{1,M+1} & \cdots & A_{1,N} \\
      \vdots & \ddots & \vdots & \vdots & \ddots & \vdots \\
      A_{M,1} & \cdots & A_{M,M} & A_{M,M+1} & \cdots & A_{M,N} \\ \hline
      A_{M+1,1} & \cdots & A_{M+1,M} & A_{M+1,M+1} & \cdots & A_{M+1,N} \\
      \vdots & \ddots & \vdots & \vdots & \ddots & \vdots\\
      A_{N,1} & \cdots & A_{N,M}  & A_{N, M+1} &\cdots & A_{N,N} \\
    \end{array}
  \right]
  \left[
    \begin{array}{c}
      u_{1} \\
      \vdots \\
      u_{M} \\ \hline
      u_{M+1} \\
      \vdots \\
      u_{N} \\
    \end{array}
  \right]
  =
  \left[
    \begin{array}{c}
      b_{1} \\
      \vdots \\
      b_{M} \\ \hline
      b_{M+1} \\
      \vdots \\
      b_{N} \\
    \end{array}
  \right]\,,
\end{equation}$$

Defining submatrices
and vectors for the partitions, we can write

$$\begin{equation}
  \begin{bmatrix}
    \BA_{11}& \BA_{12} \\
    \BA_{21}& \BA_{22} \\
  \end{bmatrix}
  \begin{bmatrix}
    \Bu_{1} \\
    \Bu_{2} \\
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Bb_{1} \\
    \Bb_{2} \\
  \end{bmatrix}
\end{equation}$$

or

$$\begin{equation}
  \begin{aligned}
    \BA_{11} \Bu_{1} + \BA_{12} \Bu_{2} &= \Bb_{1} \\
    \BA_{21} \Bu_{1} + \BA_{22} \Bu_{2} &= \Bb_{2}\,,
  \end{aligned}
\end{equation}$$

Let $\Bu_2 = \bar{\Bu}$ and $\Bb_1 = \bar{\Bb}$ have defined values. The objective
is to solve for unknown $\Bu_1$ and $\Bb_2$. We have

$$\begin{equation}
  \Bu_1 = \BA_{11}\inv (\Bb_1 - \BA_{12}\Bu_2)
  \label{eq:u1staticcond1}
\end{equation}$$

and

\begin{align}
  \Bb_2 = (\BA_{22}-\BA_{21}\BA_{11}\inv\BA_{12})\Bu_2 +  \BA_{21}\BA_{11}\inv\Bb_1
\end{align}

In case $\bar{\Bu} = \Bzero$, we have

$$\begin{equation}
  \begin{aligned}
    \Bu_1 &= \BA_{11}\inv\Bb_1 \\
    \Bb_2 &= \BA_{21}\BA_{11}\inv\Bb_1
  \end{aligned}
\end{equation}$$

and in case $\bar{\Bb} = \Bzero$, we have

$$\begin{equation}
  \begin{aligned}
    \Bu_1 &= -\BA_{11}\inv\BA_{12}\Bu_2 \\
    \Bb_2 &= (\BA_{22}-\BA_{21}\BA_{11}\inv\BA_{12})\Bu_2
  \end{aligned}
\end{equation}$$

## Example: Plane Stress and Strain in Linear Elasticity
The constitutive equation of isotropic linear elasticity reads

$$\begin{equation}
  \begin{bmatrix}
    \lambda+2\mu & \lambda & \lambda & 0 & 0 & 0 \\
    \lambda & \lambda+2\mu & \lambda & 0 & 0 & 0 \\
    \lambda & \lambda & \lambda+2\mu & 0 & 0 & 0 \\
    0 & 0 & 0 & \mu & 0 & 0 \\
    0 & 0 & 0 & 0 & \mu & 0 \\
    0 & 0 & 0 & 0 & 0 & \mu \\
  \end{bmatrix}
  \begin{bmatrix}
    \varepsilon_{11}\\
    \varepsilon_{22}\\
    \varepsilon_{33}\\
    \varepsilon_{12}\\
    \varepsilon_{23}\\
    \varepsilon_{13}\\
  \end{bmatrix}
  =
  \begin{bmatrix}
    \sigma_{11}\\
    \sigma_{22}\\
    \sigma_{33}\\
    \sigma_{12}\\
    \sigma_{23}\\
    \sigma_{13}\\
  \end{bmatrix}
\end{equation}$$

The plane stress condition reads $\sigma_{13} = \sigma_{23}= \sigma_{33} = 0$. We
group the constrained degrees of freedom together:

$$\begin{equation}
  \left[
  \begin{array}{ccc|ccc}
    \lambda+2\mu & \lambda & 0 & \lambda & 0 & 0 \\
    \lambda & \lambda+2\mu & 0 & \lambda & 0 & 0 \\
    0 & 0 & \mu & 0 & 0 & 0 \\
    \hline
    \lambda & \lambda & 0 & \lambda+2\mu & 0 & 0 \\
    0 & 0 & 0 & 0 & \mu & 0 \\
    0 & 0 & 0 & 0 & 0 & \mu \\
  \end{array}
  \right]
  \left[
  \begin{array}{c}
    \varepsilon_{11}\\
    \varepsilon_{22}\\
    \varepsilon_{12}\\
    \hline
    \varepsilon_{33}\\
    \varepsilon_{23}\\
    \varepsilon_{13}\\
  \end{array}
  \right]
  =
  \left[
  \begin{array}{c}
    \sigma_{11}\\
    \sigma_{22}\\
    \sigma_{12}\\
    \hline
    \sigma_{33}\\
    \sigma_{23}\\
    \sigma_{13}\\
  \end{array}
  \right]
\end{equation}$$

which we write as

$$\begin{equation}
  \begin{bmatrix}
  \BC_{11}&\BC_{12}\\
  \BC_{21}&\BC_{22}\\
  \end{bmatrix}
  \begin{bmatrix}
  \Bvarepsilon\\
  \Bvarepsilon'\\
  \end{bmatrix}
  =
  \begin{bmatrix}
  \Bsigma\\
  \Bsigma'\\
  \end{bmatrix}
\end{equation}$$

The purpose is to
obtain a reduced system without $\Bsigma'$ or $\Bvarepsilon'$.
We substitute the plane stress condition $\Bsigma'=\Bzero$, to obtain
$\Bvarepsilon'=-\BC_{22}\inv\BC_{21}\Bvarepsilon$. Then we have

$$\begin{equation}
  (\BC_{11}-\BC_{12}\BC_{22}\inv\BC_{21}) \Bvarepsilon = \Bsigma
\end{equation}$$

We define the plane stress version of the elasticity tensor as
$\BC_\sigma = \BC_{11}-\BC_{12}\BC_{22}\inv\BC_{21}$ which results in

$$\begin{equation}
  \BC_\sigma =
  \frac{\mu}{\lambda+2\mu}
  \begin{bmatrix}
    4(\lambda+\mu) &
    2\lambda & 0 \\
    2\lambda &
    4(\lambda+\mu) & 0 \\
    0 & 0 & \lambda+2\mu
  \end{bmatrix}
  =
  \frac{E}{1-\nu^2}
  \begin{bmatrix}
    1 & \nu & 0\\
    \nu & 1 & 0\\
    0& 0& (1-\nu)/2
  \end{bmatrix}
\end{equation}$$

The plane strain condition reads $\Bvarepsilon'=\Bzero$. This simply results in

$$\begin{equation}
  \BC_{11}\Bvarepsilon = \Bsigma
\end{equation}$$

The plane strain version of the elasticity tensor $\BC_\varepsilon=\BC_{11}$
is calculated as

$$\begin{equation}
  \BC_\varepsilon =
  \begin{bmatrix}
    \lambda + 2\mu & \lambda & 0 \\
    \lambda  & \lambda + 2\mu & 0 \\
    0 & 0 & \mu \\
  \end{bmatrix}
  =
  \frac{E}{(1+\nu)(1-2\nu)}
  \begin{bmatrix}
    1-\nu & \nu & 0\\
    \nu & 1-\nu & 0\\
    0& 0& (1-2\nu)/2
  \end{bmatrix}
\end{equation}$$

The procedure defined above is called *static condensation*,
named after its application in structural analysis. One impracticality of
this formulation is that systems do not always exist with their constrained
degrees of freedom grouped together. These are generally
scattered arbitrarily throughout the solution vector, and grouping them manually
is impractical with current data structure implementations.


# Direct Modification Approach

Suppose we have a system where $\Bu_2$ and $\Bb_1$ are known and $\Bu_1$
and $\Bb_2$ are unknown:

$$\begin{equation}
  \begin{bmatrix}
    \BA_{11}&\BA_{12}\\
    \BA_{21}&\BA_{22}\\
  \end{bmatrix}
  \begin{bmatrix}
    \Bu_1\\
    \Bu_2\\
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Bb_1\\
    \Bb_2\\
  \end{bmatrix}
\end{equation}$$

We can modify the system so that
it can be solved without separating the partitions

$$\begin{equation}
  \begin{bmatrix}
    \BA_{11}&\BA_{12}\\
    \Bzero &\BI\\
  \end{bmatrix}
  \begin{bmatrix}
    \Bu_1\\
    \Bu_2\\
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Bb_1\\
    \Bu_2\\
  \end{bmatrix}
\end{equation}$$

We can additively decompose both sides

$$\begin{equation}
  \begin{bmatrix}
    \BA_{11}&\Bzero\\
    \Bzero &\BI\\
  \end{bmatrix}
  \begin{bmatrix}
    \Bu_1\\
    \Bu_2\\
  \end{bmatrix}
  +
  \begin{bmatrix}
    \Bzero&\BA_{12}\\
    \Bzero &\Bzero\\
  \end{bmatrix}
  \begin{bmatrix}
    \Bu_1\\
    \Bu_2\\
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Bb_1\\
    \Bu_2\\
  \end{bmatrix}
\end{equation}$$

Therefore, the following is equivalent to
\eqref{eq:u1staticcond1}:

$$\begin{equation}
  \underbrace{
    \begin{bmatrix}
      \BA_{11}&\Bzero\\
      \Bzero &\BI\\
    \end{bmatrix}
  }_{\tilde{\BA}}
  \begin{bmatrix}
    \Bu_1\\
    \Bu_2\\
  \end{bmatrix}
  =
  \underbrace{
    \begin{bmatrix}
      \Bb_1 - \BA_{12}\Bu_2\\
      \Bu_2\\
    \end{bmatrix}
  }_{\tilde{\Bb}}
\end{equation}$$

This is solved for $\Bu$:

$$\begin{equation}
  \Bu = \tilde{\BA}\inv \tilde{\Bb}\,.
\end{equation}$$

The unknown right hand side can be obtained from the original matrix $\BA$

$$\begin{equation}
  \Bb = \BA\Bu = \BA \tilde{\BA}\inv \tilde{\Bb}\,,
\end{equation}$$

Observe that the modifications on $\BA$ are symmetric, so we do not need
the constrained degrees of freedom be grouped together. $\tilde{\BA}$ is
obtained by zeroing out the rows and columns corresponding to constraints and
setting the diagonal components to one. For $\tilde{\Bb}$, we do not need to
extract $\BA_{12}$; we simply let

$$\begin{equation}
  \quad \tilde{\Bb}
  \leftarrow
  \Bb_k - \BA\Bu_k
\end{equation}$$

where

$$\begin{equation}
  \Bu_k =
  \begin{bmatrix}
    \Bzero\\
    \Bu_2\\
  \end{bmatrix}

  \eqand

  \Bb_k =
  \begin{bmatrix}
    \Bb_1\\
    \Bzero\\
  \end{bmatrix}
\end{equation}$$

We then equate the constrained degrees of freedom to their specified values $\Bu_2$.

Below is a pseudocode outlining the algorithm.

```
fun solve_constrained_system(A, b_known, u_known, is_contrained):
   # A: unmodified matrix, size NxN
   # b_known: known values of the rhs, size N
   # u_known: known values of the solution, size N
   # is_constrained: bool array whether dof is constrained, size N

   N = length(b)
   A_mod = copy(A)
   b_mod = b_known - A_known*u_known # Calculate rhs vector

   for i=1 to N do:
       if is_constained[i] then:
           for j = 1 to N do:
               A_mod[i][j] = 0 # Set row to zero
               A_mod[j][i] = 0 # Set column to zero
           endfor
           A_mod[i][i] = 1 # Set diagonal to one
           b_mod[i] = u_known[i]
       endif
    endfor

    u = inverse(A_mod)*b_mod # Solve constrained system
    # Could also say solve(A_mod, b_mod)
    b = A*u # Substitute solution to get final rhs vector

    return u, b
endfun
```


# Constrained Update Schemes

When using an iterative solution approach, one generally has an update equation
of the form

$$\begin{equation}
  \Bu \leftarrow \bar{\Bu} + \Var\Bu
  \quad\text{where}\quad
  \BA \Var\Bu = \Bb
\end{equation}$$

where $\Bu$ is the solution vector of the primary unknown. The update vector $\Var\Bu$ is
obtained by solving a linear system and added to the solution vector in each
iteration. This process is usually terminated when the approximation error drops below a
threshold value.

\\When the solution vector itself is constrained, the update system needs to be
modified accordingly. Grouping the constrained degrees of freedom together,

$$\begin{equation}
  \begin{bmatrix}
    \Bu_1\\
    \Bu_2\\
  \end{bmatrix}
  \leftarrow
  \begin{bmatrix}
    \bar{\Bu}_1\\
    \bar{\Bu}_2\\
  \end{bmatrix}
  +
  \begin{bmatrix}
    \Var\Bu_1\\
    \Var\Bu_2\\
  \end{bmatrix}
\end{equation}$$

Let $\Bu_2$ be known and $\Bu_1$ be unknown.

We can make the substitution $\Var\Bu_2=\Bu_2-\bar{\Bu}_2$:

$$\begin{equation}
  \begin{bmatrix}
    \BA_{11}&\BA_{22}\\
    \BA_{21} &\BA_{22}\\
  \end{bmatrix}
  \begin{bmatrix}
    \Var\Bu_1\\
    \Bu_2-\bar{\Bu_2}\\
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Bb_1\\
    \Bb_2
  \end{bmatrix}
\end{equation}$$

This system can then be solved for the unknown $\Var\Bu_1$ and $\Var\Bb_2$
with the procedure defined in the previous
section. The only difference is that,

$$\begin{equation}
  \Var\Bu_k =
  \begin{bmatrix}
    \Bzero\\
    \Bu_2-\bar{\Bu_2}
  \end{bmatrix}
\end{equation}$$
