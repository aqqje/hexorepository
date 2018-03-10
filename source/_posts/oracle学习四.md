---
title: oracle学习四
date: 2017-11-25 20:50:39
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第四章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------


## 条件查询

- 模糊查询(between、in、like)<br/>
1.between:在某某之间<br/>
显示工资在1000～2000之间的雇员信息。<br/>
输入并执行查询：

```sql
SELECT * FROM emp WHERE sal BETWEEN 1000 AND 2000;
```

2.in: 在某某之内<br/>
显示职务为“SALESMAN'，“CLERK”和“MANAGER”的雇员信息。<br/>
输入并执行查询：

```sql
SELECT * FROM emp WHERE job IN ('SALESMAN','CLERK','MANAGER');
```


3.like:与通配符合用<br/>
通配符： <br/>  
%：代表0个或任意个字符 <br/>    
 _:代表1个字符<br/>



显示姓名以“S”开头的雇员信息。<br/>
输入并执行查询：<br/>
```sql
SELECT * FROM emp WHERE ename LIKE 'S%';
```

显示姓名第二个字符为“A”的雇员信息。<br/>
执行查询：<br/>

```sql
SELECT * FROM emp WHERE ename LIKE '_A%';
```

4.空值查询<br/>

```sql
空：is null<br/>       
非空：is not null<br/>
如：查询奖金为空的雇员信息<br/>
```
```sql
select * from emp where comm is null 	
```
---------------------------------------------

## 函数

- 数学函数
  
函  数|功  能|实  例|结  果
-|-|-|-
abs|求绝对值函数|abs(-5)|5
sqrt|求平方根函数	|sqrt(2)|1.41421356
power|求幂函数|power(2,3)|8

【训练1】  使用数值型函数练习。

步骤1：使用求绝对值函数abs。

```sql
SELECT abs(-5) FROM dual;
```

步骤2：使用求平方根函数sqrt。

```sql
SELECT sqrt(2) FROM dual;
```

步骤3：使用ceil函数。

```sql
SELECT ceil(2.35) FROM dual;	
```

步骤4：使用floor函数。

```sql
SELECT floor(2.35) FROM dual;
```

- 使用四舍五入函数round   格式: round(数字，保留的位数)

```sql
SELECT  round(45.923,2), round(45.923,0), round(45.923,-1) FROM dual; 
```
-----------------------------------------------


- 字符型函数


函数名称|功  能|实  例|结  果
-|-|-|-
ascii|获得字符的ASCII码|Ascii('A')|65
chr|返回与ASCII码相应的字符|Chr(65)|A
lower|将字符串转换成小写|lower ('SQL Course')|sql course
upper|将字符串转换成大写|upper('SQL Course')|SQL COURSE
initcap|将字符串转换成每个单词以大写开头|initcap('SQL course')|Sql Course
concat|连接两个字符串|concat('SQL', ' Course')|SQL Course
substr|给出起始位置和长度，返回子字符串|substr('String',1,3)|Str
length|求字符串的长度|length('Wellcom')|7
trim|在一个字符串中去除另一个字符串|trim('S' FROM 'SSMITH')|MITH
replace|用一个字符串替换另一个字符串中的子字符串|replace('ABC', 'B', 'D')|ADC

	  
【训练1】  如果不知道表的字段内容是大写还是小写，可以转换后比较。
输入并执行查询：

```sql
SELECT  empno, ename, deptno	FROM emp
WHERE  lower(ename) ='blake';
```sql

【训练2】  显示名称以“W”开头的雇员，并将名称转换成以大写开头。
输入并执行查询：
	 
```sql
SELECT empno,initcap(ename),job FROM emp   WHERE substr(ename,1,1)='W';
```

【训练3】  显示雇员名称中包含“S”的雇员名称及名称长度。
输入并执行查询：	

```sql
SELECT empno,ename,length(ename) FROM emp
WHERE instr(ename, 'S', 1, 1)>0;
```
--------------------------------------------------------
- 日期型函数

A.Oracle使用内部数字格式来保存时间和日期，包括世纪、年、月、日、小时、分、秒。缺省日期格式为 DD-MON-YY，如“08-05月-03”代表2003年5月8日。

B.SYSDATE是返回系统日期和时间的虚列函数。



显示1982年以后雇佣的雇员姓名和雇佣时间。

输入并执行查询：
```sql
SELECT ename,hiredate FROM emp WHERE hiredate>='1-1月-82';
```


- 显示部门10以外的其他部门的雇员。
输入并执行查询：
```sql
SELECT * FROM emp WHERE NOT deptno=10;
```
- 显示姓名第二个字符为“A”的雇员信息。
执行查询：
```sql
SELECT * FROM emp WHERE ename LIKE '_A%';
```
- 显示经理编号没有填写的雇员。
输入并执行查询：
```sql
SELECT  ename, mgr FROM emp WHERE mgr IS NULL;
```


- 如果不知道表的字段内容是大写还是小写，可以转换后比较。
输入并执行查询：
```sql
SELECT  empno, ename, deptno	FROM emp
WHERE  lower(ename) ='blake';
```
- 显示名称以“W”开头的雇员，并将名称转换成以大写开头。
输入并执行查询：
```sql
SELECT empno,initcap(ename),job FROM emp   WHERE substr(ename,1,1)='W';
```
【训练15】  显示雇员名称中包含“S”的雇员名称及名称长度。
输入并执行查询：
```sql
SELECT empno,ename,length(ename) FROM emp
WHERE instr(ename, 'S', 1, 1)>0;
```
## 在Oracle/PLSQL中，instr函数返回要截取的字符串在源字符串中的位置。只检索一次，就是说从字符的开始

到字符的结尾就结束。
语法如下：
```sql
instr( string1, string2 [, start_position [, nth_appearance ] ] )
```sql
参数分析：
- string1:源字符串，要在此字符串中查找。
- string2:要在string1中查找的字符串.
- start_position:代表string1 的哪个位置开始查找。此参数可选，如果省略默认为1. 字符串索引从1开始。如果此参数为正，从左到右开始检索，如果此参数为负，从右到左检索，返回要查找的字符串在源字符串中的开始索引。
- nth_appearance:代表要查找第几次出现的string2. 此参数可选，如果省略，默认为 1.如果为负数系统会报错。

注意：

***如果String2在String1中没有找到，instr函数返回0.***
