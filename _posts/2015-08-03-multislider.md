---
layout: post
title: 图片轮播
categories:
- Work
tags:
- jquery
- 图片
- 幻灯片
---

> 图片轮播新尝试  

> [DEMO地址](http://luolinjia.com/fe-working/jquery-multislider/)  
> [Sources地址](https://github.com/luolinjia/fe-working/tree/gh-pages/jquery-multislider)

OneNET平台首页需要一个图片轮播的插件，本以为效果跟[unslider](http://unslider.com/)差不多就可以了，没想到，设计师的需求还是相当给力的（想法还是很不错的）。

一般来讲，图片轮播有两个层级步骤就不错了，一是图片本身的展现，二是展现后会有一些特殊的效果，比如出现button，有hover链接生成，等。  

但在这里，需求为：  

1. 将图片由大变小，有一个缩小的过程；(1000ms)
2. 底层图片不变，下一张图片像展开卷轴一样展开，由右至左；(1000ms)
3. 从左透明度0-1缓慢插入；(1000ms, delay: 1000ms)
4. 图片相关白线淡入；(300ms, delay: 1000ms)

这里，步骤1和2几乎是同时进行，于是在执行动画的时候，对它们的时间都规定为同一时间。而步骤3在1和2之后再执行，第4步最后。总共步骤流程3300ms。其实对于slider，感觉还是太长了。

技术要点： 

- 步骤1：这主要是通过jquery的animate来实现，在动画开始前，将其image宽度设置为其本身的1.25倍，动画执行1000ms过程中，变成原有的宽度；
- 步骤2：这步相对麻烦，这里要考虑到一个情形：第一张如何展示，因为所有的幻灯图片在其展示的时候，都会由右至左，如展开卷轴。但是如果是第一张幻灯图片的话，却不能这样做，因为没有底图，这里就涉及到，底图如何处理。
    - 第一张图片就不需要第2个步骤，只需要第1个步骤即可，既不失优雅，又不会显得突兀；
    - 然后后面的图片在展开的时候，底图，也就是上一张图片不能消失，要在原地。这里就需要对其作判断；
    - 最后，这里的布局，除了最外层的壳为relative，里面都是absolute
- 步骤3：这里用css3的animation和transform的translateX属性；
- 步骤4：只需要css3的animation即可，设置其opacity；

来看看参数:  

{% highlight javascript %}
$('.all').multislider({
    banners: [],                // REQUIRED!!!
    width: 1920,                // the default width value
    dots: true,                 // display the dots under of it.
    number: false,              // display the number regarding dots.
    color: '#e6e6e6',           // set the default color for the dots' background-color
    verticalDuring: 1000,       // the time of animation regarding the transition vertical
    highlight: '#e63939',       // highlight the dots' background-color
    aDuring: 3500,              // set the total animation time
    aBack: true,                // set whether you need to animate something after showing the right to left.
    aDirection: 'RTL'           // 'RTL' right to left; 'LTR' left to right
});
{% endhighlight %}  

除了banners是必须的，其他都不是必须的  

- banners: 是必须的，是传入的image地址
- width: 设置幻灯片的宽度
- dots: 设置其需不需要dots，也就是幻灯片下面的具体位置导航
- number: 设置幻灯导航里面需不需要显示数字
- color: 设置幻灯导航的背景色，默认为#e6e6e6
- verticalDuring: 设置图片左刷新的动画时间
- highlight: 设置幻灯导航高亮的颜色，默认为#e63939
- aDuring: 设置一个幻灯进行的总时间
- aBack: 设置是否需要第3步和第4步
- aDirection: 设置图片幻灯刷新的方向

