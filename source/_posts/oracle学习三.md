---
title: oracle学习三
date: 2017-11-24 17:13:04
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第三章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------

## 创建表空间

- 完整版

```sql
create tablespace epet_tablespace
datafile 'c:epet.dbf'
size 100M
autoextend on next 32M maxsize unlimited
logging
extent management local
segment space mamgement auto;	
```

- 简化版

```sql
create tablespace epet_tablespace
datafile 'c:epet.dbf'
size 100M
```

## 数据查询

- 查询格式： 

```sql
select 列名 from 表名 
where 查询条件 
group by 分组列 
having 分组后条件 
order by 排序列 
```


	   
- 简单查询（查询加条件）

如：查询部门10的雇员。


输入并执行查询：
```sql
SELECT * FROM emp WHERE deptno=10;
```


- 行号

每个表都有一个虚列ROWNUM，它用来显示结果中记录的行号。我们在查询中也可以显示这个列。
【任务1】  显示EMP表的行号。


输入并执行查询：

```sql
SELECT rownum,ename FROM emp;
```


- 查询时进行计算


1、显示雇员工资上浮20%的结果。
输入并执行查询:

```sql
SELECT ename,sal,sal*(1+20/100) FROM emp;
```

2、显示每一个雇员的总收入(工资+奖金)	

```sql
update emp set comm = 0 where comm is null;  --将工资为null的改为0
select ename,sal+comm as 总收入 from emp;
```

- 使用别名


在查询中使用列别名。
输入并执行：

```sql
SELECT ename AS 名称, sal 工资 FROM emp;
```

注意：建议大家省略AS

- 在列别名上使用双引号。（当你的别名为关键字或别名中有特殊符号时需要加双引号）
输入并执行查询：

```sql
SELECT ename AS "select", sal*12+5000 AS "年度工资(加年终奖)" FROM emp;
```


- 连接运算符

连接运算符是双竖线“||”。通过连接运算可以将两个字符串连接在一起。
如： 

```sql
SELECT	ename||job AS "雇员和职务表" FROM emp;

‘5’+    5  结果为 10        ‘5’||  5 结果为 '55'          
```sql

- 查询结果的排序


ASC 表示升序 (可省略)      Desc 表示降序(不可省略)
1.升序排序
【训练1】  查询雇员姓名和工资，并按工资从小到大排序。
输入并执行查询：

```sql
SELECT ename, sal FROM emp ORDER BY sal;
```

2．降序排序
【训练2】  查询雇员姓名和雇佣日期，并按雇佣日期排序，后雇佣的先显示。
输入并执行查询：

```sql
SELECT ename,hiredate FROM emp ORDER BY hiredate DESC;
```


3.多列排序
【训练3】  查询雇员信息，先按部门从小到大排序，再按雇佣时间的先后排序。
输入并执行查询：

```sql
SELECT ename,deptno,hiredate FROM emp ORDER BY deptno,hiredate;
```


- 消除重复行

如果在显示结果中存在重复行，可以使用的关键字DISTINCT消除重复显示。
如：使用DISTINCT消除重复行显示。
输入并执行查询：

```sql
SELECT DISTINCT job FROM emp;
```


## 练习

【练习1】显示当前的账户名，显示当前账户的EMP表的结构，显示EMP表中的数据。

```sql
Show user;
Desc emp;
Select * from emp;
```

【练习2】根据EMP表和DEPT表的显示结果，说出雇员ADAMS的雇员编号、职务、经理名字、雇佣日期、工资、津贴和部门编号以及该雇员所在的部门名称和所在城市。

```sql
Select * from emp where ename = ‘ADAMS’
Select dname,loc from dept where deptno = (select deptno from emp where ename =’ADAMS’)
```

【练习3】说出职务为CLERK的工资最高的雇员是哪一位？职务为CLERK、部门在NEW YORK的雇员是哪一位?

```sql
select * from emp where sal = (Select max(sal) from emp where job = ‘CLERK’)
Select * from emp where job = ‘CLERK’ and deptno in (select deptno from dept where loc = ‘NEW YORK’)
```

【练习4】显示DEPT表的内容，按以下的形式: 
 部门ACCOUNTING所在的城市为NEW YORK    

```sql
Select ‘部门’||dname||’所在的城市为’||loc from dept 
```

