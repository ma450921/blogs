---
title: 使用latex来书写数学公式1 —— latex基础知识
tags: [latex, 排版, 论文写作]
categories: [latex]
index_img: /images/latex/latex_head.png
date: 2020-08-10 21:09:11
math: true
---
## TEX && LATEX

TEX 是Donald E. Knuth 编写的一个以排版文章及数学公式为目标的计算机程序。1977 年，在意识到恶劣的排版质量正在影响自己的著作及文章后，Knuth 开始编写TEX 排版系统引擎，探索当时开始进入出版工业的数字印刷设备的潜 力，尤为希望能扭转排版质量下滑的这一趋势。我们现在使用的TEX 系统发布于 1982 年，在 1989 年又稍做改进，增加了对 8 字节字符及多语言的支持。TEX 以其卓越的稳定性、可在不同类型的电脑上运行以及几乎没有缺陷而著称。TEX 的版本号不断趋近于 π，现在为 3.141592。

LATEX 是一个宏集，它使用一个预先定义好的专业版面，可以使作者们高质量的 排版和打印他们的作品。LATEX 最初由 Leslie Lamport 编写，它使用 TEX 程序作为排版引擎。

## LATEX源文件

LATEX 源文件为普通的ASCII 文件，你可以使用任何文本编辑器来创建。LATEX 源文件不仅包含了要排版的文本，而且也包含了告诉LATEX 如何排版这些文本内 容的命令。

### 空白

空格和制表符等空白字符在LATEX 中被看作相同的空白距离(space)。多个连续 的空白字符等同于一个空白字符。在句首的空白距离一般会被忽略，单个空行也 被认为是一个“空白距离”。

两行文本间的空白行标志着上段的结束和下段的开始。多个空白行的作用 等同于一个空白行。下面便是一个例子，上边是源文件中的文本，下边是排版后的结果。

``` latex

It does not matter whether you enter one or several    spaces after a word.

An empty line starts a new paragraph.

```
![](/images/latex/latex1_graph1.png)

### 特殊字符

下面的这些字符是 LATEX 中的保留字符(reserved characters)，它们或在LATEX 中 有特殊的意义，或不一定存在于所有字库中。如果你直接在文本中输入这些字符，通常它们不会被输出，而且还会导致 LATEX 做一些你不希望发生的事情。需要在这些字符前加上反斜线，它们才可以正常的输出到文档中。
``` latex
# $ % ^ & _ { } \ 

#添加反斜线可以输出到文档中
\# \$ \% \^{} \& \_ \{ \} \{}
```
### 命令

ATEX 命令(commands) 是大小写敏感的，有以下两种格式:

* 以一个反斜线(backslash)\开始，命令名只由字母组成。命令名后的空格 符、数字或任何非字母的字符都标志着该命令的结束。
* 由一个反斜线和非字母的字符组成。

LATEX 忽略命令之后的空白字符。如果你希望在命令后得到一个空格，可以 在命令后加上{} 和一个空格，或加上一个特殊的空格命令。{} 将阻止LATEX 吃掉命令后的所有空格。
``` latex
I read that Knuth divides the  
people working with \TeX{} into 
\TeX{}nicians and \TeX perts.\\ 
Today is \today. 
```
![](/images/latex/latex1_graph2.png)

有些命令需要一个参数(parameter)，该参数用花括号(curly braces) { } 括 住并写在命令的后面。一些命令支持可选参数(optional parameters)，可选参数可用方括号(square brackets) [ ] 括住，然后写在命令的后面。

``` latex
You can \textsl{lean} on me!
```
![](/images/latex/latex1_graph3.png)
### 注释
当LATEX 处理一个源文件时，如果遇到一个百分号%，LATEX 将忽略%后的该行内容，换行符以及下一行前的空白字符。

我们可以据此在源文件中写一些注释，而且这些注释并不会出现在最后的排版结果中。

``` latex
This is an % stupid
% Better: instructive <---- example: Supercal%
              ifragilist%
    icexpialidocious
```
![](/images/latex/latex1_graph4.png)

如果注释的内容较长，你可以使用verbatim 宏包提供的comment环境。
``` latex
This is another \begin{comment}
rather stupid,
but helpful
\end{comment}
example for embedding comments in your document.
```
![](/images/latex/latex1_graph5.png)

