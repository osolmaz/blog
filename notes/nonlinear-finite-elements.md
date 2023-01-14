---
layout: page
title:  "Nonlinear Finite Elements"
date: 2017-12-22
---

{% include shortsym.html %}
{% include lazyeqn.html %}

This post builds on the formulations I showed in my previous posts by
introducing their nonlinear versions.

In a typical nonlinear problem, the variational setting leads to the weak formulation

> Find $u\in V$ such that
>
> $$\begin{equation}
>   F(u,v) = 0
>   \label{eq:femnonlinear2}
> \end{equation}$$
>
> for all $v\in V$ where the semilinear form
> $F$ is nonlinear in terms of $u$ and linear in terms of $v$.

We linearize $F$:

$$\begin{equation}
  \Lin [F(u,v)]_{u=\bar{u}} = F(\bar{u}, v)
  + \varn{F(u,v)}{u}{\Var u}\evat_{u=\bar{u}}
  \label{eq:femnonlinear4}
\end{equation}$$

Equating \eqref{eq:femnonlinear4} to zero yields a linear system in terms of
$\Var u$

$$\begin{equation}
  \boxed{
    a(\Var u, v) = b(v)
    }
  \label{eq:femnonlinear6}
\end{equation}$$

where

$$\begin{equation}
  \boxed{
    \begin{aligned}
      a(\Var u, v) &= \varn{F(u,v)}{u}{\Var u}\evat_{u=\bar{u}} \\
      b(v) &= -F(\bar{u}, v)
      .
    \end{aligned}
  }
  \label{eq:femnonlinear5}
\end{equation}$$


We can compute the components of the matrices and vectors according to \eqref{eq:femnonlinear6}

$$\begin{equation}
  \boxed{
    \begin{alignedat}{3}
      \Aelid{I\!J}{} &= a(N^J,N^I)
      &&= \varn{F(u,N^I)}{u}{N^J}\evat_{u=\bar{u}} \\
      b^{I} &= b(N^I)
      &&= -F(\bar{u}, N^I).
    \end{alignedat}
  }
  \label{eq:femnonlinear9}
\end{equation}$$

Then the update vector $\Var \Bu = [\Var u^1, \Var u^2, \dots, \Var
u^\nnode]\tra$ is obtained by solving

$$\begin{equation}
  \BA \Var \Bu = \Bb
\end{equation}$$

Letting $\Var u$ be the difference between consequent iterates, we obtain the
update equation as

$$\begin{equation}
  \boxed{
    \Bu \leftarrow \bar{\Bu} + \Var\Bu
  }
\end{equation}$$

## Example: Nonlinear Poisson's Equation

Consider the following nonlinear Poisson's equation

$$\begin{equation}
  \begin{alignedat}{4}
    - \nabla \dtp (g(u)\nabla u) &= f \quad && \text{in} \quad && \Omega \\
    u &= 0 \quad && \text{on} \quad && \del\Omega
  \end{alignedat}
  \label{eq:femnonlinear8}
\end{equation}$$

The weak formulation reads

> Find $u\in V$ such that
>
> $$\begin{equation}
>   - \int_\Omega \nabla \dtp (g(u)\nabla u) v \dv= \int_\Omega f v \dv
> \end{equation}$$
>
> for all $v\in V$ where $V=H^1_0(\Omega)$.

Applying integration by parts and divergence theorem on the left-hand side

$$\begin{equation}
  \begin{aligned}
    \int_\Omega \nabla \dtp (g(u)\nabla u) v \dv
    &= \int_\Omega \nabla \dtp (g(u)\nabla (u) v)  \dv
    - \int_\Omega g(u)\nabla u\dtp\nabla v  \dv \\
    &= \underbrace{\int_{\del\Omega} g(u) v (\nabla u\dtp\Bn)  \da}_{v = 0
      \text{ on } \del\Omega}
    - \int_\Omega g(u)\nabla u\dtp\nabla v  \dv \\
  \end{aligned}
\end{equation}$$

Thus we have the semilinear form

