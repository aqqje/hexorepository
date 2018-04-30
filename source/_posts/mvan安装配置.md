---
title: mvan安装配置
date: 2018-04-30 22:16:20
tags: java
---
mvan安装配置
<!-- more -->

参考：https://www.cnblogs.com/eagle6688/p/7838224.html

maven : https://maven.apache.org/download.cgi

![](https://github.com/aqqje/Personal-repository/raw/master/images/maven1.png "maven1")<br/>

解压  -- >  进入目录  --> C:\java\OpenSource\apache-maven-3.5.3
![](https://github.com/aqqje/Personal-repository/raw/master/images/maven2.png "maven2")<br/>
 

配置系统变量:  
 1.新增系统变量 -- > 变量名:MAVEN_HOME 变量值：C:\java\OpenSource\apache-maven-3.5.3
2.编辑path 变量：追加 -- > %MAVEN_HOME%\bin\; 

![](https://github.com/aqqje/Personal-repository/raw/master/images/maven3.png "maven3")<br/>

 

测试:
 
![](https://github.com/aqqje/Personal-repository/raw/master/images/maven4.png "maven4")<br/>

配置Maven本地仓库:
选择一个目录用作maven的本地库 -- > C:\java\mavelocrepo\maven-repository -- >
打开D:\Program Files\Apache\maven\conf\settings.xml文件，查找下面这行代码：

<localRepository>/path/to/local/repo</localRepository> 
-- > localRepository节点默认是被注释掉的，需要把它移到注释之外，然后将localRepository节点的值改为我们在3.5.3 中创建的目录C:\java\mavelocrepo\maven-repository -- >

注意: localRepository节点用于配置本地仓库，本地仓库其实起到了一个缓存的作用，它的默认地址是 C:\Users\用户名.m2。
当我们从maven中获取jar包的时候，maven首先会在本地仓库中查找，如果本地仓库有则返回；如果没有则从远程仓库中获取包，并在本地库中保存。
此外，我们在maven项目中运行mvn install，项目将会自动打包并安装到本地仓库中。

运行一下DOS命令:

mvn help:system

如果前面的配置成功，那么D:\Program Files\Apache\maven-repository会出现一些文件。

配置Eclipse的Maven环境

![](https://github.com/aqqje/Personal-repository/raw/master/images/maven5.png "maven5")<br/>
 
![](https://github.com/aqqje/Personal-repository/raw/master/images/maven6.png "maven6")<br/>
 


