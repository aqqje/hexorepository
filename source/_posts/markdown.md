---
title: markdown
date: 2017-11-16 19:28:49
comments: ture
categories:
	- markdown
tags:
	- markdown
	- hexo

---

![](https://github.com/aqqje/Personal-repository/raw/master/images/markdownhead%20.jpg "markdownhead")<br/>
Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。

<!-- more -->

### 1.<span id = "斜体和粗体">斜体和粗体</span>
	代码：
		1.*斜体* 或 _斜体_
		2.**粗体**
		3.***加粗斜体***
		4.~~删除线~~

###### 显示效果:
- *这是一段体* 或 _这是一段体_
- **这是一段粗体**
- ***这是一段加粗斜体***
- ~~这是一段删除~~


### 2.分组标题
###### 第一种写法：
		1.这是一个一级标题
		2.============================
		3.               
		4.这是一个二级标题
		5.--------------------------------------------------

###### 第二种写法：
		1.# 一级标题
		2.## 二级标题
		3.### 三级标题
		4.#### 四级标题
		5.##### 五级标题
		6.###### 六级标题

### 3.超链接
Markdown 支持两种形式的链接语法： 行内式和参考式两种形式，行内式一般使用较多。

###### 3.1. 行内式

语法说明：<br/>
 []里写链接文字，()里写链接地址, ()中的”“中可以为链接指定title属性，title属性可加可不加。title属性的效果是鼠标悬停在链接上会出现指定的 title文字。[链接文字](链接地址 “链接标题”)’这样的形式。链接地址与链接标题前有一个空格。
	
		1.梦不若星辰[aqqje](https://github.com/aqqje)
		2.梦不若星辰[aqqje](https://github.com/aqqje "aqqje")

###### 显示效果:
梦不若星辰[aqqje](https://github.com/aqqje)

梦不若星辰[aqqje](https://github.com/aqqje "aqqje")


###### 3.2. 参考式
语法说明：<br/> 
参考式链接分为两部分，文中的写法 [链接文字][链接标记]，在文本的任意位置添加[链接标记]:链接地址 “链接标题”，链接地址与链接标题前有一个空格。

如果链接文字本身可以做为链接标记，你也可以写成[链接文字][] 
[链接文字]：链接地址的形式，见代码的最后一行
		

		1.[小米][1] [华为][2] [vivo][3] [oppo][4]
		
		3.[1]:https://www.mi.com
		4.[2]:https://www.huawei.com
		5.[3]:https://www.vivo.com.cn/
		6.[4]:https://www.oppo.com/

###### 显示效果:
[小米][1] [华为][2] [vivo][3] [oppo][4]
		
[1]:https://www.mi.com
[2]:https://www.huawei.com
[3]:https://www.vivo.com.cn/
[4]:https://www.oppo.com/


###### 3.3. 自动链接
语法说明：<br/>
Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用<>包起来， Markdown 就会自动把它转成链接。一般网址的链接文字就和链接地址一样。
		
		1.<https:www.baibu.com/>
		2.<1042136232@qq.com>

###### 显示效果:
<https://www.baibu.com/>

<1042136232@qq.com>


###### 4. 锚点


###### 5. 列表
语法说明：<br/>
使用 *，+，- 表示无序列表。


		1. - 无序列表项 一
		2. - 无序列表项 二
		3. - 无序列表项 三

###### 显示效果:
- 无序列表项 一
- 无序列表项 二
- 无序列表项 三


###### 5.2. 有序列表
语法说明：<br/>
有序列表则使用数字接着一个英文句点。

		1. 1. 有序列表项 一
		2. 2. 有序列表项 二
		3. 3. 有序列表项 三

###### 显示效果:
1. 有序列表项 一
2. 有序列表项 二
3. 有序列表项 三


###### 5.3. 定义型列表
语法说明：<br/>
定义型列表由名词和解释组成。一行写上定义，紧跟一行写上解释。解释的写法:紧跟一个缩进(Tab)


		1.梦不若星辰1
		2.：		相见亦无事，不遇长相思（左侧有一个可见的冒号和四个不可见的空格）
		3.代码2
		4.:		梦不若星辰2（左侧有一个可见的冒号和四个不可见的空格）
		5		梦不若星辰3（左侧有八个不可见的空格）

###### 显示效果:

梦不若星辰1

：		相见亦无事，不遇长相思（左侧有一个可见的冒号和四个不可见的空格）

代码2

:		梦不若星辰2（左侧有一个可见的冒号和四个不可见的空格）

		梦不若星辰3（左侧有八个不可见的空格）

###### 5.3. 列表缩进
语法说明：<br/>
列表项目标记通常是放在最左边，但是其实也可以缩进，最多 3 个空格，项目标记后面则一定要接着至少一个空格或制表符

		1.*   轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。
		2.*   那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。 
		3.*   软泥上的青荇， 油油的在水底招摇； 在康河的柔波里， 我甘心做一条水草！
		3.*   那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。 

###### 显示效果:
*   轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。
*   那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。 
*   软泥上的青荇， 油油的在水底招摇； 在康河的柔波里， 我甘心做一条水草！
*   那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。 

###### 5.4. 包含段落的列表
语法说明：<br/>
列表项目可以包含多个段落，每个项目下的段落都必须缩进 4 个空格或是 1 个制表符

		1.*   轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。
		2.	那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。 
		3.	软泥上的青荇， 油油的在水底招摇； 在康河的柔波里， 我甘心做一条水草！
		3.	那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。 

###### 显示效果:
*   轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。
  
	那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。 

	软泥上的青荇， 油油的在水底招摇； 在康河的柔波里， 我甘心做一条水草！

	那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。

###### 5.5. 包含引用的列表
语法说明：<br/>
如果要在列表项目内放进引用，那 > 就需要缩进
		
		1.*		阅读的方法
		2.
		3.	    >打开书本
		4.      >打开电灯

###### 显示效果:
*		阅读的方法

	  >打开书本
	  
      >打开电灯

###### 5.6. 包含代码区块的引用
语法说明：<br/>
如果要放代码区块的话，该区块就需要缩进两次，也就是 8 个空格或是 2 个制表符。

- 一列表项包含一个列表区块


		<1.代码写在这>


###### 5.7. 一个特殊情况
在特殊情况下，项目列表很可能会不小心产生，像是下面这样的写法：

		1.2017.Hello world!

###### 显示效果:
2017. Hello world!

换句话说，也就是在行首出现数字-句点-空白，要避免这样的状况，你可以在句点前面加上反斜杠
		1.2017\.Hello world!

###### 显示效果:
2017\.Hello world!

###### 6. 引用

语法说明：<br/>
引用需要在被引用的文本前加上>符号
		
		> 这是一个有两段文字的引用,
		> 无意义的占行文字1.
		> 无意义的占行文字2.
		> 
		> 无意义的占行文字3.
		> 无意义的占行文字4.

###### 显示效果:

 > 这是一个有两段文字的引用.
 > 无意义的占行文字1.
 > 无意义的占行文字2.
 > 
 > 无意义的占行文字3.
 > 无意义的占行文字4

##### Markdown 也允许你偷懒只在整个段落的第一行最前面加上 >
 
> 这是一个有两段文字的引用.
  无意义的占行文字1.
  无意义的占行文字2.
  
> 无意义的占行文字3.
  无意义的占行文字4

###### 6.1. 引用的多层嵌套

语法说明：<br/>
区块引用可以嵌套（例如：引用内的引用），只要根据层次加上不同数量的 >

		1.>>> 请问 Markdwon 怎么用？ - 小白
		2.>
		3.>> 自己看教程！ - 愤青
		4.>
		5.> 教程在哪？ - 小白

###### 显示效果:
>>> 请问 Markdwon 怎么用？ - 小白
>
>> 自己看教程！ - 愤青
>
> 教程在哪？ - 小白



###### 6.2. 引用其它要素
语法说明：<br/>
引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等。

		1.> 1.   这是第一行列表项。
		2.> 2.   这是第二行列表项。
		3.> 
		4.> 给出一些例子代码：
		5.> 
		6.>     return shell_exec("echo $input | 7.7.$markdown_script");

###### 显示效果:
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");

###### 7. 插入图像
图片的创建方式与超链接相似，而且和超链接一样也有两种写法，行内式和参考式写法。

###### 7.1. 行内式

语法说明：<br/>
![图片Alt](图片地址 “图片Title”)
		
		1.![神界传说](https://github.com/aqqje/images/raw/master/markdown_test/0.jpeg "神界传说")

###### 显示效果:
![神界传说](https://github.com/aqqje/images/raw/master/images/0.jpeg "神界传说")

###### 7.2. 参考式
语法说明：<br/>

		1.![神界传说][神界传说]
		2.
		3.[神界传说]:https://github.com/aqqje/images/raw/master/markdown_test/0.jpeg "神界传说" 

###### 显示效果:
![神界传说][神界传说]

[神界传说]:https://github.com/aqqje/images/raw/master/images/0.jpeg "神界传说" 
		
###### 8. 内容目录
在段落中填写 [TOC] 以显示全文内容的目录结构。

###### 9. 注脚 
语法说明：<br/>
在需要添加注脚的文字后加上脚注名字[^注脚名字],称为加注。 然后在文本的任意位置(一般在最后)添加脚注，脚注前必须有对应的脚注名字。

注意：经测试注脚与注脚之间必须空一行，不然会失效。成功后会发现，即使你没有把注脚写在文末，经Markdown转换后，也会自动归类到文章的最后。

		使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用 Leanote[^Le] 编辑器进行书写。

		[^1]:Markdown是一种纯文本标记语言
		
		[^2]:HyperText Markup Language 超文本标记语言
		
		[^Le]:开源笔记平台，支持Markdown和笔记直接发为博文
		
###### 显示效果: 
使用 Markdown[^1] 可以效率的书写文档, 直接转换成 HTML[^2],你可以使用 Leanote[^3] 编辑器进行书写。


[^1]:Markdown是一种纯文本标记语言

[^2]:HyperText Markup Language 超文本标记语言

[^3]:开源笔记平台，支持Markdown和笔记直接发为博文


###### 10. LaTeX 公式
###### 10.1. $ 表示行内公式
		
		1.质能守恒方程可以用一个很简洁的方程式 $E=mc^2$ 来表达。

###### 显示效果:
质能守恒方程可以用一个很简洁的方程式 $E=mc^2$ 来表达。

###### 10.2 $$ 表示整行公式

		1.$$\sum_{i=1}^n a_i=0$$
		2.$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$
		3.$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

###### 显示效果:
$$\sum_{i=1}^n a_i=0$$
$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$
$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

访问 [MathJax](https:) 参考更多使用方法

###### 11. 流程图

		1.flow
		2.st=>start: Start:>https://www.zybuluo.com
		3.io=>inputoutput: verification
		4.op=>operation: Your Operation
		5.cond=>condition: Yes or No?
		6.sub=>subroutine: Your Subroutine
		7.e=>end
		8.
		9.st->io->op->cond
		10.cond(yes)->e
		11.cond(no)->sub->io

###### 显示效果：

flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end
st->io->op->cond
cond(yes)->e
cond(no)->sub->io

更多语法参考：[流程图语法参考]()


###### 12 .表格
语法说明：<br/>
1.不管是哪种方式，第一行为表头，第二行分隔表头和主体部分，第三行开始每一行为一个表格行。
2.列于列之间用管道符|隔开。原生方式的表格每一行的两边也要有管道符。
3.第二行还可以为不同的列指定对齐方向。默认为左对齐，在-右边加上:就右对齐。

		
		简单方式写表格：
		1学号|姓名|分数
		2.-|-|-
		3.小明|男|75
		4.小红|女|79
		5.小陆|男|92
		
###### 显示效果：
学号|姓名|分数
-|-|-
小明|男|75
小红|女|79
小陆|男|92

		原生方式写表格：
		1.|学号|姓名|分数|
		2.|-|-|-|
		3.|小明|男|75|
		4.|小红|女|79|
		5.|小陆|男|92|

###### 显示效果：
|学号|姓名|分数|
|-|-|-|
|小明|男|75|
|小红|女|79|
|小陆|男|92|

		为表格第二列指定方向：
		1.产品|价格
		2.-|-:
		3.Leanote 高级账号|60元/年
		4.Leanote 超级账号|120元/年

###### 显示效果：
产品|价格
-|-:
Leanote 高级账号|60元/年
Leanote 超级账号|120元/年

###### 13. 分隔线
语法说明：<br/>
在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线。

		1.* * *
		2.
		3.***
		4.
		5.*****
		6.
		7.- - -
		8.
		9.---------------------------------------

###### 显示效果：
* * *

***

*****

- - -

---------------------------------------



###### 14. 代码
对于程序员来说这个功能是必不可少的，插入程序代码的方式有两种，一种是利用缩进(Tab), 另一种是利用”`”符号（一般在ESC键下方）包裹代码
语法说明：<br/>
1.插入行内代码，即插入一个单词或者一句代码的情况，使用`code`这样的形式插入。
2.插入多行代码，可以使用缩进或者“` code “`

		1.C语言里的函数 `scanf()` 怎么使用？

###### 显示效果：
C语言里的函数`scanf()`怎么使用？

###### 8.2. 缩进式多行代码
缩进 4 个空格或是 1 个制表符

一个代码区块会一直持续到没有缩进的那一行（或是文件结尾）。


   		#include <stdio.h>
    	int main(void)
    	{
        	printf("Hello world\n");
    	}

###### 显示效果：
   	#include <stdio.h>
    int main(void)
    {
        printf("Hello world\n");
    }

###### 8.3. 用六个`包裹多行代码

		```
		#include <stdio.h>
		int main(void)
		{
 		   printf("Hello world\n");
		}
		、、、

###### 显示效果：
```
#include <stdio.h>
int main(void)
{
    printf("Hello world\n");
}
```

###### 8.4. HTML 原始码
在代码区块里面， & 、 < 和 > 会自动转成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入范例用的 HTML 原始码，只需要复制贴上，剩下的 Markdown 都会帮你处理

		<table>
    		<tr>
        		<th rowspan="2">值班人员</th>
        		<th>星期一</th>
        		<th>星期二</th>
        		<th>星期三</th>
    		</tr>
    		<tr>
        		<td>李强</td>
        		<td>张明</td>
        		<td>王平</td>
    		</tr>
		</table>

###### 显示效果：
<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>


-------------------------------

![](https://github.com/aqqje/Personal-repository/raw/master/images/markdowntail.png "markdowntail")

- [参考1](http://wowubuntu.com/markdown/)
- [参考2](http://blog.csdn.net/witnessai1/article/details/52551362)

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。