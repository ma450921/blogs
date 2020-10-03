---
title: 计算机图形学2 —— 图形变换扩展
tags: [计算机图形学, 线性代数]
categories: [GAMES101]
index_img: /images/graphics2/graphics2_banner.jpeg
date: 2020-08-17 21:09:11
math: true
---
## 线性变换的组合和分解

### 线性变换的组合
在前文我们提到，所有的线性变换都可以扩展成为变换矩阵左乘于源坐标点的数学表示。当对一个向量应用多次变换时的数学表示则是按照顺序分别左乘变换矩阵：
$$
first, v_2 = Sv_1, then, v_3 = Rv_2
$$
可以被表示为：
$$
v_3 = R(Sv_1)
$$
根据矩阵计算的结合律可以推导为：
$$
v_3 = (RS)v_1
$$
根据上面的推导可以发现一个规律，矩阵经过的两次变换$S$、$R$，可以表示为一次变换矩阵$M$，其中$M = RS$。其中矩阵乘法的顺序很重要。
![](/images/graphics2/graphics2_compose.png)

### 线性变换的分解
既然多个变换矩阵可以合并为同一个变换矩阵，那么一个变换矩阵也可以被分解为多个变换矩阵的乘积。利用矩阵分解知识我们知道，对于给定 $A_{n*n}$ 的方阵，如果其具有n个线性无关的特征向量，那么可以被分解为，正交矩阵与对角矩阵相乘的形式，这个过程：
$$
A = RSR^T
$$
其中 $R$ 为特征向量组成的正交矩阵，$S$ 为其特征值组成的对角矩阵。在$n=2$的情况下，$R$ 由其特征向量 $v_1, v_2$ 组成，$R = [v_1, v_2]$。$S$的对角值分别为其特征值 $\lambda_1$、$\lambda_2$。

根据其分解出的矩阵的特点，我们可以通过几何意义来对 $A$ 进行线性变换分解，$R$ 可以看作是一个旋转变换，$S$可以看作是缩放变换，因此原变换矩阵 $A$ 被分解为以下三个步骤：

1. 将其特征向量 $v_1$ 和 $v_2$ 旋转到 $x, y$轴上即变换矩阵 $R^T$。
2. 将变换后的矩阵进行缩放，$x, y$ 方向分别被缩放 $(\lambda_1, \lambda_2)$ 即变换矩阵$S$。
3. 将变换后的特征向量 $v_1$ 和 $v_2$ 旋转回原来的位置即变换矩阵 $R$
![](/images/graphics2/graphics2_decompose.png)

### 绕任意轴旋转

前文中提到了绕 $x, y, z$轴旋转的变换矩阵，根据矩阵分解和组合的特性我们可以推倒出绕任意轴旋转的变换矩阵。绕任意轴旋转可以被分解为以下三步：
1. 通过变换矩阵$R_{uvw}^T$将需要做旋转的向量的向量空间变换到笛卡尔坐标系中，使得原向量空间下的 $u, v ,w$ 和 $x, y, z$做对齐。
2. 做绕 $x$ 轴的旋转，变换矩阵为标准旋转矩阵$R_x$
3. 通过变换矩阵$R_{uvw}$将旋转后的矩阵, 在重新变换为原向量空间下的 $u, v, w$中。

以上过程可以用以下公式表示：
$$
R_a = R_{uvw} R_x R_{uvw}^T
$$

