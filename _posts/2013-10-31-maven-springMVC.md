---
layout: post
title: maven搭建springMVC（在Eclipse下）
categories:
- Study
- Data
tags:
- maven
- Java
- springMVC
---

> 用maven搭建springMVC简单框架  

准备工具：eclipse、maven插件

##step 1  

File -> New -> Other -> Maven Project  

![1](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/11_zps7fa72fc3.png)  

##step 2  

Use default Workspace location 用默认的空间地址即可  

![2](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/12_zps8bf2c714.png)  

##step 3  
  
![3](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/13_zpsa97494c5.png)  


##step 4  
  
![4](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/14_zpsea258f1a.png)  

##step 5  

Finish -> 生成初始目录  

![5](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/15_zps9b8061f0.png)  

##step 6  

配置项目的目录，我们得配置maven的标准目录  
右键项目 -> New -> Source Folder  

![6](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/16_zps4c3c6bce.png)  

##step 7  

配置后的目录

![7](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/17_zpsf8b5388b.png)  

##step 8  

右键项目 -> Properties -> Java Build Path -> Libraries  
更改JDK支持为1.6  

![8](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/18_zps6fa0cb09.png)  

##step 9  

Java Compiler -> Compiler compliance level -> 1.6  

![9](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/19_zpsa871818d.png)  

##step 10  

Project Facets -> Convert to faceted form...  

![10](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/20_zpscbc3b7d3.png)  

##step 11  

Project Facets -> Dynamic Web Module -> 2.4  
注意这里勾上Dynamic Web Module 并在后面的版本选择2.4  

![11](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/21_zps8f7b87a0.png)  

##step 12  

配置好上面所有的东西后的目录，然后双击pom.xml  

![12](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/22_zpsac28dedd.png)  

##step 13  

pom.xml 配置：  
（注意这里写好保存后，maven会自动从管理库下载spring框架需要的lib依赖）  

{% highlight xml %}

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>test</groupId>
  <artifactId>test2</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>test2 Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>3.0.5.RELEASE</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>3.0.5.RELEASE</version>
    </dependency>
    
    <dependency>
        <groupId>org.apache.geronimo.specs</groupId>
        <artifactId>geronimo-servlet_2.5_spec</artifactId>
        <version>1.2</version>
    </dependency>
            
  </dependencies>
  <build>
    <finalName>test2</finalName>
  </build>
</project>

{% endhighlight %}

##step 14  

等待上一步骤download好之后：  
右键项目 -> Properties -> Deployment Assembly  
结果如下，我们需要将红色框内的干掉  

![14](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/23_zps8711e260.png)  

##step 15  

干掉后，按照下图新增需要部署的文件夹和lib依赖（这里是Maven Dependencies）  

![15](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/24_zps7dee8d03.png)  

##step 16  

到此步为止，算是整体搭建完毕，接下来，我们写个事例，测试一下。  

在src/main/java 下 建立 com.test.model 包  
写入Class: User.java  

{% highlight java %}

package com.test.model;

public class User {
	private String username;
	private String password;
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
}

{% endhighlight %}  

##step 17  

在src/main/java 下 建立 com.test.controller 包  
写入Class:LoginController.java  

{% highlight java %}

package com.test.controller;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.test.model.User;

@Controller
public class LoginController {
	
	@RequestMapping(value="login")
    public ModelAndView login(HttpServletRequest request,HttpServletResponse response,User user ){
        String username = user.getUsername();
        ModelAndView mv = new ModelAndView("/main/main");
		//传入参数及其值
        mv.addObject("user", username);
        mv.addObject("param1", "Hello!");
        mv.addObject("param2", "administrator");
        return mv;
    }
}

{% endhighlight %}  

##step 18  

在src/main/java/webapp/WEB-INF下配置web.xml  

{% highlight xml %}

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
    http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <servlet>
        <servlet-name>test</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>test</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>

{% endhighlight %}  

##step 19  

在src/main/java/webapp/WEB-INF下配置test-servlet.xml  

{% highlight java %}

<?xml version="1.0" encoding="UTF-8"?>
<!-- Bean头部 -->
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
    xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:context="http://www.springframework.org/schema/context"
    xmlns:util="http://www.springframework.org/schema/util"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd  
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd  
            http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd              
            http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.0.xsd">
    
    <!-- 激活@Controller模式 -->
    <mvc:annotation-driven />
    <!-- 对包中的所有类进行扫描，以完成Bean创建和自动依赖注入的功能 需要更改 -->
    <context:component-scan base-package="com.test.controller" />

    <bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter" />

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/WEB-INF/jsp/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>
</beans>
{% endhighlight %}  

##step 20  

在src/main/java/webapp/WEB-INF下配置applicationContext.xml  
这里可以配置为空BEANS，这里不涉及到数据库  

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">
<beans>
</beans>
{% endhighlight %}  

##step 21  

在src/main/java/webapp 下配置index.jsp  

{% highlight html %}

<%
     request.getRequestDispatcher("/WEB-INF/jsp/login/login.jsp").forward(request,response);
%>

{% endhighlight %}  

##step 22  

在src/main/java/webapp/WEB-INF 下新建文件夹jsp/main和jsp/login  
在jsp/main 新建main.jsp  

{% highlight html %}

<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Main.jsp</title>
</head>
<body>
     ${user },${param1 } ,${param2 }
</body>

{% endhighlight %}  

在jsp/login 新建login.jsp  

{% highlight html %}
<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Insert title here</title>
</head>
<body>
    <div>
        <form action="login" methed="get">
            <input type="text" name="username">
            <input type="submit" value="SUBMIT">
        </form>
    </div>
</body>
{% endhighlight %}  

##step 23  

所有的文件都准备完毕，编译打包到tomcat  

![23](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/25_zps324c9b70.png)  

##step 24  

效果图1：  

![24](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/26_zps939dbb35.png)  

效果图2：  

![25](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/pictures%20code/27_zps2196e885.png)  

##总结  

至此，一个简单的通过maven搭建springMVC框架成功。  
