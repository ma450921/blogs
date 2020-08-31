---
title: 计算机图形学3 —— 视图变换
tags: [计算机图形学, 投影，视图变换]
categories: [GAMES101]
index_img: /images/graphics3/graphics3_banner.png
date: 2020-08-16 22:19:33
math: true
---
视图变换（viewing transform）的主要职责是将3D物体的坐标（我们取标准坐标$(x, y, z)$映射到屏幕的一个个像素点上来。它的过程可以说是比较复杂的，因为在视图变换的过程中很多因素可以决定最后成像的效果。视图变换可以分为四个过程，模型变换（modal transform）、 视角变换（view transform）、投影变换（projection transform）以及视口变换（viewport transformation）。这个过程可以拿拍照片来类比：
1. 站好位置，摆好姿势（modal transform）
2. 找好角度，调整相机位置（view transform）
3. 拍摄照片，将3D场景拍摄到2D的照片（projection transform && viewport transformation）

在拍照片的过程中我们模拟了视图变换的过程。首先是模型变换，我们需要将模型从模型坐标变换到世界坐标中来，也就是在拍照过程中让人站好姿势、摆好位置。调整相机位置，也就是调整在世界坐标中从哪个方向去观测。这个过程我们需要确定两个方向，一个方向是观测的朝向，另一个方向就是与观测朝向“向上”的方向，比如说我们朝着$-Z$方向去看，那么“向上”方向就是$Y$方向。当以上过程都完成后，我们需要将3D场景转换到2D屏幕上，就需要通过投影的方式做像素映射。

![](/images/graphics3/graphics3_transformation.png)

## view/camera transform
结合在上一篇我们讨论的绕任意轴旋转的变换推导中，我们其实可以很容易使用类似的思路来进行推导相机变换。还记得为什么在相机变换的过程中，为什么除了看向方向（look-at direction）之外表示为$g$，还需要一个“向上”的方向$（up direction）表示为$t$，有了着两个方向以及相机的位置$e$。
我们在根据坐标系法则就可以建立起一个三位坐标$u,v,w$。定义如下：
$$
w =  -\frac{g}{\left |\left | g \right |   \right | } 
$$

$$
u = \frac{t \times w }{ \left |\left | {t \times w} \right |   \right |  }  
$$

$$
v = w \times u
$$
![](/images/graphics3/graphics3_camera.png)
摄像机变换就可以看作将$x,y,z$坐标系转换到$u,v,w$坐标系的过程。我们将变换矩阵写作$M_{view}$，令
$$
M_{view} = R_{view}T_{view}
$$
其中$R_{view}$是改变相机朝向的的变换矩阵，它的作用是使得$-Z$旋转到看向方向$u$，$Y$旋转到“向上“的方向$v$。$T_{view}$是移动相机位置的矩阵，是一个平移矩阵，可以被表示为：
$$
\begin{bmatrix}
1 & 0 & 0 & -x_e\\
0 & 1 & 0 & -y_e\\
0 & 0 & 1 & -z_e\\
0 & 0 & 0 & 1
\end{bmatrix}
$$
## pojection transform

## viewport transform
