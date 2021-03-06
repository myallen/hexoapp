---
title: 卷积神经网络CNN入门
tags: [AI, Deep learning, CNN]
---

全连接神经网络最大问题是参数爆炸，容易影响计算速度和导致过拟合问题。而CNN则通过局部连接、权值共享等方法避免了这一难题。

### CNN 一般架构
![](http://xiaoluban.bj.bcebos.com/laphiler%2FCNN_startup%2Fcnn_struct.jpg?authorization=bce-auth-v1%2F94767b1b37b14a259abca0d493cefafa%2F2017-12-20T08%3A13%3A08Z%2F-1%2Fhost%2Ff5b2ebb781f1e557b33cf2299168e5377e053f3c2fd2fffa909ad73c8566b6f2)
<!-- more -->

### 输入层
CNN通过每层不同的网络结构，将上一层的输出转化为下一层输入。
### 卷积层
**卷积核**：卷积层的过滤器(filter)或者叫内核(kernel)既**卷积核**，可以将当前层神经网络上的一个子节点矩阵转化为下一层神经网络上的一个单位节点矩阵。
卷积核的尺寸(size)或者长和宽是用人工指定的，常用的卷积核尺寸有3x3或者5x5，而卷积核处理的矩阵深度就是当前层神经网络节点的深度，顾不需要人工指定。而卷积核的深度指的是处理得到的单位矩阵的深度。
> 卷积核的尺寸指的是输入节点矩阵的大小(例5x5)，卷积核的深度指的是输出单位矩阵的深度(例1x1xZ).

**前向传播算法**:
卷积核前向传播过程就是用左侧矩阵中的节点通过激活函数(比如ReLU)计算出右侧单位矩阵中节点的过程。
### 池化层
在卷积层之间往往会加上一个池化层(pooling layer)，来缩小矩阵的尺寸，从而减少最后全连接层中的参数。池化层既可以加快计算速度，也有防止过拟合的作用。
池化层的计算：池化层的前向传播过程也是通过类似卷积核的结构完成的，但是卷积核中的计算不是节点的加权和，而是通过简单的最大值或者平均值运算，相应的称为最大池化层(max pooling)和平均池化层(average pooling)。实践中，池化层的应用较少。
### 全连接层
经过多层卷积层和池化层处理，可以理解图像中的信息被抽象出信息含量更高的特征，待特征提取完后，CNN最后一般会通过1到2个全连接层给出分类结果。
如何网络接受任意的输入？
有些时候需要把全连接层变成卷积层，这就是所谓的**卷积化**。这里需要证明卷积化的等价性。直观上理解，卷积跟全连接都是一个点乘的操作，区别在于卷积是作用在一个局部的区域，而全连接是对于整个输入而言，那么只要把卷积作用的区域扩大为整个输入，那就变成全连接了。所以我们只需要把卷积核变成跟输入的一个map的大小一样就可以了，这样的话就相当于使得卷积跟全连接层的参数一样多。举个例子，比如 AlexNet，fc6 的输入是 256x6x6，那么这时候只需要把 fc6 变成是卷积核为6x6的卷积层就好了。

与传统神经网络相比，CNN 参数和计算量更多还是更少了？
参数变少了，因为都使用一套参数，而计算量却是变大了，因为卷积窗口要滑到不同的地方，进行计算、合并等操作。

### 优化
提高泛化能力（减少 overfit）

1. 增加神经网络层数。使用卷积层极大地减小了全连接层中的参数的数目，使学习的问题更容易
2. 使用更多强有力的规范化技术（尤其是 dropout 和 regularization）来减小过度拟合
3. 使用修正线性单元而不是 S 型神经元，来加速训练-依据经验，通常是3-5倍
4. 使用 GPU 来计算
5. 利用充分大的数据集，避免过拟合
6. 使用正确的代价函数，避免学习减速
7. 使用好的权重初始化，避免因为神经元饱和引起的学习减速
