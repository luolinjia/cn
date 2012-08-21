---
layout: post
title: Markdown标记语言
categories:
- Data
- Study
tags:
- Markdown
- Text
- Jekyll
---

# 何为**Markdown**？  
[Markdown](http://daringfireball.net/projects/markdown/)只是一个工具，它将你写的符合markdown语法的text文件转化成HTML，最初的目的就是为了让我们书写的时候易于阅读、易于编写，总之就是简化人的HTML思维，让我们可以全身心投入到写作，而不必担心HTML标签和样式(样式之前先定义好)。  

一份使用Markdown格式编写的文件应该可以直接以纯文本发布，并且看起来不会像HTML一样众多的标签所构成，其语法受到一些既有的text-to-HTML格式的影响，包括[Textile](http://textism.com/tools/textile/)、[EtText](http://ettext.taint.org/doc/)，而最大灵感来源其实是纯文本电子邮件格式。

# 如何解析**Markdown**文件(jekyll下)  
一个Markdown文件，既然要被解析，就一定得对应有解析器(注意在此之前，将文件保存为\.md/\.mkd/\.mkdn/\.mark后缀格式)，一般在**jekyll博客系统**中，已经有一个默认的Markdown解析器Maruku(纯ruby写的markdown模版解释器)，不过由于效率比RDiscount(用C写的模版解释器)低很多，RDiscount解析器已经成为很多博客的选择，以下是其效率:

> BlueCloth: 13.029987s total time, 00.130300s average  
>   Maruku: 08.424132s total time, 00.084241s average  
> RDiscount: 00.082019s total time, 00.000820s average    

然后使用RubyGems进行安装：

{% highlight ruby %}
gem install rdiscount
{% endhighlight %}  

如果需要使用此解析器，在启动jekyll项目的时候加上参数：

{% highlight ruby %}
jekyll --rdiscount
{% endhighlight %}  

或者，你可以在**\_config.yml** 文件中加入以下语句：

{% highlight ruby %}
markdown: rdiscount
{% endhighlight %}  

这样markdown文件就可以被解释为HTML了，当然，这里只讨论了在jeyll博客下的情况，如果你是用在java里，也有相关的解释器，网上也有javascript类型的解释器，所以markdown只是规定了一种语法，一种书写的格式，至于你要在哪儿用，都可以人为的选择。  

# **Markdown语法**  
此部分有中文的[详细教程](http://wowubuntu.com/markdown/index.html)。  

所以在此粗略描述：  

> ### 1-6级标题：  
>> \# 一级标题  
>> \## 二级标题  
>> \###### 六级标题  

> ### 另外一种标题展示，只提供h1和h2标签  
>> 另起一行，添加4个以上的====(h1标签)，----(h2标签)

> ### 换行：  
>> 敲两个回车(或者在一行的末端加两个或两个以上的空格)  

> ### 插入一个URL超链接，用尖括号括起来即可  
>> <(网址)http://ideex.name>  

> ### 加上文字链接,鼠标放上面会出现介绍文字
>> \[google](http://google.com.hk "google")  

> ### 引用块  
>> 在内容前插入 >  符号即可(可以嵌套使用，即 > >  等)  

> ### 代码块  
>> 在内容前面敲4个空格  

> ### 水平横线  
>> 3个或以上的星号、减号、下划线单独放在一起即可生成一条横线  

> ### 斜体和粗体  
>> 斜体：两边分别加上一个\*或者\_  
>> 粗体：两边分别加上2个*或者\_  


> ### 无序列表  
>> 在每一项前面加上一个" * "、或者"+"、或者"-"即可，并且一定要跟一个空格

> ### 有序列表  
>> 在内容面前加上一个数字(随便一个数字),在加上一个".",跟空格，即可得到一个有序列表。


欲了解更多，参考官网：<http://daringfireball.net/projects/markdown/>

       

#### 参考  
- <http://www.yangzhiping.com/tech/wordpress-to-jekyll.html>  
- <http://chxt6896.github.com/blog/2012/01/02/blog-textile.html>  
- [https://github.com/mojombo/jekyll/wiki/install](https://github.com/mojombo/jekyll/wiki/install)  

   