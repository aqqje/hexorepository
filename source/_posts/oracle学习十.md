---
title: oracle学习十
date: 2017-12-05 20:33:58
comments: ture
categories:
	- 数据库
tags:
	- oracle
---


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 此文章是oracle学习系例第十章，主要是用来复习和巩固在课堂学习的知识！
{% endnote %}
{% note danger %}
<!-- more -->
{% endnote %}

--------------------------

## 笔记

- 建表

```sql
格式：create table 表名
(
列名1   类型   约束,
列名2   类型   约束,
......	
);
```

如：
步骤1：创建出版社表，输入并执行以下命令：

```sql
CREATE TABLE 出版社(
编号 VARCHAR2(2),
出版社名称 VARCHAR2(30),
地址 VARCHAR2(30),
联系电话 VARCHAR2(20)
);
```
```sql
CREATE TABLE 图书(
图书编号 VARCHAR2(5),
图书名称 VARCHAR2(30),
出版社编号 VARCHAR2(2),
作者 VARCHAR2(10),	
出版日期 DATE,	
数量 NUMBER(3),	
单价 NUMBER(7,2)	
);
```

- 通过子查询建表


步骤1：完全复制图书表到“图书1”，输入并执行以下命令：
```sql
CREATE TABLE 图书1 AS SELECT *　FROM 图书;
```

步骤2：创建新的图书表“图书2”，只包含书名和单价，输入并执行以下命令：
	
```sql	
CREATE TABLE 图书2(书名,单价) AS SELECT 图书名称,单价 FROM 图书;
```
步骤3：创建新的图书表“图书3”，只包含书名和单价，不复制内容，输入并执行以下命令：
```sql
CREATE TABLE 图书3(书名,单价) AS SELECT 图书名称,单价 FROM 图书 WHERE 1=2;
```
- 添加表的约束

约束|关键字|简写
-|-|-
主键|primary key|PK
唯一|unique|UQ
默认值|default|DF
检查约束|check|CK
外键约束|foreign key|FK

方法一：建表的同时添加约束


```sql
create table stuinfo
(
sno int primary key not null,       --主键
sname varchar2(10) unique not null,       --唯一
sex char(2) default '男' check(sex='男' or sex = '女') not null,   --默认及检查
saddress varchar2(50) not null,
phone char(11),
email varchar2(50)
);
```
```sql
create table stumarks
(
marksId int,
sno int references stuinfo(sno) not null,     --外键
score number(5,1),
examDate date default sysdate
);
```

方法二:建表完成后，再添加约束

（之前已建好了出版社表及图书表）

```sql
--主键约束
alter table 出版社 add constraint PK_编号
primary key (编号);
--唯一约束
alter table 出版社 add constraint UQ_地址
unique (地址);
--检查约束
alter table 出版社 add constraint CK_联系电话
check (联系电话 like '1%');
--默认值
alter table 出版社 modify 地址 default '湘潭';
--外键约束
alter table 图书 add constraint FK_图书编号 
foreign key (图书编号) references 
出版社(编号)
--外键约束
alter table 图书 add constraint FK_图书编号 
foreign key (图书编号) references 出版社(编号)
```

- 查看约束条件

数据字典USER_CONSTRAINTS中包含了当前模式用户的约束条件信息。其中，CONSTRAINTS_TYPE 显示的约束类型为：
	
	
C：CHECK约束。

P：PRIMARY KEY约束。

U：UNIQUE约束。

R：FOREIGN KEY约束。


其他信息可根据需要进行查询显示，可用DESCRIBE命令查看USER_CONSTRAINTS的结构。

如:检查表的约束信息：
```sql
SELECT CONSTRAINT_NAME,CONSTRAINT_TYPE,SEARCH_CONDITION FROM USER_CONSTRAINTS
WHERE TABLE_NAME='图书';
```
- 删除约束条件
```sql
ALTER TABLE 表名 DROP CONSTRAINT 约束名;
```

- 表的操作


1、删除表    drop table 表名
2、重命名表  RENAME 表名 TO 新表名;
3、查看表


