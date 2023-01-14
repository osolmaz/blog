---
layout: page
title:  "Lumped L2 Projection"
date: 2018-04-24
---

<div style="display:none">
  $
    \newcommand{\rowsum}{\mathop{\rm rowsum}\nolimits}
    \newcommand{\nnode}{n}
    \newcommand{\suml}[2]{\sum\limits_{#1}^{#2}}
  $
</div>

When utilizing Galerkin-type solutions for
[IBVP](https://en.wikipedia.org/wiki/Boundary_value_problem)s, we often
have to compute integrals using numerical methods such as
[Gauss quadrature](https://en.wikipedia.org/wiki/Gaussian_quadrature). In
such a solution, we solve for the values of a function at *mesh nodes*, whereas
the integration takes place at the *quadrature points*. Depending on the case,
we may need to compute the values of a function at mesh nodes, given their
values at quadrature points, e.g. stress recovery for mechanical problems.

There are many ways of achieving this, such as
[superconvergent patch recovery](https://www.sciencedirect.com/science/article/pii/004578259290023D).
In this post, I wanted to document a widely used solution which is easy to
implement, and which is used in research oriented codebases such as
[FEAP](http://projects.ce.berkeley.edu/feap/).

# L2 Projection

Given a function $u \in L^2(\Omega)$, its projection into a finite element space
$V_h\subset L^2(\Omega)$ is defined through the following optimization
problem:

Find $u_h\in V_h$ such that

$$\begin{equation}
  \Pi(u_h) := \frac{1}{2}\lVert u_h-u \rVert^2_{L^2(\Omega)}
  \quad\rightarrow\quad
  \text{min}
\end{equation}$$

There is a unique solution to the problem since $\Pi(\cdot)$ is convex. Taking
its variation, we have
$$\begin{equation}
  D \Pi(u_h) \cdot v_h = \langle u_h-u, v_h \rangle = 0
\end{equation}$$

for all $v_h\in V_h$. Thus we have the following variational formulation

> Find $u_h\in V_h$ such that
>
> $$\begin{equation}
>   \langle u_h,v_h\rangle = \langle u, v_h\rangle
> \end{equation}$$
>
> for all $v_h\in V_h$.

Here,

$$\begin{equation}
  \begin{alignedat}{3}
    m(u_h,v_h) &= \langle u_h,v_h\rangle && = \int_\Omega u_hv_h \,dx \quad\text{and} \\
    b(v_h) &= \langle u, v_h\rangle  && = \int_\Omega u v_h \,dx
  \end{alignedat}
\end{equation}$$


are our bilinear and linear forms respectively. Substituting FE
discretizations $u_h = \sum_{J=1}^{\nnode} u^JN^J$ and
$v_h = \sum_{I=1}^{\nnode} v^IN^I$, we have

$$\begin{equation}
  \suml{J=1}{\nnode} M^{I\!J} u^J = b^I
  \label{eq:projectionsystem1}
\end{equation}$$

for $I=1,\dots,\nnode$,
where the FE matrix and vector are defined as

$$\begin{equation}
  \begin{alignedat}{3}
    M^{I\!J} &= m(N^J,N^I) &&= \int_\Omega N^JN^I \,dx \quad\text{and} \\
    b^{I} &= b(N^I) &&= \int_\Omega u N^I \,dx
  \end{alignedat}
\end{equation}$$

Thus L2 projection requires the solution of a linear system

$$\boldsymbol{M}\boldsymbol{u}=\boldsymbol{b}$$

which depending on the algorithm used, can have a complexity of at least
$O(n^2)$ and at most $O(n^3)$.

# Lumped L2 Projection

The L2 projection requires the solution of a system which can be
computationally expensive. It is possible to convert the
matrix---called the mass matrix in literature---to a diagonal
form through a procedure called **lumping**.

The operator for row summation is defined as

$$\begin{equation}
  \rowsum{(\cdot)}_i := \suml{j=1}{\nnode} (\cdot)_{ij}
\end{equation}$$

For the mass matrix, we have

$$\begin{equation}
  \rowsum M^{I} =
  \suml{J=1}{\nnode} \int_\Omega N^JN^I \,dx
  = \int_\Omega N^I \,dx
  =: m^I
\end{equation}$$

since $\sum_{J=1}^{\nnode} N^J = 1$.
Substituting the lumped mass matrix allows us to decouple the linear system of
equations in \eqref{eq:projectionsystem1} and instead write

$$\begin{equation}
  m^I u^I = b^I
\end{equation}$$

for $I=1,\dots,\nnode$. The lumped L2 projection is then as simple as

$$\begin{equation}
  u^I = \frac{b^I}{m^I} = \frac{\displaystyle\int_\Omega u N^I\,dx}{\displaystyle\int_\Omega N^I \,dx}
\end{equation}$$

This results in a very efficient algorithm with $O(n)$ complexity.

# Conclusion

Lumped L2 projection is a faster working approximation to L2 projection that is
easy to implement for quick results. You can use it when developing a solution
for an IBVP, and don't want to wait too long when debugging, while not
forgetting that it introduces some error.

