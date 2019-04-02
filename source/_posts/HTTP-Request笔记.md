---
title: HTTP：Request 笔记
date: 2019-3-14 20:56:18
categories:
	- javaee
tags:
	- javaee
---

### 1.HTTP：Request

##### 1.1 概念
> Hyper Text Transfer Protocol 超文本传输协议

##### 1.2 传输协议：
> 定义了，客户端和服务器端通信时，发送数据的格式
- 特点：
    - 基于TCP/IP的高级协议
    - 默认端口号:80
    - 基于请求/响应模型的:一次请求对应一次响应
    - 无状态的：每次请求之间相互独立，不能交互数据


##### 1.3 历史版本：
```text
    1.0：每一次请求响应都会建立新的连接

    1.1：复用连接
```

##### 1.4 请求消息数据格式
```text
A. 请求行
    请求方式 请求url 请求协议/版本
    GET /login.html	HTTP/1.1

    请求方式：
        HTTP协议有7中请求方式，常用的有2种
            GET：
                1. 请求参数在请求行中，在url后。
                2. 请求的url长度有限制的
                3. 不太安全
            POST：
                1. 请求参数在请求体中
                2. 请求的url长度没有限制的
                3. 相对安全
B. 请求头：客户端浏览器告诉服务器一些信息
    请求头名称: 请求头值
    常见的请求头：
      1. User-Agent：浏览器告诉服务器，我访问你使用的浏览器版本信息
          可以在服务器端获取该头的信息，解决浏览器的兼容性问题

        2. Referer：http://localhost/login.html
            告诉服务器，我(当前请求)从哪里来？
                作用：
                  1. 防盗链：
                  2. 统计工作：
C. 请求空行
    空行，就是用于分割POST请求的请求头，和请求体的。
D. 请求体(正文)：
    封装POST请求消息的请求参数的

E 字符串格式：
    POST /login.html	HTTP/1.1
    Host: localhost 
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
    Accept-Encoding: gzip, deflate, br
    Accept-Language: en,zh-CN;q=0.9,zh;q=0.8
    Connection: keep-alive
    Cookie: 略
    Host: localhost:8080
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.80 Safari/537.36
    
    name = zhangshan
    	
F 响应消息数据格式
```





#### 2. Request：
##### 2.1. request对象和response对象的原理
```text
    A. request和response对象是由服务器创建的。我们来使用它们
    B. request对象是来获取请求消息，response对象是来设置响应消息
```

##### 2.2. request对象继承体系结构：	
```text
    ServletRequest		--	接口
        |	继承
    HttpServletRequest	-- 接口
        |	实现
    org.apache.catalina.connector.RequestFacade 类(tomcat)
```

##### 2.3. request功能：
```text
A. 获取请求消息数据
    a. 获取请求行数据
        * GET /aqqje/demo?name=zhangsan HTTP/1.1
        * 方法：
            1. 获取请求方式 ：GET
                String getMethod()  
            2. (*)获取虚拟目录：/dome
                String getContextPath()
            3. 获取Servlet路径: /dome
                String getServletPath()
            4. 获取get方式请求参数：name=zhangsan
                String getQueryString()
            5. (*)获取请求URI：/aqqje/demo
                String getRequestURI():		/aqqje/dome
                StringBuffer getRequestURL()  :http://localhost/aqqje/dome

                URL:统一资源定位符 ： http://localhost/aqqje/demo	中华人民共和国
                URI：统一资源标识符 : /aqqje/demo					共和国
            
            6. 获取协议及版本：HTTP/1.1
                String getProtocol()

            7. 获取客户机的IP地址：
                String getRemoteAddr()
            
    b. 获取请求头数据
        方法：
            (*)String getHeader(String name):通过请求头的名称获取请求头的值
            Enumeration<String> getHeaderNames():获取所有的请求头名称
        
    c. 获取请求体数据:
        请求体：只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
        步骤：
            1. 获取流对象
                BufferedReader getReader()：获取字符输入流，只能操作字符数据
                ServletInputStream getInputStream()：获取字节输入流，可以操作所有类型数据
                    在文件上传知识点后讲解

            2. 再从流对象中拿数据
        
        
B. 其他功能：
    a. 获取请求参数通用方式：不论get还是post请求方式都可以使用下列方法来获取请求参数
        1. String getParameter(String name):根据参数名称获取参数值    username=zs&password=123
        2. String[] getParameterValues(String name):根据参数名称获取参数值的数组  hobby=xx&hobby=game
        3. Enumeration<String> getParameterNames():获取所有请求的参数名称
        4. Map<String,String[]> getParameterMap():获取所有参数的map集合

        中文乱码问题：
            get方式：tomcat 8 已经将get方式乱码问题解决了
            post方式：会乱码
                解决：在获取参数前，设置request的编码request.setCharacterEncoding("utf-8");
    
            
    b. 请求转发：一种在服务器内部的资源跳转方式
        1. 步骤：
            1. 通过request对象获取请求转发器对象：RequestDispatcher getRequestDispatcher(String path)
            2. 使用RequestDispatcher对象来进行转发：forward(ServletRequest request, ServletResponse response) 

        2. 特点：
            1. 浏览器地址栏路径不发生变化
            2. 只能转发到当前服务器内部资源中。
            3. 转发是一次请求


    c. 共享数据：
        域对象：一个有作用范围的对象，可以在范围内共享数据
        request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
        方法：
            1. void setAttribute(String name,Object obj):存储数据
            2. Object getAttitude(String name):通过键获取值
            3. void removeAttribute(String name):通过键移除键值对

    e. 获取ServletContext：
        ServletContext getServletContext()
```

#### 4.用户登录(示例)
```text
* 用户登录案例需求：
    1.编写login.html登录页面
        username & password 两个输入框
        
    2.使用Druid数据库连接池技术,操作mysql，day14数据库中user表
    
        Properties ps = new Properties();
        InputStream resource = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
        ps.load(resource);
        ds = DruidDataSourceFactory.createDataSource(ps);
    
    3.使用JdbcTemplate技术封装JDBC
        return jdbcTemplate.queryForObject("select * from user where name = ? and password = ?",
                                new BeanPropertyRowMapper<User>(User.class), loginUser.getName(), loginUser.getPassword());
    4.使用commons下的BeanUtils工具类(需导入jar)封装成loginUser对象
        Map<String, String[]> parameterMap = request.getParameterMap();
            User loginUser = new User();
            try {
                BeanUtils.populate(loginUser, parameterMap);
                System.out.println(loginUser);
            } catch{
            ....
            }
    5.登录成功跳转到SuccessServlet展示：登录成功！用户名,欢迎您登录失败跳转到FailServlet展示：登录失败，用户名或密码错误
        UserDao dao = new UserDao();
        User user = dao.login(loginUser);

        if(user != null){
            request.setAttribute("login", user);
            request.getRequestDispatcher("/user/loginSuccess").forward(request, response);
        }else{
            request.getRequestDispatcher("/user/loginError").forward(request, response);
            }
```