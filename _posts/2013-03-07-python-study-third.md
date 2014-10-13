---
layout: post
title: Python学习笔记（三）
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

学习内容：**模块**，**数据结构**  


## 模块  

函数和变量的结合体就是模块，它基本包含了所有定义的函数和变量的文件，为了在其它程序中重用模块，模块的文件名必须以.py结尾  

用实例进行学习吧：  

{% highlight python %}
#!/usr/local/python
# Filename:test_methods.py

def cry():
	if __name__ == '__main__': #用来判断此方法执行时被调用对象是不是该实体本身
		print 'crying...main'
	else:
		print 'crying...not main'

def laugh():
	if __name__ == '__main__':
		print 'laughing...main'
	else:
		print 'laughing...not main'

def study():
	if __name__ == '__main__':
		print 'studying...main'
	else:
		print 'studying...not main'

version = '1.0'

{% endhighlight %}  

上面是创建了一个自己的模块，然后在所建立的这个模块的相同路径下或者在sys.path所列的目录之一。好了，现在就可以用别的Python程序来调用该模块：  

{% highlight python %}
#!/usr/local/python
# Filename:call_methods.py

#引用test_methods里面的laugh、cry、version方法
from test_methods import laugh,cry,version

#调用引用的方法和属性
laugh()
cry()
print 'Version is ', version

{% endhighlight %}  

然后这样就可以调用上面所写的函数，这里来详细阐释上面的两段代码：  

> 第一段代码中的这个\_\_name\_\_是一个标识，用来识别本身的程序是被程序主体调用还是被其他的python程序调用，def定义的是方法，version是属性；第二段代码中用了比较流行的from..import句式来调用其它模块的方法，from+要调用的模块名称+import+需要调用的方法  


## 数据结构  

顾名思义：数据结构就是用来存储一组相关数据的。在python中有3种内建的数据结构---->列表、元组、字典  

列表：  

{% highlight python %}
# Filename: list.py

#创建一个list，该list可以新增、删除、修改
shoplist = ['f','w','c','v']
print 'There are ', len(shoplist),'items'
print 'They are:'
#循环该list，并命名对象item
for item in shoplist:
	print item,
#向该list增加新元素h
shoplist.append('h')
print shoplist
#给该list排序
shoplist.sort()
print shoplist
#将排序后的list第一个元素赋值给变量olditem
olditem = shoplist[0]
#干掉list第一个元素
del shoplist[0]
print olditem
print shoplist
{% endhighlight %}  

> ps：python里面的元素取值方法，比如list\[0\]（取第一个元素）,list\[1:3\]（取第二个到第4个）,list\[1:\]（取第一个到后面所有）,list\[1:-1\]（取顺数第二个到倒数第一个的所有值）,list\[:\]（取所有值）

元组：  

{% highlight python %}
# filename: tuple.py

#创建一个不可变的元组，该数据是不可变的
zoo = ('wolf','monkey','elephant')
print 'Number of animals in the zoo is ', len(zoo)

#创建一个引用了其它元组的元组，相当于一个嵌套
new_zoo = ('dolphin','pig', zoo)
#可以用len函数来获取元组的长度
print 'Number of animals in the new zoo is ', len(new_zoo)
print 'All animals in new zoo are ',new_zoo
#此处的new_zoo[2]是引用其第三个元素
print 'Animals brought from old zoo are', new_zoo[2]
#此处的new_zoo[2][2]引用的是第三个元素下的第三个元素
print 'Last animal brought from old zoo is', new_zoo[2][2]
{% endhighlight %}  

字典：  

定义形式：字典 d = {key1 : value1, key2 : value2 }，键具有唯一性，并且里面的元素没有顺序。

{% highlight python %}
# Filename: dict.py

#定义一个字典ab
ab = {       'Swaroop'   : 'swaroopch@byteofpython.info',
             'Larry'     : 'larry@wall.org',
             'Matsumoto' : 'matz@ruby-lang.org',
             'Spammer'   : 'spammer@hotmail.com'
     }

print "Swaroop's address is %s" % ab['Swaroop']

# Adding a key/value pair
ab['Guido'] = 'guido@python.org'

# Deleting a key/value pair
del ab['Spammer']

print '\nThere are %d contacts in the address-book\n' % len(ab)
for name, address in ab.items():
    #打印输出，用元组进行匹配
    print 'Contact %s at %s' % (name, address)

if 'Guido' in ab: # OR ab.has_key('Guido')匹配在字典ab中符合Guido的信息
    print "\nGuido's address is %s" % ab['Guido']
{% endhighlight %}  


#### 参考  

[《简明Python教程》](http://woodpecker.org.cn/abyteofpython_cn/chinese/)
