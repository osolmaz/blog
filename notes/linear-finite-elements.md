---
layout: page
title:  "Linear Finite Elements"
date: 2017-11-14
---

{% include shortsym.html %}
{% include lazyeqn.html %}

Beginning with this post, I'll be publishing about the basics of finite element
formulations, from personal notes that accumulated over the years. This one is
about *linear* and *scalar* problems which came to be the "Hello World" for FE.
Details regarding spaces and discretization are omitted for the sake of brevity.
For those who want to delve into theory, I recommend ["The Finite Element Method:
Theory, Implementation, and Applications"](https://www.springer.com/gp/book/9783642332869)
by Larson and Bengzon.

The weak formulation of a canonical linear problem reads

> Find $u\in V$ such that
>
> $$\begin{equation}
>   a(u, v) = b(v)
>   \label{eq:femlinear1}
> \end{equation}$$
>
> for all $v \in V$ where $a(\cdot, \cdot)$ is a bilinear form and $b(\cdot)$ is a linear form.

We define the discretization of $u$ as

$$\begin{equation}
  u_h := \suml{J=1}{\nnode} u^J N^J
  ,\quad
  u_h \in V_h
  \quad\text{where}\quad
  V_h\subset V
\end{equation}$$

The discretization $u_h$ is a linear combination of **basis functions**
$N^J$ and corresponding scalars $u^J$, $J=1,\dots,\nnode$ so that $V_h$ is a
subset of $V$.
The discretization of \eqref{eq:femlinear1} then reads

$$\begin{equation}
  a(u_h, v_h) = b(v_h)
  \quad
  \forall v_h \in V_h
  .
\end{equation}$$

We then have

$$\begin{equation}
  a\rbr{\suml{J=1}{\nnode} u^J N^J, \suml{I=1}{\nnode} v^I N^I}
  = b\rbr{\suml{I=1}{\nnode} v^I N^I}
\end{equation}$$

Using the linearity properties,

$$\begin{equation}
  a(\alpha u, \beta v) = \alpha\beta\, a(u,v)
  \eqand
  b(\alpha v) = \alpha b(v)
\end{equation}$$

we obtain

$$\begin{equation}
  \suml{I=1}{\nnode} \suml{J=1}{\nnode}
  u^J v^I a(N^J, N^I)
  = \suml{I=1}{\nnode} v^I b(N^I)
  .
  \label{eq:femlinear2}
\end{equation}$$

For arbitrary test function values $v^I$, we can express \eqref{eq:femlinear2}
as a system of $\nnode$ equations

$$\begin{equation}
  \suml{J=1}{\nnode}
  u^J a(N^J, N^I) = b(N^I)
  \label{eq:femlinear3}
\end{equation}$$

for $I = 1,2,\dots,\nnode$. If we expand the summations as

$$\begin{alignat*}{6}
  &  a(N^1, N^1) u^1 &&+  a(N^2, N^1) u^2 &&+ \cdots &&+  a(N^{\nnode}, N^1) u^{\nnode} &&\quad=\quad b(N^1) \\
  &  a(N^1, N^2) u^1 &&+  a(N^2, N^2) u^2 &&+ \cdots &&+  a(N^{\nnode}, N^2) u^{\nnode} &&\quad=\quad b(N^2) \\
  & \qquad\vdots && \qquad\quad\;\vdots && \quad\;\;\vdots && \qquad\qquad\vdots && \qquad\qquad\vdots \\
  &  a(N^1, N^{\nnode}) u^1 &&+  a(N^2, N^{\nnode}) u^2 &&+ \cdots &&+  a(N^{\nnode}, N^{\nnode}) u^{\nnode} &&\quad=\quad b(N^{\nnode})
\end{alignat*}$$

we can see that the terms with $a$ constitute a matrix $\BA$ and
the terms with $b$ constitute a vector $\Bb$, allowing us to write

$$\begin{equation}
  \BA\Bu = \Bb
  \label{eq:discrete9}
\end{equation}$$

where we chose to express the unknown coefficients $u^I$ as a vector
$\Bu = [u^1,u^2,\dots,u^{\nnode}]\tra$.

\\It can be seen that the components of the $\BA$ and $\Bb$ are defined as

$$\begin{equation}
  \boxed{
    \Aelid{I\!J}{} = a(N^J,N^I)
    \eqand
    b^I = b(N^I),
  }
\end{equation}$$

we can express the linear system as

$$\begin{alignat*}{6}
  &  \Aelid{11}{} u^1 &&+  \Aelid{12}{} u^2 &&+ \cdots &&+  \Aelid{1\nnode}{} u^{\nnode} &&\quad=\quad b^1 \\
  &  \Aelid{21}{} u^1 &&+  \Aelid{22}{} u^2 &&+ \cdots &&+  \Aelid{2\nnode}{} u^{\nnode} &&\quad=\quad b^2 \\
  & \quad\vdots && \qquad\;\vdots && \quad\;\;\vdots && \qquad\;\vdots && \qquad\quad\;\;\vdots \\
  &  \Aelid{\nnode 1}{} u^1 &&+  \Aelid{\nnode 2}{} u^2 &&+ \cdots &&+  \Aelid{\nnode\nnode}{} u^{\nnode} &&\quad=\quad b^{\nnode}
\end{alignat*}$$

Note that with the given definitions, \eqref{eq:femlinear3} becomes

$$\begin{equation}
  \boxed{
    \suml{J=1}{\nnode}
    \Aelid{I\!J}{} \,u^J = b^I
    \quad\text{for}\quad
    I=1,2,\dots\nnode.
  }
  \label{eq:discrete10}
\end{equation}$$



## Example: Poisson's Equation

In the weak form of Poisson's equation

$$\begin{equation}
  \begin{alignedat}{4}
    - \Var u &= f \quad && \text{in} \quad && \Omega \\
    u &= 0 \quad && \text{on} \quad && \del\Omega
  \end{alignedat}
\end{equation}$$

The weak formulation reads

> Find $u\in V$ such that
>
> $$\begin{equation}
>   - \int_\Omega \Delta(u) v \dv= \int_\Omega f v \dv
> \end{equation}$$
>
> for all $v\in V$ where $V=H^1_0(\Omega)$.

Applying integration by parts and divergence theorem on the left-hand side

$$\begin{equation}
  \begin{aligned}
    \int_\Omega \Delta(u) v \dv
    &= \int_\Omega \nabla \dtp (\nabla (u) v)  \dv
    - \int_\Omega \nabla u\dtp\nabla v  \dv \\
    &= \underbrace{\int_{\del\Omega}  v (\nabla u\dtp\Bn)  \da}_{v = 0
      \text{ on } \del\Omega}
    - \int_\Omega \nabla u\dtp\nabla v  \dv \\
  \end{aligned}
\end{equation}$$

We have the following variational forms:

$$\begin{equation}
  \begin{aligned}
    a(u,v) &= \int_{\Omega} \nabla u \dtp \nabla v \dv\\
    b(v) &= \int_{\Omega} f \, v \dv\\
  \end{aligned}
\end{equation}$$

Following \eqref{eq:femlinear3}, we can calculate the stiffness matrix
$\BA$ as

$$\begin{equation}
  \begin{aligned}
    \Aelid{I\!J}{} = a(N^J, N^I)
    &= \int_{\Omega} \nabla N^J \dtp \nabla N^I \dv \\
    &= \int_{\Omega} \BB^J \dtp \BB^I \dv
  \end{aligned}
\end{equation}$$

where we have defined the gradient of the basis functions as

$$\begin{equation}
  \BB^I := \nabla N^I\,.
\end{equation}$$

Similarly, we integrate the force term into a vector $\Bb$ as

$$\begin{equation}
  \begin{aligned}
    b^I &= \int_{\Omega} f N^I \dv
  \end{aligned}
\end{equation}$$


