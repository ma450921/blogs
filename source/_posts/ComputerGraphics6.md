---
title: 计算机图形学6 —— 着色 
tags: [计算机图形学, 光照, 着色, 渲染模型]
categories: [GAMES101]
index_img: /images/graphics6/graphics6_banner.jpg
date: 2020-10-1 17:09:11
math: true
---
## 可见性和Z-buffer
对于在一个场景中进行绘制时，将3D模型投影到2D屏幕上的时候，总会出现遮盖的情景。那么那些部分可见，那些部分不可见是需要考虑的。最直接的方式就是一层一层，从远及近将这张图给画出来，这样前景的物体就会遮挡住背景的物体。这种绘制方式叫做画家算法（painter's algorithm）
但是这种简单的做法在很多场景是很难解决的，因为远近先后顺序这个很难做判断。比如说下面这个场景三角形$P、R、Q$相互的前后关系很难确定，实际要表现的效果是互有遮挡。因此单纯使用画家算法来一层层进行绘制，是很难达到显示效果的。
![](/images/graphics6/graphics6_painter.png)

Z-buffer算法解决了这个问题，因为对于三角形来说确定遮挡关系很难，但是对于像素来说，是可以有明确的遮挡关系的。对于2D场景中任意一个点$(x, y)$，存储其深度（depth）信息$z$，每次绘制像素点$(x, y)$时，比较其与zbuffer(x, y)的值进行比较，如果小的话则将深度其存储在zbuffer中，并将其颜色信息存储在framebuffer中。当场景计算完毕之后，再通过framebuffer进行绘制。伪代码如下：
```
for (each triangle T)
    for (each sample (x,y,z) in T)
        if (z < zbuffer[x,y]) 
            framebuffer[x,y] = rgb; // 存储颜色信息
            zbuffer[x,y] = z; // 存储z信息
        else
            ... // do nothing
```
## 光照和着色
着色（shading）过程是渲染过程非常关键的一步，简单来说就是光栅化过程中计算每个采样点的颜色色值。在词典中着色的解释如下：

```
shad·ing, [ˈʃeɪdɪŋ], noun
The darkening or coloring of an illustration or diagram with parallel lines or a block of color.
```
正如解释所说，shading过程实际上就是引入明暗不同以及颜色的变化，在图形学课程中，往往shading指对不同的物体，应用不同的材质。

对于一个物体而言，我们之所以能看见他，是因为光线在其表面发生了反射和吸收，之后传达到了人眼中。物体将特定频率的光线进行反射，也形成了其颜色。在相同材质的物体上，却有不同的颜色和明暗变化。这是因为光源以及光的反射方式的不同。如下图所示是一个光线的反射场景：
![](/images/graphics6/graphics6_shading.png)
对于光线反射而言，一共有三种反射类型：
1. 镜面反射
2. 漫反射
3. 环境光

### 镜面反射
镜面反射模型比较简单，以光线与反射面接触点（shading point）的法线方向为中界限，入射角等于出射角。影响镜面反射模型的着色因素主要有入射角度、相机角度、法线方向以及表面材质（影响折射率）。
![](/images/graphics6/graphics6_Specula.png)

### 漫反射
漫反射模型也相对比较简单。理论上在漫反射模型中，无论光源的位置在任何地方，在反射面都会形成所有方向的反射光。而且在漫反射模型中，反射光的强度在任何方向上都是一致的，无论入射光的角度在任意的方向。

但是漫反射能反射多少能量强度的光源与入射角有很直接的关系。假设光源的强度一定时，我们抽象出单位宽度的平行光源，假定在任意角度下，表面材质对漫反射的折射率一定。当入射角度为90度时，理论上反射面能接收被反射所有的光线。但是当反射面与入射光线有一定的夹脚时，反射面就只能接收部分反射光线。此时的光照强度，应该乘于夹角系数$cos\theta = l \cdot n$，其中$l$为入射角的方向，$n$为法线方向。
![](/images/graphics6/graphics6_lambertian.png)

另外，对于光线传播来说，传递的光线总是会随着传播的距离能量会随之衰减。以点光源向四面扩散为例，光源发射出来的能量其实是一定的，那么在任意两个圈上接受到的能量之和一定相等。而离圆心越远，圆的面积越大，单位面积所接受能量也就越弱。其到达某个点的光照强度应为：$I/r^2$，其中$I$为光源光照强度，$r$为距离。
![](/images/graphics6/graphics6_energy.png)

结合上面的讨论，我们可以得出shading point的上大致的漫反射模型公式了：
![](/images/graphics6/graphics6_diffuse_function.png)
其中：
* $k_d$ 为漫反射系数，可能会受到材质等影响
* $I$ 为入射光照强度，$r$ 为shading point距离光源的距离
* $n, l$ 分别如图中所示为法线向量和入射方向

### 环境光



## Blinn-Phong 反射模型

## 着色模型

### Flat Shading(平面着色)

### Gourand Shading（定点着色）

### Phong Shading（像素着色）