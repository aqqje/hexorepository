---
title: oracle学习十三
date: 2017-12-10 19:21:27
comments: ture
categories:
	- 数据库
tags:
	- oracle
---

{% note info %}
![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
- 此文章是oracle学习系例第十三章，主要是用来复习和巩固在课堂学习的知识！

{% endnote %}
{% note danger %}
<!-- more -->
{% endnote %}

--------------------------


{% cq %}
## 笔记

## IF语句

1、IF-THEN-END IF形式

```sql
   IF 条件 then 
       语句集;
   END IF;
```


2、IF-THEN-ELSE-END IF形式

```sql
   IF 条件 then 
       语句集1;
   ElSE
       语句集2
   END IF;
```


3．IF-THEN-ELSIF-ELSE-END IF形式
   
```sql
IF 条件1 THEN
	语句集1;
   ELSIF 条件2 THEN
	语句集2;
   ELSIF 条件3 THEN
	语句集3;
   ...
   ELSE
	语句集n;
   END IF;
```


## CASE语句

1．基本CASE结构
   
```sql
   CASE 变量或表达式
   When 值1 then 结果1;
   When 值2 then 结果2;
   When 值3 then 结果3;
   ...
   ELSE 结果n;
   END CASE;
```


2.搜索CASE结构

```sql
   CASE 
   When 条件1 then 结果1;
   When 条件2 then 结果2;
   When 条件3 then 结果3;
   ...
   ELSE 结果n;
   END CASE;
```


## 循环

1．基本LOOP循环

```sql
   loop
      语句集;
   exit when 条件
      语句集;
   end loop;
```


2.FOR LOOP循环

- FOR循环是固定次数循环，格式如下：

```sql		
FOR 控制变量 in [REVERSE] 下限..上限 
		
     LOOP 
 		 
	语句集;
  		
END LOOP;
```

注：循环控制变量是隐含定义的，不需要声明。
		
下限和上限用于指明循环次数。正常情况下循环控制变量的取值由下限到上限递增，REVERSE关键字表示循环控制变量的取值由上限到下限递减。

3．WHILE LOOP循环   

```sql
   while 条件 loop
       语句集;
   end loop;   
```


## 游标

## 游标概念

   游标是SQL的一个内存工作区，由系统或用户以变量的形式定义。游标的作用就是用于临时存储从数据库中提取的数据块。在某些情况下，需要把数据从存放在磁盘的表中调到计算机内存中进行处理，最后将处理结果显示出来或最终写回数据库。这样数据处理的速度才会提高，否则频繁的磁盘数据交换会降低效率。
   
游标有两种类型：显式游标和隐式游标。

在前述程序中用到的SELECT...INTO...查询语句，一次只能从数据库中     提取一行数据，系统都会使用一个隐式游标。
              
显式游标对应一个返回结果为多行多列的SELECT语句。
		
游标一旦打开，数据就从数据库中传送到游标变量中，然后应用程序再从游标变量中分解出需要的数据，并进行处理。

## 隐式游标属性

隐式游标的属性|返回值类型|意    义
-|-|-
SQL%ROWCOUNT|整型|代表DML语句成功执行的数据行数
SQL%FOUND|布尔型|值为TRUE代表插入、删除、更新或单行查询操作成功
SQL%NOTFOUND|布尔型|与SQL%FOUND属性返回值相反
SQL%ISOPEN|布尔型|DML执行过程中为真，结束后为假


如：使用隐式游标的属性，判断对雇员工资的修改是否成功。
		
步骤1：输入和运行以下程序：

```sql		
SET SERVEROUTPUT ON 
		
BEGIN
  		
UPDATE emp SET sal=sal+100 WHERE empno=1234;
 		 
IF SQL%FOUND THEN 
   		
DBMS_OUTPUT.PUT_LINE('成功修改雇员工资！');
   		
COMMIT; 
  		
ELSEDBMS_OUTPUT.PUT_LINE('修改雇员工资失败！');
 		 
END IF; 
		
END;
```

## 显式游标

游标的使用分成以下4个步骤。
a．声明游标
		
在DECLEAR部分按以下格式声明游标：

```sql		
CURSOR 游标名[(参数1 数据类型[，参数2 数据类型...])]
		 
IS SELECT语句;
```
		
参数是可选部分，所定义的参数可以出现在SELECT语句的WHERE子句中。如果定义了参数，则必须在打开游标时传递相应的实际参数。

b.打开游标		在可执行部分，按以下格式打开游标：

```sql		
OPEN 游标名[(实际参数1[，实际参数2...])];
```

		
打开游标时，SELECT语句的查询结果就被传送到了游标工作区。

c.提取数据
		
在可执行部分，按以下格式将游标工作区中的数据取到变量中。提取操作必须在打开游标之后进行。

```sql		
FETCH 游标名 INTO 变量名1[，变量名2...];
		
或
		
FETCH 游标名 INTO 记录变量;
```

		
游标打开后有一个指针指向数据区，FETCH语句一次返回指针所指的一行数据，要返回多行需重复执行，可以使用循环语句来实现。控制循环可以通过判断游标的属性来进行。
  定义记录变量的方法如下：

```sql		
变量名 表名|游标名%ROWTYPE；
```


d.关闭游标
	
```sql	
CLOSE 游标名;
```

	
显式游标打开后，必须显式地关闭。游标一旦关闭，游标占用的资源就被释放，游标变成无效，必须重新打开才能使用。

【例1】 用游标提取emp表中7788雇员的名称和职务。

```sql
SET SERVEROUTPUT ON
		
DECLARE	
 		
    v_ename VARCHAR2(10);
 		
    v_job VARCHAR2(10);
 		
    CURSOR emp_cursor IS 
 SELECT ename,job FROM emp WHERE empno=7788;
BEGIN
 	
    OPEN emp_cursor;
  	
    FETCH emp_cursor INTO v_ename,v_job;
  		
    DBMS_OUTPUT.PUT_LINE(v_ename||','||v_job);
  		
    CLOSE emp_cursor;
		
END;
```

 
【例2】  用游标提取emp表中7788雇员的姓名、职务和工资。

```sql	
SET SERVEROUTPUT ON
		
DECLARE
 		 
	CURSOR emp_cursor IS  SELECT ename,job,sal FROM emp WHERE empno=7788;
 		
	emp_record emp_cursor%ROWTYPE;
	    --用游标定义记录变量	
BEGIN 
	OPEN emp_cursor;	
   		
	FETCH emp_cursor INTO emp_record;
		  
	 DBMS_OUTPUT.PUT_LINE(emp_record.ename||','|| emp_record.job||','|| emp_record.sal);
 		
	 CLOSE emp_cursor;
		
END;
```

【例3】  显示工资最高的前3名雇员的名称和工资。

```sql	
SET SERVEROUTPUT ON
		
DECLARE
 		
	 V_ename VARCHAR2(10);
  		
	V_sal NUMBER(5);
  		
	CURSOR emp_cursor IS  SELECT ename,sal FROM emp ORDER BY sal DESC;
		
BEGIN
 		
	 OPEN emp_cursor;
 		
	 FOR I IN 1..3 LOOP
		  
 FETCH emp_cursor INTO v_ename,v_sal;
 DBMS_OUTPUT.PUT_LINE(v_ename||','||v_sal);
       
  END LOOP;
      
   CLOSE emp_cursor;

END;
```


## 游标循环（重点）


方法一：使用特殊的FOR循环形式显示全部雇员的编号和名称(省略掉定义记录变量、打开游标、提取数据、关闭游标)。

```sql
SET SERVEROUTPUT ON

DECLARE
  
 	CURSOR emp_cursor IS 
 SELECT empno, ename FROM emp;
BEGIN	
	FOR Emp_record IN emp_cursor LOOP   
   
DBMS_OUTPUT.PUT_LINE(Emp_record.empno|| Emp_record.ename);
	
	END LOOP;
	
END;
```

方法二：最简单方式

```sql
SET SERVEROUTPUT ON
 
BEGIN	 FOR re IN (SELECT ename FROM EMP)  LOOP
  
DBMS_OUTPUT.PUT_LINE(re.ename)
 
	END LOOP;

END;
```

## 利用游标属性做循环条件

【训练1】  使用游标的属性练习。

```sql
SET SERVEROUTPUT ON

DECLARE
 
	 V_ename VARCHAR2(10);
 
	 CURSOR emp_cursor IS 
  SELECT ename FROM emp;

BEGIN	 OPEN emp_cursor;	 IF emp_cursor%ISOPEN THEN
	LOOP
   
	FETCH emp_cursor INTO v_ename;	        EXIT WHEN emp_cursor%NOTFOUND;
  
	 DBMS_OUTPUT.PUT_LINE(to_char(emp_cursor%ROWCOUNT)||'-'||v_ename);	        END LOOP;
 
      ELSE
 
	  DBMS_OUTPUT.PUT_LINE('用户信息：游标没有打开！');
 
	 END IF;         CLOSE  emp_cursor;END;
```



## PL/SQL的基本构成

- PL/SQL语言是SQL语言的扩展，具有为程序开发而设计的特性，如数据封装、异常处理、面向对象等特性。PL/SQL是嵌入到Oracle服务器和开发工具中的，所以具有很高的执行效率和同Oracle数据库的完美结合。在PL/SQL模块中可以使用查询语句和数据操纵语句(即进行DML操作)，这样就可以编写具有数据库事务处理功能的模块。

- 至于数据定义(DDL)和数据控制(DCL)命令的处理，需要通过Oracle提供的特殊的DBMS_SQL包来进行。PL/SQL还可以用来编写过程、函数、包及数据库触发器。过程和函数也称为子程序，在定义时要给出相应的过程名和函数名。它们可以存储在数据库中成为存储过程和存储函数，并可以由程序来调用，它们在结构上同程序模块类似。

- PL/SQL过程化结构的特点是：可将逻辑上相关的语句组织在一个程序块内；通过嵌入或调用子块，构造功能强大的程序；可将一个复杂的问题分解成为一组便于管理、定义和实现的小块。

## PL/SQL块结构和基本语法要求

PL/SQL程序的基本单元是块(BLOCK)，块就是实现一定功能的逻辑模块。一个PL/SQL程序由一个或多个块组成。块有固定的结构，也可以嵌套。一个块可以包括三个部分，每个部分由一个关键字标识。

块中各部分的作用解释如下：


		(1)  DECLARE：声明部分标志。
		(2)  BEGIN：可执行部分标志。
		(3)  EXCEPTION：异常处理部分标志。
		(4)  END；：程序结束标志。


## 输出

- 使用函数DBMS_OUTPUT.PUT_LINE显示输出结果。

- DBMS_OUTPUT是Oracle提供的包，该包有如下三个用于输出的函数，用于显示PL/SQL程序模块的输出信息。

第一种形式：

```sql
	DBMS_OUTPUT.PUT(字符串表达式)；
```
	
用于输出字符串，但不换行，括号中的参数是要输出的字符串表达式。
		
第二种形式：

```sql
	DBMS_OUTPUT.PUT_LINE(字符串表达式)；
```
		
用于输出一行字符串信息，并换行，括号中的参数是要输出的字符串表达式。 


第三种形式：

```sql
	DBMS_OUTPUT.NEW_LINE；
```
		
用来输出一个换行，没有参数。调用函数时，在包名后面用一个点“.”和函数名分隔，表示隶属关系。 

要使用该方法显示输出数据，在SQL*Plus环境下要先执行一次如下的环境设置命令：

```sql
	SET SERVEROUTPUT ON [SIZE n]
```

		
用来打开DBMS_OUTPUT.PUT_LINE函数的屏幕输出功能，系统默认状态是OFF。其中，n表示输出缓冲区的大小。n的范围在2000～1 000 000之间，默认为2000。如果输出内容较多，需要使用SIZE n来设置较大的输出缓冲区。


在PL/SQL模块中可以使用查询语句和数据操纵语句(即进行DML操作)，所以PL/SQL程序是同SQL语言紧密结合在一起的。在PL/SQL程序中，最常见的是使用SELECT语句从数据库中获取信息，同直接执行SELECT语句不同，在程序中的SELECT语句总是和INTO相配合，INTO后跟用于接收查询结果的变量，形式如下：

```sql
	SELECT 列名1，列名2... INTO 变量1，变量2... FROM 表名 WHERE 条件；
```

注意：接收查询结果的变量类型、顺序和个数同SELECT语句的字段的类型、顺序和个数应该完全一致。并且SELECT语句返回的数据必须是一行，否则将引发系统错误。当程序要接收返回的多行结果时，可以采用后面介绍的游标的方法。

使用INSERT、DELETE和UPDATE的语法没有变化，但在程序中要注意判断语句执行的状态，并使用COMMIT或ROLLBACK进行事务处理。
		

【训练1】  查询雇员编号为7788的雇员姓名和工资。
		
步骤1：用SCOTT账户登录SQL*Plus。
		
步骤2：在输入区输入以下程序：

```sql
 /*这是一个简单的示例程序*/
SET SERVEROUTPUT ON
DECLARE--定义部分标识
  v_name  VARCHAR2(10);	--定义字符串变量v_name
  v_sal   NUMBER(5);	--定义数值变量v_sal
BEGIN			--可执行部分标识
SELECT	 ename,sal 
  INTO v_name,v_sal 
  FROM	emp 
  WHERE empno=7788; 			
	--在程序中插入的SQL语句
  	DBMS_OUTPUT.PUT_LINE('7788号雇员是：'||v_name||'，工资为：'||to_char(v_sal));
	--输出雇员名和工资
	END;					--结束标识
```


步骤3：按执行按钮或F5快捷键执行程序。
		
以上程序的作用是，查询雇员编号为7788的雇员姓名和工资，然后显示输出。这种方法同直接在SQL环境下执行SELECT语句显示雇员的姓名和工资比较，程序变得更复杂。那么两者究竟有什么区别呢？SQL查询的方法，只限于SQL环境，并且输出的格式基本上是固定的。而程序通过把数据取到变量中，可以进行复杂的处理，完成SQL语句不能实现的功能，并通过多种方式输出。


“--”是注释符号，后边是程序的注释部分。该部分不编译执行，所以在输入程序时可以省略。/*......*/中间也是注释部分，同“--”注释方法不同，它可以跨越多行进行注释。
		
PL/SQL程序的可执行语句、SQL语句和END结束标识都要以分号结束。

## 数据类型

- 量的基本数据类型同SQL部分的字段数据类型相一致，但是也有不同

变量的数据类型:<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle17.png "oracle17")<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle18.png "oracle18")<br/>


