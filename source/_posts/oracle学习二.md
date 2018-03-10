---
title: oracle学习二
date: 2017-11-23 20:57:30
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>

{% note info %}
- 此文章是oracle学习系例第二章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

-------------------

## 创建用户

说明：
创建新用户user1，口令为123，口令需要以字母开头。

- 登录SCOTT账户
 
```sql
 create user user1 identified by 123   -- 创建新用户user1
```


- 授权

```sql
 grant connect to user1;    	  -- 授予user1连接数据库权限
 grant create table to user1;   -- 授予user1创建表的权限
 grant create proedure to user1;       -- 授予user1创建存储过程的权限
 grant unlimited tablespace to user1;  -- 授予user1表空间使用权限
```

## 创建表

说明：
以创建表的方式复制数据到新账户。

```sql
 create table emp as select * from scott.emp;
 create table dept as select * from scott.dept;
 create table salgrades as select * from scott.salgrade;
```

在USER1账户下复制了SCOTT账户的三个表：EMP、DEPT和SALGRADES。
 
	

## 给用户授权

- 方式一：授予角色 

```sql
connect  :   登录
resource:     普通权限，用于操作
DBA:      管理员权限 （慎用）
```

例如：

```sql
 grant connect to user1  	
 grant connect,resource to user1
 ```
	  
- 方式二：授予单个权限


 例如： 
 
 ```sql
 grant create table to user1  授予user1建表的权限
 grant drop table to user1  授予user1删表的权限
 ```


- 方式三：将某个对象的权限授予用户

 
 例如：

```sql 
 grant select on scott.emp to user1  -- 将scott用户的emp表的查询权限授予user1
 grant  all on scott.emp to user1            将scott用户的emp表的所有权限授予user1
```


## 收回权限


 revoke 权限 from 用户
 
 
 例如： 
 
```sql
 revoke connect from user1     收回user1的connect权限
 revoke select on scott.emp from user1   收回user1对emp表的查询权限
```

