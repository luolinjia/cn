---
layout: post
title: HTML5本地存储1
categories:
- Data
- Study
- HTML5
- HTML5学习笔记
tags:
- HTML5
- Javascript
---

> HTML5本地存储有两个概念，一个是Web Storage，一个是本地数据库。这些概念是对HTML4中cookies存储机制的一个改善。  

# Web Storage  
由于HTML4使用的cookies存储永久数据有很多问题，比如：大小只能是4K；带宽受限（cookies是随HTTP事务一起被发送的，因此会浪费一部分发送cookies时使用的带宽）；正确操作cookies是一件很难的事情。  

> 注意这里并不是所有的浏览器都支持HTML5本地存储，比如IE系列，支持都不是很好，支持的浏览器：IE8.0+、FF3.5+、Safari4.0+、Chrome4.0+、Opera10.5+、iPhone2.0+、Android2.0+   


所以HTML5重新提供了一种在客户端本地保存数据的功能，这就是Web Storage。Web Storage又分为两种，一种是sessionStorage，当用户浏览某个网站的时候，从进入网站到浏览器关闭这段时间，session对象可以保存这段时间的任何数据；另外一种是我今天学习的重点，就是localStorage，这种会将数据保存在本地的硬件设备上，即使浏览器关闭了，该数据仍然存在，再一次打开浏览器，这些数据还没有消失，就有点像一个数据库了。   

存取数据方法：  

> sessionStorage：   
> 保存数据：sessionStorage.setItem(key, value);   
> 读取数据：sessionStorage.getItem(key);  
> localStorage：   
> 保存数据：localStorage.setItem(key, value);   
> 读取数据：localStorage.getItem(key);   

进行Web Storage的读写时，不管是哪个对象，都会使用getItem方法来读取数据，使用setItem方法来保存数据。并且是按键值对的形式保存，保存时不允许重复保存相同的键名。保存后可以修改键值，不允许修改键名（只能新建键名）。  


eg\. 一个本地web留言板:   

{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>简单web留言板</title>
    </head>
    <body>
        <h1>简单web留言板</h1>
        <textarea id="memo" cols="60" rows="10"></textarea><br>
        <input type="button" value="追加" onclick="saveStorage('memo');">
        <input type="button" value="初始化" onclick="clearStorage('msg');">
        <hr>
        <p id="msg"></p>
        <script type="text/javascript">
        	function saveStorage(idss){
        		var datas = document.getElementById(idss).value;
        		var times = new Date().getTime();
        		localStorage.setItem(times,datas);
        		alert('saved');
        		localStorages('msg');	
        		//alert('saved1');
        	}
        	
        	function localStorages(idss){
        		var results = '<table border="1">';
        		for(var i = 0; i < localStorage.length; i++){
        			var key = localStorage.key(i);
        			var values = localStorage.getItem(key);
        			var datte = new Date();
        			datte.setTime(key);
        			var datestr = datte.toGMTString();
        			results += '<tr><td>' + values + '</td><td>' + datestr + '</td></tr>';	
        		}	
        		results += '</table>';
        		var targets = document.getElementById(idss);
        		targets.innerHTML = results;
        	}
        	
        	function clearStorage(){
        		localStorage.clear();
        		alert('clear successed!!!!');	
        		loadStorage('msg');
        	}
        	
        </script>
    </body>
</html>
{% endhighlight %}   

预览图：   

![html5](http://i.imgur.com/dY7ka.png)  

