---
title: 计算机图形学7.5 —— 重心坐标和线性插值
tags: [计算机图形学, 重心坐标, 线性插值]
categories: [GAMES101]
index_img: /images/graphics7.5/graphics7.5_banner.png
date: 2020-10-03 17:09:11
math: true
---

## 重心坐标的定义
重心坐标是由三角形顶点点定义出的一个特征点坐标，它满足以下特征：
1. 令$\alpha$、$\beta$、$\gamma$为数字常量，且 $\alpha + \beta + \gamma = 1$。
2. 重心坐标则为三个顶点分别乘以三个系数的坐标值，即 $(x, y) = \alpha A + \beta B + \gamma C$

![](/images/graphics7.5/barycentric_coordinates.png)

此外在三角形中，重心坐标又被称为面积坐标。因为P点关于三角形ABC的重心坐标和三角形PBC, PCA及PAB的（有向）面积成比例。证明如下：（如下图所示）

我们用黑体小写字母表示对应点的向量，比如三角形ABC顶点为 $a$、$b$、$c$，P点为$p$。设三角形PBC、PCA以及PAB的比例为 $\lambda_1 : \lambda_2 : \lambda_3$，且$\lambda_1 + \lambda_2 + \lambda_3 = 1$，设射线AP与BC交于D。

![](/images/graphics7.5/Areal_coordinates.png)


$$
由相似三角形的特性可以得出：
BD : DC = \lambda_3 : \lambda_2
$$

$$
则d的坐标为：
 d = \frac{\lambda_2 b  + \lambda_3 c}{\lambda_2 + \lambda_3} 
$$

$$
同理：
AP : PC = (\lambda_3 + \lambda_2) : \lambda_1
$$

$$
则 p 的坐标为：
p = \frac{(\lambda_3 + \lambda_2) d  + \lambda1 a}{\lambda_1 + \lambda_2 + \lambda_3}
$$

$$
综上化简后，p为：
p = \lambda_1 a + \lambda_2 b + \lambda_3 c
$$

## 重心坐标的求法
$$
\left\{\begin{matrix} \lambda_1+\lambda_2+\lambda_3=1 \\  P_x=\lambda_1*P_{1x}+\lambda_2*P_{2x}+\lambda_3*P_{3x}  \\ P_y=\lambda_1*P_{1y}+\lambda_2*P_{2y}+\lambda_3*P_{3y}  \end{matrix}\right.
$$

## 重心坐标的应用——线性插值
