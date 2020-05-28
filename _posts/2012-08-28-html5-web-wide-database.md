---
layout: post
title: HTML5本地数据库
categories:
- Data
- Study
- HTML5
- HTML5学习笔记
tags:
- HTML5
- Javascript
- SQLLite
---

> 在HTML5中，内置了一种可以通过SQL语言来访问的数据库，在HTML4中，数据库只能放在服务器端，只能通过服务器来访问数据库，但是在HTML5中，可以就像访问本地文件那样轻松地对内置数据库进行直接访问。  

现在，像这种不需要存储在服务器上的，被称为"SQLLite"的文件型SQL数据库已经得到了很广泛的利用，所以HTML5中也采用了这种数据库来作为本地数据库。那么要使用SQLLite数据库，有两个必要的步骤：


+ 创建访问数据库对象  
var db = openDatabase('mydb','1.0','test',1024 * 1024);  
该方法有4个参数，分别为数据库名，版本号，数据库描述，数据库大小，如果有mydb这个数据，则访问，如果没有，就创建。  
+ 使用事务处理    
{% highlight javascript %}
db.transaction(function(tx){
	tx.executeSql(sql);
});
{% endhighlight %}   
调用transaction方法使用一个回调函数为参数，在这个函数中，执行访问数据库的语句。  


eg\.范例：HTML5本地数据库   
HTML代码：  
{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>本地数据库</title>
    </head>
    <body onload="init();">
        <h1>使用数据库实现存取数据</h1>
        <table>
        	<tr>
        		<td>用户名：</td>
        		<td><input type="text" id="username"></td>
        	</tr>
        	<tr>
        		<td>密码：</td>
        		<td><input type="text" id="pwd"></td>
        	</tr>	
        	<tr>
        		<td></td>
        		<td><input type="button" value="保存" onclick="saveData();"></td>
        	</tr>
        </table>
        <hr>
        <table id="datatable" border="1"></table>
        <p id="msg"></p>
	</body>
</html>
{% endhighlight %}   

Javascript代码:   
{% highlight javascript %}
//页面的表格元素
var datatable = null;
//创建一个数据库对象，包含4个参数
//分别为数据库名称，版本号，数据库描述，数据库大小
var db = openDatabase('UserInfo', '', 'Username and UserPwd', 1024 * 1024);
//初始化，显示所有数据库的数据
function init(){
	datatable = document.getElementById('datatable');
	showAllData();	
}
//删掉table中的所有数据，但是并不删除数据库里面的数据
function removeAllData(){
	//遍历table标签，然后删除所有子节点
	for(var i=datatable.childNodes.length-1;i>=0;i--){ 
        datatable.removeChild(datatable.childNodes(i)); 
    } 
	var tr = document.createElement('tr');
	var th1 = document.createElement('th');
	var th2 = document.createElement('th');
	th1.innerHTML = '用户名';
	th2.innerHTML = '密码';
	tr.appendChild(th1);
	tr.appendChild(th2);
	datatable.appendChild(tr);
}

function showData(row){
	var tr = document.createElement('tr');
	var td1 = document.createElement('td');
	td1.innerHTML = row.username;
	var td2 = document.createElement('td');
	td2.innerHTML = row.pwd;
	tr.appendChild(td1);	
	tr.appendChild(td2);
	datatable.appendChild(tr);
}
//此方法显示所有的数据到table中
function showAllData(){
	//对数据库执行的sql语句sql1和sql2
	//sql1:创建一个表名为UserMag的表，包含username和pwd两个字段，类型都是text
	var sql1 = 'CREATE TABLE IF NOT EXISTS UserMag(username TEXT, pwd TEXT)';
	var sql2 = 'SELECT * FROM UserMag';
	//SQLLite数据库的transaction方法，里面包含一个回调函数为参数，并执行
	db.transaction(function(tx){
		tx.executeSql(sql1,[]);	
		//查询表里面所有的数据，并定义了一个回调函数，返回结果集rs
		tx.executeSql(sql2, [], function(tx, rs){
			removeAllData();
			for(var i=0; i<rs.rows.length; i++){
				//针对每一行记录item(i)，调用showData进行处理
				showData(rs.rows.item(i));
			}	
		});
	});	
}
//此方法是保存我们输入的用户名和密码
function addData(username, pwd){
	/*
	 * 在这里，使用了"INSERT INTO UserMag VALUES(?,?)"
	 * 这句sql语句来追加数据，同时使用了[username,pwd]
	 * 数组来传入sql语句中所需要的参数，真正对数据库
	 * 执行操作时，是把里面的两个问号换成后面的值。
	 */
	db.transaction(function(tx){
		tx.executeSql('INSERT INTO UserMag VALUES(?, ?)', [username, pwd], 
					  //当追加数据成功时执行的处理
					  function(tx, rs){
					  	alert('saved!!!');	
					  },
					  //当追加失败时执行的处理
					  function(tx, error){
					  	alert(error.source + "::" + error.message);	
					  });	
	});
}

function saveData(){
	var username = document.getElementById('username').value;
	var pwd = document.getElementById('pwd').value;
	addData(username, pwd);
	showAllData();
}
{% endhighlight %} 


预览图：   

![html5](http://i.imgur.com/z8kUP.png)   

#### 参考   
- <http://www.w3.org/2008/webapps/wiki/Database>
- <http://supercharles888.blog.51cto.com/609344/856071>
- <http://blog.darkcrimson.com/2010/05/local-databases/>   



