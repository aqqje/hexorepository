---
title: python学习七
date: 2018-01-22 15:45:09
categories:
	- python
tags:
	- python


---

- requests库
- beautiful soup库
- 基于bs4库的HTML内容的遍历方法
- 简写形式
- find的扩展方法

<!-- more -->

---------

## requests库

1.安装requests库

	打开cmd -> pip install requests

2.操作实例

```
# 引入requests库
import requests
# 设置爬取的网页
r = requests.get("http://www.baidu.com")
# 打印爬取状态码
print(r.status_code)
# 设置编码格式 
r.encoding = "utf-8"
# 打印爬取网页的html代码
print(r.text)

```

3.requests库的七个主要方法

方法|说明
-|-
requests.request()|构造一个请求，支撑以下各方法的基础方法
requests.get()|获取HTML网页的主要方法，对应于HTTP的GET
requests.head()|获取HTML网页头信息的方法，对应HTTP的HEAD
requests.post()|向HTML页面提交POST请求的方法，对应HTTP的POST
requests.put()|向HTML页面提交PUT请求的方法，对应HTTP的PUT
requests.patch()|向HTML页面提交局部修改请求，对应于HTTP的PATCH
requests.delete()|向HTML页面提交删除请求，对应于HTTP的DELETE

4.get方法

	格式：
		r = requests.get(url,params = None,**kwargs)
				|		|
				|		|
			Response   Requset

		url:拟获取页面的url链接
		params:url中额外参数，字典或字节流格式，可选
		**kwargs:12个控制访问的参数

5.Response对象的属性

属性|说明
-|-
r.status_code|HTTP请求的返回状态，200表示连接成功，404表示失败
r.text|HTTP响应内容的字符串形式，即，url对应的页面内容
r.encoding|从HTTP headr 中猜测的响应内容编码方式
r.apparent_encoding|从内容中分析出的响应内容编码方式(备选编码方式)
r.content|HTTP响应内容的二进制形式
	
	r.encoding：如果header中不存在charset，则认为编码为ISO-8859-1
	r.apparent_encoding:根据页面内容分析出的编码方式

6.理解Resquests库的异常

异常|说明
-|-
requests.ConnectionError|网络连接错误异常，如DNS查询失败，拒绝连接等
requests.HTTPError|HTTP错误异常
requests.URLRequird|URL缺失异常
requests.TooManyRedirects|超过最大重定向次数，产生重定向异常
requests.ConnectTimeout|连接远程服务器超时异常
requests.Timeout|请求URL超时，产生超时异常


	r.raise_for_status() : 如果不是200，产生异常requests.HTTPerror

```
import requests

def getHTMLText(url):
    try:
        r = requests.get(url,timeout = 30)
        r.raise_for_status()#如果状态不是200，引发HTTPError异常
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return "产生异常"

if __name__ == "__main__":
    url = "//www.baidu.com"
    print(getHTMLText(url))
```


## beautiful soup库

beautiful soup库是解析，遍历，维护“标签树”的功能库，

1.安装pip install beautifulsoup4

2.测试

```
import requests

r = requests.get("http://python123.io/ws/demo.html")
print(r.status_code)
print(r.text)
dome = r.text

from bs4 import BeautifulSoup

soup = BeautifulSoup(dome , "html.parser") # html.parser 解析器
print(soup.prettify())
```

3.Beautiful soup库解析器

解析器|使用方法|条件
-|-|-
bs4的HTML解析器|BeautifulSoup(mk,"html.parser")|安装bs4库
lxml的html解析器|BeautifulSoup(mk,"lxml")|pip install lxml
lxml的XML解析器|BeautifulSoup(mk,"html.parser")|pip install lxml
html5lib的解析器|BeautifulSoup(mk,"html5lib")|pip install html5lib

4.Beautiful Soup类的基本元素

基本元素|说明
-|-
Tag|标签，最基本的信息组织单元分别用<>和</>标明开头和结尾
Name|标签的名字,<p>...</p>的名字是'p',格式：<tag>.name
Attributes|标签的属性，字典形式组织，格式：<tag>.attrs
NavigableString|标签内非属性字符串，<>...</>中字符串，格式：<tag>.string
comment|标签内字符串的注释部分，一种特殊的Comment类型

## 基于bs4库的HTML内容的遍历方法

1.标签树的下行遍历

属性|说明
-|-
.contents|子节点的列表，将<tag>所有儿子节点存入列表
.children|子节点的迭代类型，与.contents类似，用于循环遍历儿子节点
.descendants|子孙节点的迭代类型，包含所有子孙节点，用于循环遍历
	
	# 遍历儿子/子孙节点
	for child in soup.body.children:
		print(child)

2.标签树的上行遍历

属性|说明
-|-
.parent|节点的父亲标签
.parents|节点先辈标签的迭代类型，用于循环遍历先辈节点



3.标签树的平行遍历

属性|说明
-|-
.next_sibling|返回按照HTML文本顺序的下一个平行标签
.previous_sibling|返回按照HTML文本顺序的上一个平行节点标签
.next_siblings|迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
.previous_siblings|迭代类型，返回按照HTML文本顺序的前续所有平行节点标签


4.<>.find_all(name,attrs,recursive,string,**kwargs)

返回一个列表类型，存储查找的结果

- name:对标签名称的检索字符串
- attrs:对标签属性值的检索字符串，可标注属性检索
- recursive:是否对子孙全部检索，默认True
- string:<>...</>中字符串区域的检索字符串
- **kwargs:

## 简写形式
	
<tag>(..) <==>  <tag>.find_all(...)
soup(..)  <==>  soup.find_all(..)

## find的扩展方法

方法|说明
-|-
<>.find()|搜索且只返回一个结果，字符串类型，同.find_all()参数
<>.find_parents()|在先辈节点中搜索.，字符串类型，同.find_all()参数
<>.find_parent()|在先辈节点中返回一个结果，字符串类型，同.find_all()参数
<>.find_next_silbings()|在后续平行节点中搜索，返回列表类型，同.find_all()参数
<>.find_next_silbing()|在后续平行节点中返回一个结果，返回字符串类型，同.find_all()参数
<>.find_preious_siblings()|在前序平行节点中搜索，返回列表类型，同.find_all()参数
<>.find_preious_sibling()|在前序平行节点中返回一个结果，返回字符串类型，同.find_all()参数




------------------


作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
