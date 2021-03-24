---
title: 计算机图形学7.5 —— 重心坐标和线性插值
tags: [计算机图形学, 重心坐标, 线性插值]
categories: [GAMES101]
index_img: /images/graphics7.5/graphics7.5_banner.png
date: 2020-10-14 23:33:11
math: true
---

## 重心坐标的定义
重心坐标是由三角形顶点定义出的一个特征坐标，它满足以下特征：
1. 令$\alpha$、$\beta$、$\gamma$为数字常量，且 $\alpha + \beta + \gamma = 1$。
2. 重心坐标则为三个顶点分别乘以三个系数的坐标值，即 $(x, y) = \alpha A + \beta B + \gamma C$
*注意：* 重心坐标不是重心，它是一种笛卡尔坐标的线性变化，不能将其混淆。
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

## 重心坐标的表示法
给定三角形平面一点P，我们将这一点的面积坐标$\lambda_1 $ 、 $\lambda_2 $ 和 $\lambda_3$，用笛卡尔坐标表示出来。利用笛卡尔坐标中的三角形面积公式：
$$
S(ABC) = \frac{1}{2}{\begin{vmatrix} 1 & x_a & y_a \\ 1 & x_b & y_b  \\ 1 & x_c & y_c  \end{vmatrix}}
$$
可以得到：
$$
\lambda_1 = S(PAC) / S(ABC) = {\begin{vmatrix} 1 & x_p & y_p \\ 1 & x_a & y_a  \\ 1 & x_c & y_c  \end{vmatrix}} / {\begin{vmatrix} 1 & x_a & y_a \\ 1 & x_b & y_b  \\ 1 & x_c & y_c  \end{vmatrix}}
$$

根据重心坐标的定义，重心坐标可以表示为：
$$
\left\{\begin{matrix} \lambda_1+\lambda_2+\lambda_3=1 \\  p_x=\lambda_1*x_{a}+\lambda_2*x_{b}+\lambda_3*x_{c}  \\ p_y=\lambda_1*y_{a}+\lambda_2*y_{b}+\lambda_3*y_{c}  \end{matrix}\right.
$$


由此可以看出重心坐标实际上是笛卡尔坐标的一种线性变换，从而它们在边和三角形区域之间的变化是线性的。如果点在三角形内部，那么所有重心坐标属于开区间$(0, 1)$；如果一点在三角形的边上，至少有一个面积坐标$\lambda_{1...3}$为0，其余分量位于闭区间$(0, 1)$。如果有某个坐标小于0，则位于三角形外部。


## 重心坐标的应用——线性插值
除了前面利用重心坐标来判断点是否在三角形内的方法外，它在图形学中最重要的功能在于线性插值。线性插值的过程如下：

假设我们已知坐标 $(x0, y0)$ 与 $(x1, y1)$，要得到 $[x0, x1]$ 区间内某一位置 $x $在直线上的值。根据图中所示，我们得到
$$
\frac{y - y_0}{x - x_0} = \frac{y_1 - y_0}{x_1 - x_0}
$$

由于 x 值已知，所以可以从公式得到 y 的值
$$
y = y_0 + (x - x_0) \frac{y_1 - y_0}{x_1 - x_0}
$$
已知 $y$ 求 $x$ 的过程与以上过程相同，只是 $x$ 与 $y$ 要进行交换。

在图形学中常常会使用线性插值来解决很多值变化的问题，比如openGL中的 `varying variables` 就是一种使用线性插值对顶点或者片元进行着色的方式。

利用重心坐标可以很好的将顶点的值（位置、纹理坐标、颜色、法线、深度、材质等）快速平滑地过渡到三角形的每个点上。