可以通过对数据字典USER_OBJECTS的查询，显示当前模式用户的所有表。
	

如： 显示当前用户的所有表。
```sql
SELECT object_name FROM user_objects WHERE object_type='TABLE';
```
- 修改表
1、<span id="inline-yellow">增加新列:</span>
如： 为“出版社”增加一列“电子邮件”：

```sql
ALTER TABLE 出版社
ADD 电子邮件 VARCHAR2(30) CHECK(电子邮件 LIKE '%@%');
```
2、修改列
	
修改列定义有以下一些特点：
(1) 列的宽度可以增加或减小，在表的列没有数据或数据为NULL时才能减小宽度。
(2) 在表的列没有数据或数据为NULL时才能改变数据类型，CHAR和VARCHAR2之间可以随意转换。(3) 只有当列的值非空时，才能增加约束条件NOT NULL。
(4) 修改列的默认值，只影响以后插入的数据。如：修改“出版社”表“电子邮件”列的宽度为40。
```sql
ALTER TABLE 出版社 MODIFY 电子邮件 VARCHAR2(40);
```
3、删除列
如：删除“出版社”表的“电子邮件”列。
```sql
ALTER TABLE 出版社
DROP COLUMN 电子邮件;
```

--------------------------------------------------

## 数据操作

## 插入数据

【训练1】  表的部分字段插入练习。
步骤1：将新雇员插入到emp表：
```sql
INSERT INTO emp(empno,ename,job)
VALUES (1000, '小李', 'CLERK');
```
步骤2：显示插入结果
```sql
SELECT * FROM emp WHERE empno=1000;
```
         
 日期类型的字段值也要用单引号括起来，如'10-1月-03'。日期型的数据默认格式为DD-MON-YY，默认的世纪为当前的世纪，默认的时间为午夜12点。如果指定的世纪不是本世纪或时间不是午夜12点，则必须使用TO_DATE系统函数对字符串进行转换。


【训练2】  时间字段的插入练习。

步骤1：将新雇员插入到emp表：
```sql
INSERT INTO emp(empno,ename,job,hiredate)
VALUES (1001, '小马', 'CLERK', '10-1月-03');
```

【训练3】  表的全部字段的插入练习。

执行以下的查询：
```sql
INSERT INTO dept VALUES (50, '培训部','深圳');
```
【训练4】  插入空值练习。

执行以下的查询：
```sql
INSERT INTO emp(empno,ename,job,sal) VALUES(1005,'杨华', 'CLERK',null);
```
## 复制数据:该形式一次可以插入多行数据。


【训练5】  <span id="inline-yellow">通过其他表插入数据的练习。</span>

步骤1：创建一个新表manager：
```sql
CREATE TABLE manager AS SELECT empno,ename,sal FROM emp WHERE job='MANAGER';
```

步骤2：从emp表拷贝数据到manager：

```sql
INSERT INTO manager
SELECT	empno, ename, sal
FROM   emp
WHERE	job = 'CLERK';
```

##　修改数据

【训练1】  修改小李(编号为1000)的工资为3000。
执行以下的查询：
```sql
UPDATE 	emp
SET sal = 3000
WHERE empno = 1000;
```

【训练2】  将小李(编号为1000)的雇佣日期改成当前系统日期，部门编号改为50。

执行以下的查询：
```sql
UPDATE  emp
SET hiredate=sysdate, deptno=50
WHERE  empno = 1000;
```

【训练3】  为所有雇员增加100元工资。
执行以下的查询：
```sql
UPDATE  emp
SET  sal =sal+100;
```
【训练4】  根据其他表修改数据。

执行以下的查询：
```sql
UPDATE  manager
SET (ename, sal) =(SELECT ename,sal FROM emp WHERE   empno = 7788)
WHERE   empno = 1000;
```
##　删除数据
【训练1】  删除雇员编号为1000的新插入的雇员。

