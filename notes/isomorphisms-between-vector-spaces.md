---
layout: page
title:  "Isomorphisms in Linear Mappings between Vector Spaces"
date: 2017-07-22
---

{% include shortsym.html %}
{% include lazyeqn.html %}

Equipping a vector space with an inner product
results in a natural isomorphism $\CV\to\CV^\ast$, where
the metric tensor can be interpreted as the linear mapping $\Bg:\CV\to\CV^\ast$
and its inverse $\Bg\inv:\CV^\ast\to\CV$.

**Notation:** Given two real vector spaces $\CV$ and $\CW$, we denote their inner products
as $$\dabrn{\cdot,\cdot}_{\CV}$$ and $$\dabrn{\cdot,\cdot}_{\CW}$$ respectively.
Given vectors $\Bv\in\CV$ and $\Bw\in\CW$, we define their lengths as

$$\begin{equation}
  \Norm{\Bv}_\CV = \sqrt{\dabrn{\Bv,\Bv}_\CV}
  \eqand
  \Norm{\Bw}_\CW = \sqrt{\dabrn{\Bw,\Bw}_\CW}.
\end{equation}$$

Regarding $\CV$ and $\CW$,

1. their bases are denoted $\cbrn{\BE_A}$ and $\cbrn{\Be_a}$,
2. their dual bases are denoted $\cbrn{\BE^A}$ and $\cbrn{\Be^a}$,
3. their metrics are denoted $\BG$ and $\Bg$ with the components
   $$G_{AB}=\dabrn{\BE_A,\BE_B}_\CV$$ and $$g_{ab}=\dabrn{\Be_a,\Be_b}_\CW$$,

respectively. Here, the indices pertaining to $\CV$ are uppercase
$(ABC\dots)$ and the indices pertaining to $\CW$ are lowercase
$(abc\dots)$.


**Definition:** Let $\BP:\CV\to\CW$ be a linear mapping. Then the **transpose**,
or **adjoint** of $\BP$, written $\BP\tra$, is the linear mapping

$$\begin{equation}
  \boxed{
    \BP\tra: \CW\to\CV
    \quad\text{such that}\quad
    \dabrn{\Bv,\BP\tra\Bw}_\CV = \dabrn{\BP\Bv,\Bw}_\CW
  }
\end{equation}$$

for all $\Bv\in\CV$ and $\Bw\in\CW$. Carrying out the products,


$$\begin{equation}
  G_{BA} v^B (P\tra){}^{A}{}_{d} w^d = g_{ab} P{}^{b}{}_{C}v^Cw^a.
\end{equation}$$

For arbitrary $\Bv$ and $\Bw$,

$$\begin{equation}
  G_{BA} (P\tra){}^{A}{}_{a} = g_{ab} P{}^{b}{}_{A}
\end{equation}$$

from which we can obtain the components of the transpose as

$$\begin{equation}
  \boxed{
    (P\tra){}^{A}{}_{a} = g_{ab} P{}^{b}{}_{B} G^{AB}
    \eqwith
    \BP\tra = (P\tra){}^{A}{}_{a} \BE_A\dyd\Be^a
    .
  }
\end{equation}$$

If $\BB:\CV\to\CV$ is a linear mapping,
it is called **symmetric** if $\BB=\BB\tra$.


**Definition:** Let $\BP:\CV\to\CW$ be a linear mapping. Then the **dual** of $\BP$
is a metric independent mapping

$$\begin{equation}
  \boxed{
    \BP^\ast: \CW^\ast\to\CV^\ast
    \quad\text{such that}\quad
    \abrn{\Bv,\BP^\ast\Bbeta}_\CV = \abrn{\BP\Bv,\Bbeta}_\CW
  }
\end{equation}$$

defined through natural pairings for all
$\Bv\in\CV$ and $\Bbeta\in\CW^\ast$.
Carrying out the products,

$$\begin{equation}
  v^A (P^\ast){}_{A}{}^{a} \beta_a
  = P{}^{b}{}_{B} v^B \beta_b.
\end{equation}$$

For arbitrary $\Bv$ and $\Bbeta$, we obtain the components of the dual mapping as

$$\begin{equation}
  \boxed{
    (P^\ast){}_{A}{}^{a}
    = P{}^{a}{}_{A}
    \eqwith
    \BP^\ast = (P^\ast){}_{A}{}^{a} \BE^A\dyd\Be_a
    = P{}^{a}{}_{A} \BE^A\dyd\Be_a
    .
  }
\end{equation}$$

To fully appreciate the symmetry that originates from the duality, we can think
of not just the mappings between $\CV$ and $\CW$, but also between their dual
spaces.
To this end we can enumerate four mappings corresponding to
$\cbr{\CV,\CV^\ast}\to\cbr{\CW,\CW^\ast}$
and their duals, corresponding to
$\cbr{\CW,\CW^\ast}\to\cbr{\CV,\CV^\ast}$. Their definitions can be found in
the table below.

<style>
.cellheader {
    background-color: #dddddd;
}
.isomorphism-table th, td {
    padding: 1ex;
    text-align: center;
}
.isomorphism-table {
    font-size: 80%;
}
</style>