$$\begin{equation}
  F(u,v) = \int_{\Omega} g(u) \nabla u \dtp \nabla v \dv - \int_{\Omega} f \,
  v \dv = 0
\end{equation}$$

The linearized version of this problem is then with \eqref{eq:femnonlinear5}

$$\begin{equation}
  \begin{aligned}
    a(\Var u,v) &= \int_{\Omega}
    \rbr{\deriv{g}{u}\evat_{\bar{u}} \Var u\, \nabla \bar{u}
      + g(\bar{u})\nabla(\Var u)} \dtp \nabla v \dv  \\
    b(v) &= \int_{\Omega} [f \, v - g(\bar{u}) \nabla \bar{u} \dtp \nabla v] \dv\\
  \end{aligned}
\end{equation}$$

and the matrix and vector components are with \eqref{eq:femnonlinear9}

$$\begin{equation}
  \begin{aligned}
    \Aelid{I\!J}{} &= \int_{\Omega}
    \rbr{\deriv{g}{u}\evat_{\bar{u}} N^J \, \nabla \bar{u}
      + g(\bar{u})\BB^J} \dtp \BB^I \dv \\
    b^{I} &= \int_{\Omega} [f \, N^I - g(\bar{u}) \nabla \bar{u} \dtp \BB^I ] \dv\\
  \end{aligned}
\end{equation}$$

where the previous solution and its gradient are computed as

$$\begin{equation}
  \bar{u} = \suml{I=1}{\nnode} \bar{u}^I N^I
  \eqand
  \nabla \bar{u} = \suml{I=1}{\nnode} \bar{u}^I \BB^I
  .
\end{equation}$$

# Nonlinear Time-Dependent Problems

In the case of a nonlinear time-dependent problem, we have the following weak
form:

> Find $u \in V$ such that
>
> $$\begin{equation}
>   m(\dot{u}, v; t) + F(u,v; t) = 0
>   \label{eq:nonlineartimedependentweak1}
> \end{equation}$$
>
> for all $v \in V$ and $t \in [0,\infty)$
> where $F$ is a semilinear form.


Discretization yields the following nonlinear system of equations

$$\begin{equation}
  \BM(t)\Bu + \Bf(u; t) = \Bzero
\end{equation}$$

where

$$\begin{equation}
  \begin{aligned}
    M^{I\!J}(t) &= m(N^J, N^I; t) \\
    f^{I}(u;t) &= F(u, N^I; t).
  \end{aligned}
\end{equation}$$

## Explicit Euler Scheme

We discretize in time with the finite difference
$\dot{u} \approx [u_{n+1}-u_n]/{\Delta t}$ and linearity allows us to write

\begin{equation}
  \boxed{
    m(\dot{u}, v; t)
    \approx \frac{1}{\Delta t} [m(u_{n+1}, v; t_{n+1}) -  m(u_n, v; t_n)]
  }
  \label{eq:discretetimedependent1}
\end{equation}

We discretize the variational forms in time according to
\eqref{eq:discretetimedependent1}, and evaluate the remaining terms at $t_n$:

$$\begin{equation}
  \frac{1}{\Delta t} [m(u_{n+1},v;t_{n+1}) - m(u_{n},v;t_{n})] + F(u_n, v; t_n) = 0
\end{equation}$$

The corresponding system of equations is

$$\begin{equation}
  \frac{1}{\Delta t} [\BM_{n+1}\Bu_{n+1} - \BM_n\Bu_n]
  + \Bf_n = \Bzero
\end{equation}$$

where $\Bf_n = \Bf(u_n, t_n)$. This yields the following update equation

$$\begin{equation}
  \boxed{
    \Bu_{n+1} = \BM_{n+1}\inv [\BM_n\Bu_n - \Delta t \Bf_n]
  }
\end{equation}$$

For a time-independent $m$, this becomes

$$\begin{equation}
  \Bu_{n+1} = \Bu_n - \Delta t \BM\inv\Bf_n
\end{equation}$$


## Implicit Euler Scheme

For the implicit scheme, we evaluate the remaining terms at $t_{n+1}$ and let
the result be equal to

