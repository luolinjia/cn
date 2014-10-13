---
layout: post
title: 为Jekyll博客添加Atom
categories:
- Data
- Study
tags:
- Atom
- RSS
- Jekyll
- 订阅
---

> 一般情况下，一个站点都有RSS，Atom等，这些都是为了方便访客及时了解自己订阅的网站的更新，一般能够达到抢沙发等目的，当然，主要还是给订阅者（或者说老顾客）一个及时的更新反馈。那么接下来，记录一下一个Atom源是怎么创建的。   

# Atom为何物？  
不过实话讲，在为自己的博客写Atom之前，本人也只对RSS有初步的了解，对订阅有一定的了解，概念都存在一定的模糊。  


说Atom，得先了解网络上的**订阅**是什么概念，一个网站的订阅跟邮局那种普通的订阅报刊性质差不多，不过网站上的RSS/Atom订阅一般都是免费的，因为只在乎一种信息的传递，并且也可以随时退订。网站订阅有两种，一种是像我用的Google Reader这类，称之为在线订阅，如果是安装了RSS阅读器、Feed阅读器，则称之为离线订阅。  

那么Atom到底是什么？区别于RSS，Atom是由于RSS规范不统一，版本混乱而产生的替代品，Atom是[IETF](http://baike.baidu.com/view/155093.htm)的建议标准，Atom Syndication Format是基于XML格式（[RFC4287](http://tools.ietf.org/html/rfc4287)），Atom Publishing Protocol则是基于HTTP协议格式（[RFC5023](http://tools.ietf.org/html/rfc5023)）。  

按照Atom的标准格式，自己写了一份关于此博客的：  

{% highlight xml %}
---
layout: nil
---
<?xml version="1.0"?>
<feed xmlns="http://www.w3.org/2005/Atom">
 
  <title>ideex.name</title>
  <link href="http://ideex.name/cn/"/>
  <link type="application/atom+xml" rel="self" href="http://ideex.name/cn/atom.xml"/>
  <updated>{{ "{{ site.time | date_to_xmlschema " }}}}</updated>
  <id>http://ideex.name/cn/</id>
  <author>
    <name>Karl Luo</name>
    <email>ideexto@gmail.com</email>
  </author>


 {{ "{% for post in site.posts " }}%}
  <entry>
    <id>{{ "{{ site.url " }}}}{{ "{{ post.id " }}}}</id>
    <link type="text/html" rel="alternate" href="{{ "{{ site.url " }}}}{{ "{{ post.url " }}}}"/>
    <title>{{ "{{ post.title | xml_escape " }}}}</title>
    <updated>{{ "{{ post.date | date_to_xmlschema " }}}}</updated>
    <author>
      <name>Karl Luo</name>
      <uri>http://ideex.name/cn/</uri>
    </author>
    <content type="html">{{ "{{ post.content | xml_escape " }}}}</content>
  </entry>
 {{ "{% endfor " }}%}

</feed>
{% endhighlight %}     



把这段代码保存为atom.xml到主目录中，在网页上加以引用即可使用：   

{% highlight html %}
<link href="{{ site.url }}/atom.xml" rel="alternate" title="Atom Rss" type="application/atom+xml" />
{% endhighlight %}    

这样一个Atom的订阅就做好了，还是蛮简单的。不需要绕很多的圈子。  

更多细节参考：  
- <https://github.com/luolinjia/cn/blob/gh-pages/atom.xml>  


#### 参考  
- <http://www.metsky.com/archives/361.html>