<table class='isomorphism-table'>
<tr>
    <td colspan="4">Mappings</td>
</tr>
<tr>
    <th>
    <div class='cellheader'>$\BP\in\CW \dyd\CV^\ast$</div>
    <br>$P^a_{\idxsep A}=\BP(\Be^a,\BE_A)$
    <br>$\BP = P^{a}_{\idxsep A}\, \Be_a \dyd \BE^A$
    </th>
    <th>
    <div class='cellheader'>$\BQ\in\CW^\ast \dyd\CV^\ast$</div>
    <br>$Q_{aA}=\BQ(\Be_a,\BE_A)$
    <br>$\BQ = Q_{aA}\, \Be^a \dyd \BE^A$
    </th>
    <th>
    <div class='cellheader'>$\BR\in\CW \dyd\CV$</div>
    <br>$R^{aA}=\BR(\Be^a,\BE^A)$
    <br>$\BR = R^{aA}\, \Be_a \dyd \BE_A$
    </th>
    <th>
    <div class='cellheader'>$\BS\in\CW^\ast \dyd\CV$</div>
    <br>$S_a^{\idxsep A}=\BS(\Be_a,\BE^A)$
    <br>$\BS = S_{a}^{\idxsep A}\, \Be^a \dyd \BE_A$
    </th>
</tr>

<tr>
    <th>
    <div class='cellheader'>$\BP: \CV \to \CW$</div>
    <br>
    $\begin{aligned}
      \Bv &\mapsto \BP(\Be^a,\Bv) \Be_a \\
      &= \BP\Bv \\
      v^A\BE_A &\mapsto P^a_{\idxsep A} v^A \Be_a
    \end{aligned}$
    </th>
    <th><div class='cellheader'>$\BQ: \CV \to \CW^\ast$</div>
    <br>
    $\begin{aligned}
      \Bv &\mapsto \BQ(\Be_a,\Bv) \Be^a \\
      &= \BQ\Bv \\
      v^A\BE_A &\mapsto Q_{aA} v^A \Be^a
    \end{aligned}$
    </th>
    <th><div class='cellheader'>$\BR: \CV^\ast \to \CW$</div>
    <br>
    $\begin{aligned}
      \Balpha &\mapsto \BR(\Be^a,\Balpha) \Be_a \\
      &= \BR\Balpha\tra \\
      \alpha_A \BE^A &\mapsto R^{aA} \alpha_A \Be_a
    \end{aligned}$
    </th>
    <th><div class='cellheader'>$\BS: \CV^\ast \to \CW^\ast$</div>
    <br>
    $\begin{aligned}
      \Balpha &\mapsto \BS(\Be_a,\Balpha) \Be^a \\
      &= \BS\Balpha\tra \\
      \alpha_A \BE^A &\mapsto S_a^{\idxsep A} \alpha_A \Be^a
    \end{aligned}$
    </th>
</tr>


<tr>
    <th><div class='cellheader'>$\BP: \CW^\ast \times \CV \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Bbeta,\Bv) &\mapsto \BP(\Bbeta,\Bv) \\
      &=\Bbeta\BP\Bv \\
      &= \beta_a P^{a}_{\idxsep A} v^A
    \end{aligned}$
    </th>
    <th><div class='cellheader'>$\BQ: \CW \times \CV \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Bw,\Bv) &\mapsto \BQ(\Bw,\Bv) \\
      &= \Bw\tra\BQ\Bv \\
      &= w^a Q_{aA} v^A
    \end{aligned}$
    </th>
    <th><div class='cellheader'>$\BR: \CW^\ast \times \CV^\ast \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Bbeta,\Balpha) &\mapsto \BR(\Bbeta,\Balpha) \\
      &= \Bbeta\BR\Balpha\tra \\
      &= \beta_a R^{aA} \alpha_A
    \end{aligned}$
    </th>
    <th><div class='cellheader'>$\BS: \CW \times \CV^\ast \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Bw,\Balpha) &\mapsto \BS(\Bw,\Balpha) \\
      &= \Bw\tra\BS\Balpha\tra \\
      &= w^a S_a^{\idxsep A} \alpha_A
    \end{aligned}$
    </th>
</tr>

<tr>
    <td colspan="4">Duals</td>
</tr>

<tr>
    <th>
    <div class='cellheader'>$\BP^\ast\in \CV^\ast \dyd\CW$</div>
    <br>
    $P^{\ast \, a}_A=\BP^\ast(\BE_A,\Be^a)$
    <br>
    $\BP^\ast = P^{\ast \, a}_A \, \BE^A \dyd \Be_a$
    </th>
    <th>
    <div class='cellheader'>$\BQ^\ast\in \CV^\ast \dyd\CW^\ast$</div>
    <br>
    $Q^\ast_{Aa}=\BQ^\ast(\BE_A,\Be_a)$
    <br>
    $\BQ^\ast = Q^\ast_{Aa}\, \BE^A \dyd \Be^a$
    </th>
    <th>
    <div class='cellheader'>$\BR^\ast\in\CV \dyd \CW$</div>
    <br>
    $R^{\ast Aa}=\BR^\ast(\BE^A,\Be^a)$
    <br>
    $\BR^\ast = R^{\ast Aa}\, \BE_A \dyd \Be_a$
    </th>
    <th>
    <div class='cellheader'>$\BS^\ast\in\CV \dyd\CW^\ast$</div>
    <br>
    $S^{\ast A}{}_{a}=\BS^\ast(\BE^A,\Be_a)$
    <br>
    $\BS^\ast = S^{\ast A}{}_{a}\, \BE_A \dyd \Be^a$
    </th>
