---
title: CSS盒模型
date: 2020-08-27 09:44:47
updated:
tags: CSS
categories: CSS
keywords: CSS盒模型
description: 盒模型的理解
top_img: 
cover: /img/cssImg.jpeg
comments: true
toc:
toc_number:
auto_open:
copyright:
copyright_author: 赵查查
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer: true
highlight_shrink:
---
参考与[solocoder](https://juejin.im/post/6844904146168987661#heading-20)
    
问题
+ 基本概念: 标准模型 + IE模型
+ 标准模型 和 IE模型的区别
+ CSS如何设置这两种模型
+ JS如何设置这两种模型
+ 用于解释边距重叠
+ BFC 边距重叠的解决方案
## 1.基本概念

CSS 盒模型本质上是一个盒子，它包括: 边距，边框，内容，和实际内容
![css.png](/img/css.jpg)

## 2.标准模型与IE模型的区别

+ 计算方式不同
  + 标准模型计算元素只计算 content 的宽高
  + IE模型是 content + padding + border

## 3.设置这两种模型

```css
标准模式: box-sizing: content-box
IE模式  : box-sizing: border-box
```
box-sizing 默认的 content-box 就是标准模型

## 4.BFC

Block Formatting Context（块级格式化上下文）。

1. BFC的布局规则

    1. 内部的Box会在垂直方向，顺序排列。
    2. Box垂直方向的距离有margin决定，相邻Box的margin会重叠。
    3. BFC的区域不会与float box重叠。
    4. BFC就是一个隔离的容器,容器里面和外面互不影响。
    5. 计算BFC的高度时,浮动元素也参与计算
2. 创建BFC
    1. float的值不少none
    2. position的值不是static或者relative
    3. overflow


