---
title: sql五种约束
date: 2017-11-23 18:58:29
comments: ture
categories:
	- 数据库
tags:
	- SQL serve
---

SQL server的五种约束：

- 主键约束（Primay Key Coustraint）
- 外键约束（Foreign Key Counstraint）
- 唯一约束（Unique Counstraint）
- 检查约束（Check Counstraint）
- 默认约束（Default Counstraint）

<!-- more -->

-------------------------

## 主键约束（Primay Key Coustraint）

- 直接添加：
		

		create table 表名
		(
		字段名 数据类型 primary key
		);


- 联合主键：


		create table 表名
		(
		字段名1 数据类型1
		字段名2 数据类型2
		constraint PK_字段名1_字段名2       可以省略此行（约束名称）
		primary key(字段名1,字段名2)
		);


- alter添加:


		1.alter table 表名1 modify 字段名1 类型名1 primary key;
		2.alter table 表名2 add primary key(字段名2);
		3.alter table 表名3 add constrain 约束名 primary key(字段名3);


- 删除主键约束


		alter table 表名 drop primary key;



## 默认约束（Default Counstraint）

- 直接添加：


		create table 表名
		(
		字段名 类型名 default(条件)
		)	


- alter添加:


		alter table 表名1 modify 字段名1 类型名1 default 条件1;
		alter table 表名2 add constarint 字段名2 类型名 default 条件2 ;


- 改变默认约束：


		alter table 表名1 change 字段名1 字段名1 类型名1 default 条件1;


- 删除默认约束：


		alter table 表名1 modify 字段名1 类型名1;
		alter table 表名2 change 字段名2 字段名2 类型名2



## 唯一约束（Unique Counstraint）

- 直接添加：
		
		
		create table 表名1 （字段名1 类型名1 unique）;
		create table 表名2 （字段名2 类型名2，unique key(字段2)）;


- alter添加:


		alter table 表名1 modify 字段名1 类型名1 unique;
		alter table 表名2 add unique(字段名2);
		alter table 表名3 add unique key (字段名3);
		alter table 表名4 add constarint 约束名4 unique (字段名4);
		alter table 表名5 add constarint 约束名5 unique key (字段名5);


- 改变唯一约束：


		alter table 表名1 change 字段名1 字段名1 类型名1 unique;
		


- 删除唯一约束：


		alter table 表名1 drop index 字段名1;


## 检查约束（Check Counstraint）

- 直接添加：


		create table 表名1 
		(
		字段名1 类型名1 check (条件）
		);



- alter添加:



		alter table 表名1 add constarint 约束名1 default （字段名1条件）;









-------------------------



作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。