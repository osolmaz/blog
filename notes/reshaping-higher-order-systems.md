---
layout: page
title:  "Reshaping Higher Order Linear Systems"
date: 2017-11-20
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

In vectorial problems, we end up with linear systems of higher order, such as

$$\begin{equation}
    \suml{k=1}{N}
    \suml{l=1}{M} A_{ijkl} \, u_{kl}
    = b_{ij}
    \label{eq:1}
\end{equation}$$

for $i=1,\dots,N$ and $j=1,\dots,M$.

These systems cannot be solved readily with existing software. In order to be
able to solve them with existing software, we need to reshape them by
defining a **matrix of matrices**
$\BAhat$ and **vector of vectors** $\Buhat$ and $\Bbhat$:

$$\begin{equation}
  \underbrace{
    \left[
      \begin{array}{ccc|c|ccc}
        A_{1111}
        & \cdots
        & A_{111M}
        &
        & A_{11N1}
        & \cdots
        & A_{11NM}
        \\
        \vdots
        & \ddots
        & \vdots
        & \cdots
        & \vdots
        & \ddots
        & \vdots
        \\
        A_{1M11}
        & \cdots
        & A_{1M1M}
        &
        & A_{1MN1}
        & \cdots
        & A_{1MNM}
        \\[1ex]
        \hline
        & \vdots
        &
        &\ddots
        &
        & \vdots
        &
        \\
        \hline
        &&&&&&
        \\[-1.5ex]
        A_{N111}
        & \cdots
        & A_{N11M}
        &
        & A_{N1N1}
        & \cdots
        & A_{N1NM}
        \\
        \vdots
        & \ddots
        & \vdots
        & \cdots
        & \vdots
        & \ddots
        & \vdots
        \\
        A_{NM11}
        & \cdots
        & A_{NM1M}
        &
        & A_{NMN1}
        & \cdots
        & A_{NMNM}
      \end{array}
    \right]
  }_{\BAhat}
  \underbrace{
    \left[
      \begin{array}{c}
        u_{11}\\
        \vdots \\
        u_{1M} \\[1ex]
        \hline \vdots\\
        \hline \\[-1.5ex]
        u_{N1} \\
        \vdots \\
        u_{NM}
      \end{array}
    \right]
  }_{\Buhat}
  =
  \underbrace{
    \left[
      \begin{array}{c}
        b_{11}\\
        \vdots \\
        b_{1M} \\[1ex]
        \hline \vdots\\
        \hline \\[-1.5ex]
        b_{N1} \\
        \vdots \\
        b_{NM}
      \end{array}
    \right]}_{\Bbhat}
\end{equation}$$

This allows us to express the linear system as

$$\begin{equation}
  \BAhat \Buhat = \Bbhat
  \label{eq:3}
\end{equation}$$

Here, we **reshape** the system by defining a map $i_d$ that maps original
indices to the reshaped indices

$$\begin{equation}
  i_d :=
  \left\{
    \begin{array}{rl}
      [1,N]\times[1,M] & \to [1,NM]\\[1ex]
      (i,j) & \mapsto M(i-1)+j\\
    \end{array}
  \right.
\end{equation}$$

where we used 1-based indexing of the arrays.
We set

$$\begin{equation}
  \boxed{
    \begin{alignedat}{3}
      \alpha &:= i_d(i,j) &&= M(i-1) + j \\
      \beta &:= i_d(k,l) &&= M(k-1) + l \\
    \end{alignedat}
  }
\end{equation}$$

and write

$$\begin{equation}
  \hat{A}_{\alpha\beta} = A_{ijkl}
  \quad,\quad
  \hat{u}_{\beta} = u_{kl}
  \eqand
  \hat{b}_{\alpha} = b_{ij}
\end{equation}$$

For reference, the inverse of the index mapping reads

$$\begin{equation}
  i_d^{-1} :=
  \left\{
    \begin{array}{rl}
      [1,NM] & \to [1,N]\times[1,M] \\[1ex]
      \alpha & \mapsto (1+(\alpha-\modop(\alpha,M))/M\;,\; \modop(\alpha,M))
    \end{array}
  \right.
\end{equation}$$

Thus, we have for our reshaped indices,

$$\begin{equation}
  \begin{aligned}
    j &= \modop(\alpha,M) \\
    i &= 1+(\alpha-j)/M
  \end{aligned}
  \quad\eqand\quad
  \begin{aligned}
    l &= \modop(\beta,M) \\
    k &= 1+(\beta-l)/M
  \end{aligned}
\end{equation}$$

Expressed as a regular linear system \eqref{eq:3}, the higher-order system
\eqref{eq:1} can be solved with a linear solver such as
[LAPACK](https://en.wikipedia.org/wiki/LAPACK).