LOB数据类型可以存储视频、音频或图片，支持随机访问，存储的数据可以位于数据库内或数据库外，具体有四种类型：BFILE、BLOB、CLOB、NCLOB。但是操纵大对象需要使用Oracle提供的DBMS_LOB包。


## 变量定义

- 变量定义:
		
变量的作用是用来存储数据，可以在过程语句中使用。
     
变量在声明部分可以进行初始化，即赋予初值。   
      
- 变量的命名规则:
   
变量名不要和在程序中引用的字段名相重，如果相重，变量名会被当作列名来使用。

- 变量的作用范围是在定义此变量的程序范围内，如果程序中包含子块，则变量在子块中也有效。但在子块中定义的变量，仅在定义变量的子块中有效，在主程序中无效。

【训练1】  变量的定义和初始化。

		
输入和运行以下程序：

```sql
	SET SERVEROUTPUT ON 
	DECLARE		--声明部分标识
	v_job		VARCHAR2(9);
	v_count	BINARY_INTEGER DEFAULT 0;
	v_total_sal	NUMBER(9,2) := 0;
	v_date		DATE := SYSDATE + 7;
	c_tax_rate	CONSTANT NUMBER(3,2) := 8.25;
	v_valid		BOOLEAN NOT NULL := TRUE;
	BEGIN
	v_job:='MANAGER';		
	--在程序中赋值
	DBMS_OUTPUT.PUT_LINE(v_job);	
	--输出变量v_job的值
	DBMS_OUTPUT.PUT_LINE(v_count);
	--输出变量v_count的值
	DBMS_OUTPUT.PUT_LINE(v_date);	
	--输出变量v_date的值	 	
	DBMS_OUTPUT.PUT_LINE(c_tax_rate);	
	--输出变量c_tax_rate的值
	END;
```

## 根据表的字段定义变量


变量的声明还可以根据数据库表的字段进行定义或根据已经定义的变量进行定义。
		
【训练2】  根据表的字段定义变量。
		
输入并执行以下程序：

