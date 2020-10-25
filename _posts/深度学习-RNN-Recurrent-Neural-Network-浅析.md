---
title: 深度学习-RNN(Recurrent Neural Network)浅析
date: 2019-09-29 19:28:53
tags: 深度学习
---

　　RNN是两种神经网络模型的缩写，递归神经网络(Recursive Neural Network)和循环神经网络(Recurrent Neural Network)，本文主要介绍后者。<!-- more -->

### RNN原理

　　一个典型的RNN结构如下图所示，由输入x<sub>t</sub>、神经元A、输出h<sub>t</sub>组成：

<img src="https://upload-images.jianshu.io/upload_images/8762099-8744d2099ebb17e2.png?imageMogr2/auto-orient/strip|imageView2/2/w/201/format/webp" style="zoom:50%;" >

　　实际上，上图结构可以展开为

<img src="https://upload-images.jianshu.io/upload_images/8762099-d2766b2abf5e40c8.png?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp" style="zoom:70%;">

　　由于t时刻之前的状态会影响到时刻t的状态，所以这个神经网络会具有时间记忆的功能，我们把图解详细展开看一下：

<img src="https://img-blog.csdn.net/20180528103427942?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyMjQxMTg5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" style="zoom: 50%">

　　其中下标代表某时刻的状态，x表示输入、s表示记忆状态、o表示输出的状态、U表示样本输入权重、W表示上一时刻状态权重、V表示输出样本权重。

　　我们定义

　　　s<sub>t</sub>的表达式为：s<sub>t</sub>=f(W&times;s<sub>t-1</sub>+U&times;x<sub>t</sub>)   (f可以是激活函数)。

　　　输出表达式为：o<sub>t</sub>=g(V&times;s<sub>t</sub>)               (g通常为softmax或其他)

　　　注意：一般初始化输入s<sub>0</sub>=0，随机初始化W，U，V，上述公式可以这么理解s=(过去总结+现有输入)



### RNN反向传播

　　定义每次输出值o<sub>t</sub>都会产生一个误差e<sub>t</sub>，则总误差可以表示为<img src="http://latex.codecogs.com/gif.latex?E=\sum_{t}e_t" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />

　　将参数进行梯度下降，所要求的参数的梯度如下

<img src="http://latex.codecogs.com/gif.latex?\bigtriangledown U=\frac{\partial E}{\partial U}=\sum_{t}\frac{\partial e_t}{\partial U}" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />

<img src="http://latex.codecogs.com/gif.latex?\bigtriangledown V=\frac{\partial E}{\partial V}=\sum_{t}\frac{\partial e_t}{\partial V}" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />

<img src="http://latex.codecogs.com/gif.latex?\bigtriangledown W=\frac{\partial E}{\partial W}=\sum_{t}\frac{\partial e_t}{\partial W}" title="x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" />

　　具体推导不加以赘述了，先了解概念，再细扣细节，这样才能保证所做的努力，是以研究方向正确为前提。