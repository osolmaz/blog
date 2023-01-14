---
layout: page
title:  "Taylor and Volterra Series"
date: 2017-09-20
---

{% include shortsym.html %}
{% include lazyeqn.html %}


In the theory of computational mechanics, there are a few operations used that
are not taught in Calculus 101, which can be confusing without taking a lecture
in calculus of variations. One of them is taking *variations* (a.k.a. Gateaux
derivatives), akin to taking directional derivatives, but with functions of
functions called *functionals*.

You need to take variations when you are linearizing a nonlinear problem for the
purpose of solving with a numerical scheme. Linearization is the process of
expanding a function or functional into a series, and discarding terms that are
of order higher than linear---i.e. quadratic, cubic, quartic, etc. These
expansions are called Taylor for functions, and Volterra for functionals.

# Taylor Series

A function $f:\IR\to\IR$ can be expanded about a point $\bar{x}$ as a power series:

$$\begin{equation}
  \begin{aligned}
    f(x) &= f(\bar{x}) + \frac{\dif f}{\dif x}\evat_{\bar{x}} \frac{(x-\bar{x})}{1!}
    + \frac{\dif^2 f}{\dif x^2}\evat_{\bar{x}}\frac{(x-\bar{x})^2}{2!}
    + \frac{\dif^3 f}{\dif x^3}\evat_{\bar{x}}\frac{(x-\bar{x})^3}{3!}
    + \cdots \\
    &= \suml{n=0}{\infty} \frac{\dif^n f}{\dif x^n} \evat_{\bar{x}}\frac{(x-\bar{x})^n}{n!}
  \end{aligned}
\end{equation}$$

Letting $x$ be a perturbation $\var x$ from the expansion point
$\bar{x}$, that is $x\to\bar{x}+\var x$, the series can also be phrased as follows

$$\begin{equation}
  \begin{aligned}
    f(\bar{x}+\var x) &= f(\bar{x}) + \frac{\dif f}{\dif x}\evat_{\bar{x}}
    \frac{\var x}{1!}
    + \frac{\dif^2 f}{\dif x^2}\evat_{\bar{x}}\frac{\var x^2}{2!}
    + \frac{\dif^3 f}{\dif x^3}\evat_{\bar{x}}\frac{\var x^3}{3!}
    + \cdots \\
    &= \suml{n=0}{\infty} \frac{\dif^n f}{\dif x^n} \evat_{\bar{x}}\frac{\var x^n}{n!}
  \end{aligned}
  \label{eq:2}
\end{equation}$$

This is what is taught in Calculus 101 and everyone knows. Now for the part that
you may have missed:

# Variation

Let $X$ be the space of functions $\IR\to\IR$. The **variation**
of a functional $F\in X$ is defined as

$$\begin{equation}
  \boxed{
    \varn{F(u)}{u}{v}
    := \lim_{\eps\to 0} \frac{F(u+\eps v) - F(u)}{\eps}
    \equiv \deriv{}{\eps} F(u + \eps v) \evat_{\eps = 0}
  }
\end{equation}$$

where $v \in X$ is called the **perturbation** of the variation. This
operation is analogous to taking the directional derivative of a function.

## Shorthand notation

When working with variational formulations, writing out variations can be a
bit of a hassle if there are many symbols involved. Therefore we use the
following shorthand for variations:

$$\begin{equation}
  \Var F := \varn{F(u)}{u}{v}
\end{equation}$$

Here, we assume that there is no chance of confusing the varied function or
perturbation. We use this shorthand in contexts where the perturbation does
not play an important role.

The shorthand for evaluation is

$$\begin{equation}
  \bar{F} := F(\bar{u})
  \eqand
  \bar{\Var} F := \varn{F(u)}{u}{v}\evat_{\bar{u}}
\end{equation}$$

where there is no risk of confusion for $\bar{u}\in X$.

# Volterra Series

Let $X$ be the space of functions $\IR\to\IR$. Analogous to the Taylor series,
a functional $F\in X$ can be expanded about a point $\bar{u}$ as a power series:

$$\begin{equation}
  \boxed{
    \begin{aligned}
      F(\bar{u}+v) &= F(\bar{u})
      + \frac{1}{1!} \varn{F(u)}{u}{v}\evat_{\bar{u}}
      + \frac{1}{2!} D^2_u F(u) \dtp v^2 \evat_{\bar{u}}
      + \frac{1}{3!} D^3_u F(u) \dtp v^3 \evat_{\bar{u}}
      + \cdots \\
      &= \suml{n=0}{\infty} \frac{1}{n!} D^n_u F(u) \dtp v^n \evat_{\bar{u}}
    \end{aligned}
  }
  \label{eq:6}
\end{equation}$$

where $v\in X$ is the perturbation of the expansion. This is called
the **Volterra series** expansion of $F$. Verbally, *the
Volterra series expansion of a functional about a function is the infinite sum of the
variations of the functional with increasing degree, evaluated at that function,
each divided by the factorial of the degree.*

In shorthand notation, the expansion is rendered

$$\begin{equation}
  \boxed{
    F = \bar{F}
    + \frac{\bar{\Var} F}{1!}
    + \frac{\bar{\Var}^2 F}{2!}
    + \frac{\bar{\Var}^3 F}{3!}
    + \cdots
  }
  \label{eq:7}
\end{equation}$$

To me, there is an elegance in \eqref{eq:7} that is not reflected in
\eqref{eq:2} or \eqref{eq:6}.



