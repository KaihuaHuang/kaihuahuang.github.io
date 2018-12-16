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

>- $w^l_{jk}$ denotes the weight for the connection from the $k^{th}$ neuron in $(l-1)^{th}$ layder to $j^{th}$ neuron in $l^{th}$ layer
>- $b^l_j$ denotes the bias of $j^{th}$ neuron in $l^{th}$ layer
>- $a^l_j$ denotes the activation(output) of $j^{th}$ neuron in $l^{th}$ layer
>- $z^l_j$ denotes intermediate result for $j^{th}$ neuron in $l^{th}$ layer
>- $\sigma$ denotes activation function

Above notations have relationship below:

$$a^l_j = \sigma(\sum_k w^l_{jk}a^{l-1}_k + b^l_j)$$

$$a^l_j = \sigma(z^l_j)$$

Vectorized:

$$a^l = \sigma(w^l*a^{l-1} + b^l)$$

$$a^l = \sigma(z^l)$$

>- $\delta^l_j$ denotes the error for $j^{th}$ neuron in $l^{th}$ layer

The definition of error $\delta^l_j$ is as below:

$$\delta^l_j = \frac{\partial C}{\partial z^l_j}$$

Then BP will relate intermediate result $\delta^l_j$ with our final target $\frac{\partial C}{\partial w}$ and $\frac{\partial C}{\partial b}$.

Next, let's prove backpropagation step by step.

$$\delta^L_j = \frac{\partial C}{\partial z^L_j} = \frac{\partial C}{\partial a^L}*\frac{\partial a^L}{\partial z^L_j} = \frac{\partial C}{\partial a^L_j}*\sigma'(z^L_j)$$

>- L is the last layer (output layer)
>- Cost function $C$ is a function of all the outputs $a^L$
>- $\frac{\partial a^L_m}{\partial z^L_j} = 0 (m \neq j)$. So $\frac{\partial a^L}{\partial z^L_j} = \frac{\partial a^L_j}{\partial z^L_j}$.










## References
[Neural Networks and Deep Learning](neuralnetworksanddeeplearning.com/chap1.html
)