```sql
	SET SERVEROUTPUT ON 
	DECLARE
	v_ename	emp.ename%TYPE;--根据字段定义变量
	BEGIN
	SELECT	ename		 
	INTO		v_ename  		 
	FROM		emp  		 
	WHERE	empno = 7788;
	DBMS_OUTPUT.PUT_LINE(v_ename);	
	--输出变量的值
	END;
```

说明：变量v_ename是根据表emp的ename字段定义的，两者的数据类型总是一致的。
		
如果我们根据数据库的字段定义了某一变量，后来数据库的字段数据类型又进行了修改，那么程序中的该变量的定义也自动使用新的数据类型。使用该种变量定义方法，变量的数据类型和大小是在编译执行时决定的，这为书写和维护程序提供了很大的便利。


## 结合变量的定义和使用


我们还可以定义SQL*Plus环境下使用的变量，称为结合变量。结合变量也可以在程序中使用，该变量是在整个SQL*Plus环境下有效的变量，在退出SQL*Plus之前始终有效，所以可以使用该变量在不同的程序之间传递信息。结合变量不是由程序定义的，而是使用系统命令VARIABLE定义的。在SQL*Plus环境下显示该变量要用系统的PRINT命令。


【训练3】  定义并使用结合变量。
		
步骤1：输入和执行下列命令，定义结合变量g_ename：

```sql
	VARIABLE  g_ename VARCHAR2(100) 
```

		
步骤2：输入和执行下列程序：


```sql
   SET SERVEROUTPUT ON 
	BEGIN
  	 :g_ename:=:g_ename|| 'Hello~ ';			--在程序中使用结合变量
  	 DBMS_OUTPUT.PUT_LINE(:g_ename);	
	--输出结合变量的值
	END;
```

		
步骤3：重新执行程序。
		
输出结果：
       ?

步骤4：程序结束后用命令显示结合变量的内容：

```sql 
	PRINT g_ename 
```

说明：g_ename为结合变量，可以在程序中引用或赋值，引用时在结合变量前面要加上“∶”。在程序结束后该变量的值仍然存在，其他程序可以继续引用。


## 记录变量的定义


还可以根据表或视图的一个记录中的所有字段定义变量，称为记录变量。记录变量包含若干个字段，在结构上同表的一个记录相同，定义方法是在表名后跟%ROWTYPE。记录变量的字段名就是表的字段名，数据类型也一致。
		
【训练4】  根据表定义记录变量。
		
输入并执行如下程序：

```sql
	SET SERVEROUTPUT ON 
	DECLARE
	emp_record		emp%ROWTYPE;--定义记录变量
	BEGIN
 	 SELECT * INTO	emp_record
 	 FROM			emp
 	 WHERE		mpno = 7788;--取出一条记录
  	DBMS_OUTPUT.PUT_LINE(emp_record.ename);	--输出记录变量的某个字段 
	END;
```


## TABLE类型变量

在PL/SQL中可以定义TABLE类型的变量。 TABLE数据类型用来存储可变长度的一维数组数据，即数组中的数据动态地增长。要定义TABLE变量，需要先定义TABLE数据类型。通过使用下标来引用TABLE变量的元素。




【训练5】  定义和使用TABLE变量：

```sql
	SET SERVEROUTPUT ON 
	DECLARE
 	TYPE type_table IS TABLE OF VARCHAR2(10) INDEX BY BINARY_INTEGER;  --类型说明
	 v_t	  type_table;     --定义TABLE变量
	BEGIN
	 v_t(1):='MONDAY';
	 v_t(2):='TUESDAY';
 	v_t(3):='WEDNESDAY';
	 v_t(4):='THURSDAY';
 	v_t(5):='FRIDAY';
	DBMS_OUTPUT.PUT_LINE(v_t(3));  --输出变量的内容
	END;
```

说明：本例定义了长度为10的字符型TABLE变量，通过赋值语句为前五个元素赋值，最后输出第三个元素。


## 运算符和函数

PL/SQL常见的运算符和函数包括以下方面(这里只做简单的总结，可参见SQL部分的例子)：

		算术运算：加(+)、减(-)、乘(*)、除(/)、指数(**)。
		关系运算：小于(<)、小于等于(<=)、大于(>)、大于等于(>=)、等于(=)、不等于(!=或<>)。
		字符运算：连接(||)。
		逻辑运算：与(AND)、或(OR)、非(NOT)。
		
特殊运算:<br/>

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle18.png "oracle18")<br/>

IS NULL或IS NOT NULL用来判断运算对象的值是否为空，不能用“=”去判断。另外，对空值的运算也必须注意，对空值的算术和比较运算的结果都是空，但对空值可以进行连接运算，结果是另外一部分的字符串。例如：


```sql
	NULL+5的结果为NULL。
	NULL>5的结果为NULL。
	NULL|| 'ABC' 的结果为'ABC'

```
	

## 结构控制语句


## 分支结构


- 分支结构是最基本的程序结构，分支结构由IF语句实现。


## IF-THEN-END IF形式

		
这是最简单的IF结构，练习如下： 
		
【训练1】  如果温度大于30℃，则显示“温度偏高”。
		
输入并执行以下程序：


```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 V_temprature		NUMBER(5):=32;
 	 V_result         	BOOLEAN:=false;
	BEGIN
	  V_result:= v_temprature >30;
 	 IF V_result THEN 
   		DBMS_OUTPUT.PUT_LINE('温度'|| V_temprature ||'度,偏高');
 	 END IF; 
	END;
	试修改温度的初值为25℃，重新执行，观察结果。
```


## IF-THEN-ELSE-END IF形式

这种形式的练习如下：
		
【训练2】  根据性别，显示尊称。
		
输入并执行以下程序：


```sql
	SET SERVEROUTPUT ON
	DECLARE
	 v_sex	VARCHAR2(2);
  	v_titil  	VARCHAR2(10);
	BEGIN
  v_sex:='男';
  IF v_sex ='男' THEN
    v_titil:='先生';
  ELSE
    v_titil:='女士';
  END IF; 
  DBMS_OUTPUT.PUT_LINE(v_titil||'您好！');
	END;
```

	
说明：该程序根据性别显示尊称和问候，无论性别的值为何，总会有显示结果输出。如果V_sex的值不是‘男’和‘女’，那么输出结果会是什么？

## IF-THEN-ELSIF-ELSE-END IF形式
		
这种形式的练习如下：
		
【训练3】  根据雇员工资分级显示税金。
	
输入并运行以下程序：


```sql
SET SERVEROUTPUT ON
DECLARE
  v_sal  NUMBER(5);
  v_tax  NUMBER(5,2);
BEGIN
  SELECT sal INTO v_sal
  FROM emp
  WHERE empno=7788;
IF v_sal >=3000 THEN 
  		V_tax:= v_sal*0.08;--税率8%
  ELSIF v_sal>=1500 THEN
  	 V_tax:= v_sal*0.06; --税率6%
  ELSE
 	 V_tax:= v_sal*0.04; --税率4%
 	 END IF;
  	DBMS_OUTPUT.PUT_LINE('应缴税金:'||V_tax);
	END;
```


## 选择结构

CASE语句适用于分情况的多分支处理，可有以下三种用法。
1．基本CASE结构

2．表达式结构CASE语句<br/>
在Oracle中，CASE结构还能以赋值表达式的形式出现，它根据选择变量的值求得不同的结果。

3．搜索CASE结构<br/>
Oracle还提供了一种搜索CASE结构，它没有选择变量，直接判断条件表达式的值，根据条件表达式决定转向。
		

		

## 基本CASE结构

训练1】  使用CASE结构实现职务转换。
		
输入并执行程序：


```sql
SET SERVEROUTPUT ON
DECLARE
v_job  VARCHAR2(10);
BEGIN
SELECT job INTO v_job
FROM emp
WHERE empno=7788;
CASE v_job
WHEN 'PRESIDENT' THEN 
 DBMS_OUTPUT.PUT_LINE('雇员职务:总裁');
WHEN 'MANAGER' THEN  
 DBMS_OUTPUT.PUT_LINE('雇员职务:经理');
WHEN 'SALESMAN' THEN  
 DBMS_OUTPUT.PUT_LINE('雇员职务:推销员');
WHEN 'ANALYST' THEN  
 DBMS_OUTPUT.PUT_LINE('雇员职务:系统分析员');
WHEN 'CLERK' THEN  
 DBMS_OUTPUT.PUT_LINE('雇员职务:职员');
ELSE  
 DBMS_OUTPUT.PUT_LINE('雇员职务:未知');
END CASE;
END;
```
		
