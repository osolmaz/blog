---
layout: page
title:  "Vectorial Finite Elements"
date: 2017-11-21
---

{% include shortsym.html %}
{% include lazyeqn.html %}

<div style="display:none">
  $
    \newcommand{\BAhat}{\hat{\boldsymbol{A}}}
    \newcommand{\Buhat}{\hat{\boldsymbol{u}}}
    \newcommand{\Bbhat}{\hat{\boldsymbol{b}}}
  $
</div>

Many initial boundary value problems require solving for unknown vector
fields, such as solving for displacements in a mechanical problem.
Discretization of weak forms of such problems leads to higher-order linear
systems which need to be reshaped to be solved by regular linear solvers. There
are also more indices involved than a scalar problem, which can be confusing. In
this post, I'll try to elucidate the procedure by deriving for a basic
higher-order system and giving an example.

The weak formulation of a linear vectorial problem reads

> Find $\Bu\in V$ such that
>
> $$\begin{equation}
>   a(\Bu, \Bv) = b(\Bv)
> \end{equation}$$
>
> for all $\Bv\in V$.

Discretizations of vectorial problems requires the expansion of vectorial quantities
as linear combinations of the basis vectors $\Be_i$:

$$\begin{equation}
  \Bu = \suml{i=1}{\ndim} u_i \,\Be_i
  \label{eq:discrete6}
\end{equation}$$

where $\cbr{u_i}_{i=1}^{\ndim}$ are the components corresponding to the
basis vectors and $\ndim=\dim V$. Here, we chose Cartesian basis vectors for simplicity.

We can therefore express its discretization as

$$\begin{equation}
  \Bu_h = \suml{I=1}{\nnode} \Bu^I N^I
  = \suml{I=1}{\nnode}\suml{i=1}{\ndim} u^I_i \Be_i N^I.
  \label{eq:discrete7}
\end{equation}$$

Substituting discretized functions in the weak formulation, we obtain

$$\begin{equation}
  \begin{aligned}
    a(\Bu_h, \Bv_h) &= \suml{i=1}{\ndim}\suml{j=1}{\ndim} \,
    a(u_{h,j}\,\Be_j, v_{h,i}\,\Be_i)\\
    &= \suml{I=1}{\nnode}\suml{J=1}{\nnode}
    \suml{i=1}{\ndim}\suml{j=1}{\ndim} u^J_j v^I_i \,a(\Be_j N^J, \Be_i N^I)
  \end{aligned}
\end{equation}$$

and

$$\begin{equation}
  b(\Bv_h) = \suml{i=1}{\ndim}  b(v_{h,i}\,\Be_i)
  = \suml{I=1}{\nnode} \suml{i=1}{\ndim} v^I_i b(\Be_i N^I).
\end{equation}$$

We define the following arrays

$$\begin{equation}
  \boxed{
    \begin{aligned}
      A^{I\!J}_{ij} &= a(\Be_j N^J, \Be_i N^I) \\
      b^{I}_{i} &= b(\Be_i N^I).
    \end{aligned}
  }
\end{equation}$$

Hence we can express the linear system

$$\begin{equation}
  a(\Bu_h,\Bv_h) = b(\Bv_h)
\end{equation}$$

as

$$\begin{equation}
  \suml{I=1}{\nnode}\suml{J=1}{\nnode}
  \suml{i=1}{\ndim}\suml{j=1}{\ndim} u^J_j v^I_i \,A^{I\!J}_{ij}
  =
  \suml{I=1}{\nnode} \suml{i=1}{\ndim} v^I_i b^{I}_{i}.
\end{equation}$$

For arbitrary $\Bv_h$, this yields the following system of equations

$$\begin{equation}
  \boxed{
    \suml{J=1}{\nnode}
    \suml{j=1}{\ndim} A^{I\!J}_{ij} \,u^J_j
    = b^{I}_{i}
  }
  \label{eq:discrete8}
\end{equation}$$

for $i=1,\dots,\ndim$ and $I=1,\dots,\nnode$.

We reshape this higher-order system as shown in the previous post
[Reshaping Higher Order Linear Systems](/math/2017/11/20/reshaping-higher-order-systems/):

$$\begin{equation}
  \BAhat \Buhat = \Bbhat
\end{equation}$$

by defining a map $i_d$ that maps original indices to the reshaped indices

