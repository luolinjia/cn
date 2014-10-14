---
layout: post
title: 滚动条事件（向上和向下）
logo: http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/scroll_zps1b718c07.jpg
categories:
- Study
- 前端
tags:
- css3
- jQuery
- 滚动条
---

开发的时候，我们很多时候需要检测**滚动条**的**向上**滚和**向下**滚事件，那么这里的思路：  

> 用一个标志的位置记住当前滚动的位置  
> 再滚动时，位置变化，再进行位置更新  
> 如果变化的新位置比老的位置大，那么就是向下的，反之为向上  

示例：  
{% highlight html %}
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>test the scroll</title>
    <style>
        *{
            padding: 0;
            margin: 0;
        }
        #header {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 60px;
            background-color: #333;
        }
        #header.in {
            top: 0;
            -ms-transition: all 0.6s ease;
            -webkit-transition: all 0.6s ease;
            -moz-transition: all 0.6s ease;
            -o-transition: all 0.6s ease;
            transition: all 0.6s ease;
        }
        #header.out {
            top: -85px;
            -ms-transition: all 0.6s ease;
            -webkit-transition: all 0.6s ease;
            -moz-transition: all 0.6s ease;
            -o-transition: all 0.6s ease;
            transition: all 0.6s ease;
        }

        #content {
            width: 100%;
            height: 2000px;
            background-color: #eee;
        }
    </style>
    <script type="text/javascript" src="js/jquery-1.11.1.min.js"></script>
    <script type="text/javascript">

        var currentScroll = 0;

        $(function(){

            $(window).scroll(function(){
                myScroll();
            });
        });

        function myScroll() {
            var nextScroll = $(this).scrollTop();

            console.log('scrollPosition' + $(document).scrollTop() + 'currentScroll: => ' + currentScroll + '  nextScroll: => ' + nextScroll);

            if (nextScroll > currentScroll){
                $('#header').removeClass('in').addClass('out');
                if (nextScroll >= $(document).height()-$(window).height()){
                    $('#header').removeClass('out').addClass('in');
                }
                if ($(document).scrollTop() <= 0) {
                    $('#header').removeClass('out').addClass('in');
                }
            }
            else {
                $('#header').removeClass('out').addClass('in');
            }
            //Updates current scroll position
            currentScroll = nextScroll;
        }
    </script>

</head>
<body>
<div id="header"></div>
<div id="content"></div>
</body>
</html>
{% endhighlight %}
