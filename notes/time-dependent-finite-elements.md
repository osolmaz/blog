---
layout: page
title:  "Time-Dependent Finite Elements"
date: 2017-12-07
---

{% include shortsym.html %}
{% include lazyeqn.html %}

Time dependent problems are commonplace in physics, chemistry and many other
disciplines. In this post, I'll introduce the FE formulation of linear
time-dependent problems and derive formulas for explicit and implicit Euler
integration.

The weak formulation of a first order time-dependent problem reads:

> Find $u \in V$ such that
>
> $$\begin{equation}
>   m(\dot{u}, v; t) + a(u,v; t) = b(v; t)
>   \label{eq:timedependentweak1}
> \end{equation}$$
>
> for all $v \in V$ and $t \in [0,\infty)$.


We can convert \eqref{eq:timedependentweak1} into a system of equations

$$\begin{equation}
  \BM(t)\dot{\Bu} + \BA(t)\Bu = \Bb(t)
\end{equation}$$

where the components of the matrices and vectors involved are calculated as

$$\begin{equation}
  \begin{aligned}
    M^{I\!J}(t) &= m(N^J, N^I; t) \\
    A^{I\!J}(t) &= a(N^J, N^I; t) \\
    b^{I}(t) &= b(N^I; t).
  \end{aligned}
\end{equation}$$

If we further discretize in time with the finite difference
$\dot{u} \approx [u_{n+1}-u_n]/{\Delta t}$, linearity allows us to write

$$\begin{equation}
  \boxed{
    m(\dot{u}, v; t)
    \approx \frac{1}{\Delta t} [m(u_{n+1}, v; t_{n+1}) -  m(u_n, v; t_n)]
  }
  \label{eq:discretetimedependent1}
\end{equation}$$


This reflects on the system as

$$\begin{equation}
  \BM(t)\dot{\Bu} \approx
  \frac{1}{\Delta t} [\BM_{n+1}\Bu_{n+1} - \BM_n\Bu_n]
  \label{eq:discretetimedependent2}
\end{equation}$$

Here, $u_{n+1}:= u(x, t_{n+1})$,
$\BM_{n+1} = \BM(t_{n+1})$
and vice versa for $u_n$ and $\BM_n$.

## Explicit Euler Scheme

For the explicit Euler scheme, we substitute evaluate the remaining terms at $t_n$

$$\begin{equation}
  \frac{1}{\Delta t} [m(u_{n+1}, v; t_{n+1}) -  m(u_n, v; t_n)]
  + a(u_n,v; t_n) = b(v; t_n)
  \quad
  \forall v \in V\,.
\end{equation}$$

The corresponding system is

$$\begin{equation}
  \frac{1}{\Delta t} [\BM_{n+1}\Bu_{n+1} - \BM_n\Bu_n]
  + \BA_n\Bu_n = \Bb_n
\end{equation}$$

The update equation becomes

$$\begin{equation}
  \boxed{
    \Bu_{n+1} = \BM_{n+1}\inv [\BM_n\Bu_n + \Delta t(\Bb_n - \BA_n\Bu_n)]
  }
\end{equation}$$

If $m$ is time-independent, that is $m(\dot{u}, v;t) = m(\dot{u}, v)$, we have

$$\begin{equation}
  \Bu_{n+1} = \Bu_n + \Delta t\, \BM\inv(\Bb_n - \BA_n\Bu_n)
\end{equation}$$


## Implicit Euler Scheme

For the implicit Euler scheme, we substitute evaluate the remaining terms at $t_{n+1}$

$$\begin{equation}
  \frac{1}{\Delta t} [m(u_{n+1}, v; t_{n+1}) -  m(u_n, v; t_n)]
  + a(u_n,v; t_{n+1}) = b(v; t_{n+1})
  \quad
  \forall v \in V\,.
\end{equation}$$

The corresponding system is

$$\begin{equation}
  \frac{1}{\Delta t} [\BM_{n+1}\Bu_{n+1} - \BM_n\Bu_n]
  + \BA_{n+1}\Bu_{n+1} = \Bb_{n+1}
\end{equation}$$

The update equation becomes

$$\begin{equation}
  \boxed{
    \Bu_{n+1} = [\BM_{n+1}+\Delta t \BA_{n+1}]\inv [\BM_n\Bu_n + \Delta t \,\Bb_{n+1}]
  }
\end{equation}$$

If $m$ is time-independent, one can just substitute $\BM=\BM_{n+1}=\BM_n$.

## Example: Reaction-Advection-Diffusion Equation

The IBVP of a linear reaction-advection-diffusion problem reads

$$\begin{equation}
  \begin{alignedat}{4}
    \partd{u}{t} &=
    \nabla\dtp(\BD\nabla u) - \nabla\dtp(\Bc u) + ru + f
    \qquad&& \text{in} \qquad&& \Omega\times I\\
    u &= \bar{u} && \text{on} && \del\Omega\times I\\
    u &= u_0 && \text{in} && \Omega, t = 0 \\
  \end{alignedat}
\end{equation}$$

where $t\in I = [0,\infty)$,

- $\BD$ is a second-order tensor describing the diffusivity of $u$,
- $\Bc$ is a vector describing the velocity of advection,
- $r$ is a scalar describing the rate of reaction,
- and $f$ is a source term for $u$.

The weak formulation is then

> Find $u \in V$ such that
>
> $$\begin{equation}
> \int_\Omega \dot{u} v \dv =
> \int_\Omega [\nabla\dtp(\BD\nabla u) -  \nabla\dtp(\Bc u)  + ru + f] v \dv
> \end{equation}$$
>
> for all $v \in V$ and $t \in I$.

We have the following integration by parts relationships:

$$\require{cancel}\begin{equation}
  \int_\Omega \nabla \dtp(\BD\nabla u) v \dv
  = \cancel{\int_\Omega \nabla\dtp(v\BD\nabla u) \dv}
  - \int_\Omega (\BD\nabla u)\dtp\nabla v \dv
\end{equation}$$

for the diffusive part and

$$\begin{equation}
  \int_\Omega \nabla\dtp(\Bc u) v \dv
  = \cancel{\int_\Omega \nabla \dtp (\Bc u v) \dv}
  - \int_\Omega u \Bc \dtp \nabla v \dv
\end{equation}$$

for the advective part. The canceled terms are due to divergence theorem and
the fact that $v=0$ on the boundary. Then our variational formulation is of
the form \eqref{eq:timedependentweak1} where

$$\begin{align*}
  m(\dot{u}, v) &= \int_\Omega \dot{u} v \dv \\
  a(u, v) &= \int_\Omega (\BD\nabla u) \dtp \nabla v  \dv
            - \int_\Omega u\Bc \dtp \nabla v \dv
            - \int_\Omega ruv \dv \\
  b(v) &= \int_\Omega fv \dv
\end{align*}$$

From these forms, we obtain the following system matrices and vector

$$\begin{align*}
  M^{I\!J} &= \int_\Omega N^J N^I \dv \\
  A^{I\!J} &= \int_\Omega (\BD\BB^J) \dtp \BB^I \dv
             - \int_\Omega N^J\Bc \dtp \BB^I \dv
             - \int_\Omega r N^JN^I \dv \\
  b^I &= \int_\Omega f N^I \dv
\end{align*}$$

where $\BM$ is constant through time.

