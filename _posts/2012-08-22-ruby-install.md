---
layout: post
title: Ruby安装和配置(windows下)
categories:
- Data
- Study
tags:
- Ruby
- Windows
- Jekyll
---

> 我承认，这Ruby的安装和Python一样，过程基本没区别，甚至Ruby还简单些。  

# 下载Ruby  
[点此下载Ruby](http://rubyinstaller.org/downloads/)，进去选择你需要的版本，我这里选择的是Ruby 1.9.2-p290，这个版本比较稳定。 

# 安装  
 
下载完后，就一直傻瓜式操作一步到底，最后安装需要你选择的选项尽量都勾上，如果不勾上，也可以自己在计算机属性-环境变量里面进行配置，如果不需要关联\.rb文件，也可以取消Associated选项。  

# 测试  

是否安装成功，到cmd，直接输入ruby，会跳到ruby的编辑页面，输入：  

{% highlight ruby %}
puts "Hello wolrd!"
{% endhighlight %}  

回车，再按Ctrl+D，即可打印出Hello world！ 