$$\begin{equation}
  G(u_{n+1}, v)
  := \frac{1}{\Delta t} [m(u_{n+1},v;t_{n+1}) - m(u_{n},v;t_{n})] + F(u_{n+1}, v; t_{n+1}) = 0
\end{equation}$$

We will hereon replace $u_{n+1}$ with $u$ for brevity.
The update of this nonlinear system requires the linearization of
$G(u, v)$:

$$\begin{equation}
  \Lin[G(u,v)]_{u=\bar{u}}
  = G(\bar{u}, v) + \varn{G}{u}{\Var u}\evat_{u=\bar{u}} = 0
\end{equation}$$

We thus have the following linear setting for the Newton update $\Var u$:

$$\begin{equation}
  a(\Var u, v) = b(v)
\end{equation}$$

where

$$\begin{equation}
  \begin{aligned}
    a(\Var u, v)
    &:= \varn{G}{u}{\Var u} \evat_{u=\bar{u}}
    = \frac{1}{\Delta t} m(\Var u, v; t_{n+1})
    + \varn{F(u, v; t_{n+1})}{u}{\Var u} \evat_{u=\bar{u}} \\
    b(v) &:= -G(\bar{u}, v)
    = - F(\bar{u}, v; t_{n+1})
    -\frac{1}{\Delta t} [m(\bar{u},v;t_{n+1}) - m(u_{n},v;t_{n})]
  \end{aligned}
\end{equation}$$

Discretization yields

$$\begin{equation}
  \rbr{\frac{1}{\Delta t} \BM_{n+1} + \tilde{\BA}}\Var \Bu
  = \Bb
\end{equation}$$

where

$$\begin{equation}
  \tilde{A}^{I\!J} := \varn{F(u, N^I;t_{n+1})}{u}{N^J} \evat_{u=\bar{u}}
  \eqand
  b^I := b(N^I)
\end{equation}$$

The Newton update is rendered

$$\begin{equation}
  \boxed{
    \Bu \leftarrow \bar{\Bu} + \Var\Bu
    \eqwith
    \Var \Bu
    = [\frac{1}{\Delta t} \BM_{n+1} + \tilde{\BA}]\inv\Bb
  }
\end{equation}$$

which is repeated until the solution for the next timestep $\Bu$ converges
to a satisfactory value.


# Nonlinear Coupled Problems

For a nonlinear coupled problem, the weak formulation is as follows

> Find $u\in V_1$, $y\in V_2$ such that
>
> $$\begin{equation}
>   \begin{aligned}
>     F(u, y, v) &= 0 \\
>     G(u, y, w) &= 0 \\
>   \end{aligned}
>   \label{eq:nonlinearcoupled1}
> \end{equation}$$
>
> for all $v\in V_1$, $w \in V_2$ where
> $F(\cdot,\cdot, \cdot)$, $G(\cdot, \cdot, \cdot)$ are nonlinear in terms of
> $u$ and $y$ and linear in terms of $v$ and $w$.

We linearize the semilinear forms about the nonlinear terms:

$$\begin{equation}
  \begin{alignedat}{4}
    \Lin[F(u, y, v)]_{\bar{u},\bar{y}}
    &= F(\bar{u},\bar{y},v)
    &&+ \varn{F(u, y, v)}{u}{\Var u} \evat_{\bar{u},\bar{y}}
    &&+ \varn{F(u, y, v)}{y}{\Var y} \evat_{\bar{u},\bar{y}} \\
    \Lin[G(u, y, w)]_{\bar{u},\bar{y}}
    &= G(\bar{u},\bar{y},w)
    &&+ \varn{G(u, y, w)}{u}{\Var u} \evat_{\bar{u},\bar{y}}
    &&+ \varn{G(u, y, w)}{y}{\Var y} \evat_{\bar{u},\bar{y}}
  \end{alignedat}
  \label{eq:nonlinearcoupled2}
\end{equation}$$

where the evaluations take place at $u=\bar{u}$ and $y=\bar{y}$.

Equating the linearized residuals to zero, we obtain a linear system of the form

