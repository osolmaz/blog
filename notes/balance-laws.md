---
layout: post
title:  "Balance Laws"
tags: math physics
comments: true
---

{% include shortsym.html %}
{% include lazyeqn.html %}

Calculus is all about relating the change in one quantity to another quantity.

$$ \Var A = B$$

Imagine it this way: You have a box full of marbles, and you decide to put some
more in.
$A$ is the variable representing the amount of marbles, while $B$ is the variable
representing the amount of marbles that you put in. If you had $A_1$ marbles at
the beginning, you have

$$A_2 = A_1+\Var A = A_1 + B$$

marbles following your action. This is the most fundamental algebraic pattern
that characterizes balance laws.

Take the [first law of thermodynamics](https://en.wikipedia.org/wiki/First_law_of_thermodynamics)
for example---a.k.a. balance of energy. We have

$$\Var U = Q + W$$

where $U$ is the internal energy of a closed system, $Q$ is the amount of heat
supplied to the system, and $W$ is the amount of work done on the system on its
surroundings. Here, $A\equiv U$ and $B\equiv Q+W$. Despite having three
quantities, it is the combined effect of two which is related to the remaining
quantity. Balance laws derived by physicists and chemists can get quite
complex and hard to understand.

It's always that **the change in one quantity
is related to the combined effect of remaining quantities**. Keeping
separate track of
your main variable $A$ and affecting variables that compose $B$ gives you a
mental model which helps you remember and even build your own balance laws.

# Introducing Time

Let $A: t \to A(t)$ be a function of time. We can rewrite the
equation in terms of the change in $A$ in a time period $\Var t$:

$$\frac{\Var A}{\Var t} = C$$

where the new variable $C$ represents the change in quantity in $\Var t$ amount
of time. In our previous analogy, $C$ is the amount of marbles put in, say,
a minute. As $\Var t \to 0$, we have

$$\deriv{A}{t} = C(t).$$

This prototypical balance law allows us to relate the *rates of change* of
quantities.

Let's introduce time into the balance of energy. The equation becomes

$$\deriv{U}{t} = P_T(t) + P_M(t)$$

where the new quantities $P_T$ and $P_M$ are called *thermal power*
and *mechanical power*, representing the thermal and mechanical work done on the
system per unit time, respectively. Given power functions and initial
conditions, integrating them would give us the evolution of the internal energy
through time.

# Introducing Space

Let's say we are not satisfied with an abstract box where the amount of stuff
that goes in is measured automatically. We want to write a balance law over
different shapes of bodies and we need to specify exactly where the stuff goes
in and out.

To do that, we need to rephrase our laws to work over a continuous domain. The
branch of physics that focuses on such problems is called continuum mechanics.

We introduce our spatial domain $\Omega$ and its boundary $\del\Omega$. Our
quantities now vary over both space and time, so we need to integrate them over
the whole domain in order to relate them:

$$\ddt\int_\Omega a \dx = \int_\Omega b \dx + \int_{\del\Omega} \Bc \dtp \Bn \ds$$

where

- $a(x,t)$ is the variable representing the main continuous quantity,
- $b(x,t)$ is the variable representing the rate of change of the quantity
  **inside** the domain,
- and $\Bc(x,t)$ is the variable representing the negative rate of change of the
  quantity **on the boundary** of the domain---negative due to surface normals
  $\Bn$ having outward direction by definition.

Notice that when we introduce space, our prototypical balance law needs an
additional vectorial quantity, $\Bc$. In physical laws, one needs
to differentiate actions *inside* a body from actions *on the surface* of the
body. That's because one is over a volume and the other over an area, and they
have to be integrated separately.

The area integral is actually a flux where the vectorial quantity $\Bc$
is penetrating the surface with a given direction. Given that it's positive when
stuff exits the domain, it's called the **efflux** of the underlying quantity.
Similarly, we name the rate of change field $b$ as the **supply**
of the underlying quantity, because it being positive results in an
increase.

The idea is to get rid of the integrals by a process called "localization". In
order to localize, we have to convert the surface integral into a volume
integral using the divergence theorem:

$$\int_{\del\Omega} \Bc \dtp \Bn \ds = \int_\Omega \nabla\dtp\Bc \dx$$

Assuming $\Omega$ doesn't move, we can also write

$$\ddt\int_\Omega a \dx = \int_\Omega \deriv{a}{t} \dx$$

Collecting the results, we have

$$\int_\Omega \deriv{a}{t} \dx = \int_\Omega b \dx + \int_\Omega \nabla\dtp\Bc \dx$$

Notice that all integrals are over $\Omega$ now. This allows us to make the
balance law more strict by enforcing it point-wise:

$$\deriv{a}{t} = b + \nabla\dtp \Bc \quad \forall x \in \Omega$$

This is the localized version of the prototypical balance law that is used
everywhere in continuum mechanics.
Unfortunately, I can't give the energy balance example, because it would require
too many additional definitions. For that, I recommend the excellent
[Mathematical Foundations of Elasticity](http://store.doverpublications.com/0486678652.html)
by Marsden and Hughes.

# Conclusion

In physics and chemistry, one shouldn't blindly memorize formulas, but try to
see the underlying logic. In this case, I tried to elucidate balance laws,
which all build upon the same algebraic and geometrical concepts. I went from
discrete to continuous by introducing time and space to the equations, which
became more complex but retained the same idea: putting things in a box and
trying to calculate how that changes the contents.

<!-- **Reynolds Transport Theorem:** For an Eulerian quantity $a(\Bx,t)$, -->
<!-- the following identity holds -->

<!-- $$ -->
<!-- \ddt \int_\Omega a \dx -->
<!-- =\int_\Omega \rbr{\partd{a}{t} + a\div\Bv} \dx -->
<!-- $$ -->

<!-- where $\Bv(x, t)$ is the velocity of a point, given that the domain is moving. -->
<!-- I won't offer a proof because it's out of the scope of this post. -->

<!-- Applying the theorem on the balance law, we get -->

<!-- $$\int_\Omega \rbr{\partd{a}{t} + a\div\Bv} \dx = \int_\Omega b \dx + \int_{\del\Omega} \Bc \dtp \Bn \ds $$ -->

