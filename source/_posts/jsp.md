---
title: JSP笔记
comments: ture
type: "tags"
categories:
	- javaee
tags:
	- javaee
---

#### JSP笔记

##### 1.概念:
    java server pages: java服务器端页面
        可以理解为:一个特殊的页面,其中即可以指定定义html标签,又可以定义java代码
        用于简化书写!!!

##### 2.原理:
    JSP本质上就是一个Servlet

##### 3.JSP的脚本: JSP定义java代码的方式
    <% 代码 %>:定义的java代码,在service方法中,service方法可以定义什么,该脚本中就可以定义什么.
    <%! 代码 %>定义的java代码,在jsp转换后的java类的成员位置
    <%= 代码 %>定义的java代码,会出到页面,输出语句中可以定义什么,该脚本就可以定义什么.

##### 4.JSP的内置对象
    在jsp页面中不需要获取和创建,可以直接使用的对象
    jsp一共有9个内置对象
        request
        response
        out:字符输出流对象可以将数据输出到页面上.和response.getWriter()类似
            response.getWriter().write()和out.write()的区别
                在tomcat服务器真正给客户端做出之间,会先找response缓冲区数据,再找out缓冲区数据
                response.getWirter()数据输出永远在out.write()之间

##### 5.指令
```text
    A.作用：用于配置JSP页面，导入资源文件
    
    B.格式：
        <%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>
    
    C.分类：
        a. page:配置JSP页面
            i. contentType：等同于response.setContentType()
                1. 设置响应体的mime类型以及字符集
                2. 设置当前jsp页面的编码（只能是高级的IDE才能生效，如果使用低级工具，则需要设置pageEncoding属性设置当前页面的字符集）
            
            ii. import：导包
            
            iii. errorPage：当前页面发生异常后，会自动跳转到指定的错误页面
            
            iv. isErrorPage：标识当前也是是否是错误页面。
                1. true：是，可以使用内置对象exception
                2. false：否。默认值。不可以使用内置对象exception
                
        b. include:页面包含的。导入页面的资源文件
            <%@include file="top.jsp"%>
        
        c. taglib：导入资源
            <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
                prefix：前缀，自定义的
```

##### 6.注释:
```text
A. html注释：
    <!-- -->:只能注释html代码片段

B. jsp注释：推荐使用
    <%-- --%>：可以注释所有

C. 内置对象
    在jsp页面中不需要创建，直接使用的对象
    一共有9个：
        变量名					真实类型                          作用
        pageContext				PageContext                     当前页面共享数据，还可以获取其他八个内置对象
        request					HttpServletRequest              一次请求访问的多个资源(转发)
        session					HttpSession                     一次会话的多个请求间
        application				ServletContext                  所有用户间共享数据
        response				HttpServletResponse             响应对象
        page					Object当前页面(Servlet)的对象    this
        out				        JspWriter                       输出对象，数据输出到页面上
        config					ServletConfig                   Servlet的配置对象
        exception				Throwable                       异常对象
```