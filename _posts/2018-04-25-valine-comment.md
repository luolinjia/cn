---
layout: post
title: 第三方评论
categories:
- Technology
tags:
- Valine
- Jekyll
---

disqus被墙以后，还真不好找类似的可替代方案，国内的友言不支持https，畅言又需要备案，通过google找到了这么一个适合我这种静态博客的第三方评论：[valine](https://valine.js.org/quickstart/)，其存储用的LeanCloud，还算比较方便。  

### HTML 引入

{% highlight html %}
<head>
    ...
</head>
<body>
    ...
    <div id="comment"></div>
    <script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
    <script src='//unpkg.com/valine/dist/Valine.min.js'></script>
    <script>
        new Valine({
            el: '#comment', // 绑定的dom唯一id
            notify: false,  // 邮件通知，默认false
            verify: false,  // 提交验证，这个体验很不好，默认false
            appId: 'LeanCloud上面的appid',
            appKey: 'LeanCloud上面的appkey',
            placeholder: '说点什么吧',
            path:window.location.pathname,  // 页面url地址，保存在leadcloud里面
            avatar:'mm' // 头像类型，可以自己选择定义
        });
    </script>
</body>
{% endhighlight %}

> 样式，可以自己调整，保证其权重比它默认的高就可以

如果要导入之前的disqus评论，这里有个转换的网址：<http://disqus.panjunwen.com/>，但是存在一个问题，就是之前评论的时间会遗漏，也就是这里的评论时间会按照存储的createAt来，这里我已经给对应的project提了issue，希望他们可以修复吧。我这个小站，人也不多，就暂时不用管这个issue了。  

这样，就可以体验评论了，希望这种方式可以长久点！