步骤1：删除编号为1000的雇员：
```sql
DELETE FROM emp WHERE empno=1000;
```
步骤2：显示删除结果：
```sql
SELECT * FROM emp WHERE empno=1000;
```
【训练2】  彻底删除manager表的内容。

执行以下的命令：
```sql
TRUNCATE TABLE manager;
```
DELETE命令进行的删除可以撤销，但TRUNCATE命令进行的删除不可撤销。

注意：TRUNCATE TABLE命令用来删除表的全部数据而不是删除表，表依旧存在。


## 数据库事务 

## 数据库事务的概念

两次连续成功的COMMIT或ROLLBACK之间的操作，称为一个事务。在一个事务内，数据的修改一起提交或撤销，如果发生故障或系统错误，整个事务也会自动撤销。

比如，我们去银行转账，操作可以分为下面两个环节：
(1) 从第一个账户划出款项。
(2) 将款项存入第二个账户。

在这个过程中，两个环节是关联的。第一个账户划出款项必须保证正确的存入第二个账户，如果第二个环节没有完成，整个的过程都应该取消，否则就会发生丢失款项的问题。

 整个交易过程，可以看作是一个事物，成功则全部成功，失败则需要全部撤消，这样可以避免当操作的中间环节出现问题时，产生数据不一致的问题。

## 数据库事务的应用


数据库事务处理可分为隐式和显式两种。显式事务操作通过命令实现，隐式事务由系统自动完成提交或撤销(回退)工作，无需用户的干预。

隐式提交的情况包括：当用户正常退出SQL*Plus或执行CREATE、DROP、GRANT、REVOKE等命令时会发生事务的自动提交。

系统的环境变量AUTOCOMMIT设置为ON(默认状态为OFF)

SET AUTOCOMMIT ON/OFF
	
隐式回退的情况包括：当异常结束SQL*Plus或系统故障发生时，会发生事务的自动回退。


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle15.png "oracle15")<br/>


【训练1】  <span id="inline-yellow">学习使用COMMIT和ROLLBACK。</span>
```sql
--步骤1：执行以下命令，提交尚未提交的操作：
COMMIT;
--步骤2：修改雇员SCOTT的工资：
UPDATE emp SET sal=sal+100 WHERE empno=7788;
SELECT ename,sal FROM emp WHERE empno=7788;
--步骤3：假定修改操作后发现增加的工资应该为1000而不是100，为了取消刚做的操作，可以执行以下命令：
ROLLBACK;
--显示回退后SCOTT的工资恢复为3000：
SELECT ename,sal FROM emp WHERE empno=7788;
--步骤4：重新修改雇员SCOTT的工资，工资在原有基础上增加1000：
UPDATE emp SET sal=sal+1000 WHERE empno=7788;
--显示修改后SCOTT的工资：
SELECT ename,sal FROM emp WHERE empno=7788;
--步骤5：经查看修改结果正确，提交所做的修改：
COMMIT;
```
注意：在事务执行过程中，随时可以预览数据的变化。
对于比较大的事务，可以使用SAVEPOINT命令在事务中间划分一些断点，用来作为回退点。


【训练2】  <span id="inline-yellow">学习使用SAVEPOINT命令。</span>
```sql
--步骤1：插入一个雇员：
INSERT INTO emp(empno, ename, job)
VALUES (3000, '小马','STUDENT');
--步骤2：插入保存点，检查点的名称为PA：
SAVEPOINT pa;
--步骤3：插入另一个雇员：
INSERT INTO emp(empno, ename, job)
VALUES (3001, '小黄','STUDENT');
--步骤4：回退到保存点PA，则后插入的小黄被取消，而小马仍然保留。
ROLLBACK TO　pa;
--步骤5: 提交所做的修改：
COMMIT;
```sql
【训练3】  观察数据的读一致性。

