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
着色（shading）过程是

```
shad·ing, [ˈʃeɪdɪŋ], noun
The darkening or coloring of an illustration or diagram with parallel lines or a block of color.
```
![](/images/graphics6/graphics6_shading.png)
## Blinn-Phong 反射模型

## 着色模型

### Flat Shading(平面着色)

### Gourand Shading（定点着色）

### Phong Shading（像素着色）