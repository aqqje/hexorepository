---
title: oracle学习十二
date: 2017-12-09 17:15:14
comments: ture
categories:
	- 数据库
tags:
	- oracle
---

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第十二章，主要是用来复习和巩固在课堂学习的知识！

{% endnote %}
{% note danger %}
<!-- more -->
{% endnote %}

--------------------------

## 笔记

- 索引

索引(INDEX)是为了加快数据的查找而创建的数据库对象，特别是对大表，索引可以有效地提高查找速度，也可以保证数据的惟一性
创建索引一般要掌握以下原则：只有较大的表才有必要建立索引，表的记录应该大于50条，查询数据小于总行数的2%～4%。虽然可以为表创建多个索引，但是无助于查询的索引不但不会提高效率，还会增加系统开销。因为当执行DML操作时，索引也要跟着更新，这时索引可能会降低系统的性能。
创建索引：           

```sql
CREATE INDEX 索引名 ON 表名(列名);
```

删除索引：

```sql
DROP INDEX 索引名；
```

- 同义词

同义词(SYNONYM)是为模式对象起的别名，可以为表、视图、序列、过程、函数和包等数据库模式对象创建同义词。
创建私有同义词：

```sql
CREATE SYNONYM BOOK FOR 图书；
```

创建公有同义词(先要获得创建公有同义词的权限)：

```sql
CREATE PUBLIC SYNONYM BOOK FOR SCOTT.图书；
```

删除同义词：

```sql
DROP SYNONYM 同义词名；
```


- 数据库链接

数据库链接(DATABASE LINK)是在分布式环境下，为了访问远程数据库而创建的数据通信链路。

格式：

```sql
CREATE DATABASE LINK 链接名 CONNECT TO 账户 IDENTIFIED BY 口令 USING 服务名;
```

数据库链接一旦建立并测试成功，就可以使用以下形式来访问远程用户的表。

表名@数据库链接名

- PL/sql

1.块结构和基本语法要求

块中各部分的作用解释如下：

```
(1)  DECLARE：声明部分标志。

(2)  BEGIN：可执行部分标志。

(3)  EXCEPTION：异常处理部分标志。

(4)  END；：程序结束标志。
```

2、输出
第一种形式：

```sql
DBMS_OUTPUT.PUT(字符串表达式)；
```

第二种形式：

```
DBMS_OUTPUT.PUT_LINE(字符串表达式)；
```

第三种形式：

````
DBMS_OUTPUT.NEW_LINE；
```

3、变量赋值：

- 第一种形式：SELECT 列名1，列名2... INTO 变量1，变量2... FROM 表名 WHERE 条件；

- 第二种形式：变量名:=值

例：查询雇员编号为7788的雇员姓名和工资。SET SERVEROUTPUT ON
		
```sql		
DECLARE--定义部分标识 		
v_name  VARCHAR2(10);	--定义字符串变量v_name		
v_sal   NUMBER(5);	--定义数值变量v_sal		
BEGIN			--可执行部分标识SELECT	 ename,sal INTO v_name,v_sal 
FROM emp 
WHERE empno=7788;--在程序中插入的SQL语句		
DBMS_OUTPUT.PUT_LINE('7788号雇员是：'||v_name||'，工资为：'||to_char(v_sal));
--输出雇员名和工资	
END;
```	

4、结合变量的定义和使用（即全局变量）
   
- 该变量是在整个SQL*Plus环境下有效的变量，在退出SQL*Plus之前始终有效，所以可以使用该变量在不同的程序之间传递信息。结合变量不是由程序定义的，而是使用系统命令VARIABLE定义的。

例：定义并使用结合变量

步骤1：输入和执行下列命令，定义结合变量g_ename：
	
```sql	
VARIABLE  g_ename VARCHAR2(100) 
```
		
步骤2：输入和执行下列程序：
	
```sql	
SET SERVEROUTPUT ON 
		
BEGIN
  		 
:g_ename:=:g_ename|| 'Hello~ ';			--在程序中使用结合变量
  
DBMS_OUTPUT.PUT_LINE(:g_ename);                --输出结合变量的值
		
