---
layout:     post
title:      Neural Network - 2
subtitle:   Backpropagation
date:       2018-12-16
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Neural Network
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

# Neural NetWork
## Backpropagation

Backpropagation is about understanding how changing the weights and biases in a network changes the cost function. Using backpropagation, our final target is calculate partial derivatives $\frac{\partial C}{\partial w}$ and $\frac{\partial C}{\partial b}$.

All the mathmatic behind it is just chain rule. First, let's go through the notation.

<center><img src = 'http://neuralnetworksanddeeplearning.com/images/tikz16.png'></center>

> $w^l_{jk}$ denotes the weight for the connection from the $k^{th}$ neuron in $(l-1)^{th}$ layder to $j^{th}$ neuron in $l^{th}$ layer
> $b^l_j$ denotes the bias of $j^{th}$ neuron in $l^{th}$ layer
> $a^l_j$ denotes the activation(output) of $j^{th}$ neuron in $l^{th}$ layer

Above notations have relationship below:

$$a^l_j = \sigma(\sum_k w^l_{jk}a^{l-1}_k + b^l_j)$$

Vectorized ($w_{j*k}$, $a_{k*1}$, $b_{j*1}$):

$$a^l = \sigma(w^l*a^{l-1} + b^l)$$




## References
[Neural Networks and Deep Learning](neuralnetworksanddeeplearning.com/chap1.html
)







