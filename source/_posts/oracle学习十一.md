---
title: oracle学习十一
date: 2017-12-06 14:49:51
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>

{% note info %}
- 此文章是oracle学习系例第十一章，主要是用来复习和巩固在课堂学习的知识！
<!-- more -->
{% endnote %}

--------------------------

## 笔记


一、分区表
在某些场合会使用非常大的表，比如人口信息统计表。如果一个表很大，就会降低查询的速度，并增加管理的难度。一旦发生磁盘损坏，可能整个表的数据就会丢失，恢复比较困难。根据这一情况，可以创建分区表，把一个大表分成几个区(小段)，对数据的操作和管理都可以针对分区进行，这样就可以提高数据库的运行效率。分区可以存在于不同的表空间上，提高了数据的可用性。例：创建和使用分区表。

<span id="inline-yellow">创建按成绩分区的考生表，共分为3个区：</span>

```sql
CREATE TABLE 考生 (	
考号 VARCHAR2(5),
姓名 VARCHAR2(30),
成绩 NUMBER(3)
)

PARTITION BY RANGE(成绩)	
(PARTITION A VALUES LESS THAN (300)
TABLESPACE USERS,
PARTITION B VALUES LESS THAN (500)
TABLESPACE USERS,
PARTITION C VALUES LESS THAN (MAXVALUE)
TABLESPACE USERS
);

步骤3：检查A区中的考生：

SELECT *  FROM  考生 PARTITION(A);

步骤4：检查全部的考生：

SELECT *  FROM  考生;
```
## 视图

## 视图的概念


视图不同于表，视图本身不包含任何数据。而视图只是一种定义，对应一个查询语句。视图的数据都来自于某些表，这些表被称为基表。    视图可以在表能够使用的任何地方使用，但在对视图的操作上同表相比有些限制，特别是插入和修改操作。对视图的操作将传递到基表，所以在表上定义的约束条件和触发器在视图上将同样起作用。

## 视图的创建
 
- 格式：

```sql
create [or replace] view 视图名 
as
select 语句;
```

例：创建图书作者视图：
```sql
CREATE VIEW 图书作者(书名,作者) 	
AS SELECT 图书名称,作者 FROM 图书;
```
查询视图全部内容
```sql
SELECT * FROM 图书作者;    
```
查询部分视图：
```sql
SELECT 作者 FROM 图书作者;
```
## 删除视图：
```sql
DROP VIEW 清华图书;
```
## 创建只读视图

创建只读视图要用WITH READ ONLY选项。

例：创建emp表的经理视图：
```sql
CREATE OR REPLACE VIEW manager 
AS SELECT * FROM emp WHERE job= 'MANAGER'
WITH READ ONLY;
```
## 使用WITH CHECK OPTION选项

使用WITH CHECK OPTION选项。

使用该选项，可以对视图的插入或更新进行限制，即该数据必须满足视图定义中的子查询中的WHERE条件，否则不允许插入或更新。
例：
```sql
CREATE OR REPLACE VIEW 清华图书 	
AS SELECT * FROM 图书 WHERE 出版社编号= '01'	
WITH CHECK OPTION;注：插入数据时，由于带了with check option的选项，则只能插入出版社编为'01'的数据
```

## 来自基表的限制

除了以上的限制，基表本身的限制和约束也必须要考虑。如果生成子查询的语句是一个分组查询，或查询中出现计算列，这时显然不能对表进行插入。另外，主键和NOT NULL列如果没有出现在视图的子查询中，也不能对视图进行插入。在视图中插入的数据，也必须满足基表的约束条件。

## 视图的查看

字典|说明
-|-
USER_VIEWS|字典中包含了视图的定义。
USER_UPDATABLE_COLUMNS|字典包含了哪些列可以更新、插入、删除。
USER_OBJECTS|字典中包含了用户的对象。

可以通过DESCRIBE命令查看字典的其他列信息。

例：查看用户拥有的视图：
```sql
SELECT object_name FROM user_objects WHERE object_type='VIEW';
```

--------------------------------------------

## 表和视图


- 图书表

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle13.png "oracle13")<br/>

- 出版社表

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle14.png "oracle14")<br/>


【训练1】  创建图书和出版社表。

