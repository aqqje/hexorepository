---
title: oracle学习五
date: 2017-11-26 20:54:06
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第五章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
<!-- more -->

--------------------------


## 函数专项

- 数值型函数<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle3.png  "oracle3")<br/>

- 字符型函数

字符型函数包括大小写转换和字符串操作函数。大小写转换函数有3个.

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle4.png    "oracle4")<br/>


- 日期型函数

A.Oracle使用内部数字格式来保存时间和日期，包括世纪、年、月、日、小时、分、秒。缺省日期格式为 DD-MON-YY，如“08-05月-03”代表2003年5月8日。

B.SYSDATE是返回系统日期和时间的虚列函数

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle5.png "oracle5")<br/>

【训练1】  返回系统的当前日期。
输入并执行查询：
```sql
SELECT sysdate FROM dual;
```

【训练2】  返回2003年2月的最后一天。
输入并执行查询：
```sql
SELECT last_day('08-2月-03') FROM dual;
```

【训练3】  假定当前的系统日期是2003年2月6日，求再过1000天的日期。
输入并执行查询：
```sql
SELECT sysdate+1000 AS "NEW DATE" FROM dual;
```

【训练4】  假定当前的系统日期是2003年2月6日，显示部门10雇员的雇佣天数。
输入并执行查询：
```sql
SELECT ename, round(sysdate-hiredate) DAYS
FROM   emp
WHERE  deptno = 10;
```

------------------

- 	转换函数
	
Oracle的类型转换分为自动类型转换和强制类型转换。常用的类型转换函数有TO_CHAR、TO_DATE或TO_NUMBER

自动类型转换
Oracle可以自动根据具体情况进行如下的转换：
* 字符串到数值。
* 字符串到日期。
* 数值到字符串。
* 日期到字符串。


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle6.png "oracle6")<br/>

【训练1】  自动转换字符型数据到数值型。
输入并执行查询：
```sql
SELECT '12.5'+11 FROM dual;
```
【训练2】  自动转换数值型数据到字符型。
执行以下查询：
```sql
SELECT '12.5'||11 FROM dual;
```

## 日期类型转换

将日期型转换成字符串时，可以按新的格式显示。
如格式YYYY-MM-DD HH24:MI:SS表示“年-月-日 小时:分钟:秒”。Oracle的日期类型是包含时间在内的。


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle7.png "oracle7")<br/>
![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle8.png "oracle8")<br/>

【训练3】  将日期转换成带时间和星期的字符串并显示。
执行以下查询：
```sql
SELECT TO_CHAR(sysdate,'YYYY-MM-DD HH24:MI:SS AM DY') FROM dual;
```
【训练4】  将日期显示转换成中文的年月日。
输入并执行查询：
```sql
SELECT TO_CHAR(sysdate,'YYYY"年"MM"月"DD"日"') FROM dual;
```

【训练5】  将雇佣日期转换成字符串并按新格式显示。
输入并执行查询：
```sql
SELECT	ename, to_char(hiredate, 'DD Month YYYY') HIREDATE
FROM  	emp;
```

【训练6】  以全拼和序列显示时间。
执行以下查询：
```sql
SELECT SYSDATE,to_char(SYSDATE,'yyyysp'),to_char(SYSDATE,'mmspth'),
to_char(SYSDATE,'ddth') FROM dual;
```

【训练7】  时间显示的大小写。
步骤1：执行以下查询：
```sql
SELECT SYSDATE,to_char(SYSDATE,'yyyysp') FROM dual;
```
步骤2：执行以下查询：
```sql
SELECT to_char(SYSDATE,'Yyyysp') FROM dual;
```

步骤3：执行以下查询：
```sql
SELECT SYSDATE,to_char(SYSDATE,'YYyysp') FROM dual;
```
--------------------------

- 数字类型转换

将数字型转换成字符串时，也可以按新的格式显示

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle9.png "oracle9")<br/>


【训练8】  将数值转换成字符串并按新格式显示。
执行以下查询：
```sql
SELECT TO_CHAR(123.45,’9999.99'), TO_CHAR(12345,'L9.9EEEE') FROM dual;
```
【训练9】  将数值转换成字符串并按新格式显示。
执行以下查询：
```sql
SELECT TO_CHAR(sal,'$99,999') SALARY FROM emp
WHERE ename = 'SCOTT';
```
--------------------------------

- 其他函数

Oracle还有一些函数，如decode和nvl，这些函数也很有用

空值的转换:

如果对空值NULL不能很好的处理，就会在查询中出现一些问题。在一个空值上进行算术运算的结果都是NULL。最典型的例子是，在查询雇员表时，将工资sal字段和津贴字段comm进行相加，如果津贴为空，则相加结果也为空，这样容易引起误解。

使用nvl函数，可以转换NULL为实际值。该函数判断字段的内容，如果不为空，返回原值；为空，则返回给定的值。

如下3个函数，分别用新内容代替字段的空值：

nvl(comm, 0)：用0代替空的Comm值。
nvl(hiredate, '01-1月-97')：用1997年1月1日代替空的雇佣日期。
nvl(job, '无')：用“无”代替空的职务

decode函数:

decode函数可以通过比较进行内容的转换，完成的功能相当于分支语句。
在参数的最后位置上可以存在单独的参数，如果以上比较过程没有找到匹配值，则返回该参数的值，如果不存在该参数，则返回NULL。


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle10.png "oracle10")<br/>

【训练1】  使用nvl函数转换空值。
执行以下查询：
```sql
SELECT	ename,nvl(job,'无'),nvl(hiredate,'01-1月-97'),nvl(comm,0) FROM	 emp;
```
【训练2】  将职务转换成中文显示。
执行以下查询：
```sql
SELECT	ename,decode(job, 'MANAGER', '经理', 'CLERK','职员', 'SALESMAN','推销员', 'ANALYST','系统分析员','未知') FROM emp;
```


## 最大、最小值函数
```sql
greatest返回参数列表中的最大值，least返回参数列表中的最小值。
```
如果表达式中有NULL，则返回NULL。



## 练习


【练习1】显示雇员名称和雇佣的星期数。
```sql
Select ename,round((sysdate-hiredate)/7) from emp;
```

【练习2】显示从本年1月1日开始到现在经过的天数(当前时间取SYSDATE的值)。
```sql
Select sysdate - to_date(‘2017-1-1’,’YYYY-MM-DD’ )from dual;
```
【练习3】显示2008年的8月8日为星期几。
```sql
Select to_char(‘8-8月-2008’,’DY’) from dual;
```
【练习4】对部门表的部门名称和城市名进行转换。 
```sql
select * from dept;
select decode(dname,'ACCOUNTING','统计部','RESEARCH','研发部','SALES','销售部','OPERATIONS','其它部门') 部门,
decode(loc,'NEW YORK','纽约','DALLAS','达拉斯','CHICAGO','芝加哥','BOSTON','波士顿') 城市 
from dept;
```
【练习5】判断用户的角色是否为SYSDBA。
```sql
Select userenv(‘ISDBA’) from dual;
```
