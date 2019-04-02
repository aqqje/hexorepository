---
title: response 笔记
date: 2019-2-12 20:56:18
categories:
	- javaee
tags:
	- javaee
---

### response 笔记

#### 1. HTTP协议：
```text
A. 请求消息：客户端发送给服务器端的数据
		* 数据格式：
			1. 请求行
			2. 请求头
			3. 请求空行
			4. 请求体
			
B. 响应消息：服务器端发送给客户端的数据
    * 数据格式：
        . 响应行
            1. 组成：协议/版本 响应状态码 状态码描述
            2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态。
                1. 状态码都是3位数字 
                2. 分类：
                    1. 1xx：服务器就收客户端消息，但没有接受完成，等待一段时间后，发送1xx多状态码
                    2. 2xx：成功。代表：200
                    3. 3xx：重定向。代表：302(重定向)，304(访问缓存)
                    4. 4xx：客户端错误。
                        * 代表：
                            * 404（请求路径没有对应的资源） 
                            * 405：请求方式没有对应的doXxx方法
                    5. 5xx：服务器端错误。代表：500(服务器内部出现异常)
                        
        b. 响应头：
            1. 格式：头名称： 值
            2. 常见的响应头：
                1. Content-Type：服务器告诉客户端本次响应体数据格式以及编码格式
                2. Content-disposition：服务器告诉客户端以什么格式打开响应体数据
                    * 值：
                        * in-line:默认值,在当前页面内打开
                        * attachment;filename=xxx：以附件形式打开响应体。文件下载
        c. 响应空行
        d. 响应体:传输的数据
        
C. 响应字符串格式
        HTTP/1.1 200 OK
        Content-Type: text/html;charset=UTF-8
        Content-Length: 101
        Date: Wed, 06 Jun 2018 07:08:42 GMT

        <html>
          <head>
            <title>$Title$</title>
          </head>
          <body>
          hello , response
          </body>
        </html>
```
#### 2. Response对象
```text
A. 功能：设置响应消息

    a. 设置响应行
        1. 格式：HTTP/1.1 200 ok
        2. 设置状态码：setStatus(int sc) 
        
    b. 设置响应头：setHeader(String name, String value) 
        
    c. 设置响应体：
        使用步骤：
            1. 获取输出流
                字符输出流：PrintWriter getWriter()
                字节输出流：ServletOutputStream getOutputStream()
            2. 使用输出流，将数据输出到客户端浏览器
```

