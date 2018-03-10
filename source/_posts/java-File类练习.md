---
title: java File类练习
date: 2018-02-20 16:37:42
categories: java
tags: java
---
{% note info %}
对File类进行了解！
{% endnote %}

```java
package test;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class file {
	public static void main(String[] args) throws IOException {
		//初始化
		File f1 = new File("h:\\mmjpg");
		//判断是不是存在
		System.out.println(f1.exists());
		//判断是不是文件
		System.out.println(f1.isFile());
		//判断是不是文件夹
		System.out.println(f1.isDirectory());
		//获取文件大小(不能用于文件夹)
		System.out.println(f1.length() + "字节！");
		//获取文件最后修改的时间
		long l1 = f1.lastModified();
		Date d1 = new Date(l1);
		SimpleDateFormat sdf = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss");
		System.out.println(sdf.format(d1));
		//获得文件名称(初始化的文件名称"D:\\mmjpg")
		System.out.println("文件名称" + f1.getName());
		//获得文件的上层目录
		System.out.println(f1.getParent());
		//获得文件的绝对路径
		System.out.println(f1.getAbsoluteFile());
		//获得标准绝对路径
		System.out.println(f1.getCanonicalPath());
		//获得分区的总大小
		System.out.println(f1.getTotalSpace());
		//获得分区未使用大小
		System.out.println(f1.getFreeSpace());
		//重命名
		//File f2 = new File("D:\\mm");
		//f1.renameTo(f2);
	}
}

```