$$\begin{equation}
  i_d :=
  \left\{
  \begin{array}{rl}
    [1,\nnode]\times[1,\ndim] & \to [1,\nnode\ndim]\\[1ex]
    (I,i) & \mapsto \ndim(I-1)+i\\
  \end{array}
  \right.
\end{equation}$$

where we used 1-based indexing of the arrays.
We set

$$\begin{equation}
  \boxed{
    \begin{alignedat}{3}
      \alpha &:= i_d(I,i) &&= \ndim(I-1) + i \\
      \beta &:= i_d(J,j) &&= \ndim(J-1) + j \\
    \end{alignedat}
  }
\end{equation}$$

and write

$$\begin{equation}
  \hat{A}_{\alpha\beta} = A^{I\!J}_{ij}
  \quad,\quad
  \hat{u}_{\beta} = u^{J}_{j}
  \eqand
  \hat{b}_{\alpha} = b^{I}_{i}
\end{equation}$$

The inverse index mapping can be obtained as shown in the previous post.

## Example: Linear Elasticity

Our initial-boundary value problem is

$$\begin{equation}
  \begin{alignedat}{4}
    -\div\Bsigma &= \rho\Bgamma \qquad&& \text{in} \qquad&& \Omega\\
    \Bu &= \bar{\Bu} && \text{on} && \del\Omega_u \\
    \Bt &= \bar{\Bt} && \text{on} && \del\Omega_t \\
  \end{alignedat}
\end{equation}$$

The weak formulation reads

> Find $\Bu\in V$ such that
>
> $$\begin{equation}
>   -\int_\Omega \div\Bsigma \dtp \Bv \dv = \int_\Omega \rho \Bgamma\dtp\Bv \dv
> \end{equation}$$
>
> for all $\Bv\in V$ where $V=H^1(\Omega)$.

We apply integration by parts on the left-hand side

$$\begin{equation}
    \int_\Omega \div\Bsigma\dtp\Bv \dv
    = \int_\Omega \div(\Bsigma\Bv) \dv - \int_\Omega \Bsigma : \nabla\Bv \dv
\end{equation}$$

and apply the divergence theorem to the first resulting term:

$$\begin{equation}
  \int_\Omega \div(\Bsigma\Bv) \dv = \int_{\del\Omega_t} \bar{\Bt}\dtp\Bv \da
\end{equation}$$

Substituting the linear stress $\Bsigma=\IC:\Bvareps=\IC:\nabla\Bu$, we obtain
the following variational forms:

$$\begin{align}
  \label{eq:linelastdiscretebilinear}
  a(\Bu,\Bv) &= \int_\Omega \nabla\Bv:\IC:\nabla\Bu \dv \\
  \label{eq:linelastdiscretelinear}
  b(\Bv) &= \int_\Omega \rho \Bgamma\dtp\Bv \dv
  + \int_{\del\Omega_t} \bar{\Bt}\dtp\Bv \da
\end{align}$$

We have the following discretizations of the unknown function and test function

$$\begin{equation}
  \Bu_h = \suml{J=1}{\nnode} \Bu^J N^J
  \eqand
  \Bv_h = \suml{I=1}{\nnode} \Bv^I N^I.
\end{equation}$$

With the given discretizations, the matrix corresponding to
\eqref{eq:linelastdiscretebilinear} can be calculated as

$$\begin{equation}
  \begin{aligned}
    A^{I\!J}_{ij} = a(\Be_j N^J, \Be_i N^I)
    &= \int_\Omega \nabla(\Be_iN^I) : \IC : \nabla(\Be_jN^J) \dv \\
    &= \int_\Omega (\Be_i\dyd \nabla N^I) : \IC : (\Be_j \dyd \nabla N^J) \dv \\
    &= \int_\Omega \partd{N^I}{x_k} \, C_{ikjl} \, \partd{N^J}{x_l} \dv,
  \end{aligned}
\end{equation}$$

and finally obtain

$$\begin{equation}
  \boxed{
    A^{I\!J}_{ij} = \int_\Omega B^I_k \, C_{ikjl} \, B^J_l \dv \,.
  }
\end{equation}$$

The vector corresponding to \eqref{eq:linelastdiscretelinear} is calculated as

$$\begin{equation}
    b^{I}_{i} = b(\Be_i N^I)
    = \int_{\del\Omega_t} \bar{\Bt}\dtp(\Be_iN^I) \da
    + \int_\Omega\rho\Bgamma\dtp(\Be_iN^I) \dv
\end{equation}$$

which yields

$$\begin{equation}
  \boxed{
    b^{I}_{i}
    = \int_{\del\Omega_t} \bar{t}_i N^I \da + \int_\Omega \rho \gamma_i N^I \dv
  }
\end{equation}$$

