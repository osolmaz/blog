---
layout: page
title:  "Simple recursive implementation of deCasteljau's algorithm for Bezier curves in Python"
date: 2014-11-17
---

I could not find a simple demonstrative example of *\*insert title here\**. I am
leaving this out here for future reference.

Note that a recursive implementation for deCasteljau's is not efficient, since
it results in unnecessary multiple computation of some intermediary points.

```python
def deCasteljau(points, u, k = None, i = None, dim = None):
    """Return the evaluated point by a recursive deCasteljau call
    Keyword arguments aren't intended to be used, and only aid
    during recursion.

    Args:
    points -- list of list of floats, for the control point coordinates
              example: [[0.,0.], [7,4], [-5,3], [2.,0.]]
    u -- local coordinate on the curve: $u \in [0,1]$

    Keyword args:
    k -- first parameter of the bernstein polynomial
    i -- second parameter of the bernstein polynomial
    dim -- the dimension, deduced by the length of the first point
    """
    if k == None: # topmost call, k is supposed to be undefined
        # control variables are defined here, and passed down to recursions
        k = len(points)-1
        i = 0
        dim = len(points[0])

    # return the point if downmost level is reached
    if k == 0:
        return points[i]

    # standard arithmetic operators cannot do vector operations in python,
    # so we break up the formula
    a = deCasteljau(points, u, k = k-1, i = i, dim = dim)
    b = deCasteljau(points, u, k = k-1, i = i+1, dim = dim)
    result = []

    # finally, calculate the result
    for j in range(dim):
        result.append((1-u) * a[j] + u * b[j])

    return result
```

A demonstration of the above function

```python
import numpy as np
import pylab as pl
import math

# insert deCasteljau function definition here

points = [[0.,0.], [7,4], [-5,3], [2.,0.]]

def plotPoints(b):
    x = [a[0] for a in b]
    y = [a[1] for a in b]
    pl.plot(x,y)

curve = []

for i in np.linspace(0,1,100):
    curve.append(deCasteljau(points, i))

plotPoints(curve)
pl.show()
```

#### For Rational Bezier Curves

With a small modification, same function can be used for rational Bezier
curves

```python
def rationalDeCasteljau(points, u, k = None, i = None, dim = None):
    """Return the evaluated point by a recursive deCasteljau call
    Keyword arguments aren't intended to be used, and only aid
    during recursion.

    Args:
    points -- list of list of floats, for the control point coordinates
              example: [[1.,0.,1.], [1.,1.,1.], [0.,2.,2.]]
    u -- local coordinate on the curve: $u \in [0,1]$

    Keyword args:
    k -- first parameter of the bernstein polynomial
    i -- second parameter of the bernstein polynomial
    dim -- the dimension, deduced by the length of the first point
    """
    if k == None: # topmost call, k is supposed to be undefined
        # control variables are defined here, and passed down to recursions
        k = len(points)-1
        i = 0
        dim = len(points[0])-1

    # return the point if downmost level is reached
    if k == 0:
       return points[i]

    # standard arithmetic operators cannot do vector operations in python,
    # so we break up the formula
    a = rationalDeCasteljau(points, u, k = k-1, i = i, dim = dim)
    b = rationalDeCasteljau(points, u, k = k-1, i = i+1, dim = dim)
    result = []

    # finally, calculate the result
    for j in range(dim+1):
        result.append((1-u) * a[j] + u * b[j])

    # at the end of first and topmost call, when the recursion is done,
    # normalize the result by dividing by the weight of that point
    if k == len(points)-1:
        for i in range(dim):
            result[i] /= result[dim]
            # dimension is also the index with the weight

    return result
```

We can demonstrate by e.g. comparing the algorithm's results with a circular arc

```python
import numpy as np
import pylab as pl
import math

# insert rationalDeCasteljau function definition here

points = [[1.,0.,1.], [1.,1.,1.], [0.,2.,2.]]

def plotPoints(b):
    x = [a[0] for a in b]
    y = [a[1] for a in b]
    pl.plot(x,y)

curve = []

# limit to 5 points to show the difference with analytic solution
for i in np.linspace(0,1,5):
    curve.append(rationalDeCasteljau(points, i))

plotPoints(curve)

# plot the actual circular arc
arc_x = np.linspace(0,1,100)
arc_y = []

for i in arc_x:
    arc_y.append(math.sqrt(1-i*i))

pl.plot(arc_x, arc_y)

pl.show()
```

I am not actually working on Bezier curves, but NURBS. My reference for studying
is _The NURBS Book_ by Piegl and Tiller, which is excellent so far.
