---
layout:     post
title:      Neural Network - 1
subtitle:   Neuron and MLPs
date:       2018-12-14
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
## Perceptrons Neuron

Perceptrons were developed in the 1950s and 1960s by Frank Rosenblatt which were earliest types of neural network. A perceptrons takes several binary inputs (0 or 1) and outputs a single binary output.

<center><img src = 'http://neuralnetworksanddeeplearning.com/images/tikz0.png'></center>

In the example the perceptron has three inputs $x_1$, $x_2$ and $x_3$ and there are weights $w_1$, $w_2$ and $w_3$ for each input. If the weighted sum $\sum w_jx_j$ are higher than a threshold value, the perceptron will output 1, vice versa.

$$ output=\left\{
\begin{array}{rcl}
1       &      & {\sum w_jx_j > threshold}\\
0     &      & {\sum w_jx_j \leq  threshold}\\
\end{array} \right. $$

Set bias $b = -threshold$, above expression can be simplified as:

$$ output=\left\{
\begin{array}{rcl}
1       &      & {\sum w_jx_j + b> 0}\\
0     &      & {\sum w_jx_j +b \leq  0}\\
\end{array} \right. $$

Using several perceptrons, we can build a simple neural network.

<center><img src = 'http://neuralnetworksanddeeplearning.com/images/tikz1.png'></center>

## Sigmoid Neuron

For perceptrons neuron, there is a significan drawbacks. The output changes suddenly, so small changes in weights or bias won't change the output. It would be very difficult to calibrate weigts and bias. 


<center><img src = 'http://ww1.sinaimg.cn/large/83d6b255ly1fy6zbvfyvdj20ba07c3yd.jpg'></center>

To address this issue, we can apply so called **activation function**. If we use sigmoid function as activation function, then the neuron is called sigmoid neuron. sigmoid function has expression below:

$$\sigma(z) = \frac{1}{1+e^{-z}}$$

$$z = \sum w_jx_j + b$$

And its shape was like below:

<center><img src = 'http://ww1.sinaimg.cn/large/83d6b255ly1fy70ehhy3oj20bn07l747.jpg'></center>

After applying this activation function, any small changes in weights or bias will result in small change in output which is a good property we desired.

If there is network which is made up by multiple layer of sigmold neurons, such network would be called multilayer perceptrons or **MLPs** despite it's not made up by perceptrons.



## References
[Neural Networks and Deep Learning](neuralnetworksanddeeplearning.com/chap1.html
)



