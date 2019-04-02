---
title:  Servlet&HTTP&Request笔记
date: 2019-02-12 20:56:18
categories:
	- java
tags:
	- java
---
### Servlet&HTTP&Request笔记

#### 1. Servlet

##### 1.1 概念：
>运行在服务器端的小程序

- Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则。
- 将来我们自定义一个类，实现Servlet接口，复写方法。


##### 1.2 示例
```text
    A. 创建JavaEE项目

    B. 定义一个类，实现Servlet接口
        public class ServletDemo1 implements Servlet
    
    C. 实现接口中的抽象方法

    D. 配置Servlet
        在web.xml中配置：
```
```xml
<!--配置Servlet -->
        <servlet>
            <servlet-name>demo1</servlet-name>
            <servlet-class>cn.itcast.web.servlet.ServletDemo1</servlet-class>
        </servlet>
    
        <servlet-mapping>
            <servlet-name>demo1</servlet-name>
            <url-pattern>/demo1</url-pattern>
        </servlet-mapping>
```


##### 1.3 执行原理：
```text
    A. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
    
    B. 查找web.xml文件，是否有对应的<url-pattern>标签体内容。
    
    C. 如果有，则在找到对应的<servlet-class>全类名
    
    D. tomcat会将字节码文件加载进内存，并且创建其对象
   
    E. 调用其方法
```


##### 1.4 Servlet中的生命周期方法：

```text
A. 被创建：执行init方法，只执行一次
    Servlet什么时候被创建？
        默认情况下，第一次被访问时，Servlet被创建
        可以配置执行Servlet的创建时机。
            在<servlet>标签下配置
                1. 第一次被访问时，创建
                    <load-on-startup>的值为负数
                2. 在服务器启动时，创建
                    <load-on-startup>的值为0或正整数
                    Servlet的init方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的
                        多个用户同时访问时，可能存在线程安全问题。
                        解决：尽量不要在Servlet中定义成员变量。即使定义了成员变量，也不要对修改值
                    
B. 提供服务：执行service方法，执行多次
    每次访问Servlet时，Service方法都会被调用一次。

C. 被销毁：执行destroy方法，只执行一次
    a. Servlet被销毁时执行。服务器关闭时，Servlet被销毁
    b. 只有服务器正常关闭时，才会执行destroy方法。
    c. destroy方法在Servlet被销毁之前执行，一般用于释放资源
```

##### 2. Servlet3.0
- A. 好处：
```text
    a. 支持注解配置。可以不需要web.xml了。
```

- B. 步骤：
```text
    a. 创建JavaEE项目，选择Servlet的版本3.0以上，可以不创建web.xml
    b. 定义一个类，实现Servlet接口
    c. 复写方法
    d. 在类上使用@WebServlet注解，进行配置
        @WebServlet("资源路径")
```

- C. @WebServlet
```java
@Target({ElementType.TYPE})
        @Retention(RetentionPolicy.RUNTIME)
        @Documented
        public @interface WebServlet {
            String name() default "";//相当于<Servlet-name>
        
            String[] value() default {};//代表urlPatterns()属性配置
        
            String[] urlPatterns() default {};//相当于<url-pattern>
        
            int loadOnStartup() default -1;//相当于<load-on-startup>
        
            ...
            }
```
#### 3. Servlet的体系结构	
```text
    Servlet -- 接口
            |
        GenericServlet -- 抽象类
            |
        HttpServlet  -- 抽象类


    GenericServlet：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象
        将来定义Servlet类时，可以继承GenericServlet，实现service()方法即可
    
    HttpServlet：对http协议的一种封装，简化操作
        a. 定义类继承HttpServlet
        b. 复写doGet/doPost方法
```


#### 4. Servlet相关配置
```text
A. urlpartten:Servlet访问路径
    a. 一个Servlet可以定义多个访问路径 ： @WebServlet({"/d4","/dd4","/ddd4"})
    b. 路径定义规则：
        1. /xxx：路径匹配
        2. /xxx/xxx:多层路径，目录结构
        3. *.do：扩展名匹配
```