说明：以上实例检索雇员7788的职务，通过CASE结构转换成中文输出。


## 表达式结构CASE语句

【训练2】  使用CASE的表达式结构。


```sql
	SET SERVEROUTPUT ON
	DECLARE
	  v_grade	VARCHAR2(10);
		 v_result	VARCHAR2(10);
	BEGIN
	 v_grade:='B';
	 v_result:=CASE v_grade
		  WHEN 'A' THEN '优'
	WHEN 'B' THEN '良'
	  	WHEN 'C' THEN '中'
	 	 WHEN 'D' THEN '差'
		ELSE '未知'
		END;
 		DBMS_OUTPUT.PUT_LINE('评价等级:'||V_result);
		END;
```

说明：该CASE表达式通过判断变量v_grade的值，对变量V_result赋予不同的值。



## 搜索CASE结构


【训练3】  使用CASE的搜索结构。


```sql
	SET SERVEROUTPUT ON
	DECLARE
	   v_sal	NUMBER(5);
	BEGIN
	   SELECT sal INTO v_sal FROM emp 
		 WHERE empno=7788;
	CASE 
 		WHEN v_sal>=3000 THEN 
   	DBMS_OUTPUT.PUT_LINE('工资等级：高');
		 WHEN v_sal>=1500 THEN
	DBMS_OUTPUT.PUT_LINE('工资等级：中');
	   ELSE
   DBMS_OUTPUT.PUT_LINE('工资等级：低');
	END CASE;
	END;
```


## 循环结构

循环结构是最重要的程序控制结构，用来控制反复执行一段程序。比如我们要进行累加，则可以通过适当的循环程序实现。PL/SQL循环结构可划分为以下3种：
		
1.基本LOOP循环。<br/>
EXIT用于在循环过程中退出循环，WHEN用于定义EXIT的退出条件。如果没有WHEN条件，遇到EXIT语句则无条件退出循环。

2.FOR LOOP循环。<br/>
 FOR循环是固定次数循环，格式如下：


```sql
FOR 控制变量 in [REVERSE] 下限..上限 
		LOOP 
 		 语句1;
  		语句2;
   		END LOOP;
```

循环控制变量是隐含定义的，不需要声明。

下限和上限用于指明循环次数。正常情况下循环控制变量的取值由下限到上限递增，REVERSE关键字表示循环控制变量的取值由上限到下限递减。
		

3.WHILE LOOP循环。

多重循环

## 基本LOOP循环

【训练1】　求：1２+3２+5２+...+15２ 的值。
		
输入并执行以下程序：


```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 v_total		NUMBER(5):=0;
  	 v_count		NUMBER(5):=1;
	BEGIN
	LOOP
	    v_total:=v_total+v_count**2;
    	EXIT WHEN v_count=15;--条件退出
	v_count:=v_count+2;
  		END LOOP;
 	 DBMS_OUTPUT.PUT_LINE(v_total);
	END;
```


说明：基本循环一定要使用EXIT退出，否则就会成为死循环。

【训练2】  用FOR循环输出图形。


```sql
	SET SERVEROUTPUT ON
	BEGIN
	FOR I IN 1..8 
	LOOP   DBMS_OUTPUT.PUT_LINE(to_char(i)||rpad('*',I,'*'));
	END LOOP;
	END;
```


说明：该程序在循环中使用了循环控制变量I，该变量隐含定义。在每次循环中根据循环控制变量I的值，使用RPAD函数控制显示相应个数的“*”。

【练习2】为以上程序增加REVERSE关键字，观察执行结果。

【训练3】  输出一个空心三角形。


```sql
	 BEGIN 
	FOR I IN 1..9
	LOOP 
	 IF I=1 OR I=9 THEN
 	 DBMS_OUTPUT.PUT_LINE(to_char(I)||rpad(' ',12-I,' ')||rpad('*',2*i-1,'*')); 
	ELSE
  DBMS_OUTPUT.PUT_LINE(to_char(I)||rpad(' ',12-I,' ')||'*'||rpad(' ',I*2-3,' ')||'*'); 
 	END IF;
	END LOOP; 
	END;
```


说明：该实例采用循环和IF结构相结合，对第1行和第9行(I=1 OR I=9)执行同样的输出语句，其他行执行另外的输出语句。


## WHILE LOOP循环
		
【训练3】　使用WHILE 循环向emp表连续插入5个记录。
		
步骤1：执行下面的程序：


```sql
SET SERVEROUTPUT ON
DECLARE
v_count	NUMBER(2) := 1;
BEGIN
  WHILE v_count <6 LOOP
    INSERT INTO emp(empno, ename)
    VALUES (5000+v_count, '临时');
v_count := v_count + 1;
  END LOOP;
  COMMIT;
END;
```


步骤2：显示插入的记录：

```sql
	SELECT empno,ename FROM emp WHERE ename='临时';
```

步骤3：删除插入的记录：

```sql
	DELETE FROM emp WHERE ename='临时';
	COMMIT;
```

## 多重循环(了解)


循环可以嵌套，以下是一个二重循环的练习。
		
【训练4】　使用二重循环求1！+2！+...+10！的值。
		
步骤1：第1种算法：

```sql
SET SERVEROUTPUT ON
DECLARE
  v_total	NUMBER(8):=0;
  v_ni	NUMBER(8):=0;
  J		NUMBER(5);
BEGIN
FOR I IN 1..10
  LOOP
    J:=1;
	  v_ni:=1;
    WHILE J<=I
    LOOP
      v_ni:= v_ni*J;
      J:=J+1;
    END LOOP;--内循环求n!
v_total:=v_total+v_ni;
  END LOOP;--外循环求总和
  DBMS_OUTPUT.PUT_LINE(v_total);
END;
```

步骤2：第2种算法：

```sql
SET SERVEROUTPUT ON
DECLARE
  v_total		NUMBER(8):=0;
  v_ni		NUMBER(8):=1;
BEGIN
  FOR I IN 1..10
  LOOP
    v_ni:= v_ni*I;	--求n!
    v_total:= v_total+v_ni;
  END LOOP;		--循环求总和
  DBMS_OUTPUT.PUT_LINE(v_total);
END;
```



## 阶段训练

训练1】  插入雇员，如果雇员已经存在，则输出提示信息。

```sql
	SET SERVEROUTPUT ON 
	DECLARE
  	v_empno NUMBER(5):=7788;
  	v_num VARCHAR2(10); 
  	i NUMBER(3):=0;
	BEGIN		
 	 SELECT count(*) INTO v_num FROM SCOTT.emp WHERE empno=v_empno;
	IF v_num=1 THEN
  DBMS_OUTPUT.PUT_LINE('雇员'||v_empno||'已经存在！');
  ELSE
  INSERT INTO emp(empno,ename) VALUES(v_empno,'TOM');
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('成功插入新雇员！');
	END IF; 
	END;
```

说明：在本程序中，使用了一个技巧来判断一个雇员是否存在。如果一个雇员不存在，那么使用SELECT...INTO来获取雇员信息就会失败，因为SELECT...INTO形式要求查询必须返回一行。但如果使用COUNT统计查询，返回满足条件的雇员人数，则该查询总是返回一行，所以任何情况都不会失败。COUNT返回的统计人数为0说明雇员不存在，返回的统计人数为1说明雇员存在，返回的统计人数大于1说明有多个满足条件的雇员存在。本例在雇员不存在时进行插入操作，如果雇员已经存在则不进行插入。

【训练2】  输出由符号“*”构成的正弦曲线的一个周期(0～360°)。

```sql
SET SERVEROUTPUT ON SIZE 10000
SET LINESIZE 100
SET PAGESIZE 100
DECLARE
  v_a	NUMBER(8,3);
  v_p 	NUMBER(8,3);
BEGIN
  FOR I IN 1..18

	LOOP
    v_a:=I*20*3.14159/180;--生成角度，并转换为弧度
    v_p:=SIN(v_a)*20+25;--求SIN函数值，20为放大倍数，25为水平位移
   	DBMS_OUTPUT.PUT_LINE(to_char(i)||lpad('*',v_p,' '));--输出记录变量的某个字段
  	END LOOP;
	END;
```

