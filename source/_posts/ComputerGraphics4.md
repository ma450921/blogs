---
title: 计算机图形学4 ——  光栅化
tags: [计算机图形学, 光栅化]
categories: [GAMES101]
index_img: /images/graphics4/graphics4_banner.png
date: 2020-08-22 21:09:11
math: true
---

> 总体来说，栅格化这个术语可以用于任何将向量图形转换成位图的过程,在通常的应用中，这个术语用来表示在计算机上显示三维形状的流行渲染算法。

> 栅格化目前是生成实时三维计算机图形最流行的算法。实时应用需要立即响应用户输入，并且通常需要至少每秒 24 帧的速率。辐射着色、光线跟踪等其它渲染技术不同，栅格化的速度非常快，但是由于它不是根据光传输的物理规律进行处理的，所以无法正确模拟许多复杂真实光照环境，只能达到足够欺骗人类眼睛的程度。

以上是Wikipedia对于光栅化的解释，大致上来说，光栅化是顶点数据变换为屏幕像素数据的过程。在光栅化中，我们需要考虑如何将位置信息数据转化为像素信息数据，例如如何在屏幕上进行展示一条线、一个圆，一个三角形。如下图所示，在屏幕中实际显示的最小单位是一个像素点。如何在屏幕中绘制像素信息，或使用哪些像素值来表示图形，是光栅化需要考虑的问题。

光栅化的基本过程可以用以下伪代码来表示：
```
for (T in trangels):
    for (P in pixels):
        determine if P is inside T
```
## 像素（pixels）的表示
我们使用一个整数坐标点代表一个像素，像素作为光栅化的最小单位，其尺寸是单位大小为1的正方形。其用其中心点$x, y$来表示其位置坐标。如下图所示：
![](/images/graphics4/graphics4_pixels.png)
## 直线光栅化表示
和绘制一个点相比，绘制一条直线就需要我们关注如何通过直线中每个像素点的位置来表示这个像素点。在photoshop中绘制一条直线时，应用自动产生的像素点的位置。通过下图可以看出，在绘制一条水平或者垂直的直线的时候，只需要连续在对应方向进行绘制即可。但是当直线的斜率为$(0, \infty)$时，表示一条直线就困难很多。此时无法利用像素点直接还原这条直线，只能通过类似表示的方式来表示这条直线。如何绘制直线，演进出很多算法，下面介绍两种直线绘制算法DDA和Bresenham。
![](/images/graphics4/graphics4_lines.png)
### DDA(Digital differience analyzer)
DDA的绘制过程比较简单，绘制像素的位置都由直线的斜率来决定。指定直线的斜率$k$，每个像素点$X, Y$
1. 如果斜率$\left | k \right |  < 1$，则$X = X + 1$, $Y = Y + k$
2. 如果斜率$\left | k \right |  > 1$，则$X = X + 1/k$, $Y = Y + 1$
以上所有结果都进行四舍五入进行取整表示，DDA的绘制过程可以通过以下代码来进行表示：

``` cpp
// calculate dx , dy
dx = X1 - X0;
dy = Y1 - Y0;

// Depending upon absolute value of dx & dy
// choose number of steps to put pixel as
steps = abs(dx) > abs(dy) ? abs(dx) : abs(dy);

// calculate increment in x & y for each steps
Xinc = dx / (float) steps;
Yinc = dy / (float) steps;

// Put pixel for each step
X = X0;
Y = Y0;
for (int i = 0; i <= steps; i++)
{
    putpixel (X, Y, RED);
    X += Xinc;
    Y += Yinc;
}
```

### Bresenham
DDA算法按照直线的函数表达式来绘制，但是并不代表它的准确性和效率就高。实际上，在绘制过程中对值取四舍五入的方式不仅浪费性能，而且可能会造成绘制不准。Bresenham算法有效地解决了这些问题，它在绘制过程中，通过对两个备选绘制点的中值进行比较来决定绘制点的位置。

如下图所示，在斜率$\left | k \right |  < 1$的情况下，当我们绘制下一个点$(x_k+1, unknown)$时，有两个备选点$(x_k+1, y_k + 1)$以及$(x_k+1, y_k)$。这时候我们只需要找出与直线与$x = x_k + 1$相交的交点距离哪个备选点更近即可。
![](/images/graphics4/graphics4_bresenham.png)

## 图形光栅化表示