END;
```

5．记录变量的定义

- 还可以根据表或视图的一个记录中的所有字段定义变量，称为记录变量。记录变量包含若干个字段，在结构上同表的一个记录相同，定义方法是在表名后跟%ROWTYPE。记录变量的字段名就是表的字段名，数据类型也一致。

如：

```sql
v_name emp.ename%TYPE;
```

---------------------------------------------------------------------------------

## 数据库模式对象

- Oracle数据库的模式对象

对  象|名  称|作  用
-|-|-
TABLE|表	|用于存储数据的基本结构
VIEW|视图|以不同的侧面反映表的数据，是一种逻辑上的表
INDEX|索引|加快表的查询速度
CLUSTER|聚簇|将不同表的字段并用的一种特殊结构的表集合
SEQUENCE|序列|生成数字序列，用于在插入时自动填充表的字段
SYNONYM|同义词|为简化和便于记忆，给对象起的别名
DATABASE LINK|数据库链接|为访问远程对象创建的通道
STORED PROCEDURE、FUNCTION|存储过程和函数|存储于数据库中的可调用的程序和函数
PACKAGE、PACKAGE BODY|包和包体|将存储过程、函数及变量按功能和类别进行捆绑


## 索引


- 索引(INDEX)是为了加快数据的查找而创建的数据库对象，特别是对大表，索引可以有效地提高查找速度，也可以保证数据的惟一性。索引是由Oracle自动使用和维护的，一旦创建成功，用户不必对索引进行直接的操作。索引是独立于表的数据库结构，即表和索引是分开存放的，当删除索引时，对拥有索引的表的数据没有影响。


- 在创建PRIMARY KEY和UNIQUE约束条件时，系统将自动为相应的列创建惟一(UNIQUE)索引。索引的名字同约束的名字一致。


- 索引有两种：B*树索引和位图(BITMAP)索引。

【B*树索引是通常使用的索引，也是默认的索引类型。在这里主要讨论B*树索引。B*树是一种平衡2叉树，左右的查找路径一样。这种方法保证了对表的任何值的查找时间都相同。】

【B*树索引可分为：惟一索引、非惟一索引、一列简单索引和多列复合索引。】


- 创建索引一般要掌握以下原则：只有较大的表才有必要建立索引，表的记录应该大于50条，查询数据小于总行数的2%～4%。虽然可以为表创建多个索引，但是无助于查询的索引不但不会提高效率，还会增加系统开销。因为当执行DML操作时，索引也要跟着更新，这时索引可能会降低系统的性能。一般在主键列或经常出现在WHERE子句或连接条件中的列建立索引，该列称为索引关键字。


## 索引的创建

- 创建索引不需要特定的系统权限。索引会自动被引用。

```sql
DROP INDEX 索引名；
```

【训练1】  创建和删除索引。

步骤1：创建索引：

```sql
CREATE INDEX EMP_ENAME ON EMP(ENAME);
```


步骤2：查询中引用索引：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE ENAME='SCOTT';
```
		
步骤3：删除索引：

```sql
DROP INDEX EMP_ENAME;
```


【训练2】  创建复合索引。索引出现在WHERE子句中的时候就会被自动被引用。
		
步骤1：创建复合索引：

```sql
CREATE INDEX EMP_JOBSAL ON EMP(JOB,SAL);
```
	       
 步骤2：查询中引用索引：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE JOB='MANAGER'AND SAL>2500;
```

如下的查询也会引用索引：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE JOB='CLERK';
```

