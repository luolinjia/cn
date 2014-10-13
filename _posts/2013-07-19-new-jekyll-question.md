---
layout: post
title: Jekyll新问题
logo: http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/764397636140_zpsf772d32b.jpg
categories:
- Study
tags:
- Jekyll
- 新规划
---

> 整理遇到的**Jekyll**问题  

近期有意更改个人网站的风格，于是重新安装本地Jekyll系统，可谓还算顺利。  

Q1：**gem install jekyll** 没反应，甚至连接不上  

A1：这当然主要归功于中国的墙了，我以前调试的那个版本是没有问题的，只是速度很慢，现在就纯粹地连接不上了，我果断怒了，google之，发现现在有一个可以完全替代的镜像：[http://ruby.taobao.org/](http://ruby.taobao.org/) ,以下是配置：  

------------------------------  
$ gem sources --remove https://rubygems.org/  
$ gem sources -a http://ruby.taobao.org/  
$ gem sources -l  
\*\*\* CURRENT SOURCES \*\*\*  
  
http://ruby.taobao.org  

------------------------------  

请确保只有 ruby.taobao.org镜像   
然后再  

$ gem install jekyll  

即可  

Q2：**warning:cannot close fd before spawn**  

A2：这似乎跟Pygments的版本有关系，用的是Python的2.7版本（如果用的是python3.2的请忽视，不过可能原理应该差不多），以下是解决方案：  

---------------------------------
$ gem uninstall pygments.rb --version "=0.5.1"  
$ gem install pygments.rb --version "=0.5.0"  

---------------------------------  

（待续，遇到问题再添加）  
