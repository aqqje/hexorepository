---
title: oracle学习六
date: 2017-11-27 17:03:02
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第六章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------
## 笔记

- 相等连接（内连接）

```sql
select ename,job,sal,comm,emp.deptno,dname
from emp,dept
where emp.deptno = dept.deptno 
```
注意：如果两个表有同名列，那么前面必须接表名 如： emp.deptno ,如果不是同名字段则表名可以省略

- inner join 的写法:
 
例如
```sql
select ename,job,sal,comm,emp.deptno,dname 
from emp inner join dept 
on emp.deptno = dept.deptno
```
 - 三表或三表以上的写法

```sql
select 字段1，字段2 , 字段3 。。。。
from 表1，表2，表3.。。
where 表1.外键 = 表2.主键  and 表1.外键 = 表3.主键 and 。。。
```

注意：两个表有一个条件 ，三个表有两个条件 ，四个表有三个条件 以此类推

- 外连接(不等连接)
   
左外连接即在内连接的基础上，左边表中有但右边表中没有的记录也以null的形式显示出来，右外连接则反之

1.写法1

例如(右外连接) 

```sql                          
select ename,d.deptno,dname
from emp e,dept d
where e.deptno(+) = d.deptno
```
  
(左外连接) 
```sql
select ename,d.deptno,dname
from emp e,dept 
dwhere d.deptno = e.deptno(+)    
```
2.写法2
```sql
select ename,d.deptno,dname 
from emp e right join dept d 
on e.deptno = d.deptno   
```
-----------------------------------

## 高级查询


- 多表联合查询

A.通过连接可以建立多表查询，多表查询的数据可以来自多个表，但是表之间必须有适当的连接条件。

B.一般N个表进行连接，需要至少N-1个连接条件，才能够正确连接。两个表连接是最常见的情况，只需要说明一个连接条件。

C.两个表的连接有四种连接方式：

* 相等连接。
* 不等连接。
* 外连接。
* 自连接。


- 相等连接
通过两个表具有相同意义的列，可以建立相等连接条件。

【训练1】  显示雇员的名称和所在的部门的编号和名称。
执行以下查询：

```sql
SELECT emp.ename,emp.deptno,dept.dname 
FROM emp,dept 
WHERE emp.deptno=dept.deptno;
```sql

【训练2】  使用表别名。
执行以下查询：

```sql
SELECT ename,e.deptno,dname 
FROM emp e,dept d 
WHERE e.deptno=d.deptno;
```
- <span id="inline-yellow">外连接

A.在以上的例子中，相等连接有一个问题：如果某个雇员的部门还没有填写，即保留为空，那么该雇员在查询中就不会出现；或者某个部门还没有雇员，该部门在查询中也不会出现。

B.为了解决这个问题可以用外连，即除了显示满足相等连接条件的记录外，还显示那些不满足连接条件的行。外连操作符为(+)，它可以出现在相等连接条件的左侧或右侧。
</span>
- 【训练4】  使用外连显示不满足相等条件的记录。
步骤1：显示雇员的名称、工资和所在的部门名称及没有任何雇员的部门。
执行以下查询：

```sql
SELECT ename,sal,dname 
FROM emp,dept 
WHERE emp.deptno(+)=dept.deptno;
```

外连接
```sql
SELECT ename,sal,dname 
FROM emp right outer join dept 
on emp.deptno = dept.deptno;
```
步骤2：显示雇员的名称、工资和所在的部门名称及没有属于任何部门的雇员。
执行以下查询：

```sql
SELECT ename,sal,dname 
FROM emp,dept 
WHERE emp.deptno=dept.deptno(+);
```
```sql
SELECT ename,sal,dname 
FROM emp left outer join dept 
on emp.deptno = dept.deptno;
```

- 不等连接
还可以进行不等的连接。

【训练5】  <span id="inline-yellow">显示雇员名称，工资和所属工资等级</span>
执行以下查询：

```sql
SELECT e.ename, e.sal, s.grade 
FROM emp e,salgrade s 
WHERE  e.sal BETWEEN s.losal AND s.hisal
```
- 自连接
最后是一个自连接的训练实例，自连接就是一个表，同本身进行连接。对于自连接可以想像存在两个相同的表(表和表的副本)，可以通过不同的别名区别两个相同的表。

【训练6】  <span id="inline-yellow">显示雇员名称和雇员的经理名称</span>
执行以下查询：

```sql
SELECT worker.ename||' 的经理是 '||manager.ename AS 雇员经理 
FROM emp worker, emp manager 
WHERE worker.mgr = manager.empno;
```
- 统计查询
Oracle提供了一些函数来完成统计工作，这些函数称为组函数，组函数不同于前面介绍和使用的函数(单行函数)。组函数可以对分组的数据进行求和、求平均值等运算。

组函数只能应用于SELECT子句、HAVING子句或ORDER BY子句中。
组函数也可以称为统计函数。


![常见组函数：](https://github.com/aqqje/Personal-repository/raw/master/images/oracle11.png "oracle11")<br/>

 组函数忽略列的空值。
对组可以应用组函数。
在组函数中可使用DISTINCT或ALL关键字。
ALL表示对所有非NULL值(可重复)进行运算。
DISTINCT 表示对每一个非NULL值，如果存在重复值，则组函数只运算一次。如果不指明上述关键字，默认为ALL。