</tr>

<tr>
    <th>
    <div class='cellheader'>$\BP^\ast: \CW^\ast \to \CV^\ast $</div>
    <br>
    $\begin{aligned}
      \Bbeta &\mapsto \BP^\ast(\BE_A,\Bbeta) \BE^A \\
      &= \BP^\ast\Bbeta\tra \\
      \beta_a\Be^a &\mapsto P^{\ast \, a}_A \beta_a \BE^A
    \end{aligned}$
    </th>
    <th>
    <div class='cellheader'>$\BQ^\ast: \CW \to\CV^\ast$</div>
    <br>
    $\begin{aligned}
      \Bw &\mapsto \BQ^\ast(\BE_A,\Bw) \BE^A \\
      &= \BQ^\ast\Bw \\
      w^a\Be_a &\mapsto Q^\ast_{Aa} w^a \BE^A
    \end{aligned}$
    </th>
    <th>
    <div class='cellheader'>$\BR^\ast: \CW^\ast \to \CV$</div>
    <br>
    $\begin{aligned}
      \Bbeta &\mapsto \BR^\ast(\BE^A,\Bbeta) \BE_A \\
      &= \BR^\ast\Bbeta\tra \\
      \beta_a\Be^a &\mapsto R^{\ast Aa} w^a \BE_A
    \end{aligned}$
    </th>
    <th>
    <div class='cellheader'>$\BS^\ast: \CW \to\CV$</div>
    <br>
    $\begin{aligned}
      \Bw &\mapsto \BS^\ast(\BE^A,\Bw) \BE_A \\
      &= \BS^\ast\Bw \\
      w^a\Be_a &\mapsto S^{\ast A}{}_{a} w^a \BE_A
    \end{aligned}$
    </th>
</tr>

<tr>
    <th>
    <div class='cellheader'>$\BP^\ast: \CV \times \CW^\ast \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Bv,\Bbeta) &\mapsto \BP^\ast(\Bv,\Bbeta) \\
      &= \Bv\tra\BP^\ast\Bbeta\tra \\
      &= v^A P^{\ast \, a}_A \beta_a
    \end{aligned}$
    </th>
    <th>
    <div class='cellheader'>$\BQ^\ast: \CV \times \CW \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Bv,\Bw) &\mapsto \BR^\ast(\Bv,\Bw) \\
      &= \Bv\tra\BQ^\ast\Bw \\
      &= v^A Q^\ast_{Aa} w^a
    \end{aligned}$
    </th>
    <th>
    <div class='cellheader'>$\BR^\ast: \CV^\ast \times \CW^\ast \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Balpha,\Bbeta) &\mapsto \BS^\ast(\Balpha,\Bbeta) \\
      &= \Balpha\BR^\ast\Bbeta\tra \\
      &= \alpha_A R^{\ast Aa} \beta_a
    \end{aligned}$
    </th>
    <th>
    <div class='cellheader'>$\BS^\ast: \CV^\ast \times \CW \to \IR$</div>
    <br>
    $\begin{aligned}
      (\Balpha,\Bw) &\mapsto \BS^\ast(\Balpha,\Bw) \\
      &= \Balpha\BS^\ast\Bw \\
      &= \alpha_A S^{\ast A}{}_{a} w^a
    \end{aligned}$
    </th>
</tr>

<caption>
Tensors $\BP$, $\BQ$, $\BR$ and $\BS$ as linear mappings (top),
and their duals
$\BP^\ast$, $\BQ^\ast$, $\BR^\ast$ and $\BS^\ast$ (bottom).
In the respective tables, the first row displays the tensor spaces, basis
vectors and components of the subsequent mappings,
and the second and third row display the representations of
the tensor as linear and bilinear mappings respectively.
The results of the mappings are given in the mapping, matrix
and index representations respectively.
The mappings are over vectors $\Bv\in\CV$, $\Bw\in\CW$ and one-forms
$\Balpha\in\CV^\ast$, $\Bbeta\in\CW^\ast$.
</caption>
</table>


The commutative diagrams pertaining to these mappings
can be found in the figure below

<figure>
<img src="/blog/img/isomorphisms_between_vector_spaces/fig1.svg">
Commutative diagrams involving
the linear mappings $\BP,\BQ,\BR,\BS$ and
their dual $\BP^\ast,\BQ^\ast,\BR^\ast,\BS^\ast$
based on the metrics $\BG$ and $\Bg$
of $\CV$ and $\CW$.
</figure>
