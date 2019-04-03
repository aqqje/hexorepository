---
title: MVC笔记
comments: ture
type: "tags"
categories:
	- javaee
tags:
	- javaee
---

#### MVC笔记

##### 1. jsp演变历史
```text
    A. 早期只有servlet，只能使用response输出标签数据，非常麻烦
    
    B. 后来又jsp，简化了Servlet的开发，如果过度使用jsp，在jsp中即写大量的java代码，有写html表，造成难于维护，难于分工协作
   
    C. 再后来，java的web开发，借鉴mvc开发模式，使得程序的设计更加合理性
```

##### 2. MVC：
```text
    A. M：Model，模型。JavaBean
        完成具体的业务操作，如：查询数据库，封装对象
    B. V：View，视图。JSP
        展示数据
    C. C：Controller，控制器。Servlet
        获取用户的输入
        调用模型
        将数据交给视图进行展示
    
    D. 优缺点：
        a. 优点：
            1. 耦合性低，方便维护，可以利于分工协作
            2. 重用性高
    
        b. 缺点：
            1. 使得项目架构变得复杂，对开发人员要求高
```

##### 3. 三层架构：软件设计架构
```TEXT
    A. 界面层(表示层)：用户看的得界面。用户可以通过界面上的组件和服务器进行交互
    
    B. 业务逻辑层：处理业务逻辑的。
    
    C. 数据访问层：操作数据存储文件。
```
