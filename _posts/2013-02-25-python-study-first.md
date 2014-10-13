---
layout: post
title: Python学习笔记（一）
categories:
- Study
- Book
tags:
- Python
- Linux
---

> 有关Python在windows下的配置就不再累述，参考博文[Python 3.2.3 安装和配置](http://ideex.name/cn/2012/08/python-config/)  

> 在linux下，一般都会默认安装Python，我这里用的是fedora17 默认的python 2.7.3 够用了，相关详细配置有待后面详细说明，这里不是重点。  

环境：fedora17  

学习内容：**常量**，**数**，**字符串**，**变量**，**标识符的命名**，**数据类型**，**对象**，**逻辑行与物理行**，**缩进**  


## 常量  


Python里面的常量通常指那些本身就有的意义，没有其他的含义。比如：27、33、\'This is a string\'等等。  


## 数  


Python中有四种数据类型：  

- 整数：比如2、27等

- 长整数：就是一些很大的字

- 浮点数：2.27和32.3E-4

- 复数：类似（-5+2i）


## 字符串  


字符串就是字符的序列。字符串基本上就是一组单词。  

- 单引号和双引号功能完全一样
例如：\'This is a string\'和\"This is a string\"是完全一样的。  
- 三引号（\'\'\'或\"\"\"）
用三引号可以指示一个多行的字符串  
例如：  

{% highlight python %}
'''Firstline.
Secondline.
Thirdline.
'''
{% endhighlight %}  

- 转义符
例如：\'what\\\'s your name?\'  
- 自然字符串
在前面加上r或者R来指定  
例如：r\"hello world\"  
- Unicode字符串
在前面加上u或者U（记住：在你处理文本文件的时候使用Unicode字符串，特别是当你知道这个文件含有用非英语的语言写的文本）  

ps：字符串是不可以改变的（好处后面讲），如果把两个字符串按字面意义相邻放着，他们会被python自动级连。  
例如：\'what\\\'s\' \'your name?\' 会被自动转为 \'what\'s your name?\'  


## 变量和标识符  


变量是标识符的例子，标识符的规则如下：  

- 第一个字符必须是字母表中的字母（大写或者小写）或者一个下划线(\'\_\')。
- 名称的其他部分可以由字母（大写或小写）、下划线（\'\_\'）或者数字（0-9）组成。
- 名称是对大小写敏感的。yourname和yourName**不是**一个标识符。  


## 数据类型  


变量可以处理不同类型的值，称为**数据类型**。基本的类型是数和字符串。  


## 对象  


Python把在程序中用到的任何东西都称为“对象”。  

如何编写Python程序：  

1. 打开你喜欢的编辑器（比如VIM）

2. 输入程序代码

3. 文件另存为扩展名为.py的文件

4. 运行解释器命令python filename.py即可


## 逻辑行与物理行  


逻辑行用“;”来表示，肉眼看到的行就是物理行，一般在python中，一个物理行只写一句逻辑行，所以一般都不用分号来区分逻辑行。  

#### 参考  

[《简明Python教程》](http://woodpecker.org.cn/abyteofpython_cn/chinese/)