$$\begin{equation}
  \begin{alignedat}{3}
    a(u, v) &+ b(y, v) &&= c(v) \\
    d(u, w) &+ e(y, w) &&= f(w) \\
  \end{alignedat}
  \label{eq:coupledweakform1}
\end{equation}$$

with the bilinear forms $a$, $b$, $d$, $e$ and the linear forms $c$, $f$ which
are defined as

$$\begin{equation}
  \begin{gathered}
  \begin{alignedat}{4}
    a(\Var u, v) &:= \varn{F(u, y, v)}{u}{\Var u} \evat_{\bar{u},\bar{y}}
    \quad &
    b(\Var y, v) &:= \varn{F(u, y, v)}{y}{\Var y} \evat_{\bar{u},\bar{y}}
    \\
    d(\Var u, w) &:= \varn{G(u, y, w)}{u}{\Var u} \evat_{\bar{u},\bar{y}}
    \quad &
    e(\Var y, w) &:= \varn{G(u, y, w)}{y}{\Var y} \evat_{\bar{u},\bar{y}}
  \end{alignedat}
  \\
  \text{and}
  \\
  \begin{aligned}
    c(v) &:= -F(\bar{u},\bar{y},v) \\
    f(w) &:= -G(\bar{u}, \bar{y}, w)
  \end{aligned}
  \end{gathered}
\end{equation}$$

Discretizing as done in the previous section, we obtain the following linear system of
equations

$$\begin{equation}
  \begin{bmatrix}
    \BA & \BB \\
    \BD & \BE
  \end{bmatrix}
  \begin{bmatrix}
    \Var \Bu \\ \Var \By
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Bc \\ \Bf
  \end{bmatrix}
\end{equation}$$

whose solution yields the update values $\Var \Bu$ and $\Var \By$. Thus the Newton
update equations are

$$\begin{equation}
  \begin{alignedat}{3}
    \Bu &\leftarrow \bar{\Bu} &&+ \Var\Bu \\
    \By &\leftarrow \bar{\By} &&+ \Var\By
    .
  \end{alignedat}
\end{equation}$$

## Example: Cahn-Hilliard Equation

The Cahn-Hilliard equation describes the process of phase separation, by which
the two components of a binary fluid spontaneously separate and form domains
pure in each component. The problem is nonlinear, coupled and time-dependent.
The IBVP reads

$$\begin{equation}
  \begin{alignedat}{4}
    \partd{c}{t} &= \nabla\dtp(\BM\nabla \mu)
    \qquad&& \text{in} \qquad&& \Omega\times I \\
    \nabla c\dtp\Bn &= 0 && \text{on} && \del\Omega\times I\\
    \nabla \mu\dtp\Bn &= 0 && \text{on} && \del\Omega\times I\\
    c &= c_0 && \text{in} && \Omega, t = 0 \\
    \mu &= 0 && \text{in} && \Omega, t = 0 \\
  \end{alignedat}
  \label{eq:cahnhilliard1}
\end{equation}$$

where

$$\begin{equation}
  \mu = \deriv{f}{c} - \nabla\dtp(\BLambda\nabla c)
  \label{eq:cahnhilliard2}
\end{equation}$$

and $t\in I = [0,\infty)$. Here,

- $c$ is the scalar variable for concentration,
- $\mu$ is the scalar variable for the chemical potential,
- $f: c \mapsto f(c)$ is the function representing chemical free energy,
- $\BM$ is a second-order tensor describing the mobility of the chemical,
- $\BLambda$ is a second-order tensor describing both the interface
  thickness and direction of phase transition.

The fourth-order PDE governing the problem can be formulated as a coupled
system of two second-order PDEs with the variables $c$ and $\mu$, as
demonstrated in \eqref{eq:cahnhilliard1}
and \eqref{eq:cahnhilliard2}.

The weak formulation then reads

> Find $c \in V_1$, $\mu\in V_2$ such that
>
> $$\begin{equation}
>   \begin{aligned}
>     \int_\Omega \partd{c}{t} v \dx
>     - \int_\Omega \nabla\dtp(\BM\nabla \mu) v \dx &=0 \\
>     \int_\Omega \sbr{\mu - \deriv{f}{c}} w \dx
>     + \int_\Omega \nabla\dtp(\BLambda\nabla c) w\dx &= 0
>   \end{aligned}
> \end{equation}$$
>
> for all $v \in V_1$, $w \in V_2$ and $t \in I$.

