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

## 相机变换 view/camera transform
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
T_{view} =
\begin{bmatrix}
1 & 0 & 0 & -x_e\\
0 & 1 & 0 & -y_e\\
0 & 0 & 1 & -z_e\\
0 & 0 & 0 & 1
\end{bmatrix}
$$
旋转的过程在前面的章节有提过，如果需要使用$R_{uvw}$变换矩阵，因此
M_{view}可以被表示为：
$$
M_{view} = 

\begin{bmatrix}
x_u & y_u & z_u & 0\\
x_v & y_v & z_v & 0\\
x_w & y_w & z_w & 0\\
0 & 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 & -x_e\\
0 & 1 & 0 & -y_e\\
0 & 0 & 1 & -z_e\\
0 & 0 & 0 & 1
\end{bmatrix}
$$
## 投影变换 pojection transform
投影变换的过程主要是将3D的点投影到2D的屏幕上来，根据变换条件的不同，可以将投影区分为正交投影和透视投影。如下图所示展现了两种投影的变换方式和成像结果。
![](/images/graphics3/graphics3_banner.png)
### 正交投影 Orthographic Projection Transformation
正交投影很好理解，在正交投影之后图形中所有点的的相对位置都不会发生改变，可以将其看作直接将图形“压扁”，然后放置在某一平面上。
![](/images/graphics3/graphic3_orthographic.png)
在实际渲染过程中，我们总是希望在某个空间区域来渲染图形，而非根据整个视图区域来做渲染。假设渲染发生在一个沿着$Z$轴负方向，底面（bottom）与$XY$平面平行的一个立方体。这时候可以定义左右面（left, right）以及近远面（near, far），以及上下面（top, bottom）用以下代数表示：
$$
x = l = left{\space}plane
$$

$$
x = r = right{\space}plane
$$

$$
y = v = bottom{\space}plane
$$

$$
y = t = left{\space}plane
$$

$$
z = n = near{\space}plane
$$

$$
z = f = far{\space}plane
$$
简单来说，正交投影的主要原理是将上述区域变换到一个”正则、规范、标准“的立方体$[-1, 1]^3$中来。这个变换过程可以分解为两个变换：
1. 将渲染盒子平移到以原点为中心的位置，即每个定点到投影平面的距离为 边长/2
2. 将渲染盒子缩放为立方体$[-1, 1]^3$
![](/images/graphics3/graphics3_orthographic_transform.png)
这个过程可以用变换矩阵表示为：
$$
M_{ortho} = 
\begin{bmatrix}
\frac{2}{r - l} & 0 & 0 & 0\\
0 & \frac{2}{t - b} & 0 & 0\\
0 & 0 & \frac{2}{n - f} & 0\\
0 & 0 & 0 & 1
\end{bmatrix} 
\begin{bmatrix}
1 & 0 & 0 & -\frac{r+l}{2}\\
0 & 1 & 0 & -\frac{t+b}{2}\\
0 & 0 & 1 & -\frac{n+f}{2}\\
0 & 0 & 0 & 1
\end{bmatrix} 
$$
### 透视投影 Perspective Projection Transformation
透视投影和正交投影最大的区别在于，