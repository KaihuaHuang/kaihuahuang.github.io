---
layout:     post
title:      Gradient Descent
subtitle:   Convex Optimization Basic
date:       2018-11-25
author:     William
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Optimization
    - Gradient Descent
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
# Gradient Descent
## Introduction

Suppose $C(w)$ is the cost function and it's convex which means it's free of local minimum.

<center><img src="http://ww1.sinaimg.cn/large/83d6b255ly1fxxn51wx60j208005eq2t.jpg"></center>

Gradient Descent is a way to minimize $C(w)$. Fisrt, let's define the gradient of $C(w)$.

$$\nabla C = (\frac{\partial C}{\partial w_1},\frac{\partial C}{\partial w_1},...,\frac{\partial C}{\partial w_n})^T$$

And we can use gradient simplify the notation in the first order Taylor Expansion on $C(w)$.

$$C(w + \triangle w) = C(w) + \nabla C * \triangle w + o(\triangle w)$$

So the $\triangle C$ would be as follow:

$$\triangle C = \nabla C * \triangle w$$

And gradient descent choose $\triangle w$ to be:

$$\triangle w = - \alpha  \nabla C$$

$\alpha$ is known as learning rate. Such $\triangle w$ can make sure $\triangle C$ is most negative. 

## The mathmatic behind $\triangle w$

Suppose there are two vector $u$ and $v$. And thier inner product are as below:

$$u\bullet v = |u||v|cos\theta$$

So when the angle $\theta$ between $u$ and $v$ equals 180, their inner product is most negative.

$$\triangle C = \nabla C * \triangle w$$

$$\triangle w = - \alpha  \nabla C$$

Actually, the direction of $\triangle w$ is determined by $-\nabla C$. So the angle between $\triangle w$ and $\nabla C$ is 180, then $\triangle C$ is most negative when the scale number $a$ is not considered.





