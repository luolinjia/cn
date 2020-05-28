---
layout: post
title: Python 3.2.3 安装和配置(windows下)
categories:
- Data
tags:
- Python
- Windows
- 安装
- 配置
---

> 虽然在windows下的开发环境都不太好安装，不过这个Python的安装和配置倒挺简单的  

# 安装  
- 首先下载<http://www.python.org/getit/>选择适合的版本，我这里选择的是3.2.3(Windows binary),下载完成以后，双击打开，傻瓜式安装即可。  

# 配置  
- 计算机属性-高级-环境变量，在**PATH**里输入你的python安装位置即可，如果是默认的，就是C:\Python； 

检查是否暗安装成功，就用流行的Hello World来进行测试。  
当环境变量配置好以后，启动cmd，键入Python即可进入编写页面，然后输入:  

{% highlight python %}
print('Hello world')
{% endhighlight %}  


如果紧接着就在下一行输出Hello world即可表示安装成功。
