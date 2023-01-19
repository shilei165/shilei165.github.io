---
title: Machine Learning-学习笔记-11-Neural Networks:Learning
mathjax: true
tags: 
  - 计算机
  - 机器学习
  - Machine Learning
  - 人工智能
categories:
  - 技术杂谈
  - Machine Learning
date: 2022-08-25 16:56:23
---
这篇文章跟大家分享一下Machine Learning的学习笔记: 11-神经网络的学习(Neural Networks: Learning)。
<!--more-->

# 代价函数(Cost function)

首先引入一些标记方法，以便于讨论：

假设神经网络的训练样本有\\(m\\)个，每个包含一组输入信号\\(x\\)和一组输出信号\\(y\\)，\\(L\\)表示神经网络层数，\\(S_l\\)表示\\(l\))层的neuron个数(不包含bias单元)。

神经网络分为两类：
+ 二元分类：\\(y=0\ or\ 1\\)；1个输出变量
+ 多元分类：\\(y\in\mathbb{R}^K\\)；K个输出变量

![NeuralNetwork_1](../images/NeuralNetwork_1.jpg)

逻辑回归中代价函数为：
$$
J(\theta)= -\frac{1}{m}[\sum_{i=1}^{m}y^{(i)}log h_\theta(x^{(i)}) + (1-y^{(i)})log(1-h_\theta(x^{(i)}))]+\frac{\lambda}{2m}\sum_{j=1}^{n}\theta_j^2
$$
与逻辑回归不同的是，神经网络有多个输出变量，或者说\\(h_\Theta(x)\\)是一个维度为K的向量。因此代价函数也会更加复杂一点：
$$
J(\Theta)= -\frac{1}{m}[\sum_{i=1}^{m}\sum_{k=1}^{k}y_k^{(i)}log (h_\Theta(x^{(i)}))\_k + (1-y_k^{(i)})log(1-(h_\Theta(x^{(i)}))\_k)]+\frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_l+1}(\Theta_{ji}^{(l)})^2
$$

其中，\\(h_\Theta(x)\in\mathbb{R}^K\\)  \\((h_\Theta(x))\_i=i^{th}\ output\\)

这个看起来复杂很多的代价函数背后的思想还是一样的，我们希望通过代价函数来观察算法预测的结果与真实情况的误差有多大，唯一不同的是，对于每一行特征，我们都会给出K个预测，基本上我们可以利用循环，对每一行特征都预测K个不同结果，然后在利用循环在K个预测中选择可能性最高的一个，将其与y中的实际数据进行比较。

正则化那一项只是排除了每一层的\\(\theta_0\\)后，每一层的\\(\theta\\)矩阵的和。

# 反向传播算法(Backpropagation algorithm)
之前我们在计算神经网络预测结果的时候我们采用了一种正向传播方法，我们从第一层开始正向一层一层进行计算，直到最后一层的\\(h_\theta(x)\\)。

现在为了计算代价函数的偏导数\\(\frac{\partial}{\partial\Theta_{ij}^{(l)}}J(\Theta)\\)，我们需要采用一种反向传播算法，也就是首先计算最后一层的误差，然后再一层一层反向求出各层的误差，直到倒数第二层。 

在不做任何正则化处理时：
$$
\frac{\partial}{\partial\Theta_{ij}^{(l)}}J(\Theta)=a_j^{(l)}\delta_i^{l+1}
$$
\\(l\\)代表目前所计算的是第几层。

\\(j\\)代表目前计算层中的激活单元的下标。

\\(i\\)代表下一层中误差单元的下标。

如果我们考虑正则化处理，并且我们的训练集是一个特征矩阵而非向量，我们需要计算每一层的误差单元来计算代价函数的偏导数。我们用\\(\Delta_{ij}^{(l)}\\)来表示这个误差矩阵。

假设我们有m个训练集，利用反向传播来计算\\(\Delta_{ij}^{(l)}\\)的算法如下：

![NeuralNetwork_2](../images/NeuralNetwork_2.jpg)

在求出了\\(\Delta_{ij}^{(l)}\\)之后，我们便可以计算代价函数的偏导数了，计算方法如下： 

\\(D_{ij}^{(l)}\ :=\ \frac{1}{m}\Delta_{ij}^{(l)} + \frac{\lambda}{m}\Theta_{ij}^{(l)}\\) \\(\ \ \ \ \ \ \ \  if\ \ \ j\neq 0\\) 

