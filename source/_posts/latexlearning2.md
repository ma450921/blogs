---
title: 使用latex来书写数学公式2 —— latex数学公式
tags: [latex, 排版, 论文写作]
categories: [latex]
index_img: /images/latex/latex_head.png
date: 2020-08-11 12:09:11
math: true
---
## 综述
LATEX 使用一种特有的模式来排版数学(mathematics) 公式。数学公式允许以 行间形式排版在一个段落之中，也可以以独立形式排版，此时段落可能会被拆 开。处于段内的数学文本要放在 `\(` 与`\)` 之间，$ 与$ 之间，或者`\begin{math}` 与 `\end{math}` 之间。

例如`c^{2}=a^{2}+b^{2}`经过latex转换为公式：

`$c^{2}=a^{2}+b^{2}$`

`100 m^{3}  \heartsuit`转换为：
`$100 m^{3} \heartsuit$`


$$\lim_{n \to \infty} \sum_{k=1}^n \frac{1}{k^2} = \frac{\pi^2}{6}$$