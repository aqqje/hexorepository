---
title: oracle学习十四
date: 2017-12-13 23:02:26
comments: ture
categories:
	- 数据库
tags:
	- oracle
---

{% note info %}
![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
- 此文章是oracle学习系例第十四章，主要是用来复习和巩固在课堂学习的知识！
<!-- more -->
{% endnote %}

--------------------------

## 笔记

一、游标参数的传递


例：

```sql
SET SERVEROUTPUT ON
    DECLARE
        V_empno NUMBER(5);
  	V_ename VARCHAR2(10);
    	CURSOR 	emp_cursor(p_deptno NUMBER,p_job VARCHAR2) IS SELECT empno,ename FROM emp WHERE	deptno = p_deptno AND job = p_job;
    BEGIN
 	OPEN emp_cursor(10, 'CLERK');
  	LOOP
  	   FETCH emp_cursor INTO v_empno,v_ename;
  	EXIT WHEN emp_cursor%NOTFOUND;
  	   DBMS_OUTPUT.PUT_LINE(v_empno||','||v_ename);
	END LOOP;
    END;
```


二、异常处理
错误处理的语法如下：


```sql
 EXCEPTION
WHEN 错误1[OR 错误2] THEN 语句序列1;
WHEN 错误3[OR 错误4] THEN 语句序列2;
...
WHEN OTHERS 语句序列n;
 END;
```

例：

```sql
SET SERVEROUTPUT ON
    DECLARE
        v_name VARCHAR2(10);
    BEGIN
        SELECT	ename  INTO v_name  FROM emp  WHERE	empno = 1234;
	DBMS_OUTPUT.PUT_LINE('该雇员名字为：'|| v_name);
	EXCEPTION
  		WHEN NO_DATA_FOUND THEN	DBMS_OUTPUT.PUT_LINE('编号错误，没有找到相应雇员！');
  		WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('发生其他错误！');
    END;
```


三、存储过程
1.创建和删除存储过程
格式：  

```sql
CREATE [OR REPLACE] PROCEDURE 存储过程名[(参数[IN|OUT|IN OUT] 数据类型...)]
	{AS|IS}
		[说明部分]   --定义需要使用的临时变量
	BEGIN
		语句集;
		[EXCEPTION]
		    [错误处理部分]
	END [过程名];

```



删除： 

```sql
 drop procedure 存储过程名;
```


## 调用存储过程


```sql
方法1： 
EXECUTE 模式名.存储过程名[(参数...)];   (适用于命今行窗口及sql窗口)
方法2： 
(适用于sql窗口)
BEGIN   
  模式名.存储过程名[(参数...)];
END;
```

  
例：编写显示雇员信息的存储过程EMP_LIST，并引用EMP_COUNT存储过程(无参存储过程 )。


```sql
CREATE OR REPLACE PROCEDURE EMP_LIST
AS
CURSOR emp_cursor IS SELECT empno,ename FROM emp;
BEGIN
	FOR Emp_record IN emp_cursor LOOP   
 	DBMS_OUTPUT.PUT_LINE(Emp_record.empno||Emp_record.ename);
	END LOOP;
	EMP_COUNT;	
END;
```


调用：


```sql
begin
	EMP_LIST;
end;

```


## 参数传递

  a.输入参数: 参数名     IN     数据类型     DEFAULT    值；
  	例：编写给雇员增加工资的存储过程CHANGE_SALARY，通过IN类型的参数传递要增加工资的雇员编号和增加的工资额。

```sql
		CREATE OR REPLACE PROCEDURE CHANGE_SALARY(P_EMPNO IN NUMBER DEFAULT 7788,P_RAISE NUMBER DEFAULT 10)  --形参P_EMPNO及P_RAISE
		AS
		   V_ENAME VARCHAR2(10);
		   V_SAL NUMBER(5);
		BEGIN
 		   SELECT ENAME,SAL INTO V_ENAME,V_SAL FROM EMP WHERE EMPNO=P_EMPNO;
		   UPDATE EMP SET SAL=SAL+P_RAISE WHERE EMPNO=P_EMPNO;
		   DBMS_OUTPUT.PUT_LINE('雇员'||V_ENAME||'的工资被改为'||TO_CHAR(V_SAL+P_RAISE));
		   COMMIT;
		EXCEPTION
		  WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('发生错误，修改失败！');
		  ROLLBACK; --如果出了异常则撤消
		END;
```

调用：

```sql
begin
	CHANGE_SALARY(7788,80)
end;
```


b.输出参数: 参数名     OUT    数据类型     DEFAULT    值；
        

例：统计雇员的人数
          
```sql
 CREATE OR REPLACE PROCEDURE EMP_COUNT(P_TOTAL OUT NUMBER)   --P_TOTAL为输出参数
	AS
 BEGIN
	SELECT COUNT(*) INTO P_TOTAL FROM EMP;
  END;
```

调用：

```sql
DECLARE
	V_EMPCOUNT NUMBER;   --定义变量接收过程求出的结果
	     BEGIN
		 EMP_COUNT(V_EMPCOUNT);
		 DBMS_OUTPUT.PUT_LINE('雇员总人数为：'||V_EMPCOUNT);
 END;
````

  
  c.输入输出参数： 参数名  IN OUT   数据类型   DEFAULT   值；
	

例：使用IN OUT类型的参数，给电话号码增加区码。
		

```sql
CREATE OR REPLACE PROCEDURE ADD_REGION(P_HPONE_NUM IN OUT VARCHAR2)
		AS
		BEGIN
		   P_HPONE_NUM:='024-'||P_HPONE_NUM;
		END;
```

	
调用： 


```sql
DECLARE
	V_PHONE_NUM VARCHAR2(15);
     BEGIN
 V_PHONE_NUM:='26731092';
   	ADD_REGION(V_PHONE_NUM);
	DBMS_OUTPUT.PUT_LINE('新的电话号码：'||V_PHONE_NUM);
 END;
```


## 错误处理


错误处理部分位于程序的可执行部分之后，是由WHEN语句引导的多个分支构成的。错误处理的语法如下：

```sql
	EXCEPTION
	WHEN 错误1[OR 错误2] THEN
	语句序列1；
	WHEN 错误3[OR 错误4] THEN
```



语句序列2；

```sql
WHEN OTHERS
语句序列n；
END;
```
 
其中：
错误是在标准包中由系统预定义的标准错误，或是由用户在程序的说明部分自定义的错误，参见下一节系统预定义的错误类型。
语句序列就是不同分支的错误处理部分。


凡是出现在WHEN后面的错误都是可以捕捉到的错误，其他未被捕捉到的错误，将在WHEN OTHERS部分进行统一处理，OTHENS必须是EXCEPTION部分的最后一个错误处理分支。如要在该分支中进一步判断错误种类，可以通过使用预定义函数SQLCODE( )和SQLERRM( )来获得系统错误号和错误信息。
		
如果在程序的子块中发生了错误，但子块没有错误处理部分，则错误会传递到主程序中。
		
下面是由于查询编号错误而引起系统预定义异常的例子。



【训练1】  查询编号为1234的雇员名字。

```sql
SET SERVEROUTPUT ON
DECLARE
v_name VARCHAR2(10);
BEGIN
   SELECT	ename
   INTO		v_name
   FROM		emp
   WHERE	empno = 1234;
DBMS_OUTPUT.PUT_LINE('该雇员名字为：'|| v_name);
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('编号错误，没有找到相应雇员！');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('发生其他错误！');
END;
```


说明：在以上查询中，因为编号为1234的雇员不存在，所以将发生类型为“NO_DATA_	FOUND”的异常。“NO_DATA_FOUND”是系统预定义的错误类型，EXCEPTION部分下的WHEN语句将捕捉到该异常，并执行相应代码部分。在本例中，输出用户自定义的错误信息“编号错误，没有找到相应雇员!”。如果发生其他类型的错误，将执行OTHERS条件下的代码部分，显示“发生其他错误!”。



【训练2】  由程序代码显示系统错误。

```sql
SET SERVEROUTPUT ON
DECLARE
v_temp NUMBER(5):=1;
BEGIN
v_temp:=v_temp/0;
EXCEPTION
  WHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('发生系统错误！');
    DBMS_OUTPUT.PUT_LINE('错误代码：'|| SQLCODE( ));
    DBMS_OUTPUT.PUT_LINE('错误信息：' ||SQLERRM( ));
		END;
```


说明：程序运行中发生除零错误，由WHEN OTHERS捕捉到，执行用户自己的输出语句显示错误信息，然后正常结束。在错误处理部分使用了预定义函数SQLCODE( )和SQLERRM( )来进一步获得错误的代码和种类信息。


## 预定义错误

		
Oracle的系统错误很多，但只有一部分常见错误在标准包中予以定义。定义的错误可以在EXCEPTION部分通过标准的错误名来进行判断，并进行异常处理。


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle22.jpg "oracle22")<br/>

比如，如果程序向表的主键列插入重复值，则将发生DUP_VAL_ON_INDEX错误。
		
如果一个系统错误没有在标准包中定义，则需要在说明部分定义，语法如下：

```sql
错误名 EXCEPTION;
```

		
定义后使用PRAGMA EXCEPTION_INIT来将一个定义的错误同一个特别的Oracle错误代码相关联，就可以同系统预定义的错误一样使用了。语法如下：
		
```sql
PRAGMA EXCEPTION_INIT(错误名，- 错误代码)；
```


【训练1】  定义新的系统错误类型。

```sql
	SET SERVEROUTPUT ON
	DECLARE
	V_ENAME VARCHAR2(10);
	NULL_INSERT_ERROR EXCEPTION;
	PRAGMA EXCEPTION_INIT(NULL_INSERT_ERROR,-1400);
	BEGIN
	INSERT INTO EMP(EMPNO) VALUES(NULL);
EXCEPTION
WHEN NULL_INSERT_ERROR THEN
    DBMS_OUTPUT.PUT_LINE('无法插入NULL值！');
  WHEN OTHERS  THEN
    DBMS_OUTPUT.PUT_LINE('发生其他系统错误！');
END;

```
	
说明：NULL_INSERT_ERROR是自定义异常，同系统错误1400相关联。



## 自定义异常
		
程序设计者可以利用引发异常的机制来进行程序设计，自己定义异常类型。可以在声明部分定义新的异常类型，定义的语法是：

```sql
	错误名 EXCEPTION;
```

		
用户定义的错误不能由系统来触发，必须由程序显式地触发，触发的语法是：

```sql
	RAISE 错误名；
```


RAISE也可以用来引发模拟系统错误，比如，RAISE ZERO_DIVIDE将引发模拟的除零错误。
		
使用RAISE_APPLICATION_ERROR函数也可以引发异常。该函数要传递两个参数，第一个是用户自定义的错误编号，第二个参数是用户自定义的错误信息。使用该函数引发的异常的编号应该在20 000和20 999之间选择。
		
自定义异常处理错误的方式同前。


【训练1】  插入新雇员，限定插入雇员的编号在7000～8000之间。


```sql
SET SERVEROUTPUT ON
DECLARE
new_no NUMBER(10);
new_excp1 EXCEPTION;
new_excp2 EXCEPTION;
BEGIN
 new_no:=6789;
INSERT INTO	emp(empno,ename)
  VALUES(new_no, '小郑');
  IF new_no<7000 THEN
    RAISE new_excp1;
  END IF;
  IF new_no>8000 THEN
    RAISE new_excp2;
  END IF;
  COMMIT;
EXCEPTION
WHEN new_excp1  THEN
    ROLLBACK;
    DBMS_OUTPUT.PUT_LINE('雇员编号小于7000的下限！');
  	WHEN new_excp2  THEN
    ROLLBACK;
    DBMS_OUTPUT.PUT_LINE('雇员编号超过8000的上限！');
	END;
```

说明：在此例中，自定义了两个异常：new_excp1和new_excp2，分别代表编号小于7000和编号大于8000的错误。在程序中通过判断编号大小，产生对应的异常，并在异常处理部分回退插入操作，然后显示相应的错误信息。


【训练2】  使用RAISE_APPLICATION_ERROR函数引发系统异常。

```sql
SET SERVEROUTPUT ON
DECLARE
New_no NUMBER(10);
BEGIN
  New_no:=6789;
 INSERT INTO	emp(empno,ename)
  VALUES(new_no, 'JAMES');
IF new_no<7000 THEN
    ROLLBACK;
    RAISE_APPLICATION_ERROR(-20001, '编号小于7000的下限！');
  END IF;
  IF new_no>8000 THEN
    ROLLBACK;
    RAISE_APPLICATION_ERROR (-20002, '编号大于8000的下限！');
  END IF;
END;
```


说明：在本训练中，使用RAISE_APPLICATION_ERROR引发自定义异常，并以系统错误的方式进行显示。错误编号为20001和20002。
		
注意：同上一个训练比较，此种方法不需要事先定义异常，可直接引发。


可以参考下面的程序片断将出错信息记录到表中，其中，errors为记录错误信息的表，SQLCODE为发生异常的错误编号，SQLERRM为发生异常的错误信息。



```sql
DECLARE
  v_error_code      NUMBER;
  v_error_message   VARCHAR2(255);
BEGIN
...
EXCEPTION
...
WHEN OTHERS THEN
    v_error_code := SQLCODE ;
    v_error_message := SQLERRM ;
    INSERT INTO errors 
    VALUES(v_error_code, v_error_message);
END;

```

## 阶段训练

【训练1】  将雇员从一个表复制到另一个表。
		
步骤1：创建一个结构同EMP表一样的新表EMP1：

```sql
CREATE TABLE emp1 AS SELECT * FROM SCOTT.EMP WHERE 1=2;
```


步骤2：通过指定雇员编号，将雇员由EMP表移动到EMP1表：


```sql
SET SERVEROUTPUT ON 
DECLARE
v_empno NUMBER(5):=7788;
emp_rec emp%ROWTYPE;
BEGIN
 SELECT * INTO emp_rec FROM emp WHERE empno=v_empno;
 DELETE FROM emp WHERE empno=v_empno;
INSERT INTO emp1 VALUES emp_rec;
 IF SQL%FOUND THEN
  COMMIT;
  DBMS_OUTPUT.PUT_LINE('雇员复制成功！');
 ELSE 
  ROLLBACK;
  DBMS_OUTPUT.PUT_LINE('雇员复制失败！');
 END IF;
END;
```


步骤2：显示复制结果：

```sql
SELECT empno,ename,job FROM emp1;
```

说明：emp_rec变量是根据emp表定义的记录变量，SELECT...INTO...语句将整个记录传给该变量。INSERT语句将整个记录变量插入emp1表，如果插入成功(SQL%FOUND为真)，则提交事务，否则回滚撤销事务。试修改雇员编号为7902，重新执行以上程序。


【训练2】  输出雇员工资，雇员工资用不同高度的█表示。
		
输入并执行以下程序：

```sql
SET SERVEROUTPUT ON 
BEGIN
 FOR re IN (SELECT ename,sal FROM EMP)  LOOP
  DBMS_OUTPUT.PUT_LINE(rpad(re.ename,12,' ')||rpad(' █ ',re.sal/100,' █ '));
 END LOOP;
END;
```

说明：第一个rpad函数产生对齐效果，第二个rpad函数根据工资额产生不同数目的*。该程序采用了隐式的简略游标循环形式。

【训练3】  编写程序，格式化输出部门信息。
		

输入并执行如下程序：


```sql
SET SERVEROUTPUT ON 
	DECLARE
	v_count number:=0;
	CURSOR dept_cursor IS SELECT * FROM dept;
	BEGIN
	DBMS_OUTPUT.PUT_LINE('部门列表');
DBMS_OUTPUT.PUT_LINE('---------------------------------');
 	FOR Dept_record IN dept_cursor LOOP   
   	DBMS_OUTPUT.PUT_LINE('部门编号：'|| Dept_record.deptno);
   	DBMS_OUTPUT.PUT_LINE('部门名称：'|| Dept_record.dname);
	DBMS_OUTPUT.PUT_LINE('所在城市：'|| Dept_record.loc);
  DBMS_OUTPUT.PUT_LINE('---------------------------------');
	  v_count:= v_count+1;
  	END LOOP;
 	 DBMS_OUTPUT.PUT_LINE('共有'||to_char(v_count)||'个部门！');
	END;
```

 		
说明：该程序中将字段内容垂直排列。V_count变量记录循环次数，即部门个数。



【训练4】  已知每个部门有一个经理，编写程序，统计输出部门名称、部门总人数、总工资和部门经理。
		
输入并执行如下程序：

```sql
SET SERVEROUTPUT ON 
DECLARE
 v_deptno number(8);
 v_count number(3);
 v_sumsal number(6);
 v_dname  varchar2(15);
v_manager  varchar2(15);
 CURSOR list_cursor IS
   SELECT deptno,count(*),sum(sal) FROM emp group by deptno;
BEGIN
  OPEN list_cursor; 
  DBMS_OUTPUT.PUT_LINE('----------- 部 门 统 计 表 -----------');
DBMS_OUTPUT.PUT_LINE('部门名称   总人数  总工资   部门经理');
  FETCH list_cursor INTO v_deptno,v_count,v_sumsal; 
  WHILE list_cursor%found LOOP  
 SELECT dname INTO v_dname FROM dept
    WHERE deptno=v_deptno;
    SELECT ename INTO v_manager FROM emp 
    WHERE deptno=v_deptno and job='MANAGER';
DBMS_OUTPUT.PUT_LINE(rpad(v_dname,13)||rpad(to_char(v_count),8)
      ||rpad(to_char(v_sumsal),9)||v_manager);
    FETCH list_cursor INTO v_deptno,v_count,v_sumsal; 
  	END LOOP;
  		DBMS_OUTPUT.PUT_LINE('--------------------------------------');
  		CLOSE list_cursor;
		END;
```


说明：游标中使用到了起分组功能的SELECT语句，统计出各部门的总人数和总工资。再根据部门编号和职务找到部门的经理。该程序假定每个部门有一个经理。


【训练5】  为雇员增加工资，从工资低的雇员开始，为每个人增加原工资的10%，限定所增加的工资总额为800元，显示增加工资的人数和余额。
		
输入并调试以下程序：

```sql
SET SERVEROUTPUT ON 
DECLARE 
  V_NAME CHAR(10);
  V_EMPNO NUMBER(5);
  V_SAL NUMBER(8);
  V_SAL1 NUMBER(8);
  V_TOTAL NUMBER(8) := 800; 	--增加工资的总额
V_NUM NUMBER(5):=0;		--增加工资的人数
 	CURSOR emp_cursor IS 
	 SELECT EMPNO,ENAME,SAL FROM EMP ORDER BY SAL ASC;
	BEGIN
 	 OPEN emp_cursor;
  	DBMS_OUTPUT.PUT_LINE('姓名      原工资  新工资'); 
  	DBMS_OUTPUT.PUT_LINE('---------------------------'); 
 	 LOOP
    FETCH emp_cursor INTO V_EMPNO,V_NAME,V_SAL;
EXIT WHEN emp_cursor%NOTFOUND;
	  V_SAL1:= V_SAL*0.1;
    IF V_TOTAL>V_SAL1 THEN
     V_TOTAL := V_TOTAL - V_SAL1;
     V_NUM:=V_NUM+1;
	DBMS_OUTPUT.PUT_LINE(V_NAME||TO_CHAR(V_SAL,'99999')||
	TO_CHAR(V_SAL+V_SAL1,'99999'));
     UPDATE EMP SET SAL=SAL+V_SAL1
	   WHERE EMPNO=V_EMPNO;
   	 ELSE
DBMS_OUTPUT.PUT_LINE(V_NAME||TO_CHAR(V_SAL,'99999')||TO_CHAR(V_SAL,'99999'));
   	 END IF;
  	END LOOP;
  	DBMS_OUTPUT.PUT_LINE('---------------------------');
  	DBMS_OUTPUT.PUT_LINE('增加工资人数:'||V_NUM||' 剩余工资：'||V_TOTAL);  
 	 CLOSE emp_cursor; 
 	 COMMIT;
 	 END;
```


## 识存储过程和函数

存储过程和函数也是一种PL/SQL块，是存入数据库的PL/SQL块。但存储过程和函数不同于已经介绍过的PL/SQL程序，我们通常把PL/SQL程序称为无名块，而存储过程和函数是以命名的方式存储于数据库中的。和PL/SQL程序相比，存储过程有很多优点，具体归纳如下：


* 存储过程和函数以命名的数据库对象形式存储于数据库当中。存储在数据库中的优点是很明显的，因为代码不保存在本地，用户可以在任何客户机上登录到数据库，并调用或修改代码。
		
* 存储过程和函数可由数据库提供安全保证，要想使用存储过程和函数，需要有存储过程和函数的所有者的授权，只有被授权的用户或创建者本身才能执行存储过程或调用函数。

* 存储过程和函数的信息是写入数据字典的，所以存储过程可以看作是一个公用模块，用户编写的PL/SQL程序或其他存储过程都可以调用它(但存储过程和函数不能调用PL/SQL程序)。一个重复使用的功能，可以设计成为存储过程，比如：显示一张工资统计表，可以设计成为存储过程；一个经常调用的计算，可以设计成为存储函数；根据雇员编号返回雇员的姓名，可以设计成存储函数。

* 像其他高级语言的过程和函数一样，可以传递参数给存储过程或函数，参数的传递也有多种方式。存储过程可以有返回值，也可以没有返回值，存储过程的返回值必须通过参数带回；函数有一定的数据类型，像其他的标准函数一样，我们可以通过对函数名的调用返回函数值。
		  
存储过程和函数需要进行编译，以排除语法错误，只有编译通过才能调用。


## 创建和删除存储过程


创建存储过程，需要有CREATE PROCEDURE或CREATE ANY PROCEDURE的系统权限。该权限可由系统管理员授予。创建一个存储过程的基本语句如下：

```sql	
CREATE [OR REPLACE] PROCEDURE 存储过程名[(参数[IN|OUT|IN OUT] 数据类型...)]
{AS|IS}
[说明部分]
BEGIN
可执行部分
[EXCEPTION
错误处理部分]
END [过程名];
```

其中：
		
可选关键字OR REPLACE 表示如果存储过程已经存在，则用新的存储过程覆盖，通常用于存储过程的重建。
		
参数部分用于定义多个参数(如果没有参数，就可以省略)。参数有三种形式：IN、OUT和IN OUT。如果没有指明参数的形式，则默认为IN。
		
关键字AS也可以写成IS，后跟过程的说明部分，可以在此定义过程的局部变量。
		
编写存储过程可以使用任何文本编辑器或直接在SQL*Plus环境下进行，编写好的存储过程必须要在SQL*Plus环境下进行编译，生成编译代码，原代码和编译代码在编译过程中都会被存入数据库。编译成功的存储过程就可以在Oracle环境下进行调用了。


一个存储过程在不需要时可以删除。删除存储过程的人是过程的创建者或者拥有DROP ANY PROCEDURE系统权限的人。删除存储过程的语法如下：

```sql
	DROP PROCEDURE 存储过程名；
```


如果要重新编译一个存储过程，则只能是过程的创建者或者拥有ALTER ANY PROCEDURE系统权限的人。语法如下：

```sql
	ALTER PROCEDURE 存储过程名 COMPILE；
```

执行(或调用)存储过程的人是过程的创建者或是拥有EXECUTE ANY PROCEDURE系统权限的人或是被拥有者授予EXECUTE权限的人。执行的方法如下：

```sql
	方法1：
	EXECUTE 模式名.存储过程名[(参数...)];
	方法2：
	BEGIN
	模式名.存储过程名[(参数...)];
	END;
```

传递的参数必须与定义的参数类型、个数和顺序一致(如果参数定义了默认值，则调用时可以省略参数)。参数可以是变量、常量或表达式，用法参见下一节。
		
如果是调用本账户下的存储过程，则模式名可以省略。要调用其他账户编写的存储过程，则模式名必须要添加。
		
以下是一个生成和调用简单存储过程的训练。注意要事先授予创建存储过程的权限。


【训练1】  创建一个显示雇员总人数的存储过程。
		
步骤1：登录SCOTT账户(或学生个人账户)。
		
步骤2：在SQL*Plus输入区中，输入以下存储过程：
		
```sql
CREATE OR REPLACE PROCEDURE EMP_COUNT
AS
V_TOTAL NUMBER(10);
BEGIN
 SELECT COUNT(*) INTO V_TOTAL FROM EMP;
 DBMS_OUTPUT.PUT_LINE('雇员总人数为：'||V_TOTAL);
END;
```

步骤3：按“执行”按钮进行编译。
		
如果存在错误，就会显示:
		
警告: 创建的过程带有编译错误。
		
如果存在错误，对脚本进行修改，直到没有错误产生。
		
如果编译结果正确，将显示：
		
过程已创建。
		
步骤4：调用存储过程，在输入区中输入以下语句并执行：

```sql
	EXECUTE EMP_COUNT;
```

显示结果为：

雇员总人数为：14
	
PL/SQL 过程已成功完成。


说明：在该训练中，V_TOTAL变量是存储过程定义的局部变量，用于接收查询到的雇员总人数。
		
注意：在SQL*Plus中输入存储过程，按“执行”按钮是进行编译，不是执行存储过程。
 		
如果在存储过程中引用了其他用户的对象，比如表，则必须有其他用户授予的对象访问权限。一个存储过程一旦编译成功，就可以由其他用户或程序来引用。但存储过程或函数的所有者必须授予其他用户执行该过程的权限。
		
存储过程没有参数，在调用时，直接写过程名即可。


【训练2】  在PL/SQL程序中调用存储过程。
		
步骤1：登录SCOTT账户。
		
步骤2：授权STUDENT账户使用该存储过程，即在SQL*Plus输入区中，输入以下的命令：

```sql		
GRANT EXECUTE ON EMP_COUNT TO STUDENT
```

授权成功。
		
步骤3：登录STUDENT账户，在SQL*Plus输入区中输入以下程序：

```sql
	SET SERVEROUTPUT ON
	BEGIN
	SCOTT.EMP_COUNT;
	END;
```


步骤4：执行以上程序，结果为：
		
雇员总人数为：14
		
PL/SQL 过程已成功完成。 
		 
说明：在本例中，存储过程是由SCOTT账户创建的，STUDEN账户获得SCOTT账户的授权后，才能调用该存储过程。
		  
注意：在程序中调用存储过程，使用了第二种语法。



【训练3】  编写显示雇员信息的存储过程EMP_LIST，并引用EMP_COUNT存储过程。
		
步骤1：在SQL*Plus输入区中输入并编译以下存储过程：


```sql	
CREATE OR REPLACE PROCEDURE EMP_LIST
AS
CURSOR emp_cursor IS 
  SELECT empno,ename FROM emp;
BEGIN
FOR Emp_record IN emp_cursor LOOP   
 DBMS_OUTPUT.PUT_LINE(Emp_record.empno||Emp_record.ename);
END LOOP;
EMP_COUNT;
END;
```

执行结果：
		
过程已创建。

步骤2：调用存储过程，在输入区中输入以下语句并执行：

```sql
EXECUTE EMP_LIST
```

显示结果为：
7369SMITH
7499ALLEN
7521WARD
7566JONES
    		
执行结果：
		
雇员总人数为：14
		
PL/SQL 过程已成功完成。



说明：以上的EMP_LIST存储过程中定义并使用了游标，用来循环显示所有雇员的信息。然后调用已经成功编译的存储过程EMP_COUNT，用来附加显示雇员总人数。通过EXECUTE命令来执行EMP_LIST存储过程。
		
【练习1】编写显示部门信息的存储过程DEPT_LIST，要求统计出部门个数。


## 参数传递


参数的作用是向存储过程传递数据，或从存储过程获得返回结果。正确的使用参数可以大大增加存储过程的灵活性和通用性。


![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle23.jpg "oracle23")<br/>


参数的定义形式和作用如下：

```sql		
参数名     IN     数据类型     DEFAULT    值；
```
		
定义一个输入参数变量，用于传递参数给存储过程。在调用存储过程时，主程序的实际参数可以是常量、有值变量或表达式等。DEFAULT 关键字为可选项，用来设定参数的默认值。如果在调用存储过程时不指明参数，则参数变量取默认值。在存储过程中，输入变量接收主程序传递的值，但不能对其进行赋值。


```sql		
参数名     OUT     数据类型；
```

		
定义一个输出参数变量，用于从存储过程获取数据，即变量从存储过程中返回值给主程序。



在调用存储过程时，主程序的实际参数只能是一个变量，而不能是常量或表达式。在存储过程中，参数变量只能被赋值而不能将其用于赋值，在存储过程中必须给输出变量至少赋值一次。

```sql		
参数名 IN OUT 数据类型 DEFAULT 值；
```
		
定义一个输入、输出参数变量，兼有以上两者的功能。在调用存储过程时，主程序的实际参数只能是一个变量，而不能是常量或表达式。DEFAULT 关键字为可选项，用来设定参数的默认值。在存储过程中，变量接收主程序传递的值，同时可以参加赋值运算，也可以对其进行赋值。在存储过程中必须给变量至少赋值一次。
		
如果省略IN、OUT或IN OUT，则默认模式是IN。



【训练1】  编写给雇员增加工资的存储过程CHANGE_SALARY，通过IN类型的参数传递要增加工资的雇员编号和增加的工资额。
		
步骤1：登录SCOTT账户。
		  
步骤2：在SQL*Plus输入区中输入以下存储过程并执行：
		
```sql
CREATE OR REPLACE PROCEDURE CHANGE_SALARY(P_EMPNO IN NUMBER DEFAULT 7788,P_RAISE NUMBER DEFAULT 10)
	AS
	V_ENAME VARCHAR2(10);
	V_SAL NUMBER(5);
	BEGIN
 	SELECT ENAME,SAL INTO V_ENAME,V_SAL FROM EMP WHERE EMPNO=P_EMPNO;
	 UPDATE EMP SET SAL=SAL+P_RAISE WHERE EMPNO=P_EMPNO;
	DBMS_OUTPUT.PUT_LINE('雇员'||V_ENAME||'的工资被改为'||TO_CHAR(V_SAL+P_RAISE));
COMMIT;
	EXCEPTION
	 WHEN OTHERS THEN
 	DBMS_OUTPUT.PUT_LINE('发生错误，修改失败！');
 	ROLLBACK;
	END;
```


执行结果为：
		
过程已创建。
		
步骤3：调用存储过程，在输入区中输入以下语句并执行：

```sql
EXECUTE CHANGE_SALARY(7788,80)
```
		
显示结果为：
		
雇员SCOTT的工资被改为3080 
		
说明：从执行结果可以看到，雇员SCOTT的工资已由原来的3000改为3080。


参数的值由调用者传递，传递的参数的个数、类型和顺序应该和定义的一致。如果顺序不一致，可以采用以下调用方法。如上例，执行语句可以改为：


```sql
EXECUTE CHANGE_SALARY(P_RAISE=>80,P_EMPNO=>7788);

```


可以看出传递参数的顺序发生了变化，并且明确指出了参数名和要传递的值，=>运算符左侧是参数名，右侧是参数表达式，这种赋值方法的意义较清楚。
	
【练习1】创建插入雇员的存储过程INSERT_EMP，并将雇员编号等作为参数。
		
在设计存储过程的时候，也可以为参数设定默认值，这样调用者就可以不传递或少传递参数了。


【训练2】  调用存储过程CHANGE_SALARY，不传递参数，使用默认参数值。
		
在SQL*Plus输入区中输入以下命令并执行：


```sql		
EXECUTE CHANGE_SALARY

```


显示结果为：
		
雇员SCOTT的工资被改为3090 
		
说明：在存储过程的调用中没有传递参数，而是采用了默认值7788和10，即默认雇员号为7788，增加的工资为10。


【训练3】  使用OUT类型的参数返回存储过程的结果。
		
步骤1：登录SCOTT账户。
		
步骤2：在SQL*Plus输入区中输入并编译以下存储过程：
		
```sql
CREATE OR REPLACE PROCEDURE EMP_COUNT(P_TOTAL OUT NUMBER)
	AS
	BEGIN
	SELECT COUNT(*) INTO P_TOTAL FROM EMP;
	END;
```


执行结果为：
		
过程已创建。
		
步骤3：输入以下程序并执行：
		
```sql
DECLARE
	V_EMPCOUNT NUMBER;
	BEGIN
	EMP_COUNT(V_EMPCOUNT);
	DBMS_OUTPUT.PUT_LINE('雇员总人数为：'||V_EMPCOUNT);
	END;
```

显示结果为：
		
雇员总人数为：14
		
PL/SQL 过程已成功完成。
 		  
说明：在存储过程中定义了OUT类型的参数P_TOTAL，在主程序调用该存储过程时，传递了参数V_EMPCOUNT。在存储过程中的SELECT...INTO...语句中对P_TOTAL进行赋值，赋值结果由V_EMPCOUNT变量带回给主程序并显示。


以上程序要覆盖同名的EMP_COUNT存储过程，如果不使用OR REPLACE选项，就会出现以下错误：
		
ERROR 位于第 1 行:
		
ORA-00955: 名称已由现有对象使用。
		
【练习2】创建存储过程，使用OUT类型参数获得雇员经理名。




以上程序要覆盖同名的EMP_COUNT存储过程，如果不使用OR REPLACE选项，就会出现以下错误：
		
ERROR 位于第 1 行:
		
ORA-00955: 名称已由现有对象使用。
		
【练习2】创建存储过程，使用OUT类型参数获得雇员经理名。



【训练4】  使用IN OUT类型的参数，给电话号码增加区码。
		
步骤1：登录SCOTT账户。
		
步骤2：在SQL*Plus输入区中输入并编译以下存储过程：
		
```sql
CREATE OR REPLACE PROCEDURE ADD_REGION(P_HPONE_NUM IN OUT VARCHAR2)
	AS
	BEGIN
	 P_HPONE_NUM:='024-'||P_HPONE_NUM;
END;
```

执行结果为：
		
过程已创建。
		

步骤3：输入以下程序并执行：

```sql
SET SERVEROUTPUT ON
DECLARE
V_PHONE_NUM VARCHAR2(15);
BEGIN
V_PHONE_NUM:='26731092';
ADD_REGION(V_PHONE_NUM);
DBMS_OUTPUT.PUT_LINE('新的电话号码：'||V_PHONE_NUM);
END;
```

显示结果为：
		
新的电话号码：024-26731092
		
PL/SQL 过程已成功完成。
		
说明：变量V_HPONE_NUM既用来向存储过程传递旧电话号码，也用来向主程序返回新号码。新的号码在原来基础上增加了区号024和-。


## 创建和删除存储函数

创建函数，需要有CREATE PROCEDURE或CREATE ANY PROCEDURE的系统权限。该权限可由系统管理员授予。创建存储函数的语法和创建存储过程的类似.


在可执行部分的RETURN(表达式)，用来生成函数的返回值，其表达式的类型应该和定义部分说明的函数返回值的数据类型一致。在函数的执行部分可以有多个RETURN语句，但只有一个RETURN语句会被执行，一旦执行了RETURN语句，则函数结束并返回调用环境。
		
一个存储函数在不需要时可以删除，但删除的人应是函数的创建者或者是拥有DROP ANY PROCEDURE系统权限的人。其语法如下：

```sql
	DROP FUNCTION 函数名；
```


重新编译一个存储函数时，编译的人应是函数的创建者或者拥有ALTER ANY PROCEDURE系统权限的人。重新编译一个存储函数的语法如下：
		
```sql
ALTER PROCEDURE 函数名 COMPILE；
```		

函数的调用者应是函数的创建者或拥有EXECUTE ANY PROCEDURE系统权限的人，或是被函数的拥有者授予了函数执行权限的账户。函数的引用和存储过程不同，函数要出现在程序体中，可以参加表达式的运算或单独出现在表达式中，其形式如下：
		
```sql
变量名:=函数名(...)
```

【训练1】  创建一个通过雇员编号返回雇员名称的函数GET_EMP_NAME。
		

步骤1：登录SCOTT账户。
		

步骤2：在SQL*Plus输入区中输入以下存储函数并编译：
		
```sql
CREATE OR REPLACE FUNCTION GET_EMP_NAME(P_EMPNO NUMBER DEFAULT 7788)
	RETURN VARCHAR2
	AS
	V_ENAME VARCHAR2(10);
	BEGIN
 	SELECT ENAME INTO V_ENAME FROM EMP WHERE EMPNO=P_EMPNO;
RETURN(V_ENAME);
EXCEPTION
 WHEN NO_DATA_FOUND THEN
  DBMS_OUTPUT.PUT_LINE('没有该编号雇员！');
  RETURN (NULL);
 WHEN TOO_MANY_ROWS THEN
  DBMS_OUTPUT.PUT_LINE('有重复雇员编号！');
  RETURN (NULL);
 WHEN OTHERS THEN
  DBMS_OUTPUT.PUT_LINE('发生其他错误！');
  RETURN (NULL);
END;
```



步骤3：调用该存储函数，输入并执行以下程序：

```sql
BEGIN
 DBMS_OUTPUT.PUT_LINE('雇员7369的名称是：'|| GET_EMP_NAME(7369));
 DBMS_OUTPUT.PUT_LINE('雇员7839的名称是：'|| GET_EMP_NAME(7839));
END;
```


## 存储过程和函数的查看

可以通过对数据字典的访问来查询存储过程或函数的有关信息，如果要查询当前用户的存储过程或函数的源代码，可以通过对USER_SOURCE数据字典视图的查询得到。USER_SOURCE的结构如下：

```sql
	DESCRIBE USER_SOURCE
```

【训练1】 查询过程EMP_COUNT的脚本。
		
在SQL*Plus中输入并执行如下查询：

```sql
	select TEXT  from user_source WHERE NAME='EMP_COUNT';
```


		
【训练2】  查询过程GET_EMP_NAME的参数。
		
在SQL*Plus中输入并执行如下查询：
		
```sql
DESCRIBE GET_EMP_NAME
```
		

【训练2】  查询过程GET_EMP_NAME的参数。
		
在SQL*Plus中输入并执行如下查询：
		
```sql
DESCRIBE GET_EMP_NAME
```		

【训练3】  在发生编译错误时，显示错误。
		
```sql
SHOW ERRORS
```		
 		
说明：查询一个存储过程或函数是否是有效状态(即编译成功)，可以使用数据字典USER_OBJECTS的STATUS列.


【训练4】  查询EMP_LIST存储过程是否可用：
		
```sql
SELECT STATUS FROM USER_OBJECTS WHERE OBJECT_NAME='EMP_LIST';
```

当一个存储过程编译成功，状态变为VALID，会不会在某些情况下变成INVALID。结论是完全可能的。比如一个存储过程中包含对表的查询，如果表被修改或删除，存储过程就会变成无效INVALID。所以要注意存储过程和函数对其他对象的依赖关系。
		

如果要检查存储过程或函数的依赖性，可以通过查询数据字典USER_DENPENDENCIES来确定，该表结构如下：
		
```sql
DESCRIBE USER_DEPENDENCIES;
```


还有一种情况需要我们注意：如果一个用户A被授予执行属于用户B的一个存储过程的权限，在用户B的存储过程中，访问到用户C的表，用户B被授予访问用户C的表的权限，但用户A没有被授予访问用户C表的权限，那么用户A调用用户B的存储过程是失败的还是成功的呢？答案是成功的。如果读者有兴趣，不妨进行一下实际测试。

