---
title: Machine Learning-Linear Regression with Multiple Variables
mathjax: true
tags: 
  - 计算机
  - 机器学习
  - Machine Learning
  - 人工智能
categories:
  - 技术杂谈
  - Machine Learning
date: 2022-08-14 21:25:23
---

这篇文章跟大家分享一下Machine Learning的学习笔记: 04-多变量线性回归(Linear Regression with Multiple Variables)。
<!--more-->
***
# 多维特征(Multiple Features)

对于一个要度量的对象，一般来说会有不同维度的多个特征。比如之前的房屋价格预测例子中，除了房屋的面积大小，可能还有房屋的年限、房屋的层数等等其他特征：
|Size (\\(feet^2\\))|Number of bedrooms|Number of floors|Aage of home (years)|Price ($1000)|
|----|----|-----|-----|-----|
|2104|5|1|45|460|
|1416|3|2|40|232|
|1534|3|2|30|315|
|...|...|...|...|...|


由于有多个特征，引入了一些新的记号：

+ n = 特征的总数

+ \\(x^{(i)}\\) = 代表样本矩阵中的第i行，也就是第i个训练实例。

+ \\(x_j^{(i)}\\) =  代表样本矩阵中第i行的第j列，也就是第i个训练实例的第j个特征。

例如上面的例子：
$$
x^{(2)}=
\begin{bmatrix}
1416\\\\3\\\\2\\\\40\\\\
\end{bmatrix},
x_1^{(2)}=1416
$$
多变量假设函数表示为：
$$
h_\theta(x)=\theta_0+\theta_1x_1+\theta_2x_2+...+\theta_nx_n
$$
在添加一个特征向量\\(x_0\\)之后，可以将假设函数简化为：

$$
h_\theta(x)=
\begin{bmatrix}
\theta_0&\theta_1&...&\theta_n
\end{bmatrix}
\begin{bmatrix}
x_0\\\\x_1\\\\...\\\\x_n
\end{bmatrix}
$$
# 多变量梯度下降(Gradient Descent for Multiple Variables)
多变量cost function类似于单变量cost function，即：
$$
J(\theta_0,\theta_1,\theta_2...,\theta_n) = \frac{1}{2m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})^2
$$
多变量下梯度下降公式：
Repeat {
$$
\theta_j :=\theta_j- \alpha\frac{\partial}{\partial{\theta_j}}{J({\theta_0,\theta_1,\theta_2...,\theta_n}})
$$
}

解出偏导得到：
$$
\theta_j :=\theta_j- \alpha\frac{1}{m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}
$$

# 梯度下降实践1-特征缩放(Feature Scaling)
在应用梯度下降算法实践时，由于各特征值的范围不一，可能会影响代价函数收敛速度。为了优化梯度下降的收敛速度，采用特征缩放的技巧，使各特征值的范围尽量一致。

除了人工选择并除以一个参数的方式，均值归一化(Mean normalization)方法更为便捷，可采用它来对所有特征值统一缩放：

$$
x_i = \frac{x_i-average(x)}{max(x)-min(x)}, 从而使得 x_i\in(-1,1)
$$
或者可以使用标准方差来代替\\(max(x)-min(x)\\)，也就是：
$$
x_i = \frac{x_i-average(x)}{std(x)}, 从而使得 x_i\in(-1,1)
$$

但不一定必须\\(-1\leq x\leq1\\)，类似于\\(1\leq x\leq3\\)也是可以的。
# 梯度下降实践2-学习速率(Learning Rate)

对于学习速率，如果\\(\alpha\\)过大，代价函数(Cost Function)无法收敛，如果过小，代价函数(Cost Function)收敛的太慢。当然， 足够小时，代价函数在每轮迭代后一定会减少。

通过不断改变\\(\alpha\\)值，绘制并观察图像，并以此来确定合适的学习速率。 尝试时可取\\(\alpha\\)如: ...0.001, 0.003, 0.01, 0.03, 0.1,...

# 特征和多项式回归(Features and Polynomial Regression)

在特征选取时，我们也可以自己归纳总结，定义一个新的特征，用来取代或拆分旧的一个或多个特征。比如，对于房屋面积特征来说，我们可以将其拆分为长度和宽度两个特征，反之，我们也可以合并长度和宽度这两个特征为面积这一个特征。

线性回归只能以直线来对数据进行拟合，有时候需要使用曲线来对数据进行拟合，即多项式回归(Polynomial Regression)。

比如一个二次方模型：\\(h_\theta(x)=\theta_0+\theta_1x_1+\theta_2x_2^2\\)

或者三次方模型： \\(h_\theta(x)=\theta_0+\theta_1x_1+\theta_2x_2^2+\theta_3x_3^3\\)

或者平方根模型： \\(h_\theta(x)=\theta_0+\theta_1x_1+\theta_2x_2^2+\theta_3\sqrt{(x_3)}\\)

在使用多项式回归时，要记住非常有必要进行特征缩放，比如\\(x_1\\)的范围为 1-1000，那么\\(x_1^2\\)的范围则为 1- 1000000，不适用特征缩放的话，范围更有不一致，也更易影响效率。
# 正规方程(Normal Equation)

正规方程法，即令\\(\frac{\partial}{\partial{\theta_j}}J({\theta_j})=0\\)，通过解析函数的方式直接计算得出参数向量的值。

$$
\theta = (X^TX)^-1X^Ty
$$

Octave/Matlab代码：
```
theta = pinv(X'*X)*X'*y
```
下表列出了正规方程法与梯度下降算法的对比

|条件|梯度下降|正规方程|
|----|----|-----|
|是否需要选取\\(\alpha\\)|是|否|
|是否需要迭代运算|是|否|
|特征量大时是否适用|是，\\(O(kn^2)\\)|否，\\(O(n^3)\\)|
|适用范围|各类模型|只适用线性模型，且矩阵需可逆|



# 不可逆性正规方程(Normal Equation Noninvertibility (Optional))

正规方程无法应用于不可逆的矩阵，发生这种问题的概率很小，通常由于:
+ 特征之间线性相关
+ 特征数量大于训练集的数量。

如果发现\\(X^TX\\)的结果不可逆，可尝试:
+ 减少多余/重复特征
+ 增加训练集数量
+ 使用正则化（后文）