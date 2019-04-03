---
title: Cookie&&Session笔记
comments: ture
type: "tags"
categories:
	- javaee
tags:
	- javaee
---

#### Cookie&&Session笔记

##### 1.会话技术
```text
A.会话: 一次会话中包含多次请求和响应.
    a.一次会话:浏览器第一次给服务器资源发送请求,会话建立,直到有一方断开为止
B.功能: 在一次会话的范围内的多次请求间,共享数据
C.方式:
    a.客户端会话技术 cookie
    b.服务器端会话技术: session

```

##### 2.cookie
```text
A.概念:客户端会话技术,将数据保存到客户端

B.快速入门:
    使用步骤:
        a.创建cookie对象,绑定数据
            new Cookie(String name, String value)
        b.发送cookie
            response.addCookie(Cookie cookie)
        c.获取Cookie,拿到数据
            Cookie[] request.getCookies()
            
C.实现原理
    基于响应头set-cookie和请求头cookie实现  

D.cookie的细节
    a.一次可不可以发送多个cookie?
        可以:可以创建多个Cookie对象,使用response调用多次addCookie方法发送cookie即可.
    b.cookie在浏览器中保存多长时间?
        1.默认情况下,当浏览器关闭后,cookie数据被销毁
        2.持久化存储:
            setMaxAge(int seconds)
                i.正数:将Cookie数据写到硬盘的文件中,持久化存储,cookie存活时间
                ii.负数:默认值
                iii.零:删除cookie信息
    c.cookie能不能存中文?
        在tomcat8之前cookie中不能直接存储中文数据.
            需要将中文数据转码, 一般采用URL编码($E3)
        在tomcat8之后,cookie支持中文数据.特殊字符还是不支持,建议使用URL编码存储,URL解码
    d.cookie获取范围多大?   
        1.假设在一个tomcat服务器中,部署了多个web项目,那么在这些web项目中cookie能不能共享?
            默认情况下cookie不能共享
            setPath(String path):设置cookie的获取范围.默认情况下,设置当前的虚拟目录
                如果要共享,即可以将path设置为"/"
        2.不同的tomcat服务器间cookie共享问题?
            setDomain(String path):如果设置一级域名,那么多个服务器之间cookie可以共享
                setDomain(".baidu.com"),那么tieba.baidu.com和news.baidu.com中的cookie可以共享 

E.cookie的特点和作用
    a.cookie存储数据在客户端浏览器
    b.浏览器对于单个cookie的大小有限制(4kb)以及对同一个域名下的总cookie数量也有限制(20个)
    作用:
        i.cookie一般用于存储少量的不太敏感的数据
        ii.在不登录的情况下,完成服务器对客户端的身份识别              
```
#### 案例:记住上一次访问时间
```text
A.需求:
    a.访问一个Servlet,如果是第一次访问,则提示:您好,欢迎您首次访问
    b.如果不是第一次访问,则提示:欢迎回来,您上次访问时间为:显示时间字符串
B.分析:
    a.可以采用cookie来完成
    b.在服务器上的Servlet判断是否有一个名为lastTime的cookie
        i.有: 不是第一次访问
            1.响应数据:欢迎回来,您上次访问时间为:2019年04月3日 13:04:55
        ii.没有: 是第一次访问
            1.响应数据:您好,欢迎您首次访问
            2.写回cookie:lastTime=2019年04月3日 13:04:55
    
```

##### 3.session笔记
```text
A.概念:服务器端会话技术,在一次会话的多冷请求间共享数据,将数据保存在服务器端的对象中,Httpsession

B.获取HttpSession对象:
  Httpsession session = request.getSession();
  
C.使用HttpSession对象:
  Object getAttribute(String name)
  void setAttribute(String name,Object value)
  void removeAttribute(String name)
  
D.原理
  session的实现是依赖于cookie的
  
E.细节:
  a.当客户端关闭后,服务器不关闭,两次获取session是否为同一个?
      默认情况下,不是
      如果需要相同,则可以创建cookie,键为SESSIONID,设置最大存活时间,让cookie持久化保存.
          Cookie c = new Cookie("JSESSION", session.getId());
          c.setMaxAge(60*60);
          response.addCookie(c);
  b.客户端不关闭,服务器关闭后,两次获取的session是同一个吗?
      1.不是同一个,但是要确保数据不丢失
          i.session的纯化
              在服务器正常关闭之前,将session对系列化到硬盘上
          ii.session的活化:
              在服务器启动后,将session文件转化这内存中的session对象即可.
              
  3.session什么时候被销毁?
      a.服务器关闭
      b.session对象调用invalidate().
      c.session默认失效时间 30分钟
          tomcat配置
          <session config>
              <session-timeout>30</session-timeout>
          </session config>
          
F.session的特点
  a.session用于存储一次会话的多冷请求的数据,存在服务器端
  b.sessions可以存储任意类型,任意大小的数据
  
  c.session与cookie的区别:
      1.session存储数据在服务器端,cookie在客户端
      2.session没有数据大小限制,cookie有
      3.session数据安全,cookie相对于不安全    
```
#### 案例:验证码        
```text
A.需求
    a.访问带验证码的登录login.jsp
    b.用户输入用户名,密码以及验证码
        如果用户和密码输入有误,跳转登录页面,提示:用户名或密码错误
        如果验证码输入有有误,跳转登录页面提示:验证码错误
        如果全部输入正确,则跳转到主页success.jsp显示:用户名,欢迎您
```

