---
layout: post
title: Github page创建
categories:
- Study
- Data
tags:
- Jekyll
- GithubPages
---

> 创建一个gh-pages分支的工程  

在github中，我们可以自由创建自己的项目，这里有个延伸的github功能，就是创建一个gh-pages分支，那么github会给你自动创建一个外部可访问的工程页，方便展示自己的静态项目。那么接下来就一步步简单创建一个项目gh-pages分支。  

# Step 1  

在官网创建一个新项目，如下图：  

![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/2_zps3bf64e71.png)  


# Step 2  

然后按照创建新项目的步骤创立好，即分支为master，如下图：  

![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/3_zps1b1ff328.png)  

# Step 3  

然后按照以下步骤进行：  

- 基于master分支建立分支gh-pages  
$ git checkout -b gh-pages  
- 删除暂存区文件，即相当于清空暂存区  
$ rm .git/index  
- 创建项目首页index.html  
$ printf "hello world.\n" > index.html  
- 添加文件index.html到暂存区  
$ git add index.html  
- 用Git底层命令创建新的根提交，并将分支gh-pages重置  
$ git reset --hard $(echo "branch gh-pages init." | git commit-tree $(git write-tree))  
- 执行推送命令，在GitHub远程版本库创建分支gh-pages  
$ git push -u origin gh-pages  

如下图：  

![](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/4_zpsf64678e9.png)  


# Step 4  

上传index.html文件，验证gh-pages是否配置好，用网址的默认地址+"/newcn"，即我们刚才创建的项目，如果显示为index.html里面的内容。即成功。  


#### 参考  

[http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html](http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html)  
