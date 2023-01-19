
---
title: Machine Learning-学习笔记-01-Linear Regression with One Variable
mathjax: true
tags: 
  - 计算机
  - 机器学习
  - Machine Learning
  - 人工智能
categories:
  - 技术杂谈
  - Machine Learning
date: 2022-08-11 23:05:23
---

这篇文章跟大家分享一下Machine Learning的学习笔记: 01-单变量线性回归(Linear Regression with One Variable)。
<!--more-->
***
# 单变量线性回归

**Hypothesis:** 
$$h_{\theta}(x) = {\theta_0} + {\theta_1x}$$

**Parameters:** \\({\theta_0}, {\theta_1}\\) 

**Cost Function:** 
$$J({\theta_0},{\theta_1})= \frac{1}{2m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})^2$$

其中，\\(h_\theta(x^{(i)})\\)是预测值，\\(y^{(i)}\\)是真实值。Cost function越小说明我们的模型跟真实值之间的差距越小，也就是说预测会越准确。所以Meachine learning的目的是找到\\(J\({\theta_0},{\theta_1}\)\\)的最小值。

# 当\\(\theta_0=0\\)时的简化情况
当\\(\theta_0=0\\)时,hypothesis可以写为：
$$h_\theta(x)=\theta_1x$$
这时我们只有一个参数\\(\theta_1\\),此时Cost function变为：
$$J(\theta_1)=\frac{1}{2m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})^2$$
这里由于\\(h_\theta(x)=\theta_1x\\),所以cost function也可以写为：
$$J(\theta_1)=\frac{1}{2m}\sum_{i=1}^{m}(\theta_1x^{(i)}-y^{(i)})^2$$
这时，我们需要建立模型找到\\(J(\theta_1)\\)的最小值。通过不断改变\\(\theta_1\\)的值，我们就可以找到cost function\\(J(\theta_1)\\)的最小值。或者可以通过绘制二维图形，来非常简单的找到最优解。

# 两个参数的情况
当有两个参数\\(\theta_0,\theta_1\\)时，Hypothesis的图像就变成了一个三维图像，最小值为图像的最低点。
找到cost function最小值的步骤为：
1. 从某个特定的\\(\theta_0,\theta_1\\)开始代入计算，比如\\(\theta_0=0,\theta_1=0\\)
2. 不断改变\\(\theta_0,\theta_1\\)的值来减小\\(J\({\theta_0},{\theta_1}\)\\)的值，直到找到\\(J\({\theta_0},{\theta_1}\)\\)的最小值。

那么如何不断改变\\(\theta_0,\theta_1\\)的值呢？我们需要用到下面的公式：
$$\theta_j := \theta_j - \alpha\frac{\partial}{\partial{\theta_j}}{J({\theta_0},{\theta_1})}\ \ \ \ (for\ \ j=0\ and\ j=1)$$ 

这里将:=后面的值重新赋予\\(\theta_j\\)。由于方程中出现了两个参数，改变其中一个两一个也会随之改变，所以在赋值的时候，我们需要将\\(\theta_0,\theta_1\\)同时赋值。

通过对cost function求偏导就可以来确定我们参数改变的方向是朝着\\({J\({\theta_0},{\theta_1}\)}\\)更小的方向来赋值。\\(\alpha\\)是learning rate，用于调整我们改变参数时候的变化大小，选择合适的\\(\alpha\\)会让我们更快更准确的找到最优解。

当\\(\alpha\\)太小的时候，方程求解的过程会十分漫长；但如果\\(\alpha\\)选择的太大，就有可能错过最优解，从而导致无法解出方程。

# 对\\(\theta_0, \theta_1\\)求偏导

上面提到了在不断对\\(\theta_0, \theta_1\\)赋值的过程中，我们需要对\\(\theta_0, \theta_1\\)求偏导，那么简化之后来看一下会得到什么样的结果吧。
$$
\frac{\partial}{\partial{\theta_j}}{J({\theta_0},{\theta_1})} = 
\frac{\partial}{\partial{\theta_j}}{\frac{1}{2m}}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y{(i)})^2=
\frac{\partial}{\partial{\theta_j}}{\frac{1}{2m}}\sum_{i=1}^{m}(\theta_0+\theta_1x^{(i)}-y^{(i)})^2
$$
所以\\(\theta_0, \theta_1\\)分别为：
$$
\theta_0\ :\  \frac{\partial}{\partial{\theta_j}}{J({\theta_0},{\theta_1})} = \frac{1}{m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})
$$
$$
\theta_1\ :\  \frac{\partial}{\partial{\theta_j}}{J({\theta_0},{\theta_1})} = \frac{1}{m}\sum_{i=1}^{m}[(h_\theta(x^{(i)})-y{(i)})x^{(i)}]
$$
类似的，如果我们有多个参数，对\\(\theta_j\\)求偏导，我们会得到类似的结果：
$$
\theta_j\ :\  \frac{\partial}{\partial{\theta_j}}{J({\theta_j})} = \frac{1}{m}\sum_{i=1}^{m}[(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}]
$$