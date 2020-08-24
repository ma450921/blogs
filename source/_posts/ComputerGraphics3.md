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