步骤1：创建出版社表，输入并执行以下命令：
```sql
CREATE TABLE 出版社(
编号 VARCHAR2(2),
出版社名称 VARCHAR2(30),
地址 VARCHAR2(30),
联系电话 VARCHAR2(20)
);

步骤2：创建图书表，输入并执行以下命令：

CREATE TABLE 图书(
图书编号 VARCHAR2(5),
图书名称 VARCHAR2(30),
出版社编号 VARCHAR2(2),
作者 VARCHAR2(10),
出版日期 DATE,
数量 NUMBER(3),
单价 NUMBER(7,2)
);



步骤3：使用DESCRIBE显示图书表的结构，输入并执行以下命令：

DESCRIBE 图书
```
## 通过子查询创建表

【训练2】  通过子查询创建新的图书表。

步骤1：完全复制图书表到“图书1”，输入并执行以下命令：
```sql
CREATE TABLE 图书1 AS SELECT *　FROM 图书;

步骤2：创建新的图书表“图书2”，只包含书名和单价，输入并执行以下命令：

CREATE TABLE 图书2(书名,单价) AS SELECT 图书名称,单价 FROM 图书;

步骤3：创建新的图书表“图书3”，只包含书名和单价，不复制内容，输入并执行以下命令：

CREATE TABLE 图书3(书名,单价) AS SELECT 图书名称,单价 FROM 图书 WHERE 1=2;
```
## 设置列的默认值

可以在创建表的同时指定列的默认值，默认值由DEFAULT部分说明。


【训练3】  创建表时设置默认值。

步骤1：创建表时，设置表的默认值。
```sql
CREATE TABLE 图书4(
图书编号 VARCHAR2(5) DEFAULT NULL,
图书名称 VARCHAR2(30) DEFAULT '未知',
出版社编号 VARCHAR2(2) DEFAULT NULL,
出版日期 DATE DEFAULT '01-1月-1900',
作者 VARCHAR2(10) DEFAULT NULL,
数量 NUMBER(3) DEFAULT 0,
单价 NUMBER(7,2) DEFAULT NULL,
借出数量 NUMBER(3) DEFAULT 0
);


步骤2：插入数据。

INSERT INTO 图书4(图书编号) VALUES('A0001');

步骤2：查询插入结果。

SELECT * FROM 图书4;
```

【练习1】创建图书出借信息表，设置适当的默认值，并插入数据。

结构如下：


名称|是否为空?|类型
-|-|-
图书编号|not null|VARCHAR2(10)
 借书人|not null|VARCHAR2(10)
借书日期|not null|DATE
归还日期|not null|DATE



## 删除已创建的表

表的删除者必须是表的创建者或具有DROP ANY TABLE权限。

【训练4】  删除“图书1”表。

DROP TABLE 图书1; 
执行结果：
表已丢弃。


## 表的操作

- 表的重命名


只有表的拥有者，才能修改表名。

- 清空表


清空表的语法为：
```sql
TRUNCATE TABLE 表名；
```
清空表可删除表的全部数据并释放占用的存储空间。

- 查看表


可以通过对数据字典USER_OBJECTS的查询，显示当前模式用户的所有表。
	
【训练1】  显示当前用户的所有表。
```sql
SELECT object_name FROM user_objects WHERE object_type='TABLE';
```
- 数据完整性约束


在创建表和修改表时，可通过定义约束条件来保证数据的完整性和一致性。约束条件是一些规则，在对数据进行插入、删除和修改时要对这些规则进行验证，从而起到约束作用。

**完整性包括数据完整性和参照完整性，数据完整性定义表数据的约束条件，参照完整性定义数据之间的约束条件。**
**数据完整性由主键(PRIMARY KEY)、非空(NOT NULL)、惟一(UNIQUE)和检查(CHECK)约束条件定义**
**参照完整性由外键(FOREIGN KEY)约束条件定义。在表的创建过程中，应该先创建主表，后创建子表。**



- 约束条件的创建


