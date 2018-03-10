---
title: oracle学习八
date: 2017-11-29 15:58:26
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第八章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------
## 笔记

- 子查询

A.通过把一个查询的结果作为另一个查询的一部分,子查询一般出现在SELECT语句的WHERE子句中，Oracle也支持在FROM或HAVING子句中出现子查询。

B.子查询比主查询先执行，结果作为主查询的条件，在书写上要用圆括号扩起来，并放在比较运算符的右侧。
1．单行子查询

如：查询比SCOTT工资高的雇员名字和工资。
执行以下查询：
```sql
SELECT ename,sal 
FROM emp
WHERE sal>(SELECT sal FROM emp WHERE empno=7788);
```
- 多行子查询
例如
如果子查询返回多行的结果，则我们称它为多行子查询。多行子查询要使用不同的比较运算符号，它们是IN、ANY和ALL。

如：查询工资低于任意一个“CLERK”的工资的雇员信息。
执行以下查询：

```
SELECT  empno, ename, job,sal 
FROM emp
WHERE   sal < ANY 
(SELECT sal FROM emp WHERE job = 'CLERK')
AND job <> 'CLERK';
```
	
如：查询工资比所有的“SALESMAN”都高的雇员的编号、名字和工资。
执行以下查询：
```sql
SELECT  empno, ename,sal 
FROM emp
WHERE sal > ALL
(SELECT sal FROM emp WHERE job= 'SALESMAN');
```
	
如：查询部门20中职务同部门10的雇员一样的雇员信息。
执行以下查询：

```sql
SELECT  empno, ename, job FROM emp 
WHERE  job IN (SELECT job FROM emp WHERE deptno=10) 
AND deptno =20;
```

- 多列子查询
	
如果子查询返回多列，则对应的比较条件中也应该出现多列，这种查询称为多列子查询。以下是多列子查询的训练实例。

查询职务和部门与SCOTT相同的雇员的信息。

```sql
SELECT  empno, ename, sal FROM emp WHERE (job,deptno) =(SELECT job,deptno FROM emp WHERE empno=7788);
```
- 在FROM从句中使用子查询

在FROM从句中也可以使用子查询，在原理上这与在WHERE条件中使用子查询类似。有的时候我们可能要求从雇员表中按照雇员出现的位置来检索雇员，很容易想到的是使用rownum虚列。比如我们要求显示雇员表中6～9位置上的雇员，可以用以下方法。
	
如：查询雇员表中排在第6～9位置上的雇员。
```sql
SELECT ename,sal FROM (SELECT rownum as num,ename,sal FROM emp WHERE rownum<=9 )
WHERE num>=6;
```

- 集合运算
	
操  作|描    述
-|-
UNION|并集，合并两个操作的结果，去掉重复的部分
UNION ALL|并集，合并两个操作的结果，保留重复的部分
MINUS|差集，从前面的操作结果中去掉与后面操作结果相同的部分
INTERSECT|交集，取两个操作结果中相同的部分

如：查询部门10和部门20的所有职务。

```sql
SELECT  job FROM emp WHERE deptno=10
UNION
SELECT  job FROM emp WHERE deptno=20;
```

如：查询部门10和20中是否有相同的职务和工资。

```sql
SELECT  job,sal FROM emp WHERE deptno=10
INTERSECT
SELECT  job,sal FROM emp WHERE deptno=20;
```

如：查询只在部门表中出现，但没有在雇员表中出现的部门编号。
```sql
SELECT  deptno FROM dept 
MINUS 
SELECT  deptno FROM emp ;
```


-------------------------------------

## 子查询

例如  
我们可能会提出这样的问题，在雇员中谁的工资最高，或者谁的工资比SCOTT高。                 
通过把一个查询的结果作为另一个查询的一部分，可以实现这样的查询功能。                  
出现在其他查询中的查询称为子查询，包含其他查询的查询称为主查询。


A.子查询一般出现在SELECT语句的WHERE子句中，Oracle也支持在FROM或HAVING子句中出现子查询。
              
B.子查询比主查询先执行，结果作为主查询的条件，在书写上要用圆括号扩起来，并放在比较运算符的右侧。
              
C.子查询可以嵌套使用，最里层的查询最先执行。子查询可以在SELECT、INSERT、UPDATE、DELETE等语句中使用。


- 单行子查询

