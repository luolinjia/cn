---
layout: post
title: Jekyll 配置 HTTPS
logo: https://ws2.sinaimg.cn/large/006tNc79gy1fj1sbwv641j30g4026wee.jpg
categories:
- Tech
tags:
- HTTPS
- Jekyll
---

这几天折腾本站的评论系统,可谓干出了很多的事情,首当其冲,就是站点的 HTTPS 的配置,由于我是 Jekyll 搭建在 Github 上的, Github Pages 不支持证书上传,所以就智能找动态的 HTTPS 证书,这里,我就用 Cloudflare（CF）免费提供的 HTTPS, 实现的方式,就是通过 CF 的域名解析服务来绑定 HTTPS,当然这样会替换掉之前你在域名服务商本身提供的 DNS 解析。

流程:

- 注册 Cloudflare 账号 <www.cloudflare.com>
- 注册成功后,点击右上角的『+Add Site』,输入自己的网址,并点击『Begin Scan』,然后就会出现自己域名的 DNS 记录,不过这里奇怪的是,我是用的 oray.com 域名服务商,但是却解析不出来,所以,只有等会进去后再进行解析的配置
- 点击『Continue』后选择『Free Website』,再点击『Continue』,现在就会出现如下图的配置:

![](https://ws1.sinaimg.cn/large/006tNc79ly1fj1ldl0dauj31j20nqabh.jpg)

- 再到域名服务商那里,将 DNS server 的名字改成上面解析出来的CF 的两个域名服务器,保存后,等待10分钟左右,再查看 CF 面板上的 Overview 里面的 status 状态,为 Active 时,说明就解析成功了
- 再然后,选择 CF 菜单的『Crypto』,把默认的 full 改成 flexible
- 再选择 CF 菜单的『Page Rules』,免费的 plan 有创建3个 rules 的限制,现在创建一个新的,把自己的网站重置到 HTTPS 网站。第一行输入你的 http://luolinjia.com,第二行选择『Forwarding URL』和『301-Permanent Redirect』,第三行再输入你的 https://luolinjia.com
- 按照上面的步骤,创建另外一个新的 rule, 第一行通配http://luolinjia.com/*,第二行选择『Always Use HTTPS』,这个差不多需要5-30分钟,然后差不多就可以看到自己的网站有安全标识了,如果是谷歌浏览器,就会是一把小绿锁。

稍微注意的是,如果自己的网站引用的资源不是 https 的,小绿锁是显示不出来的,由于用上了 iPic,那么它默认的上传地址就是新浪的免费图床,也是默认的https, 还是蛮般配的。