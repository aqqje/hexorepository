---
title: ORACLEU总结
date: 2018-03-04 22:25:03
comments: ture
categories:
	- 数据库
tags:
	- oracle
---
![](https://github.com/aqqje/Personal-repository/raw/master/images/oracle1.jpg "oracle1")<br/>
{% note info %}
- 对之前的学习做更深入的巩固，及加深体会，扎实基础！
{% endnote %}
<!-- more -->

---------
## 使用system用户解锁scott用户

```sql
alter user scott account unlock;
alter user scott identified by tiger;	
```

## 创建用户并授权

```sql
create user newUser identified by newPwd;
grant connect to newUser;
grant create table to newUser;
grant create proedure to newUser;
grant unlimited tablespace to newUser;
```

---------------------

## <span id="inline-yellow">创建表空间</span>

```
create tablespace emp_tablespace
datafile 'c:emp_tablespace.dbf'
size 100mb
```

## newUser复制scott的emp表

```
grant select[all] on scott.emp to newUser; --system  account
create table newEmp as select * from scott.emp;
```

## 显示EMP表的行号

``
select rownum from emp;
```

## <span>显示emp表每一个雇员的总收入(工资+奖金)</span>

```sql
update emp set comm = 0 from emp where comm is null;
select ename,(sal+comm) [as] 总收入 from emp;
```

***<span id="inline-red">在列别名上使用双引号。（当你的别名为关键字或别名中有特殊符号时需要加双引号）</span>***

***<span id="inline-blue">'5'+    5  结果为 10        '5'||  5 结果为 '55'</span>***


***ASC 表示升序 (可省略)      Desc 表示降序(不可省略)***

## 查询emp表雇员姓名和工资，并按工资从小到大(从大到小)排序

```sql
select ename, sal from emp order by sal asc(desc)
```

## 查询emp表雇员信息，先按部门从小到大排序，再按雇佣时间的先后排序
```sql
select  * from emp order by deptno, hiredate;
```

## <span id="inline-yellow">使用DISTINCT消除重复行显示</span>

```sql
select distinct job from emp;
```

## 部门ACCOUNTING所在的城市为NEW YORK

```sql
select '部门' || dname || '所在的城市为' || loc from dept;
```

------------------------

## 显示雇员名称中包含“S”的雇员名称及名称长度
```sql
-- 方法一：
select ename, length(ename) from emp where like '%s%';
-- 方法二：
select ename, length(ename) from emp where instr(ename, 's', 1, 1) > 0;
```

## <span id="inline-yellow">显示名称以“W”开头的雇员，并将名称转换成以大写开头</span>

```sql
select initcap(ename) from emp where substr(ename, 1, 1) = 'W';
```

## 如果不知道表的字段内容是大写还是小写，可以转换后比较

```sql
select * from emp where lower[upper](ename) in 'blake'['BLAKE'];
```
## 3.3434保留两位小数;

```sql
select round(3.3434, 2) from dual
```

-------------------

## 判断用户的角色是否为SYSDBA

```sql 
select userenv('ISDBA') from dual;
```

## <span id="inline-yellow">对部门表的部门名称和城市名进行转换</span>

```sql
select decode(dname, 'ACCOUNTING', '统计部', 'RESEARCH', '研发部', 'SALES', '销售部', 'OPERATIONS', '其它部门') 部门,
decode(loc, 'NEW YORK', '纽约', 'DALLAS', '达拉斯', 'CHICAGO', '芝加哥', 'BOSTON', '波士顿') 城市 
from dept;
```
## <span id="inline-yellow">显示2008年的8月8日为星期几</span>

```sql 
select to_char('08-08月-2008', 'DY') from dual;
```

## 显示从本年1月1日开始到现在经过的天数(当前时间取SYSDATE的值)

```sql
select sysdate - to_date('2018-1-1', 'YYYY-MM-DD') from dual;
```

-----------------------

## <span id="inline-yellow">显示emp表雇员名称和雇员的经理名称</span>

```sql
select worker.ename || '雇员的经理:' || manager.ename 
from emp worker, emp manager
where worker.mgr = manager.empno
```

## 相等连接（内连接）ename,job,sal,comm,emp.deptno,dname

```sql
--方法一：
select ename, job, sal, comm, emp.deptno, dname
from emp, dept where emp.deptno = dept.deptno;
--方法二：
select ename, job, sal, comm, emp.deptno, dname 
from emp inner join dept on emp.deptno = dept.deptno;
```
## 求各部门平均工资的最高值

```sql
select deptno, max(ave(sal)) from emp group by deptno;
```

## 统计各部门的最高工资，排除最高工资小于3000的部门

```sql
select deptno, max(sal) from emp 
group by deptno
haning max(sal) > 3000; 
```

## 查询雇员表中排在第6～9位置上的雇员

```sql
select ename, sal from (select rownum as num, ename, sal from emp where rownum <= 9 )
where num >= 6
```

## 查询工资低于任何一个“CLERK”的工资的雇员信息

```sql
select ename, job, sal from emp 
where sal < any(select sal from emp where lower(job) = 'clerk')
and lower(job) <> 'clerk'
```
## 创建从2000起始，增量为1 的序列abc：

```sql
create sequence abc increment by 1 start with 2000
maxvalue 99999 cycle nocache;
```

## 创建按成绩分区的考生表，共分为3个区：
```sql
create table student(
id varchar(5),
name varchar(30)
score number(3)
)

partition by range(成绩)(
partition a less than (300) tablespace users,
partition b less than (600) tablespace users,
partition c less than (maxvalue) tablespace
);
```
## 创建emp表的经理视图(只读)
```sql
creat or replace view manager
as 
select * from emp where lower(job) = "manager" 
with read only;
```








