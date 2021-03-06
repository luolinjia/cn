---
layout: post
title: Python学习笔记（二）
categories:
- Study
- Book
tags:
- Python
- Linux
---

> 有关Python在windows下的配置就不再累述，参考博文[Python 3.2.3 安装和配置](http://luolinjia.com/cn/2012/08/python-config/)  

> 在linux下，一般都会默认安装Python，我这里用的是fedora17 默认的python 2.7.3 够用了，相关详细配置有待后面详细说明，这里不是重点。  

环境：fedora17  

学习内容：**运算符与表达式**，**控制流**，**函数**  


## 运算符与表达式  


我们所编写的大部分语句其实都包含表达式，一个表达式可以分解为运算符和操作数。  

Ps：这部分其实只需要注意一点：就是运算的优先级，其实这个更简单，用括号来确定优先级。  


## 控制流  


这个概念相对简单，就是：如果我出生在89年，就是80后；出生在90年，那么就是标准的90后；如果都不是，说明既不是80后，也不是90后。  

用程序的思维来讲，就是：

{% highlight javascript %}
if(my_birth < 1990 && my_birth >= 1980){
	alert('80后');
}else(my_birth < 2000 && my_birth >= 1990){
	alert('90后');
}else{
	alert('要么你是前辈，要么你是少年');
}
{% endhighlight %}  

那么在python里面如何来表达：在python中有3种控制流语句：if、for和while。  

if：  

{% highlight python %}
#!/usr/bin/python
# Filename:if.py

number = 23
guess = int(raw_input('Enter an integer : ')) #类似于从键盘输入信息

if guess == number:
	print 'Congratulations, you guessed it.' # New block starts here
	print "(but you do not win any prizes!)" # New block ends here
elif guess < number:
	print 'No, it is a little higer than that' # Another block
	# You can do whatever you want in a block
else:
	print 'No, it is a little lower than that'

print 'Done'

{% endhighlight %}  

for：  

{% highlight python %}
#!/usr/bin/python
# Filename:for.py

for i in range(1,7,2):
	print i

shoplist = ['x','i','w','r']

#其实这里有点类似于java里面的新版for循环对象
for item in shoplist:
	print item,

{% endhighlight %}  

while：  

{% highlight python %}
#!/usr/bin/python
# Filename:while.py

number = 23
running = True #定义一个True，在python里面，如果while的属性不为True时才终止，否则一直循环

while running:
	guess = int(raw_input('Enter an integer: '))
	
	if guess == number:
		print 'Success!!!!!'
		running = False # This will break the loop
	elif guess < number:
		print 'Higher!!!!'
	else:
		print 'Lower!!!!!'
else:
	print 'Just done it!!!'

print 'Done'

{% endhighlight %}  


## 函数  


其实就是方法，定义： def关键字后跟一个函数的标识符，然后再跟一对圆括号。里面可以包括变量名，以冒号结尾。接下来是一块语句，它们就是函数体。  

例如：  

{% highlight python %}
#!/usr/bin/python
# Filename:func.py

def sayHello(): #定义一个python函数，括号里面可以带参数
	print 'call success!!'

sayHello()

{% endhighlight %}  

> 函数行参、局部变量、默认参数值、关键参数、return语句、DocString

1、函数行参：  

例如：  

{% highlight python %}
#!/usr/bin/python
# Filename:func_param.py

x = int(raw_input('Enter a:'))
y = int(raw_input('Enter b:'))
def printMax(a,b): #其实就是在函数里面放参数
	if a > b:
		print a, '>', b
	elif a == b:
		print a, '=', b
	else:
		print a, '<', b

printMax(x,y)
print('Done')

{% endhighlight %}  

2、局部变量：  

{% highlight python %}
#!/usr/bin/python
# Filename:func_global.py

def func_global():
	global x #如果没有关键字global，那么这里的x就是一个局部变量，
		 #有了这个关键字，那么就是全局变量
	print 'x is', x
	x = 2
	print 'Changed local x to ', x

x = 50
func()
print 'Value of x is ', x

{% endhighlight %}  

3、默认参数值：  

{% highlight python %}
#!/usr/bin/python
# Filename:func_default.py

def say(message,times = 1): #加上参数就代表次数了，这点跟java区别还是蛮大的，java是参数必须要对应。
	print message*times

say('hello')
say('world',5)

{% endhighlight %}  

4、关键参数：  

{% highlight python %}
#!/usr/bin/python
# Filename:func_key.py

def func(a, b=5, c=10):
	print 'a is ', a, 'and b is ', b, 'and c is ', c

func(3,7)
func(25, c=24)
func(c=50, a=100) #最牛逼就是这种顺序都可以随意置换，在java里面是不可想象的

{% endhighlight %}  

5、return语句：  

这个好像没什么好说的。  

6、DocStrings：  

DocStrings：文档字符串，为python的一个程序文档机制。由于是一个重要的工具，能够帮助我们理解程序更加容易，所以尽量使用它。甚至可以在程序运行的时候，从函数恢复文档字符串。  

{% highlight python %}
#!/usr/bin/python
# Filename:func_doc.py

def printMax(x,y):
	'''Prints the max of two numbers.

	First,the two numbers must be integers.''' #就是这种形式的文档，必须是三分号的段落，并且中间要空一行
	x = int(x)
	y = int(y)

	if x > y:
		print x,'is max'
	else:
		print y,'is max'
printMax(4,8)
print printMax.__doc__

{% endhighlight %}  



#### 参考  

[《简明Python教程》](http://woodpecker.org.cn/abyteofpython_cn/chinese/)