【训练1】  创建带有约束条件的出版社表(如果已经存在，先删除)：
```sql
CREATE TABLE 出版社(
编号 VARCHAR2(2) CONSTRAINT PK_1 PRIMARY KEY,
出版社名称 VARCHAR2(30) NOT NULL ,
地址 VARCHAR2(30) DEFAULT '未知',
联系电话 VARCHAR2(20)
);
```
【训练2】  创建带有约束条件(包括外键)的图书表(如果已经存在，先删除)：
```sql
CREATE TABLE 图书(图书编号 VARCHAR2(5) CONSTRAINT PK_2 PRIMARY KEY,
图书名称 VARCHAR2(30) NOT NULL,
出版社编号 VARCHAR2(2) CHECK(LENGTH(出版社编号)=2) NOT NULL,
作者 VARCHAR2(10) DEFAULT '未知',
出版日期 DATE DEFAULT '01-1月-1900',
数量 NUMBER(3) DEFAULT 1 CHECK(数量>0),
单价 NUMBER(7,2),
CONSTRAINT YS_1 UNIQUE(图书名称,作者),
CONSTRAINT FK_1 FOREIGN KEY(出版社编号) REFERENCES 出版社(编号) ON DELETE CASCADE
);
```
说明：因为两个表同属于一个用户，故约束名不能相重，图书表的主键为“图书编号”列，主键名为PK_2。其中，约束条件CHECK(LENGTH(出版社编号)=2)表示出版社编号的长度必须是2，约束条件UNIQUE(图书名称,作者)表示“图书名称”和“作者”两列的内容组合必须惟一。FOREIGN KEY(出版社编号) REFERENCES 出版社(编号) 表示图书表的“出版社编号”列参照出版社的“编号”主键列。出版社表为主表，图书表为子表，出版社表必须先创建。ON DELETE CASCADE表示当删除出版社表的记录时，图书表中的相关记录同时删除，比如删除清华大学出版社，则图书表中清华大学出版社的图书也会被删除。

如果同时出现DEFAULT和CHECK，则DEFAULT需要出现在CHECK约束条件之前。


【训练3】插入数据，验证约束条件。
```sql
步骤1：插入出版社信息：

INSERT INTO 出版社 VALUES('01','清华大学出版社','北京','010-83456272');

继续插入

INSERT INTO 出版社 VALUES('01','电子科技大学出版社','西安','029-88201467');

重新执行：

INSERT INTO 出版社 VALUES('02','电子科技大学出版社','西安','029-88201467');

步骤2：插入图书信息：

INSERT INTO 图书(图书编号,图书名称,出版社编号,作者,单价) VALUES('A0001','计算机原理','01','刘勇',25.30);

继续插入：

INSERT INTO 图书(图书编号，图书名称，出版社编号，作者，单价) VALUES('A0002',' C语言程序设计','03','马丽', 18.75);
INSERT INTO 图书(图书编号，图书名称，出版社编号，作者，单价) VALUES(‘A0002’,‘ C语言程序设计’,‘02’,‘马丽’, 18.75);

继续插入：

INSERT INTO 图书(图书编号,图书名称,出版社编号,作者,数量,单价) VALUES('A0003','汇编语言程序设计','02','黄海明',0,20.18);

重新执行：

INSERT INTO 图书(图书编号,图书名称,出版社编号,作者,数量,单价) VALUES('A0003','汇编语言程序设计','02','黄海明',15,20.18);

 步骤3：显示插入结果：

SELECT * FROM 出版社;

继续查询：

SELECT * FROM 图书;

步骤4：提交插入的数据：

COMMIT;
```
说明：在图书表中，没有插入的数量取默认值1，没有插入的出版日期取默认值01-1月-00(即1900年1月1日)。


【训练4】  通过删除数据验证ON DELETE CASCADE的作用。
```sql
步骤1：删除出版社01(清华大学)：

DELETE FROM 出版社 WHERE 编号='01';

步骤2：显示删除结果：

显示出版社表结果：

SELECT * FROM 出版社;

显示图书表结果：

SELECT * FROM 图书;

步骤3：恢复删除:

ROLLBACK；
```
- 查看约束条件


数据字典USER_CONSTRAINTS中包含了当前模式用户的约束条件信息。其中，CONSTRAINTS_TYPE 显示的约束类型为：

C：CHECK约束。
P：PRIMARY KEY约束。
U：UNIQUE约束。
R：FOREIGN KEY约束。

其他信息可根据需要进行查询显示，可用DESCRIBE命令查看USER_CONSTRAINTS的结构。


【训练1】  检查表的约束信息：
```sql
SELECT CONSTRAINT_NAME,CONSTRAINT_TYPE,SEARCH_CONDITION FROM USER_CONSTRAINTS
WHERE TABLE_NAME='图书';
```


## 修改表结构

- 增加新列

【训练1】  为“出版社”增加一列“电子邮件”：
```sql
ALTER TABLE 出版社
ADD 电子邮件 VARCHAR2(30) CHECK(电子邮件 LIKE '%@%');
```

- 修改列


修改列定义有以下一些特点：

(1) 列的宽度可以增加或减小，在表的列没有数据或数据为NULL时才能减小宽度。
(2) 在表的列没有数据或数据为NULL时才能改变数据类型，CHAR和VARCHAR2之间可以随意转换。
(3) 只有当列的值非空时，才能增加约束条件NOT NULL。
(4) 修改列的默认值，只影响以后插入的数据。

