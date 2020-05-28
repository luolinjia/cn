---
layout: post
title: 解决google+无法登录(转)
categories:
- Technology
- Data
tags:
- Google
- Chrome
- 登录
---

这里提供一种不翻墙上Google+的一种方法，只需修改一hosts文件，轻松解决Google+被墙：

只需将下面的内容添加到C:\Windows\System32\drivers\etc\hosts文件中即可

>  #GooglePlus

> 203\.208\.46\.29 plus.google.com  
> 203\.208\.46\.29 talkgadget.google.com  
> 203\.208\.46\.29 picasaweb.google.com  
> 203\.208\.46\.29 lh1.ggpht.com  
> 203\.208\.46\.29 lh2.ggpht.com  
> 203\.208\.46\.29 lh3.ggpht.com  
> 203\.208\.46\.29 lh4.ggpht.com  
> 203\.208\.46\.29 lh5.ggpht.com  
> 203\.208\.46\.29 lh6.ggpht.com  
> 203\.208\.46\.29 lh6.googleusercontent.com  
> 203\.208\.46\.29 lh5.googleusercontent.com  
> 203\.208\.46\.29 lh4.googleusercontent.com  
> 203\.208\.46\.29 lh3.googleusercontent.com  
> 203\.208\.46\.29 lh2.googleusercontent.com  
> 203\.208\.46\.29 lh1.googleusercontent.com  

这种方法如果用IE内核的程序有可能不成功，那么就下载最新的google浏览器即可解决。
