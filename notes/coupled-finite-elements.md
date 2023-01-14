---
layout: page
title:  "Coupled Finite Elements"
date: 2017-12-12
---

{% include shortsym.html %}
{% include lazyeqn.html %}


In this post, I'll introduce the FE formulation of a generalized **linear** and
**coupled** weak form.
Said weak formulation has the form

> Find $u\in V_1$, $y\in V_2$ such that
>
> $$\begin{equation}
>   \begin{alignedat}{3}
>     a(u, v) &+ b(y, v) &&= c(v) \\
>     d(u, w) &+ e(y, w) &&= f(w) \\
>   \end{alignedat}
>   \label{eq:coupledweakform1}
> \end{equation}$$
>
> for all $v\in V_1$, $w \in V_2$ where
> $a(\cdot, \cdot): V_1\times V_1 \to \IR$,
> $b(\cdot, \cdot): V_2\times V_1 \to \IR$,
> $d(\cdot, \cdot): V_1\times V_2 \to \IR$,
> $e(\cdot, \cdot): V_2\times V_2 \to \IR$
> are bilinear forms and
> $c(\cdot): V_1\to \IR$,
> $f(\cdot): V_2\to \IR$ are linear forms.


Here, the objective is to solve for the two unknown functions $u$ and $y$. One
can also imagine an arbitrary degree of coupling between $n$ variables with $n$
equations.

We introduce the following discretizations

$$\begin{equation}
  \begin{alignedat}{3}
    u_h &= \suml{J=1}{n_n^1} u^J N^J
    \qquad\qquad &
    v_h &= \suml{I=1}{n_n^1} u^I N^I
    \qquad\qquad &
    u_h, v_h\in V_{h1}
    \\
    y_h &= \suml{L=1}{n_n^2} u^L N^L
    &
    w_h &= \suml{K=1}{n_n^2} u^K N^K
    &
    y_h, w_h\in V_{h2}
    \\
  \end{alignedat}
\end{equation}$$

where the corresponding number of shape functions are $n_n^1$ and $n_n^2$, respectively.

Substituting the discretizations in \eqref{eq:coupledweakform1}, we obtain two
linear systems of equations

$$\begin{equation}
  \begin{alignedat}{3}
    \suml{J=1}{n_n^1} a(N^J, N^I) \,u^J
    &+ \suml{L=1}{n_n^2} b(N^L, N^I) \, y^L
    &&= c(N^I) \\
    \suml{J=1}{n_n^1} d(N^J, N^K) \,u^J
    &+ \suml{L=1}{n_n^2} e(N^L, N^K) \, y^L
    &&= f(N^K) \\
  \end{alignedat}
\end{equation}$$

for $I=1,\dots,n_n^1$ and $K=1,\dots,n_n^2$.

We write this system as

$$\begin{equation}
  \boxed{
    \begin{alignedat}{3}
      \BA \Bu &+ \BB\By &&= \Bc \\
      \BD \Bu &+ \BE\By &&= \Bf \\
    \end{alignedat}
    \eqor
    \begin{bmatrix}
      \BA & \BB \\
      \BD & \BE
    \end{bmatrix}
    \begin{bmatrix}
      \Bu \\ \By
    \end{bmatrix}
    =
    \begin{bmatrix}
      \Bc \\ \Bf
    \end{bmatrix}
  }
  \label{eq:coupledsystem1}
\end{equation}$$

where the components of given matrices and vectors are defined as

$$\begin{equation}
  \begin{alignedat}{6}
    A^{I\!J} &:= a(N^J, N^I)
    \qquad &  B^{I\!L} &:= b(N^L, N^I)
    \qquad &  c^{I} &:= c(N^I)\\
    D^{K\!J} &:= d(N^J, N^K)
    & E^{K\!L} &:= e(N^L, N^K)
    & f^{K} &:= f(N^K)\\
  \end{alignedat}
\end{equation}$$

Solution of \eqref{eq:coupledsystem1} yields the unknown vectors $\Bu$ and $\By$.