说明：在本程序中使用到了固定次数的循环以及SIN和LPAD函数，通过正确地设置步长、幅度和位移的参数，在屏幕上可正确地显示图形。



## 游标的概念


游标是SQL的一个内存工作区，由系统或用户以变量的形式定义。游标的作用就是用于临时存储从数据库中提取的数据块。在某些情况下，需要把数据从存放在磁盘的表中调到计算机内存中进行处理，最后将处理结果显示出来或最终写回数据库。这样数据处理的速度才会提高，否则频繁的磁盘数据交换会降低效率。

游标有两种类型：显式游标和隐式游标。

 在前述程序中用到的SELECT...INTO...查询语句，一次只能从数据库中     提取一行数据，系统都会使用一个隐式游标。
               
显式游标对应一个返回结果为多行多列的SELECT语句。
		
游标一旦打开，数据就从数据库中传送到游标变量中，然后应用程序再从游标变量中分解出需要的数据，并进行处理。

## 隐式游标

如前所述，DML操作和单行SELECT语句会使用隐式游标，它们是：

		* 插入操作：INSERT。
		* 更新操作：UPDATE。
		* 删除操作：DELETE。
		* 单行查询操作：SELECT ... INTO ...。


当系统使用一个隐式游标时，可以通过隐式游标的属性来了解操作的状态和结果，进而控制程序的流程。
      
通过SQL游标名总是只能访问前一个DML操作或单行SELECT操作的游标属性。所以通常在刚刚执行完操作之后，立即使用SQL游标名来访问属性。游标的属性有四种

隐式游标属性：

![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle20.png "oracle20")

【训练1】　使用隐式游标的属性，判断对雇员工资的修改是否成功。

步骤1：输入和运行以下程序：

```sql
	SET SERVEROUTPUT ON 
	BEGIN
  	UPDATE emp SET sal=sal+100 WHERE empno=1234;
 	 IF SQL%FOUND THEN 
   	DBMS_OUTPUT.PUT_LINE('成功修改雇员工资！');
   	COMMIT; 
  	ELSE
	DBMS_OUTPUT.PUT_LINE('修改雇员工资失败！');
 	 END IF; 
	END;
```
		
		
步骤2：将雇员编号1234改为7788，重新执行以上程序：
		
		
说明：本例中，通过SQL%FOUND属性判断修改是否成功，并给出相应信息。



## 显式游标

## 游标的定义和操作

游标的使用分成以下4个步骤。
		
1．声明游标
		
在DECLEAR部分按以下格式声明游标：


```sql	
CURSOR 游标名[(参数1 数据类型[，参数2 数据类型...])]
	 IS SELECT语句;
```

参数是可选部分，所定义的参数可以出现在SELECT语句的WHERE子句中。如果定义了参数，则必须在打开游标时传递相应的实际参数。


语句的查询结果就被传送到了游标工作区。SELECT语句是对表或视图的查询语句，甚至也可以是联合查询。可以带WHERE条件、ORDER BY或GROUP BY等子句，但不能使用INTO子句。在SELECT语句中可以使用在定义游标之前定义的变量。

2．打开游标

在可执行部分，按以下格式打开游标：

```sql
	OPEN 游标名[(实际参数1[，实际参数2...])];
	打开游标时，SELECT
```


3．提取数据
		
在可执行部分，按以下格式将游标工作区中的数据取到变量中。提取操作必须在打开游标之后进行。

```sql
	FETCH 游标名 INTO 变量名1[，变量名2...];
	或
	FETCH 游标名 INTO 记录变量;
```

游标打开后有一个指针指向数据区，FETCH语句一次返回指针所指的一行数据，要返回多行需重复执行，可以使用循环语句来实现。控制循环可以通过判断游标的属性来进行。


下面对这两种格式进行说明：
		
第一种格式中的变量名是用来从游标中接收数据的变量，需要事先定义。变量的个数和类型应与SELECT语句中的字段变量的个数和类型一致。
		

第二种格式一次将一行数据取到记录变量中，需要使用%ROWTYPE事先定义记录变量，这种形式使用起来比较方便，不必分别定义和使用多个变量。

定义记录变量的方法如下：

```sql
	变量名 表名|游标名%ROWTYPE；
```

其中的表必须存在，游标名也必须先定义。


4．关闭游标

```sql
	CLOSE 游标名;
```

显式游标打开后，必须显式地关闭。游标一旦关闭，游标占用的资源就被释放，游标变成无效，必须重新打开才能使用。
		
以下是使用显式游标的一个简单练习。
		
【训练1】  用游标提取emp表中7788雇员的名称和职务。

```sql
	SET SERVEROUTPUT ON
	DECLARE	
 	 v_ename VARCHAR2(10);
 	 v_job VARCHAR2(10);
 		CURSOR emp_cursor IS 
 	SELECT ename,job FROM emp WHERE empno=7788;
	BEGIN
 	 OPEN emp_cursor;
  	FETCH emp_cursor INTO v_ename,v_job;
  	DBMS_OUTPUT.PUT_LINE(v_ename||','||v_job);
  	CLOSE emp_cursor;
	END;
```

说明：该程序通过定义游标emp_cursor，提取并显示雇员7788的名称和职务。

作为对以上例子的改进，在以下训练中采用了记录变量。

【训练2】  用游标提取emp表中7788雇员的姓名、职务和工资。

```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 CURSOR emp_cursor IS  SELECT ename,job,sal FROM emp WHERE empno=7788;
 	 emp_record emp_cursor%ROWTYPE;
	BEGIN
	OPEN emp_cursor;	
   		FETCH emp_cursor INTO emp_record;
	   DBMS_OUTPUT.PUT_LINE(emp_record.ename||','|| emp_record.job||','|| emp_record.sal);
 		 CLOSE emp_cursor;
		END;
```

说明：实例中使用记录变量来接收数据，记录变量由游标变量定义，需要出现在游标定义之后。
		
注意：可通过以下形式获得记录变量的内容：
		
记录变量名.字段名。


【训练3】  显示工资最高的前3名雇员的名称和工资。

```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 V_ename VARCHAR2(10);
  	V_sal NUMBER(5);
  	CURSOR emp_cursor IS  SELECT ename,sal FROM emp ORDER BY sal DESC;
	BEGIN
 	 OPEN emp_cursor;
 	 FOR I IN 1..3 LOOP
	   FETCH emp_cursor INTO v_ename,v_sal;
	DBMS_OUTPUT.PUT_LINE(v_ename||','||v_sal);
  END LOOP;
  CLOSE emp_cursor;
	END;
```
 		
说明：该程序在游标定义中使用了ORDER BY子句进行排序，并使用循环语句来提取多行数据。



##游标循环


【训练1】  使用特殊的FOR循环形式显示全部雇员的编号和名称。

```sql
SET SERVEROUTPUT ON
DECLARE
  CURSOR emp_cursor IS 
  SELECT empno, ename FROM emp;
BEGIN
FOR Emp_record IN emp_cursor LOOP   
    DBMS_OUTPUT.PUT_LINE(Emp_record.empno|| Emp_record.ename);
	END LOOP;
	END;
```

说明：可以看到该循环形式非常简单，隐含了记录变量的定义、游标的打开、提取和关闭过程。Emp_record为隐含定义的记录变量，循环的执行次数与游标取得的数据的行数相一致。

【训练2】  另一种形式的游标循环。

```sql
SET SERVEROUTPUT ON 
BEGIN
 FOR re IN (SELECT ename FROM EMP)  LOOP
  DBMS_OUTPUT.PUT_LINE(re.ename)
 END LOOP;
END;
```

说明：该种形式更为简单，省略了游标的定义，游标的SELECT查询语句在循环中直接出现。




## 显式游标属性


虽然可以使用前面的形式获得游标数据，但是在游标定义以后使用它的一些属性来进行结构控制是一种更为灵活的方法


标的属性:


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle20.png "oracle20")



可按照以下形式取得游标的属性：

```sql
	游标名%属性
```

要判断游标emp_cursor是否处于打开状态，可以使用属性emp_cursor%ISOPEN。如果游标已经打开，则返回值为“真”，否则为“假”。具体可参照以下的训练。

【训练1】  使用游标的属性练习。