但以下查询不会引用索引，因为没有先引用索引关键字的主键：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE SAL>2500;
```

## 查看索引

- 通过查询数据字典USER_INDEXES可以检查创建的索引。

通过查询数据字典USER_IND_COLUMNS可以检查索引的列。
		
【训练1】  显示emp表的索引：

```
SELECT INDEX_NAME, INDEX_TYPE, UNIQUENESS FROM USER_INDEXES WHERE TABLE_NAME='EMP';
```

【训练2】  显示索引的列。

```sql
SELECT COLUMN_NAME FROM USER_IND_COLUMNS 
WHERE  INDEX_NAME='EMP_JOBSAL';
```


## 同义词

- 同义词(SYNONYM)是为模式对象起的别名，可以为表、视图、序列、过程、函数和包等数据库模式对象创建同义词。同义词有两种：公有同义词和私有同义词。公有同义词是对所有用户都可用的。创建公有同义词必须拥有系统权限CREATE PUBLIC SYNONYM；创建私有同义词需要CREATE SYNONYM系统权限。私有同义词只对拥有同义词的账户有效，但私有同义词也可以通过授权，使其对其他用户有效。同义词通过给本地或远程对象分配一个通用或简单的名称，隐藏了对象的拥有者和对象的真实名称，也简化了SQL语句。

- 如果同义词同对象名称重名，私有同义词又同公有同义词重名，那么，识别的顺序是怎样的呢？如果存在对象名，则优先识别，其次识别私有同义词，最后识别公有同义词。比如,执行以下的SELECT语句：

```sql
SELECT * FROM ABC;
```


如果存在表ABC，就对表ABC执行查询语句；如果不存在表ABC，就去查看是否有私有同义词ABC，如果有就对ABC执行查询(此时ABC是另外一个表的同义词)；如果没有私有同义词ABC，则去查找公有同义词；如果找不到，则查询失败。


## 同义词的创建和使用

```sql
DROP SYNONYM 同义词名；
```


【训练1】  创建同义词。
		
步骤1：创建私有同义词：

```sql
CREATE SYNONYM BOOK FOR 图书；
```

步骤2：创建公有同义词(先要获得创建公有同义词的权限)：

```sql
CREATE PUBLIC SYNONYM BOOK FOR SCOTT.图书；
```

步骤3：使用同义词：

```sql
SELECT *　FROM BOOK;
```

## 同义词的查看

- 通过查询数据字典USER_OBJECTS和USER_SYNONYMS，可以查看同义词信息。 

		
【训练1】  查看用户拥有的同义词：

```sql
SELECT OBJECT_NAME FROM　USER_OBJECTS WHERE OBJECT_TYPE='SYNONYM';
```
		
## 系统定义同义词

- 系统为常用的对象预定义了一些同义词，利用它们可以方便地访问用户的常用对象

- Oracle数据库模式对象

对 象|名 称|作    用
-|-|-|
DICT|DICTIONARY|数据字典
CAT|USER_CATALOG|用户拥有的表、视图、同义词和序列
CLU|USER_CLUSTERS|用户拥有的聚簇
IND|USER_INDEXES|用户拥有的索引
OBJ|USER_OBJECTS|用户拥有的对象
SEQ|USER_SEQUENCES|用户拥有的序列
SYN|USER_SYNONYMS|用户拥有的私有同义词
COLS|USER_TAB_COLUMNS|用户拥有的表、视图和聚簇的列
TABS|USER_TABLES|用户拥有的表


【训练1】  查看用户拥有的表：

```sql		
SELECT TABLE_NAME FROM TABS;
```


## 聚簇【了解】

- 所谓聚簇(CLUSTER)，形象地说，就是生长在一起的表。聚簇包含一张或多张表，表的公共列被称为聚簇关键字，在公共列上具有同一值的列物理上存储在一起。那么在什么情况下需要创建聚簇呢？通常在多个表有共同的列时，应使用聚簇。比如有一张学生基本情况表，其中包含学生的学号、姓名、性别、住址等信息。另外，还设计了一张学生成绩表，其中除了包含学生成绩，也包含学生的学号、姓名、性别。那么这两张表共同的列就可以创建成聚簇。这样两张表的共同的学号、姓名和性别，就存放在了一起，相同的值只存放一次。如果两个表通过聚簇列进行联合，则会大大提高查询的速度，但对于插入等操作则会降低效率。

- 创建聚簇后，要创建使用聚簇的表，对聚簇还应该建立索引。如果不对聚簇建立索引，则不能对聚簇表进行插入、修改和删除操作。

- 创建聚簇需要CREATE CLUSTER系统权限。


【训练1】  创建和使用聚簇。
		
步骤1：创建聚簇：

```
CREATE CLUSTER COMM(STUNO NUMBER(5),STUNAME VARCHAR2(10),SEX VARCHAR2(2))
SIZE 500
TABLESPACE USERS;
```


步骤2：创建第一张聚簇表：

```sql
CREATE TABLE STUDENT(
STUNO NUMBER(5),
STUNAME VARCHAR2(10),
SEX VARCHAR2(2),
ADDRESS VARCHAR2(20),
E_MAIL VARCHAR2(20)
)
CLUSTER COMM(STUNO,STUNAME,SEX);
```
		
步骤3：创建第二张聚簇表：

```sql
CREATE TABLE SCORE(
STUNO NUMBER(5),
STUNAME VARCHAR2(10),
SEX VARCHAR2(2),
CHINESE NUMBER(3),
MATH NUMBER(3),
ENGLISH NUMBER(3) )
CLUSTER COMM(STUNO,STUNAME,SEX);
```

		
步骤4：为聚簇创建索引：	

```sql
CREATE INDEX INX_COMM ON CLUSTER COMM;
```
	
步骤5：向表中插入数据：

```sql
INSERT INTO STUDENT VALUES(10001,'黄凯','男','宝安','HK123@163.COM');
INSERT INTO STUDENT VALUES(10002,'苏丽','女','罗湖','SL99@163.COM');
INSERT INTO STUDENT VALUES(10003,'刘平平','男','南山','PP2003@SHOU.COM');
INSERT INTO SCORE VALUES(10001,'黄凯','男',70,85,93);
INSERT INTO SCORE VALUES(10002,'苏丽','女',65,74,83);
INSERT INTO SCORE VALUES(10003,'刘平平','男',88,75,69);
```

		
步骤6：删除聚簇及聚簇表：

```sql
DROP CLUSTER COMM INCLUDING TABLES CASCADE CONSTRAINTS;
```

## 数据库链接

- 数据库链接(DATABASE LINK)是在分布式环境下，为了访问远程数据库而创建的数据通信链路。数据库链接隐藏了对远程数据库访问的复杂性。通常，我们把正在登录的数据库称为本地数据库，另外的一个数据库称为远程数据库。  

- 有了数据库链接，可以直接通过数据库链接来访问远程数据库的表。常见的形式是访问远程数据库固定用户的链接，即链接到指定的用户，创建这种形式的数据库链接的语句


如下：

```sql
CREATE DATABASE LINK 链接名 CONNECT TO 账户 IDENTIFIED BY 口令
USING 服务名;
```
		
- 创建数据库链接，需要CREATE DATABASE LINK系统权限。
		
数据库链接一旦建立并测试成功，就可以使用以下形式来访问远程用户的表。

		表名@数据库链接名



【训练1】  在局域网上创建和使用数据库链接。

步骤1：创建远程数据库的服务名，假定局域网上另一个数据库服务名为MYDB_REMOTE。
		
步骤2：登录本地数据库SCOTT账户，创建数据库链接：

```sql
CONNECT SCOTT/TIGER@MYDB
CREATE DATABASE LINK abc CONNECT TO scott IDENTIFIED BY tiger USING 'MYDB_REMOTE';
```

步骤3：查询远程数据库的数据：

```sql
SELECT *　FROM emp@abc;
```

结果略。
		
步骤4：一个分布查询：

```sql
SELECT ename,dname FROM emp@abc e,dept d WHERE e.deptno=d.deptno;
```
		
说明：在本例中，远程数据库服务名是MYDB_REMOTE，创建的数据库链接名称是abc.emp@abc表示远程数据库的emp表。步骤4是一个联合查询，数据来自本地服务器的dept表和远程服务器的emp表。



-------------------------------------------------------------

## 例题


【训练1】  创建和删除索引。
		
步骤1：创建索引：

```sql
CREATE INDEX EMP_ENAME ON EMP(ENAME);
```

步骤2：查询中引用索引：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE ENAME='SCOTT';
```
		
