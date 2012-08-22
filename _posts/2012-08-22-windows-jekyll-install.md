---
layout: post
title: 安装本地Jekyll系统(在windows下)
categories:
- Data
- Study
tags:
- Markdown
- Ruby
- Jekyll
- DevKit
- Pygments
---

本文主要说明本地Jekyll在windows下的安装流程：

# Step1:  

安装Ruby，具体参考：[Ruby安装和配置](http://ideex.name/cn/2012/08/ruby-install/)  

# Step2:  
下载DevKit([点此下载](http://rubyinstaller.org/downloads/))，解压，cmd进入此目录，输入以下命令：  
ruby dk\.rb init  
ruby dk\.rb review  
ruby dk\.rb install  
分别提示以下信息即表示成功，所以之前的Step1安装的Ruby尽量是Installer，否则此处有可能报错

![](http://i.imgur.com/JGiOB.jpg)  

![](http://i.imgur.com/uncjK.png)  

# Step3  

通过RubyGem下载安装jekyll，通过命令：  

{% highlight ruby %}
$ gem install jekyll
{% endhighlight %}  

![](http://i.imgur.com/ho9rF.png)  
ps:此处一定要耐心等待，不要看没有任何反应不耐心就关掉cmd或者强制推出，此刻正在下载jekyll包，下载完成就会自动解压安装。  

# Step4  

安装markdown解释器RDiscount，通过命令：  

{% highlight ruby %}
$ gem install rdiscount
{% endhighlight %}  

![](http://i.imgur.com/o7ffs.png)

# Step5  

安装Python，参见博文[Python 3.2.3 安装和配置](http://ideex.name/cn/2012/08/python-config/)  

# Step6  

安装Pygments语法高亮  

- 首先下载distribute\_setup.py文件（[点此下载](http://python-distribute.org/distribute_setup.py)）  
- 然后安装easy\_install，在cmd控制台输入命令:python\.exe distribute\_setup.py  
- 接着安装Pygments，通过上一步安装好的easy\_install，输入命令：easy\_install\.exe pygments  
- 最后配置Pygmentize\.exe，在系统变量的PATH路径里，添加其路径，比如在我的方案里，添加：C:\Python32\Scripts  

ps：如果运行Pygmentize\.exe报如下错  
> Liquid error: Bad file descriptor  

那么你需要[参考这里](https://gist.github.com/1166390)，然后修改文件C:\Ruby192\lib\ruby\gems\1.9.1\gems\albino-1.3.3\lib\albino.rb。  
或者下载补丁（[点此下载](https://gist.github.com/gists/1185645/download)），解压到C:\Ruby192\lib\ruby\gems\1.9.1\gems\albino-1.3.3\lib\下，然后通过git-bash或者类linux工具（比如Devkit），通过如下命令打补丁：

{% highlight ruby %}
$ patch < 0001-albino-windows-refactor.patch
{% endhighlight %}  

即可。

-----------------------  
以上便是Jekyll系统在本地安装的过程。
   