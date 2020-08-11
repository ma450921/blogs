---
title: 计算机图形学1 —— 图形变换矩阵
tags: [计算机图形学, 线性代数]
categories: [GAMES101]
index_img: /images/graphics1/graphics1_banner.jpeg
date: 2020-08-10 21:09:11
math: true
---

## 摘要
在图形学中，图形位置形态的数学定义是坐标中的一个个坐标点的集合，因此图形的变换操作通过数学计算来进行。为了简化计算思路，通常我们会将这些变化通过线性代数的方式抽象为变换矩阵 (Transformation Marices)。一切物体的缩放，旋转，位移，都可以通过变换矩阵作用得到。当我们确定好了某种变化的变换矩阵后，可以将其应用在任何其他图形上。本文将介绍基本图形变化中如何利用变换矩阵来进行图形变换。

## 2D线性变换
在2D图形中，我们规定 $[x, y]^T$ 为图形上的任意一个点（也可以看作是一个二维向量），2D基于原点的线性变化可以被定义为向量 $[x, y]^T$ 左乘变换矩阵的数学表示：

$$
\begin{bmatrix}
 a_{11} & a_{12}\\
 a_{21} & a_{22}
\end{bmatrix}
\begin{bmatrix}
 x \\
 y
\end{bmatrix}
=
\begin{bmatrix}
 a_{11}x & a_{12}y\\
 a_{21}x & a_{22}y
\end{bmatrix}
$$

### 缩放（scale）
缩放操作是将图形中所有点的坐标整体放大或者缩小某个数值，如下图所示的变换分别将$x, y$分别缩小了0.5:

![](/images/graphics1/graphics1_scale.png)
$[x, y]^T$经过变换后表示为$[x\prime, y\prime]^T$，经过变化后的值可以表示为：
$$
x\prime = 0.5 \times x + 0 \times y
$$

$$
y\prime = 0 \times x + 0.5 \times y
$$

上述操作可以使用变换矩阵表示为：
$$
scale(s_x, s_y)
= 
\begin{bmatrix}
 s_x & 0\\
 0 & s_y
\end{bmatrix} 
$$

### 剪切（Shear）
剪切变换可以简单理解为将图形的一段固定，将其对端沿着固定方向拉动。以固定x轴为例：
![](/images/graphics1/graphics1_shear.png)
所有点的$y$值保持不变，x坐标加上了对应的y值，经过变化后的值表示为：
$$
x\prime =  x + 1 \times y
$$

$$
y\prime = 0 \times x + y
$$

上述操作可以使用变换矩阵表示为：
$$
shearX(s)
= 
\begin{bmatrix}
 1 & s\\
 0 & 1
\end{bmatrix} 
$$

$$
shearY(s)
= 
\begin{bmatrix}
 1 & 0\\
 s & 1
\end{bmatrix} 
$$

### 翻转（reflection）
翻转一般是通过将某个坐标轴的所有值取反得到的，其变换矩阵可以表示为：
上述操作可以使用变换矩阵表示为：
$$
reflectionY
= 
\begin{bmatrix}
 -1 & 0\\
 0 & 1
\end{bmatrix} 
$$

$$
reflectionX
= 
\begin{bmatrix}
 1 & 0\\
 0 & -1
\end{bmatrix} 
$$


### 旋转（rotate）
旋转的几何意义很容易理解，图形绕着原点逆时针旋转，但是数学上的变换关系并没有上述几种变换直观。为了便于理解，我们先假定图形中的一个任意向量$\vec{a}$，它与x轴的夹角为$\alpha$，坐标为$[x_a, y_a]$。向量的长度表示为$r^2 = x_a^2 + y_a^2$，因此其坐标可以表示为：
$$
x_a = rcos\alpha
$$
$$
y_a = rsin\alpha
$$
当$\vec{a}$旋转了任意角度$\varphi$之后得到$\vec{b}$，此时$\vec{b}$与x轴的夹角为$(\alpha + \varphi$)$，坐标为$[x_a, y_a]$被表示为：
$$
x_b = rcos(\alpha + \varphi) = rcos\alpha cos\varphi - rsin\alpha sin\varphi
$$
$$
y_b = rsin(\alpha + \varphi) = rsin\alpha cos\varphi + r sin\alpha sin\varphi
$$

根据$x_a = rcos\alpha$以及$y_a = rsin\alpha$我们可以化简得到：
$$
x_b = x_a cos\varphi - y_a sin\varphi
$$
$$
y_b = y_a cos\varphi + x_a sin\varphi
$$

变换矩阵可以表示为：
$$
rotate(\varphi) = 
\begin{bmatrix}
 cos\varphi & -sin\varphi\\
 sin\varphi & cos\varphi
\end{bmatrix} 
$$