步骤3：删除索引：

```sql
DROP INDEX EMP_ENAME;
```

【训练2】  创建复合索引。索引出现在WHERE子句中的时候就会被自动被引用。
		 
步骤1：创建复合索引：

```sql
CREATE INDEX EMP_JOBSAL ON EMP(JOB,SAL);
```
	     
步骤2：查询中引用索引：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE JOB='MANAGER'AND SAL>2500;
```

如下的查询也会引用索引：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE JOB='CLERK';
```

但以下查询不会引用索引，因为没有先引用索引关键字的主键：

```sql
SELECT ENAME,JOB,SAL FROM EMP WHERE SAL>2500;
```

【训练3】  显示emp表的索引：

```sql
SELECT INDEX_NAME, INDEX_TYPE, UNIQUENESS FROM USER_INDEXES WHERE TABLE_NAME='EMP';
```
 
【训练4】  显示索引的列。

```sql
SELECT COLUMN_NAME FROM USER_IND_COLUMNS 
WHERE  INDEX_NAME='EMP_JOBSAL';
```
		
【训练5】  创建同义词。
		
步骤1：创建私有同义词：

```sql
CREATE SYNONYM BOOK FOR 图书；
```
     
步骤2：创建公有同义词(先要获得创建公有同义词的权限)：

