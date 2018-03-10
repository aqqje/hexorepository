---
title: oracle学习九
date: 2017-11-30 09:07:27
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第九章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------

## 笔记

- 增删改
```sql
增: insert into 表名 (列名) values (值)
删: delete from 表名 where 条件             (truncate table 表名:清空表)
改：update 表名 set 列名1=值1,列名2=值2 。。。 where  条件
```
- <span id="inline-blue">复制数据</span>

1、通过一条查询语句创建一个新表(要求目标表不存在)
```sql
CREATE TABLE manager AS SELECT empno,ename,sal FROM emp WHERE job='MANAGER';
```
2、通过一条查询语句复制数据(要求目标表必须已建好)
```sql
INSERT INTO manager
SELECT empno, ename, sal FROM   emp WHERE	job = 'CLERK';
```
- 序列1、创建序列

<span id="inline-yellow">创建从2000起始，增量为1 的序列abc：</span>
```sql
CREATE SEQUENCE abc INCREMENT BY 1  START  WITH  2000 
MAXVALUE  99999  CYCLE  NOCACHE; 
```

- <span id="inline-yellow">使用序列</span>

序列名.nextval: 代表下一个值
序列名.currval: 代表当前值

```sql  
INSERT INTO manager     
VALUES(abc.nextval,'小王',2500);
```
```sql
INSERT INTO manager     
VALUES(abc.nextval,'小赵',2800);
```
- 事务

A.两次连续成功的COMMIT或ROLLBACK之间的操作，称为一个事务。在一个事务内，数据的修改一起提交或撤销，如果发生故障或系统错误，整个事务也会自动撤销

B.数据库事务处理可分为隐式和显式两种。显式事务操作通过命令实现，隐式事务由系统自动完成提交或撤销(回退)工作，无需用户的干预。
	
C.隐式提交的情况包括：当用户正常退出SQL*Plus或执行CREATE、DROP、GRANT、REVOKE等命令时会发生事务的自动提交。

显示事务:

-|-
COMMIT|数据库事务提交，将变化写入数据库
ROLLBACK|数据库事务回退，撤销对数据的修改
SAVEPOINT|创建保存点，用于事务的阶段回退


