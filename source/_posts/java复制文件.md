---
title: java复制文件
date: 2018-02-20 15:20:39
categories: java
tags: java
---
<a id="download" href="https://git-scm.com/download/win"><i class="fa fa-download"></i><span>Download Now</span></a>
{% note info %}
- 1.字节流:每次只能处理一个字节
- 2.java中所有字节流都以InPutStream和OutputStream作为祖先类
- 3.InputStream类
	- 最核心方法read
	- 其次是读取一个字节
	- 无参,其返回值是int类型,里面存放的就是所读到的那个字节的信息
	- 下次再调用read方法时,读取的并  不是刚才所读到的那个字节,而下一个字节  
	- 以此类推,直到把数据源中每一个字节都读取完毕
	- 当read所返回值是-1表示读到数据源的末尾
- 4.OutputStream类
	- 最核心方法write
	- 是向数据源中输出一个字节
	- 参数int类型,其意义就是向数据源输出的信息,真实写入数据源的仅是一个字节
	- write就去没有所谓的结束,理论上,有多少字节都可以逐一写入数据源中
- 5.无论是输入流还是输出流,在使用完之后,都要关闭流对象,否则会导致系统资源耗尽
- 6.调用流对象的close方法实现关闭
{% endnote %}
	   

## 读取<span id="inline-purple">单个字节</span>复制	   
	   
```java
package test;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

public class fileIO {
	public static void main(String[] args) {
		//伪初始化
		InputStream fis = null;
		OutputStream fos = null;
		String inpath = "D:\\test.txt";
		String outpath = ("f:\\test.txt");
		File f1 = new File(inpath);
		File f2 = new File(outpath.substring(0, 8));
		//判断是不是存在源文件和目标目录
		if(!(f1.isFile() == true && f2.isDirectory() == true)) {
			System.out.println("复制失败！源文件或目标目录存在");
		} else {
			try {
				//初始化
				fis = new FileInputStream(inpath);
				fos = new FileOutputStream(outpath);
				int temp;
				//开始复制文件
				while((temp=fis.read())!=-1) {
					fos.write(temp);
				}
				System.out.println("文件复制成功！");
			} catch (FileNotFoundException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
				//关闭IO流并处理异常
			} finally {
				if(fis!=null) {
					try {
						fis.close();
					} catch (IOException e) {
						// TODO Auto-generated catch block；
						e.printStackTrace();
					}
					if(fos!=null) {
						try {
							fos.close();
						} catch (IOException e) {
							// TODO Auto-generated catch block
							e.printStackTrace();
						}
					}
				}
			}
		}
	}
}
```

## 读取<span id="inline-purple">一组字节</span>复制

重载read()读取一组字节
- int read(byte[] b)
- int read(byte[] b, int off, int len)
重载write()写入一组字节
- int write(byte[] b)
- int write(byte[] b, int off, int len)

```java			     
int readByte
Byte[] buff = new Byte[1024]
while((readByte=fis.read(buff))!=-1){
	fos.write(buff,0,readByte)
}
```