```sql
CREATE PUBLIC SYNONYM BOOK FOR SCOTT.图书；
```
     
步骤3：使用同义词：

```sql
SELECT *　FROM BOOK;
```

【训练6】  查看用户拥有的同义词：

```sql
SELECT OBJECT_NAME FROM　USER_OBJECTS WHERE OBJECT_TYPE='SYNONYM';
```

【训练7】  查看用户拥有的表：

```sql
SELECT TABLE_NAME FROM TABS;
```
		
【训练8】  创建和使用聚簇。
		
步骤1：创建聚簇：

```sql
CREATE CLUSTER COMM(STUNO NUMBER(5),STUNAME VARCHAR2(10),SEX VARCHAR2(2))
SIZE 500
TABLESPACE USERS;
```

步骤2：创建第一张聚簇表：

```sql
CREATE TABLE STUDENT(
STUNO NUMBER(5),
STUNAME VARCHAR2(10),
SEX VARCHAR2(2),
ADDRESS VARCHAR2(20),
E_MAIL VARCHAR2(20)
)CLUSTER COMM(STUNO,STUNAME,SEX);
```
 
步骤3：创建第二张聚簇表：
	
```sql	
CREATE TABLE SCORE(
STUNO NUMBER(5),
STUNAME VARCHAR2(10),
SEX VARCHAR2(2),
CHINESE NUMBER(3),
MATH NUMBER(3),
ENGLISH NUMBER(3) )
CLUSTER COMM(STUNO,STUNAME,SEX);
```

步骤4：为聚簇创建索引：

		CREATE INDEX INX_COMM ON CLUSTER COMM;
	  
步骤5：向表中插入数据：

```sql
INSERT INTO STUDENT VALUES(10001,'黄凯','男','宝安','HK123@163.COM');
INSERT INTO STUDENT VALUES(10002,'苏丽','女','罗湖','SL99@163.COM');
INSERT INTO STUDENT VALUES(10003,'刘平平','男','南山','PP2003@SHOU.COM');
INSERT INTO SCORE VALUES(10001,'黄凯','男',70,85,93);
INSERT INTO SCORE VALUES(10002,'苏丽','女',65,74,83);
INSERT INTO SCORE VALUES(10003,'刘平平','男',88,75,69);
```
	  
步骤6：删除聚簇及聚簇表：

```sql		
DROP CLUSTER COMM INCLUDING TABLES CASCADE CONSTRAINTS;
```

【训练9】  在局域网上创建和使用数据库链接。
		
步骤1：创建远程数据库的服务名，假定局域网上另一个数据库服务名为MYDB_REMOTE。
		
步骤2：登录本地数据库SCOTT账户，创建数据库链接：

```sql
CONNECT SCOTT/TIGER@ORACLE
CREATE DATABASE LINK abc CONNECT TO scott IDENTIFIED BY tiger USING 'MYDB_REMOTE';
```
        
步骤3：查询远程数据库的数据：

```sql
SELECT *　FROM emp@abc;
```

步骤4：一个分布查询：

```sql
SELECT ename,dname FROM emp@abc e,dept d WHERE e.deptno=d.deptno;
```
		

