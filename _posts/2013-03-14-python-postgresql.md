---
layout: post
title: Fedora 17 下安装postgresql数据库
categories:
- Study
- Book
tags:
- fedora
- postgresql
---

> 安装这个东东可真是浪费了不少时间，就按照步骤一步步来吧。  

1、 mkdir pgsql -----> cd pgsql  

2、 wget -c http://ftp.postgresql.org/pub/source/v9.1.1/postgresql-9.1.1.tar.bz2  

3、 tar -vfx postgresql-9.1.1.tar.bz2  

4、 cd 解压出来的源码文件夹  

5、 yum install zlib-devel  

6、 /configure --prefix=/usr/local/pgsql --without-readline  

7、 make（编译，准备好） ------> make install（真正开始安装）

> 安装好了，只是开始，接下来的配置很重要  

1、 groupadd postgres   ------>    useradd -g postgres postgres (创建用户组和用户)  

2、 mkdir /usr/local/pgsql/data   ---->   cd /usr/local/pgsql    ----->   chown postgres.postgres data（创建数据库存储目录，并用root用户赋权限）  

3、 su - postgres（切换用户）   ---->   /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data（初始化数据）   ------>  /usr/local/pgsql/bin/postmaster -D /usr/local/pgsql/data(启动数据库)  

4、 配置监听地址和端口：  vim /usr/local/pgsql/data/postgresql.conf  

> 取消listen\_address和port两行注释，并赋值  
> listen\_address = "\*"  
> port = 5432  

5、 让postgresql数据库随系统启动而启动：  

> 将启动脚本拷贝到/etc/init.d/目录下，具体执行如下命令：  
> cd /etc/rc.d/init.d  
> cp (第一步解压的安装文件目录)/postgresql-9.1.1/contrib/start-scripts/linux postgresql  
> chmod +x postgresql  
> vi postgresql  
> prefix=/usr/local/pgsql  
> PGDATA="/usr/local/pgsql/data"  
> PGUSER=postgres  
> PGLOG="/var/log/pgsql.log"  


![postgresql](http://i.imgur.com/U3jA5yI.png)  

6、 执行chkconfig --add postgresql  

7、 启动数据库 service postgresql start  

> ps：一般是安装pgadmin3来管理数据库:  
> yum install pgadmin3 -y
