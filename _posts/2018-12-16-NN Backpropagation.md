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
## Backpropagation Proof

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


**Step.1 Forward propagation.**

The input $x$ can be treated as $a^1$. Once we know $w$ and $b$, all the activation $a^2,a^3...a^L$ and intermediate result $z^2, z^3 ... z^L$ can be calculated using following equation:

$$z^l = w^l*a^{l-1} + b^l$$

$$a^l = \sigma(z^l)$$

This is called Forward Propagation.

**Step.2 Calculate error at last layer**

$$\delta^L_j = \frac{\partial C}{\partial z^L_j} = \frac{\partial C}{\partial a^L}*\frac{\partial a^L}{\partial z^L_j} = \frac{\partial C}{\partial a^L_j}*\sigma'(z^L_j)$$

>- L is the last layer (output layer)
>- Cost function $C$ is a function of all the outputs $a^L$
>- $\frac{\partial a^L_m}{\partial z^L_j} = 0 (m \neq j)$. So $\frac{\partial a^L}{\partial z^L_j} = \frac{\partial a^L_j}{\partial z^L_j}$.
>- $a^L_j = \sigma(z^L_j)$

Vectorized:

$$\delta^L = \frac{\partial C}{\partial a^L} \odot \sigma'(z^L) = \bigtriangledown_aC \odot \sigma'(z^L)$$

>- $\odot$ is element-wise product.
>- $\bigtriangledown_aC$ denotes the gradient of function $C$ on $a$.

**Step.3 Back propagation error**

$$z^{l+1}_j = \sum_k w^{l+1}_{jk}*a^l_k + b^{l+1}_j = \sum_k w^{l+1}_{jk}*\sigma(z^l_k) + b^{l+1}_j$$

$$\frac{\partial z^{l+1}_j}{\partial z^l_k} = w^{l+1}_{jk} * \sigma'(z^l_k)$$

$$\delta^l_k = \frac{\partial C}{\partial z^l_k} = \sum_j\frac{\partial C}{\partial z^{l+1}_j} * \frac{\partial z^{l+1}_j}{\partial z^l_k} = \sum_j\delta^{l+1}_j*w^{l+1}_{jk} * \sigma'(z^l_k)$$


Vectorized:

$$z^{l+1} = w^{l+1}*\sigma(z^l) + b^{l+1}$$

$$\delta^l = ((w^{l+1})^T\delta^{l+1})\odot\sigma'(z^l)$$

Uisng above equation, we can calculate backforward for all the $\delta^2, \delta^3 ... \delta^L$.

**Step.4 Calculate partial derivatives**

$$\frac{\partial C}{\partial b^l_j} = \frac{\partial C}{\partial z^l_j}*\frac{\partial z^l_j}{\partial b^l_j} = \delta^l_j*1 = \delta^l_j$$

$$\frac{\partial C}{\partial w^l_{jk}} = \frac{\partial C}{\partial z^l_j}*\frac{\partial z^l_j}{\partial w^l_{jk}} = \delta^l_j*a^{l-1}_k$$

Vectorized:

$$\frac{\partial C}{\partial b^l} = \delta^l$$

$$\frac{\partial C}{\partial w^l} = \delta^l*(a^{l-1})^T$$

## BP Slow Learning Issues - Saturated Neuron

Suppose using sigmoid function as activation function, its shape look like below:

<center><img src = 'http://ww1.sinaimg.cn/large/83d6b255ly1fy70ehhy3oj20bn07l747.jpg'></center>

As you can see, as input approching infinite or infinitesimal, its derivative approching 0.

$$\delta^L_j = \frac{\partial C}{\partial a^L_j}*\sigma'(z^L_j)$$

$$\delta^l_k = \sum_j\delta^{l+1}_j*w^{l+1}_{jk} * \sigma'(z^l_k)$$

In above two key equations, they both have $\sigma'$. So when the input for $\sigma'$ is a large positive or negative value, it will return a nearly 0 value. Then $\delta$ becomes very small, as a result, the partial derivatives are small (Neuron learns slowly).

To address this issue, the cross-entropy was introduced which will be discussed in next article.






## References
[Neural Networks and Deep Learning](neuralnetworksanddeeplearning.com/chap1.html
)