--步骤1：显示刚插入的雇员小马:
SELECT empno,ename FROM emp WHERE empno=3000;
--步骤2：删除雇员小马:
DELETE FROM emp WHERE empno=3000;
--步骤3：再次显示该雇员，显示结果为该雇员不存在：
SELECT empno,ename FROM emp WHERE empno=3000;
--步骤4：另外启动第2个SQL*Plus，并以SCOTT身份连接。执行以下命令，结果为该记录依旧存在。
SELECT empno,ename FROM emp WHERE empno=3000;
--步骤5：在第1个SQL*Plus中提交删除：
COMMIT;
--步骤6：在第2个SQL*Plus中再次显示该雇员，显示结果与步骤3的结果一致:
SELECT empno,ename FROM emp WHERE empno=3000;
```
说明：在以上训练中，当第1个SQL*Plus会话删除小马后，第2个SQL*Plus会话仍然可以看到该雇员，直到第1个SQL*Plus会话提交该删除操作后，两个会话看到的才是一致的数据。

## 表的锁定【<span id="inline-green">了解</span>】

## 锁的概念

锁出现在数据共享的场合，用来保证数据的一致性。当多个会话同时修改一个表时，需要对数据进行相应的锁定。
锁有“只读锁”、“排它锁”，“共享排它锁”等多种类型，而且每种类型又有“行级锁”(一次锁住一条记录)，“页级锁”(一次锁住一页，即数据库中存储记录的最小可分配单元)，“表级锁”(锁住整个表)。 

若为“行级排它锁”，则除被锁住的行外，该表中其他行均可被其他的用户进行修改(Update)或删除(delete)。若为“表级排它锁”，则所有其他用户只能对该表进行查询(select)操作，而无法对其中的任何记录进行修改或删除。当程序对所做的修改进行提交(commit)或回滚(rollback)后，锁住的资源便会得到释放，从而允许其他用户进行操作。 

如果两个事务，分别锁定一部分数据，而都在等待对方释放锁才能完成事务操作，这种情况下就会发生死锁。

## 隐式锁和显式锁

在Oracle数据库中，修改数据操作时需要一个隐式的独占锁，以锁定修改的行，直到修改被提交或撤销为止。如果一个会话锁定了数据，那么第二个会话要想对数据进行修改，只能等到第一个会话对修改使用COMMIT命令进行提交或使用ROLLBACK命令进行回滚撤销后，才开始执行。因此应养成一个良好的习惯：执行修改操作后，要尽早地提交或撤销，以免影响其他会话对数据的修改。


【训练1】  对emp表的SCOTT雇员记录进行修改，测试隐式锁。
```sql
步骤1：启动第一个SQL*Plus，以SCOTT账户登录数据库(第一个会话)，修改SCOTT记录，隐式加锁。

UPDATE emp SET sal=3500 where empno=7788;

步骤2：启动第二个SQL*Plus，以SCOTT账户登录数据库(第二个会话)，进行记录修改操作。

UPDATE emp SET sal=4000 where empno=7788;

步骤3：对第一个会话进行解锁操作：

COMMIT;

步骤4：查看第二个会话，此时有输出结果：

步骤5：提交第二个会话，防止长时间锁定。
```

- 表的显式锁定

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle16.png "oracle16")<br/>


##  锁定行 

【训练1】  对emp表的部门10的雇员记录加显式锁，并测试。
```sql
步骤1：对部门10加显式锁：

SELECT empno,ename,job,sal FROM emp WHERE deptno=10 FOR UPDATE;

步骤2：启动第二个SQL*Plus(第二个会话)，以SCOTT账户登录数据库，对部门10的雇员CLARK进行修改操作。

UPDATE emp SET sal=sal+100 where empno=7782;

步骤3：在第一个会话进行解锁操作：

COMMIT;

步骤4：查看第二个会话，有输出结果：
```
## 锁定表

LOCK语句用于对整张表进行锁定。
对表的锁定可以是共享(SHARE)或独占(EXCLUSIVE)模式。共享模式下，其他会话可以加共享锁，但不能加独占锁。在独占模式下，其他会话不能加共享或独占锁。

【训练1】  对emp表添加独占锁。
```sql
步骤1：对emp表加独占锁：

LOCK TABLE emp IN EXCLUSIVE MODE;

步骤2：对表进行解锁操作：

COMMIT;
```



