---
layout: post
title: HTML5本地存储2
categories:
- Data
- Study
- HTML5
- HTML5学习笔记
tags:
- HTML5
- Javascript
---

> 如何将Web Storage作为数据库形式加以利用，首先要考虑，由于普通数据库中，大多数表都分为几个字段，怎么样对列来进行管理，然后，怎样对数据进行检索，如果能够解决这些问题，就可以将Web Storage作为数据库来利用了。   

# Web Storage简易数据库   

这里要解决以上问题，就需要用到JSON数据格式作为文本来保存对象，获取该对象时再通过JSON格式来获取，这样就可以在Web Storage中保存和读取具有复杂结构的数据了。  

以下的例子用到了两个关键的方法：   

> JSON对象的stringify方法和parse方法  
> stringify(obj)：此方法会将obj转化为JSON格式的文本数据   
> parse(jsontxt)：此方法会将jsontxt（即json格式的文本）转化为JSON对象   

eg\.范例：简易数据库   
{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>简易数据库示例</title>
    </head>
    <body>
        <h1>简易数据库示例，使用Web Storage</h1>
        <table>
        	<tr>
        		<td>姓名：</td>
        		<td><input type="text" id="name"></td>
        	</tr>
        	<tr>
        		<td>Email：</td>
        		<td><input type="text" id="email"></td>
        	</tr>
        	<tr>
        		<td>电话号码：</td>
        		<td><input type="text" id="tel"></td>
        	</tr>
        	<tr>
        		<td>备注：</td>
        		<td><input type="text" id="memo"></td>
        	</tr>
        	<tr>
        		<td></td>
        		<td><input type="button" value="保存" onclick="saveStorage();"></td>
        	</tr>	
        </table>
        <hr>
        <p>检索：<input type="text" id="find">
        	     <input type="button" value="检索" onclick="findStorage();">
        </p>
        <p id="msg"></p>
        
        <script type="text/javascript">
        	//一个保存数据的方法
        	function saveStorage(){
        		var datas = new Object;
        		datas.nameid = document.getElementById('name').value;	
        		datas.email = document.getElementById('email').value;
        		datas.tel = document.getElementById('tel').value;
        		datas.memo = document.getElementById('memo').value;
        		//将对象转换成JSON格式的文本数据，并返回保存到HTML5本地存储
        		var str = JSON.stringify(datas);
        		//通过setItem方法传入键值对进行保存
        		localStorage.setItem(datas.nameid, str);
        		alert('saved!!');
        	}
        	//通过输入的key进行检索
        	function findStorage(){
        		//取得你要查询的信息key
        		var findInfo = document.getElementById('find').value;
        		var str = localStorage.getItem(findInfo);
        		if(str == null || str == ''){
        			alert('没找到相关数据！');	
        		}else{
	        		//将从localStorage中获取的数据转化成JSON对象
	        		var datas = JSON.parse(str);
	        		var results = "姓名：" + datas.nameid + '<br>';
	        		results += "Email：" + datas.email + '<br>';
	        		results += "电话号码：" + datas.tel + '<br>';
	        		results += "备注：" + datas.memo + '<br>';
	        		var tar = document.getElementById('msg');
	        		tar.innerHTML = results;	
        		}
        	}
        </script>
    </body>
</html>
{% endhighlight %}   



预览图：   

![html5](http://i.imgur.com/sap8J.png)  