【训练1】  修改“出版社”表“电子邮件”列的宽度为40。
```sql
ALTER TABLE 出版社
MODIFY 电子邮件 VARCHAR2(40);
```
- 删除列


【训练1】删除“出版社”表的“电子邮件”列。
```sql
ALTER TABLE 出版社
DROP COLUMN 电子邮件;
```
【训练2】  将“图书”表的“出版日期”列置成UNUSED，并查看。
```sql
步骤1：设置“出版日期”列为UNUSED：

ALTER TABLE 图书 SET UNUSED COLUMN 出版日期;

步骤2：显示结构：

DESC 图书;

步骤3：删除UNUSED列：

ALTER TABLE 图书 DROP UNUSED COLUMNS;
```


- 约束条件的修改


可以为表增加或删除表级约束条件。

- 增加约束条件
- 
【训练1】  为emp表的mgr列增加外键约束：
```sql
ALTER TABLE emp ADD CONSTRAINT FK_3 FOREIGN KEY(mgr) REFERENCES emp(empno);
```

- 删除约束条件


【训练2】  删除为emp表的mgr列增加的外键约束：

```sql
ALTER TABLE emp DROP CONSTRAINT FK_3;
```


## 视图创建和操作

- 视图的概念


  视图不同于表，视图本身不包含任何数据。而视图只是一种定义，对应一个查询语句。视图的数据都来自于某些表，这些表被称为基表。

视图可以在表能够使用的任何地方使用，但在对视图的操作上同表相比有些限制，特别是插入和修改操作。对视图的操作将传递到基表，所以在表上定义的约束条件和触发器在视图上将同样起作用。

- 视图的创建
- 
创建简单视图 

【训练1】  创建图书作者视图。
```sql
步骤1：创建图书作者视图：

CREATE VIEW 图书作者(书名,作者) 
AS SELECT 图书名称,作者 FROM 图书;

步骤2：查询视图全部内容

SELECT * FROM 图书作者;

步骤3：查询部分视图：

SELECT 作者 FROM 图书作者;

步骤4：删除视图：

DROP VIEW 清华图书;
```
- 创建复杂视图

【训练3】  修改作者视图，加入出版社名称。
```sql
步骤1：重建图书作者视图：

CREATE OR REPLACE VIEW 图书作者(书名,作者,出版社) 
AS SELECT 图书名称,作者,出版社名称 FROM 图书,出版社 
WHERE 图书.出版社编号=出版社.编号;

步骤2：查询新视图内容：

SELECT * FROM 图书作者;
```
【训练4】  创建一个统计视图。
```sql
步骤1：创建emp表的一个统计视图：

CREATE VIEW 统计表(部门名,最大工资,最小工资,平均工资)
AS SELECT DNAME,MAX(SAL),MIN(SAL),AVG(SAL) FROM EMP E,DEPT D
WHERE E.DEPTNO=D.DEPTNO GROUP BY DNAME;
	      
 步骤2：查询统计表：

SELECT * FROM 统计表;
```
- 创建只读视图


创建只读视图要用WITH READ ONLY选项。

【训练5】  创建只读视图。
```sql
步骤1：创建emp表的经理视图：

CREATE OR REPLACE VIEW manager 
AS SELECT * FROM emp WHERE job= 'MANAGER'
WITH READ ONLY;

步骤2：进行删除：

DELETE FROM manager;
```
- 视图的操作


对视图经常进行的操作是查询操作，但也可以在一定条件下对视图进行插入、删除和修改操作。对视图的这些操作最终传递到基表。但是对视图的操作有很多限定。如果视图设置了只读，则对视图只能进行查询，不能进行修改操作。

- 视图的插入 


【训练1】  视图插入练习。

步骤1：创建清华大学出版社的图书视图：
```sql
CREATE OR REPLACE VIEW 清华图书 
AS SELECT * FROM 图书 WHERE 出版社编号= '01';

步骤2：插入新图书：

INSERT INTO 清华图书 VALUES('A0005','软件工程','01','冯娟',5,27.3);

步骤3：显示视图：

SELECT * FROM 清华图书;

步骤4：显示基表

SELECT * FROM 图书;
```
问题:如果在“清华图书”的视图中插入其他出版社的图书，结果会怎么样呢？

结果是允许插入，但是在视图中看不见，在基表中可以看见，这显然是不合理的。




- 使用WITH CHECK OPTION选项


