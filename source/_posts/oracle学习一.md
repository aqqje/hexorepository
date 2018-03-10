---
title: oracle学习一
date: 2017-11-22 18:19:58
comments: ture
categories: 数据库
tags: oracle

---

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第一章，主要是用来复习和巩固在课堂学习的知识！
- 首先在学习之前，我们应该得安装一个oracle数据库，本人推荐oracle 11g（注意：oracle安装最好一次到位，请细心处理！）
{% endnote %}
<!-- more -->

-------------------

## 准备

- 打开oracle主服务

找到pc上的**计算机右**击，找到**管理**打开，找到并打开**服务和应用程序**，打开服务并找到OracleServiceORCL【ORCL为你数据库服务的名字，一般默认为ORCL】

- 打开**监听器 OracleOraDb11g_home1TNSListener**

监听器一般就在oracle主服务上面，如果没有监听器就需要重新配置一个

- 配置监听器

开始菜单中找到net configration assistant添加一个监听器（不详细解释）

## 第三方工具（plsqldev）【需要注册】

PL/SQL Developer是一个集成开发环境，由Allround Automations公司开发，专门面向Oracle数据库存储的程序单元的开发。如今，有越来越多的商业逻辑和应用逻辑转向了Oracle Server，因此，PL/SQL编程也成了整个开发过程的一个重要组成部分。PL/SQL Developer侧重于易用性、代码品质和生产力，充分发挥Oracle应用程序开发过程中的主要优势的。

## 解锁SCOTT用户

- 登录system用户
- 新建一个sql命令栏
- 输入下例命令：

```sql
alter user scott account unlock;         -- 解锁scott用户
alter user scott identified by tiger;	 -- 修改scott密码	
```


## 数据库知识扩展【了解】

- 数据库应用系统

典型的数据库应用有C/S(客户/服务器)和B/S(浏览器/服务器)两种模式

- 三个规范化设计规则

>第一范式(1NF)：实体的所有属性必须是单值的并且不允许重复。
>第二范式(2NF)：实体的所有属性必须依赖于实体的惟一标识。
>第三范式(3NF)：一个非惟一标识属性不允许依赖于另一个非惟一标识属性。

- CONNECT命令

```sql
CONNECT SCOTT/TIGER@MYDB                 -- 重新连接数据库
```

- 环境设置命令

在SQL*Plus环境下，可以通过SHOW ALL命令可以查看SQL*Plus的环境参数。

- 显示当前用户【注意：使用SELECT USER FROM dual命令也可以取得用户名】
	
```sql
show user
```

