---
title: el&jstl笔记
comments: ture
type: "tags"
categories:
	- javaee
tags:
	- javaee
---

#### el&jstl笔记

##### 1. EL表达式
概念：Expression Language 表达式语言
```text
A. 作用：替换和简化jsp页面中java代码的编写

B. 语法：${表达式}

C. 注意：
    jsp默认支持el表达式的。如果要忽略el表达式
        1. 设置jsp中page指令中：isELIgnored="true" 忽略当前jsp页面中所有的el表达式
        2. ${表达式} ：忽略当前这个el表达式

D. 使用：
    a. 运算：
        i. 算数运算符： + - * /(div) %(mod)
        ii. 比较运算符： > < >= <= == !=
        iii. 逻辑运算符： &&(and) ||(or) !(not)
        iv. 空运算符： empty
            功能：用于判断字符串、集合、数组对象是否为null或者长度是否为0
            ${empty list}:判断字符串、集合、数组对象是否为null或者长度为0
            ${not empty str}:表示判断字符串、集合、数组对象是否不为null 并且 长度>0
    
    2. 获取值
        a. el表达式只能从域对象中获取值
        b. 语法：
            1. ${域名称.键名}：从指定域中获取指定键的值
                域名称：
                    1. pageScope		--> pageContext
                    2. requestScope 	--> request
                    3. sessionScope 	--> session
                    4. applicationScope --> application（ServletContext）
                举例：在request域中存储了name=张三
                获取：${requestScope.name}

            2. ${键名}：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。

            3. 获取对象、List集合、Map集合的值
                1. 对象：${域名称.键名.属性名}
                    * 本质上会去调用对象的getter方法

                2. List集合：${域名称.键名[索引]}

                3. Map集合：
                    * ${域名称.键名.key名称}
                    * ${域名称.键名["key名称"]}

E. 隐式对象：
    a.el表达式中有11个隐式对象
    b.pageContext：
        获取jsp其他八个内置对象
            ${pageContext.request.contextPath}：动态获取虚拟目录
                            
```
##### JSTL
概念：JavaServer Pages Tag Library  JSP标准标签库;是由Apache组织提供的开源的免费的jsp标签 <标签>
```text
A. 作用：用于简化和替换jsp页面上的java代码		

B. 使用步骤：
    1. 导入jstl相关jar包
    2. 引入标签库：taglib指令： <%@ taglib %>
    3. 使用标签

C. 常用的JSTL标签
    a. if:相当于java代码的if语句
        1. 属性：
            test 必须属性，接受boolean表达式
                如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容
                一般情况下，test属性值会结合el表达式一起使用
         2. 注意：
             c:if标签没有else情况，想要else情况，则可以在定义一个c:if标签
             
    b. choose:相当于java代码的switch语句
        1. 使用choose标签声明         			相当于switch声明
        2. 使用when标签做判断         			相当于case
        3. 使用otherwise标签做其他情况的声明    	相当于default

    c. foreach:相当于java代码的for语句
            1. 完成重复的操作
                for(int i = 0; i < 10; i ++){
                    ...
                }
                属性：
                    begin：开始值
                    end：结束值
                    var：临时变量
                    step：步长
                    varStatus:循环状态对象
                        index:容器中元素的索引，从0开始
                        count:循环次数，从1开始
            2. 遍历容器
                List<User> list;
                for(User user : list){
    
                }
                属性：
                    items:容器对象
                    var:容器中元素的临时变量
                    varStatus:循环状态对象
                        index:容器中元素的索引，从0开始
                        count:循环次数，从1开始
```

##### 案例：用户信息列表展示
```text
A. 需求：用户信息的增删改查操作
B. 设计：
    1. 技术选型：Servlet+JSP+MySQL+JDBCTempleat+Duird+BeanUtilS+tomcat
    2. 数据库设计：
        create database dome; -- 创建数据库
        use dome; 			   -- 使用数据库
        create table user(   -- 创建表
            id int primary key auto_increment,
            name varchar(20) not null,
            gender varchar(5),
            age int,
            address varchar(32),
            qq	varchar(20),
            email varchar(50)
        );

C. 开发：
    1. 环境搭建
        1. 创建数据库环境
        2. 创建项目，导入需要的jar包

    2. 编码
D. 测试
E. 部署运维
```
##### el隐式对象
| 隐含对象         | 类型                         | 说明                                                         |
| ---------------- | ---------------------------- | ------------------------------------------------------------ |
| PageContext      | javax.servlet.ServletContext | 表示此JSP的PageContext                                       |
| PageScope        | java.util.Map                | 取得Page范围的属性名称所对应的值                             |
| RequestScope     | java.util.Map                | 取得Request范围的属性名称所对应的值                          |
| sessionScope     | java.util.Map                | 取得Session范围的属性名称所对应的值                          |
| applicationScope | java.util.Map                | 取得Application范围的属性名称所对应的值                      |
| param            | java.util.Map                | 如同ServletRequest.getParameter(String name)。回传String类型的值 |
| paramValues      | java.util.Map                | 如同ServletRequest.getParameterValues(String name)。回传String[]类型的值 |
| header           | java.util.Map                | 如同ServletRequest.getHeader(String name)。回传String类型的值 |
| headerValues     | java.util.Map                | 如同ServletRequest.getHeaders(String name)。回传String[]类型的值 |
| cookie           | java.util.Map                | 如同HttpServletRequest.getCookies()                          |
| initParam        | java.util.Map                | 如同ServletContext.getInitParameter(String name)。回传String类型的值 |



















