---
layout: post
title: 微信小程序模板使用总结
tags: [wx,app,onreachbottom,tab,loading]
date: 2022-07-28 16:23:00 +800
---

> 最近在做微信小程序开发项目，在使用模板时遇到一些问题，在此记录并总结学习。

<!--more-->

## 模板的基本使用
**template**是微信小程序定义模板的关键字，模板定义有以下两种方式：
- WXML 文件内部中定义
```html
<template is="card">
  <view class="card-container">
    <view class="row">
      <view>标题</view>
      <view>{{title}}</view>
    </view>
    <view class="row">
      <view>作者</view>
      <view>{{name}}</view>
    </view>
  </view>
</template>

<view class="page-container">
  <template is="card" wx:for="{{[{title: '标题1', name: '张三1'}, {title: '标题2', name: '张三2'}]}}" wx:key="id" data="{{...item}}"></template>
</view>
```
> 这里 is 的值就是模板的 name 属性值，用于指定使用的模板

- 单独定义模板页面，在使用的 WXML 文件中 import 进来使用
```html
<!-- template.wxml -->
<template is="card">
  <view class="card-container">
    <view class="row">
      <view>标题</view>
      <view>{{title}}</view>
    </view>
    <view class="row">
      <view>作者</view>
      <view>{{name}}</view>
    </view>
  </view>
</template>
```
```html
<!-- home.wxml -->
<!-- 声明需要使用的模板文件 -->
<import src="path/to/template.wxml" />

<!-- 使用 -->
<view class="page-container">
  <template is="card" wx:for="{{[{title: '标题1', name: '张三1'}, {title: '标题2', name: '张三2'}]}}" wx:key="id" data="{{...item}}"></template>
</view>
```

## 模板的作用域
模板拥有自己的作用域，只能使用 data 传入的数据以及模板定义文件中定义的 <wxs /> 模块。

注意：模板只能使用传入的 data 数据和 wxs 中定义的数据，不能使用 Page 或 Component 中定义的 data 属性中的值

本篇完