下面讨论下$R_{uvw}$这个矩阵，这里面会涉及到一部分向量空间的知识。为了方便理解，这里先不过多的讨论理论知识，按照方便理解的方式来进行讨论。假设$x, y, z$轴三个方向上的单位向量组成的矩阵：
$$
R_E = 
\begin{bmatrix}
1 & 0 & 0\\
0 & 1 & 0\\
0 & 0 & 1
\end{bmatrix}
$$
这三个向量互为正交，因此当我们为其做一些线性变换后(比如说绕z轴旋转45度后再绕y轴旋转45度，然后在缩放2倍)，三个向量依旧是正交的，通过线性变换 $R_{uvw}^T$ 变换之后，我们可以得到正交矩阵 $R_{uvw}$ 
$$
R_{uvw} = 
\begin{bmatrix}
x_u & x_v & x_w\\
y_u & y_v & y_w\\
z_u & z_v & z_w
\end{bmatrix}
$$
下面证明上面的表达式为何能成立，假定$u = x_ux + y_uy + z_uz$，$v, w$同理，根据正交矩阵的特性，可以得到以下推倒：
$$
u  \cdot u = v \cdot v = w \cdot w = 1
$$
$$
u \cdot v = v \cdot w = w \cdot u = 0
$$
利用这个特性，我们可以使用变换矩阵$R_{uvw}$将u变换到x轴上：
$$
R_{uvw}u = 
\begin{bmatrix}
x_u & x_v & x_w\\
y_u & y_v & y_w\\
z_u & z_v & z_w
\end{bmatrix}
\begin{bmatrix}
x_u \\
y_u \\
z_u
\end{bmatrix}
= 
\begin{bmatrix}
x_ux_u + y_vy_v + z_wz_w\\
x_vx_u + y_vy_u + z_vy_u\\
x_wx_u + y_wy_u + z_wx_u
\end{bmatrix}
$$
上式可以被化简为：
$$
R_{uvw}u = 
\begin{bmatrix}
u \cdot u\\
v \cdot u\\
w \cdot u
= 
\end{bmatrix}
=
\begin{bmatrix}
1 \\
0 \\
0
\end{bmatrix}
= 
x
$$
同理可以推导出$R_{uvw}v = y$以及$R_{uvw}w = z$
对于还原变换矩阵$R_{uvw}^T$同理也可以通过上述方式推导如何将坐标轴${x, y, z}$变换到$u, v, w$上去。
总结来看，对于任意轴的旋转可以用下列公式进行表示：
$$
R_a = R_{uvw} R_x R_{uvw}^T = 
\begin{bmatrix}
x_u & y_u & z_u\\
x_v & y_v & z_v\\
x_w & y_w & z_w
\end{bmatrix}
\begin{bmatrix}
cos\varphi & -sin\varphi & 0\\
sin\varphi & cos\varphi & 0\\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x_u & x_v & x_w\\
y_u & y_v & y_w\\
z_u & z_v & z_w
\end{bmatrix}
$$

## 齐次坐标
平移操作也是矩阵中经常会出现的情况，它变换后的坐标$x^\prime,y^\prime$可以很方便的通过公式表示:
$$
x^\prime = x + x_t
$$
$$
y^\prime = y = y_t
$$
但是平移变换很难通过与待变换矩阵同纬度的变换矩阵来进行表示，因为它不属于线性变换。这里需要引入其次坐标的概念，通过原本是n维的向量用一个n+1维向量来表示。比如我们原有的平面中的点$(x, y)$升纬到3D表示为：$[x, y, 1]^T$，这样就可以将平移操作表示3D中的线性变换矩阵：
$$
\begin{bmatrix}
m_{11} & m_{12} & x_t\\
m_{21} & m_{22} & y_t\\
0 & 0 & 1
\end{bmatrix}
$$
此时再对$x^\prime,y^\prime$做计算，可以得到：
$$
\begin{bmatrix}
x^\prime \\
y^\prime \\
1
\end{bmatrix}
=
\begin{bmatrix}
m_{11} & m_{12} & x_t\\
m_{21} & m_{22} & y_t\\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
= 
\begin{bmatrix}
m_{11}x + m_{12}y + x_t\\
m_{21}x + m_{22}y + y_t\\
1
\end{bmatrix}
$$

**注：在其次坐标中最后一维取值为0时，代表一个方向，取值为1时代表一个点**