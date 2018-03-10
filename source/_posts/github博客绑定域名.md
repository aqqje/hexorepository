---
title: github博客绑定域名
date: 2017-11-20 20:54:00
comments: true
categories:
	- hexo
tags:
	- hexo
---

![](https://github.com/aqqje/Personal-repository/raw/master/images/aliwan.jpg)
在github上成功搭建了一个属于自己的小空间后，是不是感到十分的开心，就差没跳舞了！- -，可是感觉还是不够完美！还得有一个自己的域名才好！SEE! 绑定域名...
<!--more-->

-----------------

- 1.添加CNAME文件<br/>
向 yourname.github.io/source 目录添加一个CNAME(一定要*大写*且文件名*没有后缀名*)文件<br/>
文件中增加一个域名<br/>
	如:<br/>
	aqqje.com<br/>
- 2.向你的 DNS 配置中添加 3 条记录<br/>
	@		A		192.30.252.153<br/>
	@		A		192.30.252.154<br/>
	www		CNAME	username.github.io<br/>
用你自己的 Github 用户名替换 username<br/>
我是在万网注册的域名，解析是阿里云解析
配置 DNS 推荐使用 DNSPOD 的服务，使用国外的 DNS 解析服务可能有被墙的风险<br/>
至于如何使用 DNSPOD 解析域名[参考](https://link.zhihu.com/?target=http%3A//jingyan.baidu.com/article/546ae1857c4ee81149f28cbe.html)<br/>
- 等待你的 DNS 配置生效<br/>
对DNS的配置不是立即生效的，过10分钟再去访问你的域名看看有没有配置成功<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/aliwan2.jpg)
#### 说明：此篇文章解析工具是阿里云，其实工具是否适用概不负责。


-----------------


[参考](https://www.zhihu.com/question/31377141)
作者：梦不若星辰<br/>
链接：[https://aqqje.github.io](https://aqqje.github.io)<br/>
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。<br/>

