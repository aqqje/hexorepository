---
title: python学习五
date: 2018-01-16 15:10:56
categories:
	- python
tags:
	- python


---

- 函数文档
- 定义函数的文档
- 变量的作用域
- 内部函数

<!-- more -->

---------

## 函数文档 

函数文档就是用来查看当前函数相关信息介绍的一个特定格式而已

查看函数文档的方法

	help(函数名称)

		直接输出显示函数文档的内容字符串

	函数名称.__doc__

		直接输出显示函数文档的内容元字符串(转义字符不转义)

	
## 定义函数的文档

	def 函数名称(参数...)
		'''
			在此处声明函数文档
		'''
		函数功能代码...
		函数功能代码...
		...

注意：函数文档的作用是函数进行说明，便于阅读和快速掌握函数的使用，通常函数文档需要具有以下信息：

	函数的作用
	函数的参数介绍(个数，数据类型)
	函数的返回值(数据和类型)


## 变量的作用域

变量的作用域就是指变量的有有效范围

变量按照作用范围分为2类：

	全局变量：
		在函数外部声明的变量就是全局（在函数内部需要global关键字才可以使用）

	局部变量：
		在函数内部声明的变量就是局部变量


	全局变量和局部变量的有效范围：
		1.全局变量在函数外部也可以正常使用
		2.全局变量在函数内部也可以正常使用(需要global声明)
		3.局部变量在函数内部可以正常使用
		4.局部变量在函数外部不可以被访问

**global关键字**

将局部变量提升为全局变量

	格式：
		def 函数名():
			# 提升局部变量为全局变量
			global 变量名
			
			函数的其他功能代码...
			...

注意：只有在函数内部使用global关键字对变量进行全局声明，该变量才是一个完整的全局变量，在函数内外可以进行任意获取修改删除等操作。	


## 内部函数

在函数内部声明的函数就是内部函数

	特征：
		1.内部函数的本质就是局部变量(函数就是一个变量)
		2.内部函数在函数外部不可以直接调用
		3.内部函数在函数内部调用必须定义在内部函数之后可以调用

## 闭包(函数式开发使用)

想办法将局部变量引入到全局环境中可以使用，这就是闭包操作

	闭包方法1：
		def 函数名():
			局部变量1
			局部变量2
			...
		def 内部函数1()
			pass
		def 内部函数2()
			pass
		...
	
		return (局部变量1，局部变量2...内部函数2...)

	闭包方法2：
		def 函数名():
			局部变量1
			局部变量2
			...
		def 内部函数1()
			pass
		def 内部函数2()
			pass
		...
	
		# 获取/收集所有需要进行闭包操作的内部函数和变量
		def all():
			return (局部变量1，局部变量2...内部函数2...)

		return all

闭包的优缺点：
	优点：
		1.可以方便的进行函数式编程，组织程序代码
		2.使用局部变量和内部函数在外部可以访问

	缺点：
		1.闭包操作会导致整个函数的内部环境，被长久保存，占用大量内存

	闭包环境查看：
		\_\_closure\_\_
	用于查询当前闭包操作所使用的环境中的变量和内部函数等信息

```

# 实现闭包操作1

# 全局环境

# 函数局部环境

def py():
    # 局部变量
    boy = 'aqqje'
    girl = 'yjgmy'
    # 内部函数
    def boyName():
        print(boy+'123456')
    def girlName():
        print(girl+'987654')
    # 通过return语句和容器数据将局部变量和内部函数返回
    return (boy,girl,boyName,girlName)


# 调用函数获取所有的返回值

result = py()
print(result)
# 高级写法

men,wemen,name1,name2 = result

# 初级写法

#men = result[0]
#wemen = result[1]
#name1 = result[2]
#name2 = result[3]

name2()
```

结果如下：

```
('aqqje', 'yjgmy', <function py.<locals>.boyName at 0x01DFD780>, <function py.<locals>.girlName at 0x01E5DA98>)
yjgmy987654
```

```
# 实现闭包操作2

# 全局环境

# 函数局部环境

def py():
    # 局部变量
    boy = 'aqqje'
    girl = 'yjgmy'
    # 内部函数
    def boyName():
        print(boy+'123456')
    def girlName():
        print(girl+'987654')
    def getall():
        return [boy,girl,boyName,girlName]
    return getall

result = py()
print(result)
allvar = result()
print(allvar)
```

```
# 实现闭包操作3

# 全局环境
allinner = None

# 函数局部环境

def py():
    # 局部变量
    global allinner
    boy = 'aqqje'
    girl = 'yjgmy'
    # 内部函数
    def boyName():
        print(boy+'123456')
    def girlName():
        print(girl+'987654')
    def getall():
        return [boy,girl,boyName,girlName]
    allinner = getall()

py()
n,a,b,c = allinner
print(n)
print(a)
b()
c()
```
```
# 闭包环境查看：
def demo():
        x = 6
        y = 3
        def inner1():
            pass
        def inner2():
            pass
        def all():
            return [y,inner1,inner2]
        return all

bb = demo()
print(bb.\_\_closure\_\_)
```
结果如下：
```
(<cell at 0x01E53690: function object at 0x01346A08>, <cell at 0x01E53670: function object at 0x01DFD780>, <cell at 0x01E536D0: int object at 0x5B382AA0>)

```




------------------


作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

