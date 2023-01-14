---
layout: page
title:  "Metrics and Natural Isomorphisms"
date: 2017-07-13
---

{% include shortsym.html %}
{% include lazyeqn.html %}

The assignment of an inner product to
a non-degenerate and finite-dimensional vector space $\CV$,
results in emergence of the **natural isomorphism** to its dual
$\CV\to\CV^\ast$, which means that the morphisms
$\CV\to\CV^\ast$ and $\CV^\ast\to\CV$ are of *the same structure* and one
is the *inverse* of the other.
The notion of naturality (of an isomorphism) becomes most clear in the context of
category theory;
however it should be
sufficient for now to say that a natural isomorphism
between a vector space an its dual is one that is
**basis-independent**. As the origin of the isomorphism, the inner product is
encapsulated in an object called the metric, defined below,
in order to make the resulting symmetry of the mappings more obvious.

In the context of differential geometry, the **metric** object
is used synonymously with the inner product of a vector space. More specifically,
the **metric tensor**

$$\begin{equation}
  \Bg:=
  \left\{
    \begin{aligned}
      \CV \times \CV &\to \IR \\
      (\Bv,\Bw) &\mapsto \dabrn{\Bv, \Bw}
    \end{aligned}
  \right.
\end{equation}$$

of a real vector space $\CV$ is an object whose components contain the
information necessary to linearly transform a vector to its covector. This
operation is denoted by the symbol $\flat$ and reads

$$\begin{equation}
  \flat :=
  \left\{
    \begin{aligned}
      (\CV^\ast \to \IR) &\to (\CV \to \IR) \\
      \text{or}\quad \CV &\to\CV^\ast \\
      \Bv(\cdot) &\mapsto \Bg(\Bv, \cdot).
    \end{aligned}
  \right.
\end{equation}$$

We simply define the one-form $\Bv^\flat$ as

$$\begin{equation}
  \Bv^\flat(\Bw) \equiv \Bg(\Bv,\Bw) = \dabrn{\Bv,\Bw}.
\end{equation}$$

We input the basis vectors $\Be_a$

$$\begin{equation}
  \Bv^\flat(\Be_a) = \dabrn{v^b\Be_b, \Be_a}
  = \dabrn{\Be_b, \Be_a} v^b
  = \dabrn{\Be_a, \Be_b} v^b
\end{equation}$$

and define the components of the metric tensor as

$$\begin{equation}
  \boxed{
    g_{ab} = \dabrn{\Be_a, \Be_b}
    \eqwith
    \Bg = g_{ab}\, \Be^a\dyd \Be^b.
  }
\end{equation}$$

We then simply say that the operator $\flat$ denotes an
**index lowering**[^1] through

$$\begin{equation}
  \Bv^\flat = \Bg\Bv
  \quad\text{and component-wise }\quad
  v_a = g_{ab} v^b.
\end{equation}$$

Moreover, we can define the inverse of the metric tensor as

$$\begin{equation}
  \Bg\inv:=
  \left\{
    \begin{aligned}
      \CV^\ast\times\CV^\ast &\to \IR  \\
      (\Balpha,\Bbeta) &\mapsto \dabrn{\Balpha, \Bbeta}
    \end{aligned}
  \right.
\end{equation}$$

The operation of transforming a covector to its corresponding vector is
denoted by the symbol $\sharp$ and reads

$$\begin{equation}
  \sharp :=
  \left\{
    \begin{aligned}
      (\CV \to \IR) &\to (\CV^\ast \to \IR) \\
      \text{or}\quad \CV^\ast &\to\CV \\
      \Balpha(\cdot) &\mapsto \Bg\inv(\cdot,\Balpha).
    \end{aligned}
  \right.
\end{equation}$$


Here, the vector corresponding to the covector $\Balpha$ is denoted
$\Balpha^\sharp$ and reads

$$\begin{equation}
  \Balpha^\sharp(\Bbeta) = \Bg\inv(\Bbeta,\Balpha) = \dabrn{\Bbeta, \Balpha}
\end{equation}$$

We input the dual basis vectors $\Be^a$

$$\begin{equation}
  \Balpha^\sharp(\Be^a) = \dabrn{\Be^a, \alpha_b\Be^b}
  = \dabrn{\Be^a, \Be^b} \alpha_b
\end{equation}$$

and define the components of the inverse metric $\Bg\inv$ as

$$\begin{equation}
  \boxed{
    g^{ab} = \dabrn{\Be^a,\Be^b} \eqwith \Bg\inv = g^{ab}\,\Be_a\dyd\Be_b.
  }
\end{equation}$$

Then the operator $\sharp$ denotes an **index raising** through

$$\begin{equation}
  \Balpha^\sharp = \Bg\inv\Balpha
  \quad\text{and component-wise }\quad
  \alpha^a = g^{ab}\alpha_b.
\end{equation}$$

In some literature, the natural isomorphism $\CV\to\CV^\ast$
is called the **musical isomorphism**---which is also the origin of the
notation introduced above---because the process of transforming a
vector to its dual space and a covector to the original space is analogous to
lowering and raising notes.

With the given definition of the metric, we can elaborate on
the advantage of denoting inner products of different objects with different
symbols. Whereas $\abrn{\cdot,\cdot}$ always denotes a natural pairing between a
vector space and its dual, one can write $$\dabrn{\cdot,\cdot}_\CV:\CV\times\CV\to\IR$$
to denote an inner product of vectors
and $\dabrn{\cdot,\cdot}_{\CV^\ast}:\CV^\ast\times\CV^\ast\to\IR$ to denote an
inner product of covectors. Using the metric, we can link these notations as

$$\begin{equation}
  \begin{alignedat}{5}
    &\dabrn{\Bv,\Bw}_\CV
    &&= \abrn{\Bv, \Bw^\flat}
    &&= \abrn{\Bv^\flat, \Bw}
    &&= g_{ab} v^a w^b\\
    &\dabrn{\Balpha,\Bbeta}_{\CV^\ast}
    &&= \abrn{\Balpha, \Bbeta^\sharp}
    &&= \abrn{\Balpha^\sharp, \Bbeta}
    &&= g^{ab}\alpha_a\beta_b\\
  \end{alignedat}
\end{equation}$$

for all $\Bv,\Bw\in\CV$ and $\Balpha,\Bbeta\in\CV^\ast$. Similarly,

$$\begin{equation}
  \begin{alignedat}{4}
    &\abrn{\Bv, \Balpha}
    &&= \dabrn{\Bv^\flat, \Balpha}_{\CV^\ast}
    &&= \dabrn{\Bv, \Balpha^\sharp}_{\CV}.
  \end{alignedat}
\end{equation}$$

Despite the symmetricity of the inner product, we choose
to think of the first operand as a vector and the second as a covector
in a natural pairing, as a convention.

The metric tensor has the following properties:

- For orthonormal bases, the metric tensor equals the identity tensor, that is,
  $g_{ij}=\delta_{ij}$.
- The diagonal terms equal to the square of the lengths of the basis
  vectors, that is, $g_{ii}=\Norm{\Be_i}^2$ (no summation).
- The off-diagonal terms are zero if the basis vectors are orthogonal.
  Specifically, $g_{ij}=0$ iff $\Be_i$ and $\Be_j$ are orthogonal.

[^1]: In musical notation, the flat symbol
      $\flat$ is used to lower a note by one semitone, whereas the sharp symbol
      $\sharp$ is used to raise a note by one semitone.
      It is recommended to pronounce $\Bv^\flat$ as *v-flat*
      and $\Balpha^\sharp$ *alpha-sharp*.
