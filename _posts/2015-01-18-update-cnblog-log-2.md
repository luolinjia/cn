---
layout: post
title: 博客更新日志(v3.3)
logo: http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/title_zps31f49f82.png
categories:
- Study
tags:
- 响应式
- 移动设备
- 体验
- css3
---

> 上次更新日志：[v3.2更新日志](http://ideex.name/cn/2014/12/update-cnblog-log/)

本次更新下面3个体验性功能：  

- 移动设备下，主页更换
- 移动设备下，文章列表页和具体的文章页体验性增强
- 标签页，展示方式更改

> 缘由：一直想做的页面响应式设计，搁置至今。无论是来访的朋友，还是我自己，都可以随心所欲地在自己的手机上完美浏览文章，更加方便地提供了一种可供选择的方法。  

## 移动设备下，主页更换  

针对之前的主页，在 PC 上的体验还是将就的，可切换到移动设备这里后，会有一些不一致的问题产生，首先就是事件机制，由于移动设备上的事件机制和 PC 上很多地方是不一样的，要做到完全一致，不太可能，并且受到移动设备屏幕变小的缘故，这里估计手都不好触屏。于是我在以下3个方法进行优化：

- 首页重做，目前就用一个简单的主页过度，这里就设计了2个按钮，一个中文博客链接，一个最近新开的英文博客链接，去掉了中文博客多余的2个链接，即个人页和标签页。
- 字号增大，为2.5em； 周围间距增大，为5em；行高增大，为2em
- 颜色用本博客的主打色深绿#127F78，字体同样。


正常 PC 下主页:  
![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_2_zpsac77f2ac.jpg)  

移动设备端主页:  
![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_1_zps47c8429e.jpg)  


## 移动设备下，文章列表页和文章页  

针对移动设备，作如下调整：  

- 文章列表页
  - 默认进来文章列表全隐藏，主要是考虑到移动设备偏小，所能够涵括的信息不多，所以用年份来展示
  - 导航菜单靠左放置，并把图标变大，为25px
  - 文章列表年份小方块调整为2em，具体文章列表字号调整为20px，间距调整为2em，做这样的调整，主要还是考虑用户体验，为了能够让我们的手可以轻松触发。
  
![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_3_zps6a4d811c.jpg)  

- 文章页
  - 文章 Logo 置于文章顶部
  - 文章 title 顺放，不再向左相对偏移。
  - 字号调整大，图片100%宽度显式。  

PC上文章页:  
![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_4_zpsefa260d2.jpg)  

移动端文章页:  
![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_5_zps866bd492.jpg)  

移动端文章浏览页:  
![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_6_zps693c50ea.jpg)  


## 标签页  

之前所有的文章都会在标签页加载出来，我这里更改为按需求加载标签，使之更加人性化。再也不用在标签页跳过去跳过来了，因为现在只会显式你点击标签的文章列表。（目前就是在移动设备上，还有点体验不佳的情形，待稍后更新。）  

![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/20150118_7_zpsc9bde95e.jpg) 
