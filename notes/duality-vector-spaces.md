---
layout: page
title:  "Duality of Vector Spaces"
date: 2017-07-06
---

{% include shortsym.html %}
{% include lazyeqn.html %}


<div style="display:none">
$
\newcommand{\veciup}[1]{#1^1,\ldots,#1^n}
\newcommand{\setveci}[1]{\cbrn{\veci{#1}}}
\newcommand{\setveciup}[1]{\cbrn{\veciup{#1}}}
\newcommand{\tang}{T}
$
</div>

When I was learning about Continuum Mechanics for the first time, the *covariance
and contravariance of vectors* confused the hell out of me. The concepts gain
meaning in the context of Riemannian Geometry, but it was surprising to find
that one doesn't need to learn an entire subject to grasp the logic behind
co-/contravariance. An intermediate knowledge of linear algebra is enough---that
is, one has to be acquainted with the concept of vector spaces and one-forms.

The duality of co-/contravariance arises when one has to define vectors in terms
of a non-orthonormal basis. The reason such terminology doesn't show up
in engineering education is that Cartesian coordinates are enough for most
engineering problems. But every now and then, a complex problem with funky
geometrical requirements show up, like one that requires measuring distances and
areas on non-flat surfaces. Then you end up with dual vector spaces. I'll try to
give the basics of duality below.


**Definition:** Let $\CV$ be a finite-dimensional real vector space.
The space $\CV^\ast = \CL(\CV,\IR)$,
defined as the
the space of all one-forms $\Balpha:\CV\to\IR$, is called the
**dual space** to $\CV$.

Let $B=\cbr{\Be_1,\dots,\Be_n}$ be a basis of $\CV$. Any vector $\Bv\in\CV$ can be written
in terms of $B$ as

$$\begin{equation}
  \Bv = a_1 \Be_1 + \cdots + a_n\Be_n
  \label{eq:vectorrep1}
\end{equation}$$

with the components $a_1,\dots,a_n\in\IR$.
For any $i=1,\dots,n$, we can define the $i$-th component $a_i$ by a one-form as

$$\begin{equation}
  \Be^i :=
  \left\{
    \begin{aligned}
      \CV &\to \IR \\
      \Bv &\mapsto \Be^i(\Bv) = a_i
    \end{aligned}\right.
\end{equation}$$

These elements are linear and thus are in the space
$\CL(\CV,\IR)$[^1].
Given any basis $B=\setveci{\Be}$, we call $B^\ast = \setveciup{\Be}$
the basis of $\CV^\ast$ **dual** to $B$.
The fact that $B^\ast$ really is a basis of $\CV^\ast$ can be proved
by showing that $\Be^i$ are linearly independent.
Then $\Bv$ has the following
representation

$$\begin{equation}
  \Bv = \Be^1(\Bv)\, \Be_1 + \cdots + \Be^n(\Bv)\, \Be_n.
  \label{eq:vectorrep2}
\end{equation}$$

Instead of $a_i$, it is practical to denote the components of $\Bv$ as $v^i$,
lightface of the same symbol with a raised index corresponding to
the raised index of the dual basis:

$$\begin{equation}
  \Bv = v^1 \Be_1 + \cdots + v^n \Be_n
  \eqwith
  v^i = \Be^i(\Bv).
\end{equation}$$

In fact, this convention is more compatible with
the symmetry caused by the duality.
This point will be more clear after the introduction of
dual basis representation of one-forms.



**Proposition:** Each $\Be^i \in \CL(\CV,\IR)$ can be identified by its action on the basis
$B$:

$$\begin{equation}
  \Be^i(\Be_j) =
  \begin{cases}
    1 & \text{if } i=j \\
    0 & \text{otherwise}.
  \end{cases}
  \label{eq:dualbasis2}
\end{equation}$$

**Proof:** For any $\Bv\in\CV$, $\Be^i(\Bv)$ must give $v^i$, the
  $i$-th component of $\Bv$.
  Setting $\Bv = \Be_j$, one sees that
  $\Be^i(\Bv)=v^i = 1$ when $i=j$, and is zero otherwise.

Geometrically, \eqref{eq:dualbasis2} implies that a basis vector is
perpendicular to all the dual basis vectors, except its own dual.


## Dual Basis Representation of One-Forms

Let $\Balpha$ be a one form in $\CV^\ast$ with the corresponding
dual basis $\setveciup{\Be}$. Then similar to a vector,
$\Balpha$ has the following representation

$$\begin{equation}
  \begin{aligned}
    \Balpha(\cdot)
    &= \Balpha(\Be_1)\,\Be^1(\cdot)
    + \dots
    + \Balpha(\Be_n)\,\Be^n(\cdot) \\
    &= \alpha_1 \Be^1(\cdot)
    + \dots
    + \alpha_n \Be^n(\cdot)
  \end{aligned}
\end{equation}$$

where the **components of the one-form** $\Balpha$
are defined as

$$\begin{equation}
  \alpha_i = \Balpha(\Be_i).
\end{equation}$$


**Proof:**  We substitute \eqref{eq:vectorrep2} and obtain

$$\begin{equation}
  \begin{aligned}
    \Balpha(\Bv) &= \Balpha\rbr{\suml{i=1}{n} \Be^i(\Bv)\, \Be_i}
    = \suml{i=1}{n} \Balpha(\Be_i)\, \Be^i(\Bv)  \\
  \end{aligned}
\end{equation}$$

using $\Balpha$'s linearity.

**Notation:**
Let $\CV$ be a finite-dimensional real vector space.
For $\Bv\in\CV$ and $\Balpha\in\CV^\ast$

$$\begin{equation}
  \abrn{\cdot,\cdot} :=
  \left\{\begin{aligned}
      \CV\times\CV^\ast &\to \IR \\
      (\Bv, \Balpha) &\mapsto \Balpha(\Bv)
    \end{aligned}\right.
\end{equation}$$

denotes the action of $\Balpha$ on $\Bv$, and is called
a **natural pairing** or **dual pairing**
between a vector space and its dual.
*It is of the essence to understand that $\abrn{\cdot,\cdot}$ does not
denote an inner product in $\CV$*; that is,
$\abr{\Bv,\Balpha}$ means $\Balpha(\Bv)$.


With this notation, \eqref{eq:vectorrep2} can be written as

$$\begin{equation}
  \Bv = \abrn{\Bv,\Be^1}\, \Be_1
  + \cdots
  + \abrn{\Bv,\Be^n}\, \Be_n.
\end{equation}$$

and \eqref{eq:dualbasis2} as

$$\begin{equation}
  \abrn{\Be_i, \Be^j} = \delta_{ij}.
  \label{eq:dualbasis1}
\end{equation}$$

Using the convention that $\Be_i$ are column vectors and
$\Be^i$ are row vectors,
\eqref{eq:dualbasis1} can be rearranged in the following manner

$$\begin{equation}
  \left[
    \begin{array}{@{} c|c|c|c @{}}
      \Be_1&\Be_2&\cdots&\Be_n
    \end{array}
  \right]\inv
  =
  \left[
    \begin{array}{@{} c @{}}
      \Be^1 \\ \hline \Be^2 \\ \hline \vdots \\ \hline \Be^n
    \end{array}
  \right]
  \label{eq:computedualbasis1}
\end{equation}$$

which can be used to compute a dual basis.


**Example:** Given a two-dimensional vector space $\CV$ with a basis
$\Be_1=[2,-0.5]\tra$, $\Be_2=[1,1]\tra$, we use
\eqref{eq:computedualbasis1} to compute

$$\begin{equation}
  \begin{bmatrix}
    2 & 1 \\
    -0.5 & 1
  \end{bmatrix}\inv
  =
  \begin{bmatrix}
    0.4 & -0.4 \\
    0.2 & 0.8
  \end{bmatrix}
\end{equation}$$

and obtain the dual basis vectors as
$\Be^1=[0.4,-0.4]$ and $\Be^2=[0.2,0.8]$.
The result is given in the
following figure,

<figure>
<img src="/img/duality_vector_spaces/fig2.svg">
</figure>


where one can see that $\Be_1\perp\Be^2$, $\Be^1\perp\Be_2$.

<figure>
<img src="/img/duality_vector_spaces/fig1.svg">
  A body $\CB$ embedded in $\IR^2$ with curvilinear coordinates.
  Every point $\CP$ at $\BX$ has an associated two-dimensional vector space,
  called $\CB$'s tangent space at $\BX$, denoted $\tang_\BX\CB$. The basis
  $\Be_i$ corresponding to coordinates $\theta_i$ are not necessarily
  orthogonal and can admit corresponding duals $\Be^i$, due to
  curvilinearity.
  The coordinates appear to be affine at the point's immediate vicinity,
  and thus in the tangent space.
</figure>


The introduction of the dual space
allows us to reinterpret a one-form $\Balpha$
as an object residing in the dual space. In fact,
the **canonical duality** $\CV^{\ast\ast}=\CV$
states that every vector $\Bv$ can be interpreted as a functional
on the space $\CV^\ast$ via

$$\begin{equation}
  \Bv:=
  \left\{
  \begin{aligned}
    \CV^\ast &\to \IR \\
    \Balpha &\mapsto \Bv(\Balpha) \text{ or } \abrn{\Bv, \Balpha}
  \end{aligned}\right.
\end{equation}$$

[^1]: Despite being denoted with bold letters, one-forms should not be confused with vectors.