```sql
SET SERVEROUTPUT ON
DECLARE
  V_ename VARCHAR2(10);
  CURSOR emp_cursor IS 
  SELECT ename FROM emp;
BEGIN
 OPEN emp_cursor;
 IF emp_cursor%ISOPEN THEN
 LOOP
   FETCH emp_cursor INTO v_ename;
   EXIT WHEN emp_cursor%NOTFOUND;
   DBMS_OUTPUT.PUT_LINE(to_char(emp_cursor%ROWCOUNT)||'-'||v_ename);
  END LOOP;
 ELSE
  DBMS_OUTPUT.PUT_LINE('用户信息：游标没有打开！');
 END IF;
 CLOSE  emp_cursor;
END;
```


 说明：本例使用emp_cursor%ISOPEN判断游标是否打开；使用emp_cursor%ROWCOUNT获得到目前为止FETCH语句返回的数据行数并输出；使用循环来获取数据，在循环体中使用FETCH语句；使用emp_cursor%NOTFOUND判断FETCH语句是否成功执行，当FETCH语句失败时说明数据已经取完，退出循环。
		
【练习1】去掉OPEN emp_cursor;语句，重新执行以上程序。


## 游标参数的传递


【训练1】  带参数的游标。

```sql
	SET SERVEROUTPUT ON
	DECLARE
  		V_empno NUMBER(5);
  		V_ename VARCHAR2(10);
    	CURSOR 	emp_cursor(p_deptno NUMBER, 	p_job VARCHAR2) IS
	    SELECT	empno, ename FROM emp
    	WHERE	deptno = p_deptno AND job = p_job;
BEGIN
 	 OPEN emp_cursor(10, 'CLERK');
  	LOOP
  	 FETCH emp_cursor INTO v_empno,v_ename;
  	 EXIT WHEN emp_cursor%NOTFOUND;
  	 DBMS_OUTPUT.PUT_LINE(v_empno||','||v_ename);
	  END LOOP;
	END;
```


说明：游标emp_cursor定义了两个参数：p_deptno代表部门编号，p_job代表职务。语句OPEN emp_cursor(10, 'CLERK')传递了两个参数值给游标，即部门为10、职务为CLERK，所以游标查询的内容是部门10的职务为CLERK的雇员。循环部分用于显示查询的内容。



【练习1】修改Open语句的参数：部门号为20、职务为ANALYST，并重新执行。
		
也可以通过变量向游标传递参数，但变量需要先于游标定义，并在游标打开之前赋值。对以上例子重新改动如下：
 		
【训练2】  通过变量传递参数给游标。

```sql
	SET SERVEROUTPUT ON
	DECLARE
  	v_empno NUMBER(5);
  	v_ename VARCHAR2(10);
  	v_deptno NUMBER(5);
	v_job VARCHAR2(10);
   	 CURSOR emp_cursor IS
    	SELECT empno, ename FROM emp
    	WHERE	deptno = v_deptno AND job = v_job;
	BEGIN
 	 v_deptno:=10;
 	 v_job:='CLERK';
 	 OPEN emp_cursor;
  	LOOP
  	 FETCH emp_cursor INTO v_empno,v_ename;
	   EXIT WHEN emp_cursor%NOTFOUND;
	DBMS_OUTPUT.PUT_LINE(v_empno||','||v_ename);
 	 END LOOP;
	END;
```

说明：该程序与前一程序实现相同的功能。


## 动态SELECT语句和动态游标的用法

Oracle支持动态SELECT语句和动态游标，动态的方法大大扩展了程序设计的能力。
		
对于查询结果为一行的SELECT语句，可以用动态生成查询语句字符串的方法，在程序执行阶段临时地生成并执行，语法是：

```sql
	execute immediate 查询语句字符串 into 变量1[,变量2...]; 
```

以下是一个动态生成SELECT语句的例子。


【训练1】  动态SELECT查询。

```sql
	SET SERVEROUTPUT ON 
	DECLARE 
	str varchar2(100);
	v_ename varchar2(10);
	begin
	str:='select ename from scott.emp where empno=7788';
	execute immediate str into v_ename; 
	dbms_output.put_line(v_ename);
	END; 
```

说明：SELECT...INTO...语句存放在STR字符串中，通过EXECUTE语句执行。
		
在变量声明部分定义的游标是静态的，不能在程序运行过程中修改。虽然可以通过参数传递来取得不同的数据，但还是有很大的局限性。通过采用动态游标，可以在程序运行阶段随时生成一个查询语句作为游标。要使用动态游标需要先定义一个游标类型，然后声明一个游标变量，游标对应的查询语句可以在程序的执行过程中动态地说明。

定义游标类型的语句如下：

```sql
	TYPE 游标类型名 REF CURSOR;
```

声明游标变量的语句如下：

```sql
	游标变量名 游标类型名;
```

在可执行部分可以如下形式打开一个动态游标：

```sql
	OPEN 游标变量名 FOR 查询语句字符串;
```

【训练2】  按名字中包含的字母顺序分组显示雇员信息。
		
输入并运行以下程序：

```sql
declare 
 type cur_type is ref cursor;
 cur cur_type;--声明为一个未绑定的游标
 rec scott.emp%rowtype;
 str varchar2(50);
 letter char:= 'A';
begin
 		loop  		
 		 str:= 'select ename from emp where ename like ''%'||letter||'%''';
 		 open cur for str;--打开游标变量方式1
 		 dbms_output.put_line('包含字母'||letter||'的名字：');
		  loop
  		 fetch cur into rec.ename;
  		 exit when cur%notfound;
   		dbms_output.put_line(rec.ename);
 end loop;
  exit when letter='Z';
  letter:=chr(ascii(letter)+1);
 end loop;
end;
```
	
说明：使用了二重循环，在外循环体中，动态生成游标的SELECT语句，然后打开。通过语句
letter:=chr(ascii(letter)+1)可获得字母表中的下一个字母。





## 例题（一）


【训练1】  查询雇员编号为7788的雇员姓名和工资。
		
步骤1：用SCOTT账户登录SQL*Plus。
		
步骤2：在输入区输入以下程序：

```sql
	 /*这是一个简单的示例程序*/
	SET SERVEROUTPUT ON
	DECLARE--定义部分标识
 	 v_name  VARCHAR2(10);	--定义字符串变量v_name 
 	 v_sal   NUMBER(5);	--定义数值变量v_sal 
	BEGIN			--可执行部分标识
SELECT	 ename,sal 
	  INTO v_name,v_sal 
  	FROM	emp 
 	 WHERE empno=7788; 			
	--在程序中插入的SQL语句
  	DBMS_OUTPUT.PUT_LINE('7788号雇员是：'||v_name||'，工资为：'||to_char(v_sal));
	--输出雇员名和工资
	END;								--结束标识
```

步骤3：按执行按钮或F5快捷键执行程序。
		 
【训练2】  变量的定义和初始化。

输入和运行以下程序：

```sql
	SET SERVEROUTPUT ON 
DECLARE		--声明部分标识
	v_job		VARCHAR2(9);
	v_count	BINARY_INTEGER DEFAULT 0;
	v_total_sal	NUMBER(9,2) := 0;
	v_date		DATE := SYSDATE + 7;
	c_tax_rate	CONSTANT NUMBER(3,2) := 8.25;
	v_valid		BOOLEAN NOT NULL := TRUE;
BEGIN
  	v_job:='MANAGER';		
	--在程序中赋值
 	 DBMS_OUTPUT.PUT_LINE(v_job);	
	--输出变量v_job的值
 	 DBMS_OUTPUT.PUT_LINE(v_count);
	--输出变量v_count的值
 	DBMS_OUTPUT.PUT_LINE(v_date);	
	--输出变量v_date的值
 	DBMS_OUTPUT.PUT_LINE(c_tax_rate);	
	--输出变量c_tax_rate的值
	END;
```


【训练3】  根据表的字段定义变量。
		
输入并执行以下程序：

```sql
	SET SERVEROUTPUT ON 
DECLARE
   	v_ename	emp.ename%TYPE;--根据字段定义变量
BEGIN
   	SELECT	ename 
  	 INTO		v_ename 
  	 FROM		emp 
  	 WHERE	empno = 7788;
	DBMS_OUTPUT.PUT_LINE(v_ename);	
	--输出变量的值
END;
```
		 
【训练4】  定义并使用结合变量。
步骤1：输入和执行下列命令，定义结合变量g_ename：

