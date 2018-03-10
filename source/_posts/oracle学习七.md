---
title: oracle学习七
date: 2017-11-28 21:30:03
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第七章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------

## 笔记

- 不等连接

拿一个表作为另一表的查询条件或范围
如：显示雇员名称，工资和所属工资等级。
执行以下查询：
```sql
SELECT e.ename, e.sal, s.grade 
FROM emp e,salgrade s
WHERE 	e.sal BETWEEN s.losal AND s.hisal
```
- 自连接
 
自连接就是一个表，同本身进行连接。对于自连接可以想像存在两个相同的表(表和表的副本)，可以通过不同的别名区别两个相同的表（其它就是内连接）。    

显示雇员名称和雇员的经理名称。
执行以下查询：
```sql
SELECT worker.ename||' 的经理是 '||manager.ename AS 雇员经理 	
FROM emp worker, emp manager
WHERE worker.mgr = manager.empno;
```
- 组函数

组函数只能应用于SELECT子句、HAVING子句或ORDER BY子句中。
     
A.组函数也可以称为统计函数。
	
B.组函数忽略列的空值。

C.对组可以应用组函数。

D.在组函数中可使用DISTINCT或ALL关键字。
          
E.ALL表示对所有非NULL值(可重复)进行运算。
          
F.DISTINCT 表示对每一个非NULL值，如果存在重复值，则组函数只运算一次。如果不指明上述关键字，默认为ALL。
	
函  数|说    明
-|-	
AVG|求平均值
COUNT|求计数值，返回非空行数，*表示返回所有行
MAX|求最大值
MIN|求最小值
SUM|求和
STDDEV|求标准偏差，是根据差的平方根得到的
VARIANCE|求统计方差

- 分组查询

如：按职务统计工资总和。
执行以下查询：
```sql
SELECT job,SUM(sal) 
FROM emp GROUP BY job;
```
- 多列分组
按部门和职务分组统计工资总和。

```sql
SELECT   deptno, job, sum(sal) 
FROM emp 
GROUP BY deptno, job;
```

- HAVING

HAVING从句过滤分组后的结果，它只能出现在GROUP BY从句之后，而WHERE从句要出现在GROUP BY从句之前。	统计各部门的最高工资，排除最高工资小于3000的部门。

执行以下查询：
```sql
SELECT   deptno, max(sal) 
FROM emp GROUP BY deptno
HAVING   max(sal)>=3000;
```
	
分组统计结果排序

可以使用ORDER BY从句对统计的结果进行排序，ORDER BY从句要出现在语句的最后。

如：按职务统计工资总和并排序。
执行以下查询：
```sql
SELECT job 职务, SUM(sal) 工资总和 
FROM emp GROUP BY job
ORDER BY SUM(sal);
```

- 组函数的嵌套使用	

如：求各部门平均工资的最高值。
执行以下查询：
```sql
SELECT max(avg(sal)) FROM emp GROUP BY deptno;
```
-------------------------------------

## 统计查询 

- 统计查询 
【训练1】  求雇员总人数。
执行以下查询：

```sql
SELECT COUNT(*) FROM emp;
```


【训练2】  求有佣金的雇员人数。
执行以下查询：
```sql
SELECT COUNT(comm) FROM emp;
```

【训练3】  求部门10的雇员的平均工资。
执行以下查询：
```sql
SELECT AVG(sal) FROM emp WHERE deptno=10;
```
【训练4】  求最晚和最早雇佣的雇员的雇佣日期。
执行以下查询：
```sql
SELECT MAX(hiredate),MIN(hiredate) FROM emp;
```
【训练5】  求雇员表中不同职务的个数。
执行以下查询：

```
SELECT COUNT( DISTINCT job) FROM emp;
```
- 分组统计
通过下面的训练，我们来了解分组的用法。
【训练6】  按职务统计工资总和。

步骤1：执行以下查询：

```sql
SELECT SUM(sal) FROM emp GROUP BY job;
```

步骤2：执行以下查询：

```sql
SELECT job,SUM(sal) FROM emp GROUP BY job;
```

【练习2】查看以下查询的显示结果，并解释原因。

```sql
SELECT ename,job,SUM(sal) FROM emp GROUP BY sal;
```
- 多列分组统计

【训练7】 按部门和职务分组统计工资总和。

```sql
SELECT   deptno, job, sum(sal) 
FROM emp 
GROUP BY deptno, job;
```

- 分组统计结果限定

对分组查询的结果进行过滤，要使用HAVING从句。
HAVING从句过滤分组后的结果，它只能出现在GROUP BY从句之后，而WHERE从句要出现在GROUP BY从句之前。

【训练8】  统计各部门的最高工资，排除最高工资小于3000的部门。
执行以下查询：
```sql
SELECT   deptno, max(sal) FROM emp
GROUP BY deptno
HAVING   max(sal)>=3000;
```

【训练8】  统计各部门的最高工资，排除最高工资小于3000的部门。
执行以下查询：
```sql
SELECT   deptno, max(sal) FROM emp
GROUP BY deptno
HAVING   max(sal)>=3000;
```
- 分组统计结果排序

可以使用ORDER BY从句对统计的结果进行排序，ORDER BY从句要出现在语句的最后。
例如
【训练9】  按职务统计工资总和并排序。
执行以下查询：
```sql
SELECT job 职务, SUM(sal) 工资总和 
FROM emp GROUP BY job
ORDER BY SUM(sal);
```

- 组函数的嵌套使用

在如下训练中，使用了组函数的嵌套。
【训练10】  求各部门平均工资的最高值。
执行以下查询：

```sql
SELECT max(avg(sal)) FROM emp GROUP BY deptno;
```

注意：虽然在查询中有分组列，但在查询字段中不能出现分组列。如下的查询是错误的：

```sql
SELECT deptno,max(avg(sal)) FROM emp GROUP BY deptno;
```
因为各部门平均工资的最高值不应该属于某个部门。


