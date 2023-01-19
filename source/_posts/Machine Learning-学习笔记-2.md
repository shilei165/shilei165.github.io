
---
title: Machine Learning-学习笔记-02-Linear Algebra Review
mathjax: true
tags: 
  - 计算机
  - 机器学习
  - Machine Learning
  - 人工智能
categories:
  - 技术杂谈
  - Machine Learning
date: 2022-08-12 14:03:23
---

这篇文章跟大家分享一下Machine Learning的学习笔记: 02-线性代数回顾(Linear Algebra Review)。
<!--more-->
***
# 矩阵和向量(Matrices and Vectors)

**矩阵**： 矩阵(Matrix)是一个按照长方阵列排列的复数或实数集合。例如：
$$
\begin{bmatrix}
1 & 2 & 3 \\\\ 4 & 5 & 6 \\\\ 7 & 8 & 9
\end{bmatrix}
$$
**向量**：向量(Vector)是一个维度为nx1的矩阵。比如：
$$
\begin{bmatrix}
1 \\\\ 2 \\\\ 3
\end{bmatrix}
$$
# 矩阵的加法(Matrix Addition)
只有维度相同的矩阵才可以做加法，相加时把对应的元素相加即可。例如：
$$
\begin{bmatrix}
1&0 \\\\ 2&5 \\\\ 3&1
\end{bmatrix}+ \begin{bmatrix}
4&0.5 \\\\ 2&5 \\\\ 0&1
\end{bmatrix}
= \begin{bmatrix}
5&0.5 \\\\ 4&10 \\\\ 3&2
\end{bmatrix}
$$

# 标量乘法(Scalar Multiplication)
$$
3\times\begin{bmatrix}
1 \\\\ 4 \\\\ 2
\end{bmatrix}
=\begin{bmatrix}
3 \\\\ 12 \\\\ 6
\end{bmatrix}
$$

# 矩阵乘法(Matrix-matrix Multiplication)
设A为\\(m\times p\\)的矩阵，B为\\(p\times n\\)的矩阵，那么称\\(m\times n\\)的矩阵C为矩阵A与B的乘积，记作\\(C=A\times B\\)，其中矩阵C中的第i行第j列元素可以表示为：
$$ (AB)\_{ij} = \sum_{k=1}^{p}(a_{ik}b_{kj})=a_{i1}b_{1j}+a_{i2}b_{2j}+...+a_{ip}b_{pj}$$
例如：
$$
\begin{bmatrix}
1 &3&2\\\\ 4&0&1
\end{bmatrix}\times
\begin{bmatrix}
1 &3\\\\ 0&1 \\\\5&2
\end{bmatrix}
=\begin{bmatrix}
1\times 1+3\times 0+2\times 5 & 1\times 3+3\times 1+2\times 2\\\\ 4\times 1+0\times 0+1\times 5 & 4\times 3+0\times 1+1\times 2
\end{bmatrix}
=\begin{bmatrix}
11 &10 \\\\ 9 &14
\end{bmatrix}
$$
注意：
1. 当矩阵A的列数（column）等于矩阵B的行数（row）时，A与B可以相乘。
2. 矩阵C的行数等于矩阵A的行数，C的列数等于B的列数。
3. 乘积C的第m行第n列的元素等于矩阵A的第m行的元素与矩阵B的第n列对应元素乘积之和。

# 矩阵乘法的基本性质
+ 乘法结合律： \\((AB)C=A(BC)\\)．
+ 乘法左分配律：\\((A+B)C=AC+BC\\)
+ 乘法右分配律：\\(C(A+B)=CA+CB\\)
+ 对数乘的结合性*k*\\((AB)\\)=(*k*\\(A)B\\)=\\(A(\\)*k*\\(B)\\)．
+ 转置 \\((AB)^T\\)=\\(B^TA^T\\)．

通常情况下，矩阵乘法不可以使用交换律，也就是：
$$A\times B \neq B\times A $$

但在以下两种情况下满足交换律。

+ \\(AA^\*=A^\*A\\)，A和伴随矩阵相乘满足交换律。
+ \\(AI=IA\\)，A和单位矩阵或数量矩阵满足交换律。

# 单位矩阵(Identity Matrix)
在矩阵的乘法中，有一种矩阵起着特殊的作用，如同数的乘法中的1，这种矩阵被称为单位矩阵。 它是个方阵，从左上角到右下角的对角线（称为主对角线）上的元素均为1。 除此以外全都为0。单位矩阵使用\\(I\\)或者\\(I_{n\times n}\\)来表示。例如：
$$
\begin{bmatrix}
1 &0&0\\\\ 0&1&0\\\\ 0&0&1
\end{bmatrix}
$$
# 转置矩阵(Matrix Transpose)和逆矩阵(Matrix Inverse)

**转置矩阵**：将矩阵的行列互换得到的新矩阵称为转置矩阵。例如：
$$
A=\begin{bmatrix}
1&2&0\\\\3&5&9
\end{bmatrix}
\ \ \ \ \ \  
B=A^T=\begin{bmatrix}
1&3\\\\2&5\\\\0&9
\end{bmatrix}
$$
则B为A的转置矩阵。

**逆矩阵**：实数有倒数，逆矩阵也是相同的概念，当我们把矩阵与其逆矩阵相乘，得到的是单位矩阵。例如：
$$A\times A^{-1}=I$$
这里我们用\\(A^{-1}\\)来表示A的逆矩阵。举例如下：
$$
A=\begin{bmatrix}
3&4\\\\2&16
\end{bmatrix}
\ \ \ \ \ \  
B=A^{-1}=\begin{bmatrix}
0.4&-0.1\\\\-0.05&0.075
\end{bmatrix}
$$

$$
A\times B = A\times A^{-1} =  
\begin{bmatrix}
1&0\\\\0&1
\end{bmatrix}
$$