为了避免上述情况的发生，可以使用WITH CHECK OPTION选项。使用该选项，可以对视图的插入或更新进行限制，即该数据必须满足视图定义中的子查询中的WHERE条件，否则不允许插入或更新。


【训练2】  使用WITH CHECK OPTION选项限制视图的插入。

步骤1：重建清华大学出版社的图书视图，带WITH CHECK OPTION选项：
```sql
CREATE OR REPLACE VIEW 清华图书 
AS SELECT * FROM 图书 WHERE 出版社编号= '01'
WITH CHECK OPTION;

步骤2：插入新图书：

INSERT INTO 清华图书 VALUES('A0006','Oracle数据库','02','黄河',3,39.8);
```
- 来自基表的限制


除了以上的限制，基表本身的限制和约束也必须要考虑。如果生成子查询的语句是一个分组查询，或查询中出现计算列，这时显然不能对表进行插入。另外，主键和NOT NULL列如果没有出现在视图的子查询中，也不能对视图进行插入。在视图中插入的数据，也必须满足基表的约束条件。


【训练3】基表本身限制视图的插入。
```sql
步骤1：重建图书价格视图：

CREATE OR REPLACE VIEW 图书价格 
AS SELECT 图书名称,单价 FROM 图书;

步骤2：插入新图书：

INSERT INTO 图书价格 VALUES('Oracle数据库',39.8);
```
- 视图的查看


USER_VIEWS字典中包含了视图的定义。
USER_UPDATABLE_COLUMNS字典包含了哪些列可以更新、插入、删除。
USER_OBJECTS字典中包含了用户的对象。

可以通过DESCRIBE命令查看字典的其他列信息。在这里给出一个训练例子。

【训练1】  查看清华图书视图的定义：
```sql
SELECT TEXT FROM USER_VIEWS WHERE VIEW_NAME='清华图书';
```
【训练2】  查看用户拥有的视图：
```sql
SELECT object_name FROM user_objects WHERE object_type='VIEW';
```
----------------

##  阶段训练


【训练1】  创建学生、系部、课程和成绩表，根据需要设置默认值、约束条件、主键和外键。
```sql
步骤1：创建系部表，编号为主键，系部名称非空，电话号码惟一：

CREATE TABLE 系部(
编号 NUMBER(5) PRIMARY KEY,
系部名 VARCHAR2(20) NOT NULL,
地址 VARCHAR2(30),
电话 VARCHAR2(15) UNIQUE,
系主任 VARCHAR2(10)
);


步骤2：创建学生表，学号为主键，姓名非空，性别只能是男或女，电子邮件包含@并且惟一，系部编号参照系部表的编号：

CREATE TABLE 学生 (
学号 VARCHAR2(10) PRIMARY KEY,
姓名 VARCHAR2(10) NOT NULL,
性别 VARCHAR2(2) CHECK(性别='男' OR 性别='女'),
生日 DATE,
住址 VARCHAR2(30),
电子邮件 VARCHAR2(20) CHECK(电子邮件 LIKE '%@%') UNIQUE,
系部编号 NUMBER(5),
CONSTRAINT FK_XBBH FOREIGN KEY(系部编号) REFERENCES 系部(编号)
);


步骤3：创建课程表，编号为主键，课程名非空，学分为1到5：

CREATE TABLE 课程(
编号 NUMBER(5) PRIMARY KEY,
课程名 VARCHAR2(30) NOT NULL,
学分 NUMBER(1) CHECK(学分>0 AND 学分<=5)
);


步骤4：创建成绩表，学号和课程编号为主键，学号参照学生表的学号，课程编号参照课程表的编号：

CREATE TABLE 成绩(
学号 VARCHAR2(10),
课程编号 NUMBER(5),
成绩 NUMBER (3),
CONSTRAINT PK PRIMARY KEY(学号,课程编号),
CONSTRAINT FK_XH FOREIGN KEY(学号) REFERENCES 学生(学号),
CONSTRAINT FK_KCBH FOREIGN KEY(课程编号) REFERENCES 课程(编号)
);
```sql


说明：注意表之间的主从关系，对于系部和学生表，系部表为主表，学生表为子表。学生表的外键表示插入学生的系部编号必须是系部表的编号。对于成绩表，主键是学号和课程编号，表示如果学号相同课程编号必须不同，这样就可以惟一地标识记录。课程表有两个外键，分别参照学生表和课程表，表示成绩表的学号必须是学生表的学号，成绩表的课程编号必须是课程表的编号。



