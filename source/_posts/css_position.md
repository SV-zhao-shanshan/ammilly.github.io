---
layout: post
title: 关于 CSS position 属性分析
tags: [css,position,static,relative,absolute,fixed,sticky]
date: 2022-07-18 17:00:00 +800
---

> CSS 的 position 属性有5个属性值，那么这几个属性值是什么含义，它们之间有什么关系，本文将加以分析。

<!--more-->

## 静态定位 static
static 值是默认值，任何页面元素在没有设置 position 的情况下，它默认的 position 就是 static，是元素的正常布局行为，即元素在文档常规流中当前的布局位置，此时 top, right, bottom, left 和 z-index 属性均无效。

## 相对定位 relative
这里的相对定位，很多人不清楚它是相对于谁定位，这里强调一下，一个元素设置了 position 值为 relative，实际上是相对于它**自身**进行定位，即元素在未添加定位时的位置，在不改变页面布局的前提下调整元素位置，因而会在该元素未添加定位时的位置留下空白。
1. top - 相对于自身位置向下移动 top 大小距离；
2. right - 相对于自身位置向左移动 right 大小距离；
3. bottom - 相对于自身位置向上移动 bottom 大小距离；
4. left - 相对于自身位置向右移动 left 大小距离。  

**注意** position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。

## 绝对定位 absolute
绝对定位的元素会脱离正常文档流，并不为元素预留原空间，通过指定元素相对于最近的**非 static 定位**的祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。


## 固定定位 fixed
也是一种绝对定准，元素会被移出正常文档流，不会为元素预留空间，通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。   
打印时，元素会出现在每页的固定位置。fixed 属性会创建新的层叠上下文。当元素祖先的 transform, perspective 或 filter 属性非 none 时，容器由视口改为该祖先。

## 粘性定位 sticky
粘性定位可以被认为是相对定位和固定定位的混合，元素在跨越特定阈值前为相对定位，之后为固定定位。元素根据正常文档流进行定位，然后相对它的**最近滚动祖先(nearest scolling ancestor)**和 containing block(最近块级祖先，nearest block-level ancestor)，包括 table-related 元素，基于 top, right, bottom 和 left 的值进行偏移。偏移值不影响任何其它元素的位置。  

该值总是创建一个新的**层叠上下文(stacking context)**。注意，一个 sticky 元素会"固定"在离它最近的一个拥有"滚动机制"的祖先上（当该祖先的 ovewflow 是 hidden, scroll, auto 或 overlay 时），即便这个祖先不是最近的真实可滚动的祖先，这有效地抑制了任何"sticky"行为。

```CSS
#test-id {
  position: sticky;
  top: 10px;
}
```
> 在 viewport 视口滚动到元素 top 距离小于10px 之前，元素为相对定位。之后，元素将固定在与顶部距离10px 的位置处，走到 viewport 视口滚到阈值以下。

**注意**须指定 top, right, bottom 或 left 四个其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

本篇完
