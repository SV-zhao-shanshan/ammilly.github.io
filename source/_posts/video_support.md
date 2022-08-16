---
layout: post
title: 不同编码格式视频播放各浏览器支持情况
tags: [video,chrome,firefox,edge,ie,browser]
date: 2022-08-16 17:00:00
---

> 本文列出当前浏览器对不同编码格式视频的播放支持。

<!--more-->

## 总结如下

| | AVC(H264) | HEVC(H265) | MPEG4(DIVX) | MPEG4(Xvid) |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| Chrome | 正常播放 | 有音频，无视频 | 有音频，无视频 | 有音频，无视频 |
| FireFox | 正常播放 | 有音频，无视频 | 有音频，无视频 | 有音频，无视频 |
| Edge | 正常播放 | 正常播放 | 正常播放 | 正常播放 |
| IE | 正常播放 | 不能播放 | 不能播放 | 不能播放 |


## 解决办法

参考：   
[不同编码的MP4视频在各大游览器播放总结](https://blog.csdn.net/tester1995/article/details/103509263)  
[Chrome使用video无法正常播放MP4视频的解决方案](https://www.cnblogs.com/Yellow-ice/p/13743400.html)