```text
B. 重定向: 资源跳转的方式
    a. 示例
        //1. 设置状态码为302
        response.setStatus(302);
        //2.设置响应头location
        response.setHeader("location","/day15/responseDemo2");
        //简单的重定向方法
        response.sendRedirect("/day15/responseDemo2");

    b. 重定向的特点:redirect
        1. 地址栏发生变化
        2. 重定向可以访问其他站点(服务器)的资源
        3. 重定向是两次请求。不能使用request对象来共享数据
        
    c. 转发的特点：forward
        1. 转发地址栏路径不变
        2. 转发只能访问当前服务器下的资源
        3. 转发是一次请求，可以使用request对象来共享数据

    d. 路径写法：
         1. 路径分类
            i. 相对路径：通过相对路径不可以确定唯一资源
                如：./index.html
                不以/开头，以.开头路径

                规则：找到当前资源和目标资源之间的相对位置关系
                    ./：当前目录
                    ../:后退一级目录
            ii. 绝对路径：通过绝对路径可以确定唯一资源
                    如：http://localhost/day15/responseDemo2		/day15/responseDemo2
                    以/开头的路径

                规则：判断定义的路径是给谁用的？判断请求将来从哪儿发出
                    给客户端浏览器使用：需要加虚拟目录(项目的访问路径)
                        建议虚拟目录动态获取：request.getContextPath()
                        <a> , <form> 重定向...
                    给服务器使用：不需要加虚拟目录
                        转发路径

		2. 服务器输出字符数据到浏览器
			步骤：
				i. 获取字符输出流
				ii. 输出数据

			注意：
				乱码问题：
					i. PrintWriter pw = response.getWriter();获取的流的默认编码是ISO-8859-1
					ii. 设置该流的默认编码
					iii. 告诉浏览器响应体使用的编码
					
					//简单的形式，设置编码，是在获取流之前设置
        			response.setContentType("text/html;charset=utf-8");
        			
		3. 服务器输出字节数据到浏览器
			步骤：
				1. 获取字节输出流
				2. 输出数据

		4. 验证码
			1. 本质：图片
			2. 目的：防止恶意表单注册
```
#### 3.简单的验证码
```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置宽和高
        int width = 100;
        int height = 50;

        // 1.创建图片对象
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);

        // 2.美化图片
        // 设置 bg
        Graphics g = image.getGraphics();// 获取画笔对象
        g.setColor(Color.pink);// 设置画笔颜色
        g.fillRect(0, 0, width, height);

        //画边框
        g.setColor(Color.blue);
        g.drawRect(0, 0, width -1, height - 1);

        // 生成随机字符
        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789";

        Random random = new Random();

        for (int i = 1; i <= 4 ; i++) {
            // 生成随机角标
            int index = random.nextInt(str.length());
            char ch = str.charAt(index);
            // 写验证码
            g.drawString(ch+"",width/5*i, height/2);
        }
        g.setColor(Color.green);
        // 画干扰线
        for (int i = 0; i <8 ; i++) {
            int x1 = random.nextInt(width);
            int x2 = random.nextInt(width);
            int y1 = random.nextInt(height);
            int y2 = random.nextInt(height);
            g.drawLine(x1, x2, y1, y2);
        }

        // 3.输出图片
        ImageIO.write(image, "jpg", response.getOutputStream());

    }
```
#### 4. ServletContext对象：
```text
A. 概念：代表整个web应用，可以和程序的容器(服务器)来通信

B. 获取：
        a. 通过request对象获取
            request.getServletContext();
        b. 通过HttpServlet获取
            this.getServletContext();
C. 功能：
        a. 获取MIME类型：
            MIME类型:在互联网通信过程中定义的一种文件数据类型
                格式： 大类型/小类型   text/html		image/jpeg

            获取：String getMimeType(String file)  
        b. 域对象：共享数据
            1. setAttribute(String name,Object value)
            2. getAttribute(String name)
            3. removeAttribute(String name)

            ServletContext对象范围：所有用户所有请求的数据
        c. 获取文件的真实(服务器)路径
             String getRealPath(String path)  
             String b = context.getRealPath("/b.txt");//web目录下资源访问
             System.out.println(b);
        
            String c = context.getRealPath("/WEB-INF/c.txt");//WEB-INF目录下的资源访问
            System.out.println(c);
            
            String a = context.getRealPath("/WEB-INF/classes/a.txt");//src目录下的资源访问
            System.out.println(a);
```
#### 5.简单的图片下载
```text
A.分析：
    a. 超链接指向的资源如果能够被浏览器解析，则在浏览器中展示，如果不能解析，则弹出下载提示框。不满足需求
    b. 任何资源都必须弹出下载提示框
    c. 使用响应头设置资源的打开方式：
        content-disposition:attachment;filename=xxx

B.步骤：
    a. 定义页面，编辑超链接href属性，指向Servlet，传递资源名称filename
    b. 定义Servlet
        i. 获取文件名称
        ii. 使用字节输入流加载文件进内存
        iii. 指定response的响应头： content-disposition:attachment;filename=xxx
        iv. 将数据写出到response输出流


C.问题：
    中文文件问题
        解决思路：
           1. 获取客户端使用的浏览器版本信息
           2. 根据不同的版本信息，设置filename的编码方式不同
```
D. 示例
```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.获取下载文件请求的参数
        request.setCharacterEncoding("utf-8");
        String filename = new String(request.getParameter("filename").getBytes("iso-8859-1"), "utf-8");
        System.out.println(filename);
        // 2.获取文件资源的路径
        ServletContext context = request.getServletContext();
        String realPath = context.getRealPath("/file/" + filename);
        System.out.println(realPath);
        // 3.获取输入流加载文件进行内存
        FileInputStream fis = new FileInputStream(realPath);

        // A.设置响应头
        // a.响应头类型
        String mimeType = context.getMimeType(filename);
        response.setHeader("content-type",mimeType);

        // B.针对不浏览器设置不同的编码方式
        String agent = request.getHeader("user-agent");
        filename = DownLoadUtils.getFileName(agent, filename);
        System.out.println(filename);


        // b.响应头打开方式
        response.setHeader("content-disposition", "attachment;filename="+filename);
        System.out.println(filename);
        // 4.使用输出流下载文件
        ServletOutputStream sos = response.getOutputStream();
        byte[] buff = new byte[1024*8];
        int len = 0;
        while( (len = fis.read(buff)) != -1 ){
            sos.write(buff, 0, len);
        }

        // 5.关闭资源
        fis.close();
    }
```
#### 6.request.getParament()在get和post的乱码问题
```text
A.乱码原因:
    Http请求传输时将url以ISO-8859-1编码，服务器收到字节流后默认会以ISO-8859-1编码来解码成字符流（造成中文乱码）

B. doPost():
    方法一: 
        String filename = new String(request.getParameter("filename").getBytes("iso-8859-1"), "utf-8");
    方法二:
        request.setCharacterEncoding("utf-8");
        String filename = request.getParameter("filename");
    方法三:
        对web服务器的默认编码进行修改:如Tomcat
        
        <Connector port="8080" protocol="HTTP/1.1"   
            connectionTimeout="20000"   
            redirectPort="8444"   
            useBodyEncodingForURI="true" URIEncoding="UTF-8"/> 

```
