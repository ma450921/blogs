---
title: 计算机图形学5 —— 反走样
tags: [计算机图形学, 光栅化, 反走样]
categories: [GAMES101]
index_img: /images/graphics5/graphics5_banner.jpg
date: 2020-08-30 17:09:11
math: true
---

## 走样及其原因
上一节介绍图形的光栅化的过程，经过图形光栅化过程后，可以得到一个像素集合，它将表示我们想要光栅化的图形，这些点被称为采样信号（sample Sampled Signal）
![](/images/graphics5/graphics5_sample.png)
上图表示了三角形光栅化的过程，可以看到最后形成的采样信号与原信号差距还是比较大的，这种现象就是我们所说的走样(Aliasing)。它对于实际的图形成像的影响非常大，出现走样现象的图形，会在边界处出现各种情况的锯齿，这些锯齿很影响实际的成像体验。
## 反走样原理

## MSAA

## SSAA

## FXAA

## MLAA

## SMAA

## TXAA

## DLSS