```sql
	VARIABLE  g_ename VARCHAR2(100) 
```

步骤2：输入和执行下列程序：

```sql
	SET SERVEROUTPUT ON 
	BEGIN
  	 :g_ename:=:g_ename|| 'Hello~ ';			--在程序中使用结合变量
  	 DBMS_OUTPUT.PUT_LINE(:g_ename);	
	--输出结合变量的值
	END;
```

步骤3：重新执行程序。

步骤4：程序结束后用命令显示结合变量的内容：

```sql
  	PRINT g_ename 
```

【训练5】  根据表定义记录变量。

输入并执行如下程序：

```sql
	SET SERVEROUTPUT ON 
	DECLARE
	emp_record		emp%ROWTYPE;--定义记录变量
	BEGIN
 	 SELECT * INTO	emp_record 
 	 FROM			emp 
 	 WHERE		mpno = 7788;--取出一条记录
  	DBMS_OUTPUT.PUT_LINE(emp_record.ename);	--输出记录变量的某个字段 
	END;
```

【训练6】  定义和使用TABLE变量：

```sql
	SET SERVEROUTPUT ON 
	DECLARE
 	TYPE type_table IS TABLE OF VARCHAR2(10) INDEX BY BINARY_INTEGER;  --类型说明
	 v_t	  type_table;     --定义TABLE变量
	BEGIN
	 v_t(1):='MONDAY';
	 v_t(2):='TUESDAY';
 	v_t(3):='WEDNESDAY';
	 v_t(4):='THURSDAY';
 	v_t(5):='FRIDAY';
DBMS_OUTPUT.PUT_LINE(v_t(3));  --输出变量的内容
	END;
```


【训练7】  如果温度大于30℃，则显示“温度偏高”。
输入并执行以下程序：

```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 V_temprature		NUMBER(5):=32;
 	 V_result         	BOOLEAN:=false;
BEGIN
	  V_result:= v_temprature >30;
 	  IF V_result THEN 
    	DBMS_OUTPUT.PUT_LINE('温度'|| V_temprature ||'度,偏高');
 	  END IF; 
	END;
```

试修改温度的初值为25℃，重新执行，观察结果。


【训练8】  根据性别，显示尊称。
		
输入并执行以下程序：

```sql
	SET SERVEROUTPUT ON
DECLARE
 	 v_sex	VARCHAR2(2);
  	v_titil  	VARCHAR2(10);
BEGIN
    v_sex:='男';
    IF v_sex ='男' THEN
      v_titil:='先生';
    ELSE
      v_titil:='女士';
   END IF; 
    DBMS_OUTPUT.PUT_LINE(v_titil||'您好！');
END;
```

【训练9】  根据雇员工资分级显示税金。

输入并运行以下程序：

```sql
SET SERVEROUTPUT ON
DECLARE
  v_sal  NUMBER(5);
  v_tax  NUMBER(5,2);
BEGIN
  SELECT sal INTO v_sal 
  FROM emp 
  WHERE empno=7788;
IF v_sal >=3000 THEN 
  	 	V_tax:= v_sal*0.08;--税率8%
 	 ELSIF v_sal>=1500 THEN
  		 V_tax:= v_sal*0.06; --税率6%
  	ELSE
  		 V_tax:= v_sal*0.04; --税率4%
 END IF;
  	DBMS_OUTPUT.PUT_LINE('应缴税金:'||V_tax);
END;
```


## 例题（二）

【训练1】  使用CASE结构实现职务转换。
		
输入并执行程序：

```sql
SET SERVEROUTPUT ON
DECLARE
v_job  VARCHAR2(10);
BEGIN
SELECT job INTO v_job 
FROM emp 
WHERE empno=7788;
CASE v_job 
WHEN 'PRESIDENT' THEN 
  DBMS_OUTPUT.PUT_LINE('雇员职务:总裁');
WHEN 'MANAGER' THEN  
 DBMS_OUTPUT.PUT_LINE('雇员职务:经理');
WHEN 'SALESMAN' THEN  
  DBMS_OUTPUT.PUT_LINE('雇员职务:推销员');
WHEN 'ANALYST' THEN  
  DBMS_OUTPUT.PUT_LINE('雇员职务:系统分析员');
WHEN 'CLERK' THEN  
 DBMS_OUTPUT.PUT_LINE('雇员职务:职员');
ELSE  
  DBMS_OUTPUT.PUT_LINE('雇员职务:未知');
END CASE;
END;
```
				
【训练2】  使用CASE的表达式结构。

```sql
	SET SERVEROUTPUT ON
	DECLARE
		  v_grade	VARCHAR2(10);
		 v_result	VARCHAR2(10);
	BEGIN
		 v_grade:='B';
		 v_result:=CASE v_grade 
		  WHEN 'A' THEN '优'
			WHEN 'B' THEN '良'
		WHEN 'C' THEN '中'
		 WHEN 'D' THEN '差'
		ELSE '未知'
		END;
	  DBMS_OUTPUT.PUT_LINE('评价等级:'||V_result);
		END;
```


【训练3】  使用CASE的搜索结构。

```sql
	SET SERVEROUTPUT ON
	DECLARE
	   v_sal	NUMBER(5);
	BEGIN
	   SELECT sal INTO v_sal FROM emp 
  		 WHERE empno=7788;
	CASE 
   		WHEN v_sal>=3000 THEN 
		DBMS_OUTPUT.PUT_LINE('工资等级：高');
	  	 WHEN v_sal>=1500 THEN
DBMS_OUTPUT.PUT_LINE('工资等级：中');
	  ELSE
       DBMS_OUTPUT.PUT_LINE('工资等级：低');
END CASE;
END;
```

		 
【训练4】　求：1２+3２+5２+...+15２ 的值。

输入并执行以下程序：

```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 v_total		NUMBER(5):=0;
  	v_count		NUMBER(5):=1;
	BEGIN
	LOOP
	    v_total:=v_total+v_count**2;
   		EXIT WHEN v_count=15;--条件退出
count:=v_count+2;
  	END LOOP;
 	 DBMS_OUTPUT.PUT_LINE(v_total);
	END;
```

【训练5】  用FOR循环输出图形。

```sql
	SET SERVEROUTPUT ON
	BEGIN
	FOR I IN 1..8 
	LOOP   DBMS_OUTPUT.PUT_LINE(to_char(i)||rpad('*',I,'*'));
	END LOOP;
	END;
```


【训练6】  输出一个空心三角形。

```sql
	 BEGIN 
	FOR I IN 1..9
	LOOP 
	 IF I=1 OR I=9 THEN
 	    DBMS_OUTPUT.PUT_LINE(to_char(I)||rpad(' ',12-I,' ')||rpad('*',2*i-1,'*')); 
ELSE
       DBMS_OUTPUT.PUT_LINE(to_char(I)||rpad(' ',12-I,' ')||'*'||rpad(' ',I*2-3,' ')||'*'); 
 	END IF;
	END LOOP; 
	END;
```


【训练7】　使用WHILE 循环向emp表连续插入5个记录。
步骤1：执行下面的程序：

```sql
SET SERVEROUTPUT ON
DECLARE
v_count	NUMBER(2) := 1;
BEGIN
  WHILE v_count <6 LOOP
    INSERT INTO emp(empno, ename)
    VALUES (5000+v_count, '临时');
v_count := v_count + 1;
  END LOOP;
  COMMIT;
END;
```

步骤2：显示插入的记录：

```sql
	SELECT empno,ename FROM emp WHERE ename='临时';
```

步骤3：删除插入的记录：

```sql
	DELETE FROM emp WHERE ename='临时';
	COMMIT;
```
		
【训练8】　使用二重循环求1！+2！+...+10！的值。

步骤1：第1种算法：

```sql
SET SERVEROUTPUT ON
DECLARE
  v_total	NUMBER(8):=0;
  v_ni	NUMBER(8):=0;
  J		NUMBER(5);
BEGIN
FOR I IN 1..10
  LOOP
    J:=1;
	  v_ni:=1;
    WHILE J<=I
    LOOP
      v_ni:= v_ni*J;
      J:=J+1;
    END LOOP;--内循环求n!
v_total:=v_total+v_ni;
  END LOOP;--外循环求总和
  DBMS_OUTPUT.PUT_LINE(v_total);
END;
```

步骤2：第2种算法：

