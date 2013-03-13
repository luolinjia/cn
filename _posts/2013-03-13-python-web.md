---
layout: post
title: Python web.py初识
categories:
- Study
- Book
tags:
- Python
- Linux
- Webpy
---

> python web.py框架  

> 环境： Fedora 17  


web.py 一个轻量级python web框架，通过python搭建一个服务，并且在本地能够访问，在终端控制台输出我们需要的结果。  


## 示例  

首先来看下一个简单的web.py示例：  

将code.py示例放置的位置如下：  

![0.png](http://i.imgur.com/ga6sXwM.png)  

紧接着是code.py源码:  


{% highlight python %}
# Filename : code.py
# 每个web.py应用，必须首先引入web模块
import web

#告诉web.py URL如何组织
urls = (
	'/(.*)','index'
)

class index:
	def GET(self,name):#Get request method and params
		i = web.input(times = 1)
		if not name: name = 'world'
		for c in xrange(int(i.times)): print 'hello,',name+'!'
		#print "hello world!!"
if __name__ == "__main__": 
	app = web.application(urls,globals())
	app.run()
{% endhighlight %}  

这就是一个最简单的也是完整的web.py应用，在终端用python运行code.py文件，即可看到如下，说明服务已经启动好：  

![1.png](http://i.imgur.com/FQwH7Si.png)  

然后在浏览器地址栏输入http://localhost:8080/luolijia，在控制台可以看到Hello，luolinjia。说明一个简单的web.py应用已经成功。  

![2.png](http://i.imgur.com/eXejlBG.png)  

看起来还是比较简单的，那么就详细说明一下这个简单的应用。  


## 安装web.py  

首先下载web.py的源码包，点击[此处](http://webpy.org/static/web.py-0.37.tar.gz)下载；  

然后解压到文件夹下，执行python setup.py install，（注意，在linux下需要root权限）。  

通过观察，此处安装会直接安装到/usr/lib/python2.7下，当然，其它系统估计也差不多，它会自动安装。安装好后，即可开始web.py应用的开发了。  


## URL处理  

任何一个网站最重要的部分都是它的 URL 结构，URLs 不仅仅用来让网站的访问者看到或者 email 告诉他们的朋友，它还告诉别人你的网站是按照怎样的思维模式工作的。像 [del.icio.us](http://del.icio.us/) 这样的网站，它的 URLs 甚至是用户界面的一部分。web.py 让使用很棒的 URLs 变得很容易。  

下面开始写基于 web.py 的应用程序，新建一个文本文件（让我们给它起名 code.py ）并且输入：  

{% highlight python %}
import web
{% endhighlight %}  

这句话的作用是引入 web.py 模块。  

现在需要告诉 web.py 我们的 URL 结构。刚开始我们写个简单的：  

{% highlight python %}
urls = (
  '/', 'index'
)
{% endhighlight %}  


第一部分是一个正则表达式，它用来匹配一个 URL，例如 /，/help/faq，/item/(\\d+) 等等。（\\d+ 匹配的是一串数字。括号表示匹配上部分供以后使用），第二部分是处理请求的类的名字，例如 index，view，welcome.hello（welcomes 模块的 hello 类），或者 get_\\1。\\1 用来被正则表达式捕捉到的第一部分替换，捕捉到的剩下的部分被传递给方法。  

这一行的意思是我们把 URL 是 /（例如说首页）的请求传给 index 类来处理。  

现在我们需要写 index 类。大部分人上网只是随便看看，他们不会注意到浏览器用的是一种被称为 HTTP 的语言与 World Wide Web 进行交流。这其中的细节不重要，但是有一个基本的概念，网站访问者让服务器根据 URLs（例如 / 或者 /foo?f=1）处理相应的动作（例如 GET 或 POST)。  

GET 动作我们很熟悉，它被用来请求一个文本形式的网页。当你在浏览器地址栏输入 harvard.edu 后，它会 GET 哈佛大学网站服务器的 /。POST 动作我们也不陌生，它经常被用来提交相应的表单，例如一个购买东东的请求。只要你在网上做一个提交请求的动作（比方说用信用卡下一个定单），你就会用到 POST。了解这两个动作的不同之处很关键，因为 GET 方式的 URLs 可以被传送和被搜索引擎索引，你一定希望你网站上大部分的网页被索引，但是你也一定不希望正在处理的定单被索引。  

在 web.py 代码里，我们把这两个动作区别得很清楚：  

{% highlight python %}
class index:
    def GET(self):
        print "Hello, world!"
{% endhighlight %}  

只要有人提给服务器一个 URL 为 / 的 GET 请求，上面的 GET 方法将被 web.py 调用。  

好了，现在我们需要完成程序的最后一行，告诉 web.py 开始提供网页服务：  

{% highlight python %}
if __name__ == "__main__": web.run(urls, globals())
{% endhighlight %}  

这句话是让 web.py 在这个文件（code.py）的全局名称空间里查找相应的类给上面列出的 URLs 提供相应的服务。  

现在请注意，尽管我在这儿叽叽歪歪讲了一大堆，可是我们实际上就写了 5 行代码。这就是我们写一个完整的 web.py 应用程序所需要的全部。如果你去命令行窗口输入：  

{% highlight python %}
$ python code.py
Launching server: http://0.0.0.0:8080/
{% endhighlight %}  


现在你的 web.py 应用程序就运行在一个真实的 web server 中了。访问那个 URL（http://localhost:8080），你应该看到“Hello, world!”（你可以在命令行的 code.py 后面添加 IP 地址/端口来控制 web.py 启动 server 的位置。你也可以使用 fastcgi 或者 scgi 参数来运行相应类型的服务。）  

> ps:这里要注意，如果按照示例上web.run(urls,globals())来进行输出会存在问题，会报如下错误：  

![3.png](http://i.imgur.com/PXWqYOp.png)


那么这里需要小小地更正一下，改成下面这样的代码即可：  

{% highlight python %}
if __name__ == "__main__": 
	app = web.application(urls,globals())
	app.run()
{% endhighlight %}  


#### 参考  

[web.py官网](http://www.dup2.org/files/web.py%200.2%20tutorial.html)
