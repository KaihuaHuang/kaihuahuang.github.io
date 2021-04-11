---
layout:     post
title:      Complex Number - 1
subtitle:   Basics
date:       2021-04-11
author:     William
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Complex Number
    - Exponential Notation
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: { 
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true
    }
  });
  </script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Algebra and Geometry in the Complex Plane
## The Complex Plane

The complex number can be expressed as $z = x + yi$
 - x is called the real part of z; $x = Re(z)$.
 - y is called the imaginary part of z; $y = Im(z)$.

We also call the set of complex number as complex plane $\mathbb{C}$. Real number is the subset of the complex numbers.

## Simple Algebra
### Add

Suppose we have $z_1 = x + yi$ and $z_2 = u + vi$, we define the complex number adding as:

$$z_1 + z_2 = \underbrace{(x+u)}_{Re(z_1+z_2)} + \underbrace{(y+v)}_{Im(z_1+z_2)}i$$

### Modulus

Define the modulus of complext number as:

$$|z| = \sqrt{x^2 + y^2}$$

Properties:
- $|z_1 z_2| = |z_1|\cdot|z_2|$

Inequalities:
- $-|z| \leq Re(z) \leq |z|$
- $-|z| \leq Im(z) \leq |z|$
- $|z+w| \leq |z| + |w|$
- $|z-w| \geq |z| - |w|$

> $( z - w + w) \leq |z-w| + |w|$ 

### Multiplication

Definition

$$z_1 * z_2 = (xu + yv) + (xv + yu)i$$

Properties:
- **associative**: $(z_1z_2)z_3 = z_1(z_2z_3)$
- **commutative**: $z_1z_2= z_2z_1$
- **distributive**: $z_1(z_2 + z_3) =z_1z_2 + z_1z_3$

### Conjugate

Definition

$$\bar{z} = x-yi$$

Properties:

- $\bar{z}z = x^2 + y^2 = |z|^2$
- $\bar{\bar{z}} = z$
- $\overline{z_1 + z_2} = \bar{z_1} + \bar{z_2}$
- $|\bar{z}| = |z|$
- $\frac{1}{z} = \frac{\bar{z}}{\bar{z}z} = \frac{\bar{z}}{|z|^2}$
- $\frac{z}{w} = \frac{\bar{z}}{\bar{w}}$

### Division

Definition

$$\frac{z}{w} = \frac{z\cdot \bar{w}}{w \cdot \bar{w}} = \frac{z\cdot \bar{w}}{|w|^2}$$

So that the numerator is real number.


# Polar Representation and the Root of Complex Numbers
## Polar Representation
### Difinition
Similar to polar coordinates, the complex number can be expressed by radius $r$ and angle $\theta$
$$z = x+yi=r(\sin\theta + \cos\theta i)$$

where $r = |z|$ and $\tan \theta = \frac{y}{x}$.

The principal argument of z, called Arg z, is the value of $\theta \ \text{for which} -\pi \le \theta \leq \pi$. 

Thus the $\theta$ is defined:

$$\theta = arg z = \{Arg z + 2\pi k: k = 0, \pm1, \pm2, ...\}$$

We can also use simplified exponential notation:

$$z = r\exp(i\theta)$$

### Properties

Using exponential notation, we have the following properties:
- $|e^{i\theta}| = 1$
- $\overline{e^{i\theta}} = e^{-i\theta}$
- $e^{i\theta} \cdot e^{i\varphi} = e^{i(\theta + \varphi)}$
- $(e^{i\theta})^n = e^{in\theta}$

> $(\cos \theta + i \sin \theta)^n = \cos(n\theta) + i \sin(n\theta)$
  
The multiplication in exponential form:

$$z \cdot w = r_1\cdot r_2 \cdot e^{i(\theta + \varphi)}$$

## Root of Complex Numbers

To find the n th root of $w = \rho e^{i\varphi}$, we can think reversively. Analytically, solve the following equation:

$$z^n = r^n e^{in\theta} = w$$

We got

$$ \begin{cases}
r & = \sqrt[n]{\rho} \\
\theta &= \frac{\varphi + 2k\pi}{n}, \ k = 0,1,2 ... n-1
\end{cases}
$$

# Topology in the Plane
## Sets in the Complex Plane
### Circles and Disk

We defined circles and disk centered at $z_0$ as:

$$
\begin{aligned}
B_r(z_0) &= \{ z \in \mathbb{C}: \text{z has distance less than }r\text{ from }z_0\}\\
&=\{ z \in \mathbb{C}: |z-z_0| < r\}\\
K_r(z_0) &= \{ z \in \mathbb{C}: \text{z has distance }r\text{ from }z_0\}\\
&=\{ z \in \mathbb{C}: |z-z_0| = r\}
\end{aligned}
$$

### Interior Points and Boundary Points

The points type is defined with respect to a set $E$.

A point $z_0$ is an interior point of E if:

$$\exists r, B_r(z_0) \subset E$$

A point $z_0$ is a boundary point of E if:

$$\forall r, B_r(z_0) \text{ contains a point in E and a point not in E}$$

We also define the set of all boundary points of E as $\partial E$ 

### Open and Closed Sets 

A set is open set if **every one of its points is an interior point**.

A set is closed set if **it contains all of its boundary points**.

### Closure and Interior of a Set
Closure set of E is the set E toghther with all its boundary points:

$$\bar{E} = E \cup \partial E$$

The interior set of E is $\dot{E}$ which contains all of interior points of E.

### Connectedness
Intuitively, a set is connected if it is “in one piece”.

To make it more precise, we difine seperate set first:

$\text{Two sets X, Y in }\mathbb{C} \text{ are separated if there are disjoint open set U, V so that } X\subset U\text{ and }Y \subset V.$

> Disjoint means that $U \cap V = \emptyset$

Then we define connected set as:

    If it is impossible to find two separated non-empty sets whose union equals W, we call W as connected.

It's hard to check whether a set is connected!!! But for open set we have a simple rule to determine:

    Let G be an open set in C. Then G is connected if and only if any two points in G can be joined in G by successive line segments.

![](figure/ConnetedSet.png)

### Bounded Sets
We define bounded set as:

    A set A in $\mathbb{C}$ is bounded if there exists a number R > 0 such that $A  \subset B_R(0)$. If no such R exists then A is called unbounded.