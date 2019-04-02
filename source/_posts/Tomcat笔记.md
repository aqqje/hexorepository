---
title:   Tomcat & Servlet 笔记
date: 2019-02-12 20:56:18
categories:
	- java
tags:
	- java
---
### Tomcat & Servlet 笔记

####  1. 软件架构
```text
    A. C/S：客户端/服务器端
    
    B. B/S：浏览器/服务器端
```


#### 2.资源分类

```text
    A. 静态资源：所有用户访问后，得到的结果都是一样的，称为静态资源.静态资源可以直接被浏览器解析
	如： html,css,JavaScript
	
    B. 动态资源:每个用户访问相同资源后，得到的结果可能不一样。称为动态资源。动态资源被访问后，需要先转换为静态资源，在返回给浏览器
	如：servlet/jsp,php,asp....
```

			

#### 3. 网络通信三要素
```text
    A. IP：电子设备(计算机)在网络中的唯一标识。
    
    B. 端口：应用程序在计算机中的唯一标识。 0~65536
	
    C. 传输协议：规定了数据传输的规则
        基础协议：
            a. tcp:安全协议，三次握手。 速度稍慢
            b. udp：不安全协议。 速度快
```



#### 4. web服务器软件：
```text
    A. 服务器：安装了服务器软件的计算机
    
    B. 服务器软件：接收用户的请求，处理请求，做出响应
	
    C. web服务器软件：接收用户的请求，处理请求，做出响应。
		a. 在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目web容器
```



##### 4.1 常见的java相关的web服务器软件：

```text
    A. webLogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
    B. webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
    C. JBOSS：JBOSS公司的，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
    D. Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范servlet/jsp。开源的，免费的。
```
> **_JavaEE：Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范_**

#### 5. Tomcat

##### 5.1 下载：
- http://tomcat.apache.org/

##### 5.2 安装：
- 解压压缩包即可。

> **注意**：_安装目录建议不要有中文和空格_

##### 5.3 卸载：
- 删除目录就行了

##### 5.4 启动：

- bin/startup.bat ,双击运行该文件即可
- 访问：浏览器输入：
    http://localhost:8080 回车访问自己
    http://别人的ip:8080 访问别人

##### 5.5 关闭：
- 正常关闭：
```shell
bin/shutdown.bat

ctrl+c
```

- 强制关闭：
```text
点击启动窗口的×
```

##### 5.6 配置:
- 部署项目的方式：
```text
    A. 直接将项目放到webapps目录下即可。
        a. /hello：项目的访问路径-->虚拟目录
        b. 简化部署：将项目打成一个war包，再将war包放置到webapps目录下。
             _**war包会自动解压缩**_
    B. 配置conf/server.xml文件
        在<Host>标签体中配置
        <Context docBase="D:\hello" path="/hehe" />
        docBase:项目存放的路径
        path：虚拟目录
    C. 在conf\Catalina\localhost创建任意名称的xml文件。在文件中编写
        <Context docBase="D:\hello" />
        虚拟目录：xml文件的名称
```
- 静态项目和动态项目：
```text
目录结构
    java动态项目的目录结构：
      -- 项目的根目录
          -- WEB-INF目录：
              -- web.xml：web项目的核心配置文件
              -- classes目录：放置字节码文件的目录
              -- lib目录：放置依赖的jar包
```
##### 5.7 * 可能遇到的问题：
```text
A. 黑窗口一闪而过：
    原因： 没有正确配置JAVA_HOME环境变量
    解决方案：正确配置JAVA_HOME环境变量
    
B. 启动报错：
    a. 暴力：找到占用的端口号，并且找到对应的进程，杀死该进程
        netstat -ano
    b. 温柔：修改自身的端口号
        conf/server.xml
        <Connector port="8888" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8445" />
        一般会将tomcat的默认端口号修改为80。80端口号是http协议的默认端口号。
            好处：在访问时，就不用输入端口号
```

#### 6. Servlet

##### 6.1 概念：
>运行在服务器端的小程序

- Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则。
- 将来我们自定义一个类，实现Servlet接口，复写方法。


##### 6.2 示例
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


##### 6.3 执行原理：
```text
    A. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
    
    B. 查找web.xml文件，是否有对应的<url-pattern>标签体内容。
    
    C. 如果有，则在找到对应的<servlet-class>全类名
    
    D. tomcat会将字节码文件加载进内存，并且创建其对象
   
    E. 调用其方法
```


##### 6.4 Servlet中的生命周期方法：

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





##### 7. Servlet3.0
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

#### 8. IDEA与tomcat的相关配置
```text
    A. IDEA会为每一个 tomcat 部署的项目单独建立一份配置文件
	查看控制台的 log：Using CATALINA_BASE:   "C:\Users\fqy\.IntelliJIdea2018.1\system\tomcat\_itcast"
    
    B. 工作空间项目和 tomcat 部署的web项目
	tomcat真正访问的是“tomcat部署的web项目”，"tomcat部署的web项目"对应着"工作空间项目" 的web目录下的所有资源
	WEB-INF目录下的资源不能被浏览器直接访问。
    
    C. 断点调试：使用"小虫子"启动 dubug 启动
```