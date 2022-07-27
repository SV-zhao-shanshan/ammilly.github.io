---
layout: post
title: 微信小程序 tab 切换时触发的触底事件 onReachBottom
tags: [wx,app,onreachbottom,tab,loading]
date: 2022-07-27 14:30:00 +800
---

> 最近在做微信小程序开发项目，遇到了一个比较棘手的问题，就是页面中的 tab 切换需要重新加载数据，但是此时由于 tab 内容高度变化触发了页面触底事件 onReachBottom，又加载了一次数据，导致数据的重复。

<!--more-->

## 问题描述
微信小程序的页面生命周期方法中提供了一个 onReachBottom 方法，监听页面滚动是否到底部，归页面滚动到底部时处理相应的逻辑，在我的项目中，主要是触底加载下一页数据，即上拉加载更多，当前没有数据更多数据了显示***我也是有底线的***。由于页面组成比较复杂，列表数据展示在3个不同的 tab 中，分别展示不同类型数据，当 tab 切换时去加载对应类型的数据。  
问题就在于当切换 tab 时，页面高度发生变化，同时触发了 onReachBottom 事件，导致了页面数据重复被加载显示。

## 尝试解决方案
#### 尝试方案一
由于是触发了两个事件，考虑在触发另外一个事件加载数据前加一个判断，先想到的就是增加一个标志位 isReachBottom，初始为 false，当页面触底时，设置 isReachBottom 为 true.
```JavaScript
//...
data: {
  ...
  isReachBottom: false
},
//
getList() {
  // ...
},

// ...

onReachBottom() {
  this.setData({
    isReachBottom: true
  });
  this.getList();
}

onTabChange() {
  // 其它逻辑
  if(this.data.isReachBottom) return;
  this.getList();
}
```
但是问题就在于，当其中一个 tab 页触底后，isReachBottom 就被设置成了 true，切换到其它 tab 后，这个值也仍然是 true，导致出现 tab 切换时不加载数据。

#### 尝试方案二
因为同时触发两个加载事件，增加防抖或者节流来阻止多次执行，但是这个方案引入了宏任务，导致执行顺序偶尔不符合预期，增加了代码复杂度。

```JavaScript
// 防抖
funcion debounce(fn, wait) {
  let timer = null;
  return () => {
    if(timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.call(this);
    }, wait);
  }
}
```

```JavaScript
// 节流 - 利用延时器实现
function throttle(fn, wait) {
  let timer = null;
  return () => {
    if(timer) return;
    timer = setTimeout(() => {
      fn.call(this);
      timer = null;
    }, wait)
  }
}

// 节流 - 利用时间戳实现
function throttle(fn, wait) {
  let last = 0;
  return () => {
    let now = Date.now();
    if(now - last > wait) {
      fn.call(this);
      last = Date.now();
    }
  }
}
```

## 最终解决方案
最后考虑到 onTabChange 事件是在 onReachBottom 事件触发前触发的，设置一个 isTabChange 标志，初始值为 false，当 tab 切换时，首先把 isTabChagne 设置为 true，此时后触发的 onReachBottom 在请求数据前增加判断，如果 isTabChange 为 true，则 return，否则触底继续加载数据。每次请求数据返回后把 isTabChange 置为 false。

```JavaScript

data: {
  isTabChagne: false
}

getList() {
  // ajax 请求结束
  this.setData({
    //...
    isTabChange: false
  });
}

onTabChange() {
  this.setData({
    isTabChange: true
  });
  this.getList();
}

onReachBottom() {
  if(this.data.isTabChange) return;
  this.getList();
}

```
通过 disable 触底加载逻辑，可能成功避免重复加载问题。

## 结论
最终解决方案是在经过两次失败尝试后想出来的，仔细思考一下其实就是和尝试方案一相同的思路，方案一想通过知道是否触底来在 tabchagne 时避免再次加载，而解决方案是通过知道是否是 tabchagne 来在触底时避免再次加载，还是要多多思考。

本篇完
