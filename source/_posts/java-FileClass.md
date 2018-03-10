---
title: java访问文件夹
date: 2018-02-16 17:42:48
password: 123
categories:
	- java
tags:
	- java
---
{% cq %}
具体思路：一用File类，获取需要访问的文件，二创建getFile(File file)方法判断是否存在该文件，不存在直接return,存在打印文件名称，三判断参数是否为文件夹，是则打印其大小，四递归调用。
优化分层：getFile方法增加div参数，用来定义文件的层级，分层功能写在打印名称之前，
优化统计：定义静态全局变量sum。
{% endcq %}

代码：
```java
import java.io.File;

public class testFile {
	static long sum = 0;
	public void getFile(File file,int div) {
		
		//判断是否存在该文件
		if(file == null) {
			return;
		}
		//打印层次
		for(int i = 0; i < div; i++) {
			System.out.print("\t");
		}
		//打印名称
		System.out.print(file.getName());
		//判断参数是否为文件夹
		if(!file.isDirectory()) {
			//是文件就打印文件的大小(字节)
			System.out.println("\t"+file.length() + "字节");
			//计算总文件大小
			sum = sum+file.length();
			return;
		}
		//递归调用
		System.out.println();
		File[] fils = file.listFiles();
		for(int i = 0; i < fils.length; i++) {
			getFile(fils[i],div + 1);
		}
		
	}
	
	public static void main(String[] args) {
		File file = new File("F:\\python文件");
		testFile tf = new testFile();
		tf.getFile(file,0);
		System.out.println("总文件大小："+sum+"字节");
	}
}

```

运用结果：

```
python文件
	lover-time.html	2307字节
	mzitu.html	26922字节
	pygame1.py	276字节
	python
		day_01
			first.py	3179字节
			frest.py	3568字节
			second.py	179字节
			test.py	1074字节
			third.py	266字节
			three.py	2684字节
		day_02
			first.py	1843字节
			hdf
			myfile.txt	11字节
			myfile1.txt	0字节
			myfile3.txt	7字节
			second.py	1446字节
			新建文本文档 (2).txt	0字节
			新建文本文档.txt	0字节
	python网络爬虫与信息提取
		china_unwocity.py	7413字节
		requstes.py	1618字节
总文件大小：52793字节
```