```sql
SET SERVEROUTPUT ON
DECLARE
  v_total		NUMBER(8):=0;
  v_ni		NUMBER(8):=1;
BEGIN
  FOR I IN 1..10
  LOOP
    v_ni:= v_ni*I;	--求n!
    v_total:= v_total+v_ni;
  END LOOP;		--循环求总和
  DBMS_OUTPUT.PUT_LINE(v_total);
END;
```

【训练9】  插入雇员，如果雇员已经存在，则输出提示信息。

```sql
	SET SERVEROUTPUT ON 
	DECLARE
  	v_empno NUMBER(5):=7788;
  	v_num VARCHAR2(10); 
  	i NUMBER(3):=0;
	BEGIN		
 	 SELECT count(*) INTO v_num FROM SCOTT.emp WHERE empno=v_empno;
IF v_num=1 THEN
  DBMS_OUTPUT.PUT_LINE('雇员'||v_empno||'已经存在！');
  ELSE
  INSERT INTO emp(empno,ename) VALUES(v_empno,'TOM');
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('成功插入新雇员！');
END IF; 
END;
```

【训练10】  输出由符号“*”构成的正弦曲线的一个周期(0～360°)。

```sql
	SET SERVEROUTPUT ON SIZE 10000
SET LINESIZE 100
SET PAGESIZE 100
DECLARE
  v_a	NUMBER(8,3);
  v_p 	NUMBER(8,3);
BEGIN
  FOR I IN 1..18
LOOP
    	v_a:=I*20*3.14159/180;--生成角度，并转换为弧度
   	 v_p:=SIN(v_a)*20+25;--求SIN函数值，20为放大倍数，25为水平位移
    	DBMS_OUTPUT.PUT_LINE(to_char(i)||lpad('*',v_p,' '));--输出记录变量的某个字段
  	END LOOP;
	END;
```




## ## 例题（三）


【训练1】　使用隐式游标的属性，判断对雇员工资的修改是否成功。

步骤1：输入和运行以下程序：

```sql
	SET SERVEROUTPUT ON 
	BEGIN
  	UPDATE emp SET sal=sal+100 WHERE empno=1234;
 	 IF SQL%FOUND THEN 
   	    DBMS_OUTPUT.PUT_LINE('成功修改雇员工资！');
   	COMMIT; 
  	ELSE
DBMS_OUTPUT.PUT_LINE('修改雇员工资失败！');
 	 END IF; 
	END;
```
		 
步骤2：将雇员编号1234改为7788，重新执行以上程序：

【训练2】  用游标提取emp表中7788雇员的名称和职务。

```sql
	SET SERVEROUTPUT ON
	DECLARE	
 	 v_ename VARCHAR2(10);
 	 v_job VARCHAR2(10);
 	 CURSOR emp_cursor IS 
 	 SELECT ename,job FROM emp WHERE empno=7788;
BEGIN
 	 OPEN emp_cursor;
  	FETCH emp_cursor INTO v_ename,v_job;
  	DBMS_OUTPUT.PUT_LINE(v_ename||','||v_job);
  	CLOSE emp_cursor;
	END;
```


【训练3】  用游标提取emp表中7788雇员的姓名、职务和工资。

```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 CURSOR emp_cursor IS  SELECT ename,job,sal FROM emp WHERE empno=7788;
 	 emp_record emp_cursor%ROWTYPE;
	BEGIN
OPEN emp_cursor;	
   	FETCH emp_cursor INTO emp_record;
	   DBMS_OUTPUT.PUT_LINE(emp_record.ename||','|| emp_record.job||','|| emp_record.sal);
 	 CLOSE emp_cursor;
	END;
```


【训练4】  显示工资最高的前3名雇员的名称和工资。

```sql
	SET SERVEROUTPUT ON
	DECLARE
 	 V_ename VARCHAR2(10);
  	V_sal NUMBER(5);
  	CURSOR emp_cursor IS  SELECT ename,sal FROM emp ORDER BY sal DESC;
	BEGIN
 	 OPEN emp_cursor;
 	 FOR I IN 1..3 LOOP
	   FETCH emp_cursor INTO v_ename,v_sal;
DBMS_OUTPUT.PUT_LINE(v_ename||','||v_sal);
  END LOOP;
  CLOSE emp_cursor;
END;
```


【训练5】  使用特殊的FOR循环形式显示全部雇员的编号和名称。

```sql
SET SERVEROUTPUT ON
DECLARE
  CURSOR emp_cursor IS 
  SELECT empno, ename FROM emp;
BEGIN
FOR Emp_record IN emp_cursor LOOP   
    DBMS_OUTPUT.PUT_LINE(Emp_record.empno|| Emp_record.ename);
	END LOOP;
	END;
```

【训练6】  另一种形式的游标循环。

```sql
SET SERVEROUTPUT ON 
BEGIN
 FOR re IN (SELECT ename FROM EMP)  LOOP
  DBMS_OUTPUT.PUT_LINE(re.ename)
 END LOOP;
END;
```

【训练7】  使用游标的属性练习。

```sql
SET SERVEROUTPUT ON
DECLARE
  V_ename VARCHAR2(10);
  CURSOR emp_cursor IS 
  SELECT ename FROM emp;
BEGIN
 OPEN emp_cursor;
 IF emp_cursor%ISOPEN THEN
LOOP
   FETCH emp_cursor INTO v_ename;
   EXIT WHEN emp_cursor%NOTFOUND;
   DBMS_OUTPUT.PUT_LINE(to_char(emp_cursor%ROWCOUNT)||'-'||v_ename);
  END LOOP;
 ELSE
  DBMS_OUTPUT.PUT_LINE('用户信息：游标没有打开！');
 END IF;
 CLOSE  emp_cursor;
END;
```

 【训练8】  带参数的游标。

```sql
		SET SERVEROUTPUT ON
		DECLARE
  			V_empno NUMBER(5);
  			V_ename VARCHAR2(10);
    		CURSOR 	emp_cursor(p_deptno NUMBER, 	p_job VARCHAR2) IS
		    SELECT	empno, ename FROM emp 
    		WHERE	deptno = p_deptno AND job = p_job
BEGIN
 	 OPEN emp_cursor(10, 'CLERK');
  	LOOP
  	 FETCH emp_cursor INTO v_empno,v_ename;
  	 EXIT WHEN emp_cursor%NOTFOUND;
  	 DBMS_OUTPUT.PUT_LINE(v_empno||','||v_ename);
	  END LOOP;
	END;
```


【训练9】  通过变量传递参数给游标。

```sql
		SET SERVEROUTPUT ON
		DECLARE
  		v_empno NUMBER(5);
  		v_ename VARCHAR2(10);
  		v_deptno NUMBER(5);
v_job VARCHAR2(10);
   		 CURSOR emp_cursor IS
    		SELECT empno, ename FROM emp 
    		WHERE	deptno = v_deptno AND job = v_job;
		BEGIN
 		 v_deptno:=10;
 		 v_job:='CLERK';
 		 OPEN emp_cursor;
  		LOOP
  		 FETCH emp_cursor INTO v_empno,v_ename;
		   EXIT WHEN emp_cursor%NOTFOUND;
DBMS_OUTPUT.PUT_LINE(v_empno||','||v_ename);
 		 END LOOP;
		END;
```

【训练10】  动态SELECT查询。
	
```sql
	SET SERVEROUTPUT ON 
		DECLARE 
		str varchar2(100);
		v_ename varchar2(10);
		begin
		str:='select ename from scott.emp where empno=7788';
		execute immediate str into v_ename; 
		dbms_output.put_line(v_ename);
		END; 
```

【训练11】  按名字中包含的字母顺序分组显示雇员信息。
		
输入并运行以下程序：

```sql
declare 
 type cur_type is ref cursor;
 cur cur_type;--声明为一个未绑定的游标
 rec scott.emp%rowtype;
 str varchar2(50);
 letter char:= 'A';
begin
 		loop  		
 		 str:= 'select ename from emp where ename like ''%'||letter||'%''';
 		 open cur for str;--打开游标变量方式1
 		 dbms_output.put_line('包含字母'||letter||'的名字：');
		  loop
  		 fetch cur into rec.ename;
  		 exit when cur%notfound;
   		dbms_output.put_line(rec.ename);
end loop;
  exit when letter='Z';
  letter:=chr(ascii(letter)+1);
 end loop;
end;
```	

{% endcq%}