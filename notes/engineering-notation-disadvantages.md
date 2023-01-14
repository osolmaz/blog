---
layout: page
title:  "Disadvantages of Engineering Notation in Finite Elements"
date: 2018-04-13
---


Suppose we have the following stiffness matrix of linear elasticity:

$$\begin{equation}
  A^{I\!J}_{ij} = \int_\Omega B^I_k \, C_{ikjl} \, B^J_l \,dv \label{eq:engnot1}
\end{equation}$$

where $\boldsymbol{B}^I = \nabla N^I$ are the gradients of the shape
functions $N^I$ and $\mathbb{C}$ is the linear elasticity tensor (you see the
contraction of their components in the equation).

Despite being of the most explicit form, these types of indicial expressions are
avoided in most texts on finite elements. There are two reasons for this:

- Engineers are not taught the Einstein summation convention.
- The presence of indices result in a seemingly cluttered expression.

They avoid the indicial expression by reshaping it into matrix multiplications.
In engineering notation, the left- and right-hand sides are reshaped as

$$\begin{equation}
A_{\alpha\beta} = \int_\Omega B_{\gamma\alpha}C_{\gamma\delta}B_{\delta\beta}
\,dv \label{eq:engnot2}
\end{equation}$$

which allows us to write

$$\begin{equation}
\boldsymbol{A} = \int_\Omega \tilde{\boldsymbol{B}}^T\tilde{\boldsymbol{C}}\tilde{\boldsymbol{B}} \,dv
  \label{eq:engnot3}
\end{equation}$$

The matrices $\tilde{\boldsymbol{B}}$ and $\tilde{\boldsymbol{C}}$ are set on with tildes in order to
differentiate them from the boldface symbols used in the previous sections.
Here,

- $\tilde{\boldsymbol{C}}$ is a matrix containing the unique components of the elasticity
tensor $\mathbb{C}$, according to the Voigt notation.
In this reshaping, only the minor symmetries are taken into account.
If the dimension of the vectorial problem is $d$, then $\tilde{\boldsymbol{C}}$ is of the size
$d(d+1)/2 \times d(d+1)/2$. For example, if the problem is 3 dimensional, $\tilde{\boldsymbol{C}}$
is of the size $6\times 6$:

$$\begin{equation}
  [\tilde{\boldsymbol{C}}] =
    \begin{bmatrix}
      C_{1111} & C_{1122} & C_{1133} & C_{1112} & C_{1123} & C_{1113} \\
      C_{2211} & C_{2222} & C_{2233} & C_{2212} & C_{2223} & C_{2213} \\
      C_{3311} & C_{3322} & C_{3333} & C_{3312} & C_{3323} & C_{3313} \\
      C_{1211} & C_{1222} & C_{1233} & C_{1212} & C_{1223} & C_{1213} \\
      C_{2311} & C_{2322} & C_{2333} & C_{2312} & C_{2323} & C_{2313} \\
      C_{1311} & C_{1322} & C_{1333} & C_{1312} & C_{1323} & C_{1313} \\
    \end{bmatrix}
    \label{eq:engnotC}
\end{equation}$$


- $\tilde{\boldsymbol{B}}$ is a $nd\times d(d+1)/2$ matrix whose components are
  adjusted so that \eqref{eq:engnot2} is equivalent to \eqref{eq:engnot1}. It
  has the components of $\boldsymbol{B}^I$ for $I=1,\dots,n$ where $n$ is the number of
  basis functions. Since $\tilde{\boldsymbol{B}}$ is adjusted to account for the reshaping
  of $\mathbb{C}$, it has many zero components. A 3d example:

$$\begin{equation}
  [\tilde{\boldsymbol{B}}] =
  \begin{bmatrix}
    B^1_1 & 0 & 0 & B^2_1 & 0 & 0 & \cdots & B^n_1 & 0 & 0 \\
    0 & B^1_2 & 0 & 0 & B^2_2 & 0 & \cdots & 0 & B^n_2 & 0 \\
    0 & 0 & B^1_3 & 0 & 0 & B^2_3 & \cdots & 0 & 0 & B^n_3 \\
    B^1_2 & B^1_1 & 0 & B^2_2 & B^2_1 & 0 & \cdots & B^n_2 & B^n_1 & 0 \\
    0 & B^1_3 & B^1_2 & 0 & B^2_3 & B^2_2 & \cdots & 0 & B^n_3 & B^n_2 \\
    B^1_3 & 0 & B^1_1 & B^2_3 & 0 & B^2_1 & \cdots & B^n_3 & 0 & B^n_1 \\
  \end{bmatrix}
  \label{eq:engnotB}
\end{equation}$$


Although \eqref{eq:engnot3} looks nice on paper, it is much less optimal for
implementation. Implementing it requires the implementation of
\eqref{eq:engnotB}, which adds another layer of complexity to the algorithm.
The same cannot be said for \eqref{eq:engnotC}, because using Voigt
notation might be more efficient in inelastic problems. In the most complex
problems, the most efficient method is to implement \eqref{eq:engnot1} in
conjunction with Voigt notation.

To prove the inefficiency of \eqref{eq:engnot3} we can readily compare it with
\eqref{eq:engnot1} in terms of required number of iterations. Indices in
\eqref{eq:engnot1} have the following ranges:

$$\begin{equation}
  I,J = 1,\dots,n
  \quad\text{and}\quad
  i,j,k,l = 1,\dots,d
\end{equation}$$


so $n^2d^4$ iterations are required. Indices in
\eqref{eq:engnot2} have the following ranges:

$$\begin{equation}
  \alpha,\beta=1,\dots,nd
  \quad\text{and}\quad
  \gamma,\delta=1,\dots,d(d+1)/2
\end{equation}$$

so

$$\begin{equation}
  (nd)^2\left(\frac{d(d+1)}{2}\right)^2 = n^2d^4\frac{(d+1)^2}{4}
\end{equation}$$

iterations are required. So engineering notation requires $(d+1)^2/4$ times more
equations than index notation. For $d=2$, engineering notation is $2.25$
times slower and for $d=3$ it is $4$ times slower. For example, calculation of a
stiffness matrix for $n=8$ and $d=3$ requires $20736$ iterations for
engineering notation, whereas it only requires $5184$ iterations for index notation.

Although \eqref{eq:engnot3} seems less cluttered, what actually happens is
that one
trades off complexity in one expression for a much increased complexity in
another one, in this case \eqref{eq:engnotB}.
And to make it worse, it results in a slower algorithm.

The only obstacle to the widespread adoption of
index notation seems to be its lack in undergraduate engineering curricula.
If engineers were taught the index notation and summation convention as well
as the formal notation, such expressions would not be as confusing at first
sight. A good place would be in elementary calculus and physics courses, where
one heavily uses vector calculus.


