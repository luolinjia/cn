---
layout: post
title: Github与Jekyll
categories:
- Data
- Study
tags:
- Git
- Github
- Jekyll
---

> 说道Github，不得不提Git版本控制器，Github很多的基本操作都是基于Git的基本操作，即使现在Github推出了针对windows用户的Github桌面版（还有点点像平板电脑上的感觉），但其底层仍然是用Shell命令执行的。  


# Git到底是什么？  
Git在2005年由Linus Torvalds开发，最开始只是为了替代BitKeeper来管理Linux内核开源社区的版本控制和维护代码，不过后来Git内核已经成熟到可以独立地用作版本控制。官方解释，所谓Git，就是The stupid content tracker，傻瓜内容跟踪器。Git是一个快速、可扩展的分布式版本控制系统，它具有极为丰富的命令集，对内部系统提供了高级操作和完全访问。  

- 具体关于Git的历史和来源，参考：[点击此处](http://zh.wikipedia.org/wiki/Git)  

# Github又是什么呢？  
[Github](https://github.com/)是一个面向开源及私有软件项目的托管平台，因为只支持Git作为唯一的版本库格式进行托管，故名Github。同时，Github也是一个公司，旗下的网站<https://github.com>成为全球最大的社交编程及代码托管网站，目前注册用户超过200万，托管的版本库数量已超过300万，其中有很多知名的开源项目，例如：[Ruby on Rails](https://github.com/rails/rails)、[Hibernate](https://github.com/hibernate/hibernate-orm)、[php](https://github.com/php/php-src)、[jQuery](https://github.com/jquery/jquery)、[Prototype](https://github.com/sstephenson/prototype)等等。  
Github的出现进一步推动了Git的普及，并简化了版本控制的管理和操作流程，为开发者提供了更好的平台。  

# Github Pages简介  
个人用这么久，主要还是停留在用Github Pages的时间比较多，维护我自己的博客都是通过Github Pages创建的Jekyll进行维护的。  
[Github Pages](http://pages.github.com/)是Github提供的一个工程展示页，可以是你的文档，你的工程主页等等，都是Github用户通过创建特殊名称的Git版本库或在Git库中建立特别的分支实现对主页的维护。  

# Jekyll博客系统  
那么我们的Jekyll系统就跟这个Github Pages有莫大的关联了，因为Jekyll的主页就是Github Pages的一种展示方式，Github Pages提供Jekyll所需要的所有环境，只需要用户在本地Jekyll调试成功后，将代码push到Github代码托管到名为：你的用户名.github.com的资源库（这个资源库要在github.com上新建）中即可，比如我的用户名为luolinjia，等待几分钟，那么我就可以直接在地址栏输入：luolinjia.github.com即可直接访问你刚才在本地调试成功的jekyll项目。  

- 有关本地调试搭建Jekyll环境参见：[本地Jekyll的安装(windows下)](http://ideex.name/cn/2012/08/windows-jekyll-install/)  
- 想要了解更多有关Github Pages写Jekyll博客，看文章[《理想的写作环境：git+github+markdown+jekyll》](http://www.yangzhiping.com/tech/writing-space.html)   
 

接下来就描述一下Jekyll在本地的搭建过程：  
> 如果第一次接触并且是windows用户，建议就用现在github推出的github桌面应用程序（[点此下载](http://github-windows.s3.amazonaws.com/GitHubSetup.exe)）安装好后根据操作应用程序一样操作即可，非常方便。  

这里我是通过Git Bash进行我的所有操作的（即使你不懂ruby，不懂python都没有关系，我的方法就是clone别人的代码并修改成我自己喜欢的样式，这样来得最快，新手也接受得也快，后面慢慢就很快就熟悉整个流程）：  
### step1  
下载Git（[点此下载](http://msysgit.github.com/)），并安装好，安装好后，打开gitbash，如下图：  

![gitbash](http://i.imgur.com/LweTE.png)  

如无特别说明，接下来的所有操作都是在这个窗口下完成的。  


### step2  
通过SSH让本机和github连通，输入以下命令：  

{% highlight ruby %}
$ ssh-keygen
{% endhighlight %}   

这样在C:\User\你的用户名，在此文件夹下就生成了一个隐藏文件.ssh，里面包含了两个文件，id_rsa和id_rsa.pub，它们被分别称为私钥和公钥，注意私钥文件不要随意泄露给别人。如果在执行ssh-keygen命令时你输入了口令，那么这个文件是经过加密的。公钥文件id_rsa.pub则是我们需要进行配置的文件，用记事本打开该文件，全选里面的内容（注意一定是全部内容，不能添砖加瓦，也不能缺斤少两），复制，拷贝到网站github.com下的Account Settings的SSH Keys，如下图：  

![ssh keys](http://i.imgur.com/ZPrl4.png)  

设置好后，在gitbash输入以下命令测试是否与github连通：  

{% highlight ruby %}
$ ssh -T git@github.com
{% endhighlight %}  

如果出现

{% highlight ruby %}
Hi XXX!You've successfully authenticated, but GitHub does not provide shell access.
{% endhighlight %}    

则表示成功。  

### step3  

在github网站上创建一个特殊的资源库，也就是必须以你的用户名+.github.com这样的形式。  

> 比如我的用户名是iyouhao，那么我创建的资源库的名字必须是iyouhao.github.com，这样就保证这个资源库能够直接在master分支直接帮你生成页面，如果iyouhao.github.com里面有一个index.html页面的话。如下图:  

![create repo](http://i.imgur.com/z2D5I.png)  

创建好以后，把该仓库clone到本地：  

{% highlight ruby %}
$ git clone git@github.com:iyouhao/iyouhao.github.com.git
{% endhighlight %}   

那么在本地就有了iyouhao.github.com这个文件夹，这个文件夹就是等会要写Jekyll的文件夹。  

- 创建个人主页面流程，详细步骤参见教程([点击进入](http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html#jekyll))  


### step4  

找到一个Jekyll博客网站，比如<http://tom.preston-werner.com/>，这就是一个Jekyll搭建的博客网站，在github上找到源代码<https://github.com/mojombo/mojombo.github.com>，克隆一份到自己的本地：  

{% highlight ruby %}
$ git clone git@github.com:mojombo/mojombo.github.com.git
{% endhighlight %}   

删掉隐藏的.git目录，那么剩下的就是一个Jekyll的基本框架，参考下图，凡事红色箭头指的就是不能少的目录，也就是基本目录：  

![site](http://i.imgur.com/C4spB.png)  

其中，  
\_layouts是所有系统的模版，  
\_posts是所有的文章，里面按照一定的格式命名，\_posts里面的文章会引用\_layouts里面的模版，  
\_config.yml是整个系统的配置文件，  
index.html便是个人主页要显示的网页。  

### step5  

copy这些基本目录到step3的iyouhao.github.com文件夹里面，输入以下命令：  

{% highlight ruby %}
$ git add .  
$ git status   
$ git commit -m 'commit counts'   
$ git push origin master
{% endhighlight %}   

然后就等github帮你把网页生成出来就OK了，一般10分钟就OK，然后输入iyouhao.github.com然后你搭建的Jekyll博客就OK了。

> 接下来你需要做的就是把\_posts里面的文章换成你自己需要的文章即可，写博客的时候，在\_posts里面新建一个markdown文件，文件名和文件里面的头部信息按照模版样式改如果你不熟悉Markdown语法，请看[这里](http://ideex.name/cn/2012/08/markdown-flag/)，如果你对你找的这个网站的样式不满意，那么就自己写样式，不过免于麻烦，你需要写好样式在本地Jekyll系统进行调试，调整到自己满意的样式即可，再push代码到github网站上。  



总结：Github和Jekyll两者相结合建立博客，对程序员来讲，很完美，如果对于非程序员，还真的有点不太适合。我的博客就是这样建立起来的。    


进一步了解Github和Jekyll，请阅读以下内容：   

- [Github Pages官网说明](http://pages.github.com/)  
- [Jekyll官网提供的示例网站](https://github.com/mojombo/jekyll/wiki/sites)  
- [像黑客一样写博客--Jekyll入门](http://www.soimort.org/tech-blog/2011/11/19/introduction-to-jekyll_zh.html)   
- [Jekyll官方说明文件](https://github.com/mojombo/jekyll/blob/master/README.textile)



##### 参考  
- <http://zh.wikipedia.org/wiki/Git>  
- <http://www.worldhello.net/gotgithub>  
- <https://github.com/mojombo/jekyll/blob/master/README.textile>  

  