\\(D_{ij}^{(l)}\ :=\ \frac{1}{m}\Delta_{ij}^{(l)}\\) \\(\ \ \ \ \ \ \ \  if\ \ \ j= 0\\) 

# 展开参数

在Octave 中，如果我们要使用fminuc这样的优化算法来求解求出权重矩阵，我们需要将矩阵首先展开成为向量，在利用算法求出最优解后再重新转换回矩阵。

假设我们有三个权重矩阵，Theta1，Theta2 和 Theta3，尺寸分别为 10\*11，10\*11 和1\*11， 下面的代码可以实现这样的转换：
```
thetaVec = [Theta1(:) ; Theta2(:) ; Theta3(:)]

...optimization using functions like fminuc...

Theta1 = reshape(thetaVec(1:110, 10, 11);

Theta2 = reshape(thetaVec(111:220, 10, 11);

Theta1 = reshape(thetaVec(221:231, 1, 11);
```

# 梯度检验

当我们对一个较为复杂的模型（例如神经网络）使用梯度下降算法时，可能会存在一些不容易察觉的错误，意味着，虽然代价看上去在不断减小，但最终的结果可能并不是最优解。

为了避免这样的问题，我们采取一种叫做梯度的数值检验（Numerical Gradient Checking）方法。这种方法的思想是通过估计梯度值来检验我们计算的导数值是否真的是我们要求的。

对梯度的估计采用的方法是在代价函数上沿着切线的方向选择离两个非常近的点然后计算两个点的平均值用以估计梯度。即对于某个特定的\\(\theta\\)，我们计算出在\\(\theta-\epsilon\\)处和\\(\theta+\epsilon\\)的代价值\\((\epsilon\\)是一个非常小的值，通常选取 0.001），然后求两个代价的平均，用以估计在\\(\theta\\)处的代价值。

![NeuralNetwork_3](../images/NeuralNetwork_3.jpg)

Octave 中代码如下：
```
gradApprox = (J(theta + eps) – J(theta - eps)) / (2*eps)
```
当\\(\theta\\)是一个向量时，我们则需要对偏导数进行检验。因为代价函数的偏导数检验只针对一个参数的改变进行检验，下面是一个只针对\\(\theta_1\\)进行检验的示例:\\(\frac{\partial}{\partial\theta_1}=\frac{J(\theta_1+\epsilon_1,\theta_2,\theta_3...\theta_n)-J(\theta_1-\epsilon_1,\theta_2,\theta_3...\theta_n)}{2\epsilon}\\)


代码如下：
```
for i = 1:n
  thetaPlus = theta;
  thetaPlus(i) = thetaPlus(i) + EPSILON;
  thetaMinus = theta;
  thetaMinus(i) = thetaMinus(i) - EPSILON;
  gradApprox(i) = (J(thetaPlus) - J(thetaMinus))/(2*EPSILON);
end;
```

我们需要将通过反向传播计算出的偏导数\\(D_{ij}^{(l)}\\)转换成向量DVec，然后与gradApprox进行比较。

# 随机初始化
任何优化算法都需要一些初始的参数。到目前为止我们都是初始所有参数为0，这样的初始方法对于逻辑回归来说是可行的，但是对于神经网络来说是不可行的。如果我们令所有的初始参数都为0，这将意味着我们第二层的所有激活单元都会有相同的值。同理，如果我们初始所有的参数都为一个非0的数，结果也是一样的。

我们通常初始参数为正负ε之间的随机值，假设我们要随机初始一个尺寸为10×11的参数矩阵，代码如下：
```
Theta1 = rand(10, 11) * (2*eps) - eps
```

# 综合起来

小结一下使用神经网络时的步骤：

+ 网络结构：第一件要做的事是选择网络结构，即决定选择多少层以及决定每层分别有多少个单元。

+ 第一层的单元数即我们训练集的特征数量。

+ 最后一层的单元数是我们训练集的结果的类的数量。

+ 如果隐藏层数大于1，确保每个隐藏层的单元个数相同，通常情况下隐藏层单元的个数越多越好。

+ 我们真正要决定的是隐藏层的层数和每个中间层的单元数。

训练神经网络：

+ 参数的随机初始化

+ 利用正向传播方法计算所有的\\(h_\theta(x)\\)

+ 编写计算代价函数J的代码

+ 利用反向传播方法计算所有偏导数

+ 利用数值检验方法检验这些偏导数

+ 使用优化算法来最小化代价函数

