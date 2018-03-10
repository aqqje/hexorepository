---
title: python学习十一
date: 2018-01-25 14:34:36
categories:
	- python
tags:
	- python


---

- 文件操作

<!-- more -->

---------

## 文件操作

1.open() 打开或者创建一个文件

	格式：open('文件路径'，'打开模式')
	返回值：文件io对象

	打开模式一共N种：

		w模式： 写模式write    文件不存在时会创建文件，如果文件已存在则清空文件
		r模式： 读模式read     文件不存在就报错，存在则准备读取文件
		a模式： 追加模式append 文件不存在则新建，文件存在则在文件末尾追加内容
		x模式： 抑或模式xor    文件存在则报错，文件不存在则新建文件 
		b模式： 二进制模式：    辅助模式不能单独使用
		+模式： 增强模式plus:  也是辅助模式不能单独使用 



```

# w模式 每次打开都清空文件内容
fp = open("myfile.txt","w")
result = fp.write("qwwerty")
fp.close()

# r模式 只读

fp = open("myfile.txt","r")
result = fp.read()
fp.close()
print(result)

'''
结果如下：
qwwerty
'''

# a模式 原有文件内容上追加新内容

fp = open("myfile.txt","a")
result = fp.write("1234")
fp.close()

'''
文件内容如下：
qwwerty1234
'''

'''
# x模式 抑或模式 文件存在则报错，文件不存在则新建文件
 
fp = open("myfile3.txt","x")
result = fp.write("qwwerty")
fp.close()
'''

# b模式

fp = open('myfile.txt','rb') #b 二进制模式 binary,辅助使用，不能单个使用
result = fp.read()
print(result)

```

2.readline() 读取一行文件

	格式：文件io对象.readline()
	返回值：一行内容的字符串
	
	格式：文件io对象.readline(字符长度)
	返回值：一行内容的字符串

	注意：字符长度 < 当前内容，则读取指定长度的字符串，并且一次再读取还是这个一行中获取没有读取内容。

	字符长度 >= 当前行内容，直接读取当前行

3.readlines() 将文件中的内容读取到序列当中

	格式：文件io对象.readlines()
	返回值：列表

	格式：文件io对象.readlines(字符长度)
	返回值：列表

	注意：读取的行数由字符长度决定，如果字符长度读取了N行后，还剩下没有读取，则直接读取下一行进来

4.writelines() 将序列写入文件中

	格式：文件io对象.writelines(序列)
	返回值：None

5.truncate() 字符串截取操作

	格式：文件io对象.truncate(字节长度)
	返回值：截取的字节长度
6.tell() 查看当前指针(光标)的位置

7.seek() 调整指针的位置

	格式：文件io对象.seek(N) 将指针直接调整到N的位置，从头计算第N个位置
	返回值：指针的位置

	格式：文件io对象.seek(偏移位置，参考点方式)
	返回值：指针的位置
	
	参考点方式：

	0 从文件的最开头计算偏移
	1 从文件的当前指针位置开始计算偏移
	2 从文件末尾开始计算偏移

## os模块

os 操作系统的简称（对操作系统进行操作）

导入os模块

	import os

1.os模块的函数

- getcwd() 获取当前工作目录

	格式：os.getcwd()
	返回值:路径字符串

- chdir() 修改当前工作目录

	格式：os.chdir()
	返回值：None

- listdir() 获取指定文件夹中的所有文件和文件夹组成的列表 

	格式：os.listdir(目录路径)
	返回值：目录中内容名称的列表 

- mkdir() 创建一个目录/文件夹

	格式：os.mkdir(目录路径)
	返回值：None

- rmdir() 删除一个目录/文件夹(必须空文件)

	格式：os.rmdir(目录路径)
	返回值：None

- makedirs() 递归创建目录

	格式：os.makedirs(c:/d/c/f)(c:/d,c:d/c 都不存在)
	返回值：None

- removedirs() 递归删除目录

	格式：os.removedirs(c:d/c/f)(c:/d/,c:d/c,c:d/c/f 都必须为空)
	返回值：None

- rename() 修改文件和文件夹

	格式:os.rename(源文件或文件夹，目标文件或文件夹)
	返回值：None

- stat 获取文件信息

	格式：os.rename(文件路径)
	返回值：包含文件信息的元组

- system() 执行系统命令

	格式：os.system(系统命令)
	返回值：整型
	注意：慎用！

- getenv() 获取系统环境变量

	格式：os.getenv(环境变量名)
	返回值：字符串 

- putenv() 增加系统环境变量

	格式：os.putenv(环境变量名称，值)
	返回值：无

- exit() 退出当前执行命令，直接关闭当前操作

	格式：exit()
	返回值：无

- os.environ 可以直接获取所有环境变量的信息组成的字典，如果希望更改环境变量，并且可以查询得到，就需要对os.environ进行操作

- os.path 代表一个子模块

- os.curdir 表示当前目录

- os.pardir 表示上一层文件夹的路径

- os.name 当前系统的内核名称 win -> nt  linux/unix -> posix

- os.sep 当前系统的路径分割体符号 win -> \  linux/nuin -> /	

- os.extsep 当前系统中文件名和后缀之间的分割符号，所有系统都是

- os.linesep os.linesep 当前系统的换行符号 win -> \r\n  linux/unix  ->  \n

## os函数

- abspath() 将一个相对路径转化为绝对路径

	格式：os.path.abspath(相对路径)
	返回值：绝对路径字符串

- basename() 获取路径中的文件夹或者文件名称（只要路径的最后一部分）
 
- dirname() 获取路径中的路径部分（出去最后一部分） 

------------------

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。