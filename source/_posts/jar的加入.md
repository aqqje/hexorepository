---
title: jar的加入(转)
date: 2017-11-27 17:12:51
categories:
	- jsp
tags:
	- java
---


<!-- more -->

--------------------------

现在的项目基本上都是java web项目,所以导入jar包会出现问题,主要介绍一下java项目与javaweb项目的区别:

java项目:

在classLoader加载jar和class的时候,是分开加载的,一般jar导入分两种:

1.在web-inf下的lib中直接引入

2.在user library上引入

无论以上哪种引入,jar包都能加载并且运行,classLoader会智能加载(本地JRE运行)

javaweb项目;

不是通过本地的JRE运行的，而是部署到web服务器(比如tomcat,jetty)，这些服务器都实现了自身的类加载器.

以tomcat为例:

1.common CommonClassLoader

2.server     CatalinaClassLoader

3.shared    SharedClassLoader

4.webapps webappClassLoader(加载WEB-INF下的jar)

简单来说,如果做javaweb项目引入jar包的时候,需要将jar包导入到WEB-INF下,这样服务器就能够加载并且项目跑起来的时候,项目的方法也可以调用,如果放入到user library中是不可以的,因为这样只能本地运行,服务器是加载不到的.所以项目本地调用方法的时候没有问题,但是服务器跑起来就会报出找不到相应的jar.

-------------------------

[转链地址](http://blog.csdn.net/u011165335/article/details/50670166)

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。