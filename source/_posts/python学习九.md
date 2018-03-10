---
title: python学习九
date: 2018-01-23 21:31:06
categories:
	- python
tags:
	- python


---
{% note info %}
- string模块
- ascill
- 数学模块 
- 模块提供的常见值
- 随机模块
{% endnote %}
<!-- more -->

---------

## string模块

import string

- ascill_letters 获取所有ascilll码中字母字符的字符串(包含大写和小写)
- ascill_uppercase 获取所有ascill码中的大写英文字母
- ascill_lowercase 获取所有ascill码中的ihpg英文字母
- digits 获取所有的10进制数字字符
- octdigits 获取所有的8进制数字字符
- hexdigits 获取所有16进制数字字符
- printable 获取所有可以打印的字符
- whitespace 获取所有空白字符
- punctuation 获取所有的标点符号

## ascii

美国标准信息交换代码

定制了128个常用字符，主要是英文，数字，标点符号及键盘中其他按键对应的整数值。

python中与ascill码相关的两个函数：

	chr() 将ascill编码转化为字符

		格式：chr(ascill码)
		返回值：字符

	ord() 将字符转化为对应的ascii码

		格式：ord(字符)
		返回值：ascill码

## 数学模块 

import math

ceil() 向上取整操作

	格式：math.ceil(数值)
	返回值：整型

floor() 向下取整操作

	格式：math.floor(数值)
	返回值：整型

round() 四舍五入操作

	格式：round(数值)
	返回值：整数
	注意：该函数不在math模块当中。

pow() 计算一个数值的N次方

	格式：math.pow(底数，幂)
	返回值：浮点类型
	注意：该操作相当于**运算结果为浮点型

sqrt() 开平方

	格式：math.sqrt(数值)
	返回值：浮点型

fabs() 对一个数值获取其绝对值操作

	格式：math.fabs(数值)
	返回值：浮点数

abs() 对一个数值获取其绝对值操作

	格式：abs(数值)
	返回值：可能是整数能浮点数
	注意：abs() 它是内建函数，同时返回值根据原类型决定

modf() 将一个浮点拆成整数和小数部分组成的元组

	格式：math.modf(数值)
	返回值：元组 （小数部分，整数部分）

copysign() 将第二个的正负号复制给第一个数

	格式：math.copysign(值1，值2)
	返回值：值1  符号是值2的正负

fsum() 将一个序列的数值进相加求和

	格式：math.fsum(序列)
	返回值：浮点数

sum() 将一个序列的数值进行相加求和

	格式：sum(序列)
	返回值：数值类型
	
## 模块提供的常见值

pi 圆周率

	3.14159265358793

e 自然数

	2.718181828459045

## 随机模块

import random

random() 获取0-1之间的随机小数包含0不包含1

	格式：random.random()
	返回值：浮点数

choice() 随机获取列表中的值

	格式：random.choice(序列)
	返回值：序列中的某个值

shuffle() 随机打乱序列

	格式：random.shffle(序列)
	返回值：打乱顺序的序列

randrage() 获取指定范围内指定间隔的随机整数

	格式：random.ranges(开始值，结束值[,间隔值])
	返回值：整数

uniform() 随机获取指定范围内的所有数值包含小数

	格式：random.uniform(开始值，结束值)
	返回值：随机返回范围内的所有数值（浮点型）

------------------

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