We discretize in time implicitly with $\del c/\del t \approx
(c_{n+1}-c_n)/\Var t$. **We also denote the values for the next timestep
$c_{n+1}$ and $\mu_{n+1}$ as $c$ and $\mu$ for brevity.**
Using integration-by-parts, the divergence theorem, and the given boundary
conditions, we arrive at the following nonlinear forms

$$\begin{equation}
  \begin{alignedat}{3}
    F(c,\mu,v) &= \int_\Omega \frac{1}{\Var t} (c-c_n) v \dx
    + \int_\Omega (\BM\nabla \mu)\dtp \nabla v \dx &&= 0 \\
    G(c,\mu,w) &= \int_\Omega \sbr{\mu - \deriv{f}{c}} w \dx
    - \int_\Omega (\BLambda\nabla c)\dtp \nabla w\dx &&= 0
  \end{alignedat}
\end{equation}$$

which is a nonlinear coupled system of the form \eqref{eq:nonlinearcoupled1}.

We linearize the forms according to \eqref{eq:nonlinearcoupled2} and obtain
the following variations

$$\begin{align*}
  \varn{F}{c}{\Var c}
  &= \int_\Omega \frac{1}{\Var t} \Var c\, v \dx \\

  \varn{F}{\mu}{\Var \mu}
  &= \int_\Omega (\BM\nabla (\Var\mu))\dtp \nabla v \dx \\

  \varn{G}{c}{\Var c}
  &= - \int_\Omega \dderiv{f}{c}\Var c \, w \dx
  - \int_\Omega (\BLambda\nabla (\Var c))\dtp \nabla w\dx \\

  \varn{G}{\mu}{\Var \mu}
  &= \int_\Omega \Var\mu \, w \dx
\end{align*}$$

We substitute basis functions and obtain our system matrix
and vectors

$$\begin{align*}
  P^{I\!J}
  &= \int_\Omega \frac{1}{\Var t} N^JN^I \dx \\

  Q^{IL}
  &= \int_\Omega (\BM\BB^L)\dtp\BB^I  \dx \\

  r^{I}
  &= \int_\Omega \frac{1}{\Var t}(\bar{c}-c_n)N^I \dx
  + \int_\Omega (\BM\nabla\bar{\mu})\dtp\BB^I\dx  \\

  S^{K\!J}
  &= - \int_\Omega \dderiv{f}{c}\evat_{c=\bar{c}} N^J N^K \dx
  - \int_\Omega (\BLambda \BB^J)\dtp \BB^K\dx \\

  T^{K\!L}
  &= \int_\Omega N^L N^K \dx \\

  u^{K}
  &= \int_\Omega \sbr{\bar{\mu} - \deriv{f}{c}\evat_{c=\bar{c}}} N^K \dx
    - \int_\Omega (\BLambda\nabla \bar{c})\dtp \BB^K\dx
\end{align*}$$

which constitute the system

$$\begin{equation}
  \begin{bmatrix}
    \BP & \BQ \\
    \BS & \BT
  \end{bmatrix}
  \begin{bmatrix}
    \Var \Bc \\ \Var \Bmu
  \end{bmatrix}
  =
  \begin{bmatrix}
    \Br \\ \Bu
  \end{bmatrix}
\end{equation}$$

Solution yields the update values $\Var \Bc$ and $\Var \Bmu$. The Newton
update equations are then

$$\begin{equation}
  \begin{alignedat}{3}
    \Bc &\leftarrow \bar{\Bc} &&+ \Var\Bc \\
    \Bmu &\leftarrow \bar{\Bmu} &&+ \Var\Bmu
    .
  \end{alignedat}
\end{equation}$$

The system is solved for $c_{n+1}$ and $\mu_{n+1}$ at each $t=t_n$ to obtain
the evolutions of the concentration and chemical potential.