【训练1】  查询比SCOTT工资高的雇员名字和工资。
执行以下查询：
```sql
SELECT ename,sal FROM emp
WHERE sal>(SELECT sal FROM emp WHERE empno=7788);
```

【训练2】  查询和SCOTT同一部门且比他工资低的雇员名字和工资。
执行以下查询：

```sql
SELECT ename,sal FROM emp
WHERE sal<(SELECT sal FROM emp WHERE empno=7788)
AND deptno=(SELECT deptno FROM emp WHERE empno=7788);
```
【训练3】  查询工资高于平均工资的雇员名字和工资。
执行以下查询：
```sql
SELECT ename,sal FROM emp
WHERE sal>(SELECT AVG(sal) FROM emp);
```
- 多行子查询

如果子查询返回多行的结果，则我们称它为多行子查询。多行子查询要使用不同的比较运算符号，它们是IN、ANY和ALL。
例如
【训练4】  <span id="inline-yellow">查询工资低于任何一个“CLERK”的工资的雇员信息。</span>
执行以下查询：
```sql
SELECT  empno, ename, job,sal FROM emp
WHERE   sal < ANY (SELECT sal FROM emp WHERE job = 'CLERK')
AND job <> 'CLERK';
```
【训练5】  查询工资比所有的“SALESMAN”都高的雇员的编号、名字和工资。
执行以下查询：

```sql
SELECT  empno, ename,sal FROM emp
WHERE sal > ALL(SELECT sal FROM emp WHERE job= 'SALESMAN');
```

【训练6】  查询部门20中职务同部门10的雇员一样的雇员信息。
执行以下查询：
```sql
SELECT  empno, ename, job FROM emp
WHERE   job IN (SELECT job FROM emp WHERE deptno=10)
AND deptno =20;
```

【训练7】  查询职务和SCOTT相同，比SCOTT雇佣时间早的雇员信息。
执行以下查询：

```sql
SELECT  empno, ename, job FROM emp
WHERE job =(SELECT job FROM emp WHERE empno=7788)
AND hiredate < (SELECT hiredate FROM emp WHERE empno=7788);
```

- 多列子查询
例如
如果子查询返回多列，则对应的比较条件中也应该出现多列，这种查询称为多列子查询。以下是多列子查询的训练实例。
	
【训练8】  查询职务和部门与SCOTT相同的雇员的信息。
执行以下查询：

```sql
SELECT  empno, ename, sal FROM emp
WHERE (job,deptno) =(SELECT job,deptno FROM emp WHERE empno=7788);
```
- 在FROM从句中使用子查询


在FROM从句中也可以使用子查询，在原理上这与在WHERE条件中使用子查询类似。有的时候我们可能要求从雇员表中按照雇员出现的位置来检索雇员，很容易想到的是使用rownum虚列。比如我们要求显示雇员表中6～9位置上的雇员，可以用以下方法。

【训练9】  <span id="inline-yellow">查询雇员表中排在第6～9位置上的雇员。</span>

```sql
SELECT ename,sal FROM (SELECT rownum as num,ename,sal FROM emp WHERE rownum<=9 )
WHERE num>=6;
```

说明：子查询出现在FROM从句中，检索出行号小于等于9的雇员，并生成num编号列。在主查询中检索行号大于等于6的雇员。
例如   
注意：以下用法不会有查询结果，请自行分析原因。
```sql
SELECT ename,sal FROM emp
WHERE rownum>=6 AND rownum<=9;
```


- 集合运算

多个查询语句的结果可以做集合运算，结果集的字段类型、数量和顺序应该一样。

![集合运算操作](https://github.com/aqqje/Personal-repository/raw/master/images/oracle12.png "oracle12")<br/>

【训练1】  查询部门10和部门20的所有职务。

```sql
SELECT  job FROM emp WHERE deptno=10
<span id="inline-blue">UNION</span>
SELECT  job FROM emp WHERE deptno=20;
```

【训练2】  查询部门10和20中是否有相同的职务和工资。
执行以下查询：
```sql
SELECT  job,sal FROM emp WHERE deptno=10
<span id="inline-blue">INTERSECT</span>
SELECT  job,sal FROM emp WHERE deptno=20;
```


【训练3】  查询只在部门表中出现，但没有在雇员表中出现的部门编号。
执行以下查询：

```sql
SELECT  deptno FROM dept
<span id="inline-blue">MINUS</span>
SELECT  deptno FROM emp ;
```


