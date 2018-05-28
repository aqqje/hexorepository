---
title: springmvc
date: 2018-05-12 22:48:52
tags: springmvc
---

spring MVC 学习笔记
<-- !more -->

## HelloWorld

- 配置 web.xml
 - 配置 DispatcherServlet:DispatcherServlet 
   1. 默认加载 /WEBINF/<servletName-servlet>.xml 的 Spring 配置文件,启动 WEB 层的 Spring 容器
   2. 可以通过 contextConfigLocation 初始化参数自定义配置文件的位置和名称
 
```xml 
<!--2、springmvc的前端控制器，拦截所有请求  -->
	<servlet>
		<servlet-name>dispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 使用默认值时可以省略不写 -->
		<init-param>
			<!-- contextConfigLocation 固定参数 -->
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:dispatcherServlet-servlet.xml</param-value>
		</init-param>
		<!-- 配置启动顺序 -->
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>dispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```

- 创建 Spring MVC 配置文件 dispatcherServlet-servlet.xml

 - 配置自动扫描的包
 - 配置视图解析器
```xml 
    <!--SpringMVC的配置文件，包含网站跳转逻辑的控制，配置  -->
	<context:component-scan base-package="com.aqqje">
	
	<!--配置视图解析器，方便页面返回  -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
```

- 创建请求处理器类

```java 
@Controller
public class HelloWord{

	@RequestMapping("/hellowSpringMVC")
	public String helloworld(){
		System.out.println("helloworld...");
		return "success";
	}
}
```
- 创建 index.jsp --> HelloWord


## @RequestMapping 映射请求
	
- 注解位置：
 - 1.类定义处：提供初步的请求映射信息。相对于 WEB 应用的根目录
 - 2.方法处：提供进一步的细分映射信息。相对于类定义处的 URL。若
类定义处未标注 @RequestMapping，则方法处标记的 URL 相对于
WEB 应用的根目录

- @RequestMapping 参数

 - value:请求 URL --> 注解位置
 - method:请求方法 PUT GET DELETE POST
 - params:请求参数
    – param1: 表示请求必须包含名为 param1 的请求参数
	– !param1: 表示请求不能包含名为 param1 的请求参数
	– param1 != value1: 表示请求包含名为 param1 的请求参数，但其值不能为 value1
	– {“param1=value1”, “param2”}: 请求必须包含名为 param1 和param2的两个请求参数，且 param1 参数的值必须为 value1
 - heads:请求头的映射条件[支持同 params 参数的简单表达式]

```java 

@RequestMapping(value = "testParamsAndHeaders", params = { "username",
			"age!=10" }, headers = { "Accept-Language=en-US,zh;q=0.8" })
	public String testParamsAndHeaders() {
		System.out.println("testParamsAndHeaders");
		return SUCCESS;
	}
```

- Ant 风格资源地址支持 3 种匹配符
 – ?：匹配文件名中的一个字符
 – *：匹配文件名中的任意字符
 – **：** 匹配多层路径
 
- Ant 风格的 URL

 – /user/*/createUser: 匹配
 /user/aaa/createUser、/user/bbb/createUser 等 URL
 – /user/**/createUser: 匹配
 /user/createUser、/user/aaa/bbb/createUser 等 URL
 – /user/createUser??: 匹配
 /user/createUseraa、/user/createUserbb 等 URL
 
- @PathVariable 映射 URL 绑定的占位符

 - @PathVariable 可以来映射 URL 中的占位符到目标方法的参数中.
 
```java 
	@RequestMapping("/testPathVariable/{id}")
	public String testPathVariable(@PathVariable("id") Integer id) {
		System.out.println("testPathVariable: " + id);
		return SUCCESS;
	}
```

## REST (Representational State Transfer)表现层状态转化

- 具体说，就是 HTTP 协议里面，四个表示操作方式的动
词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET 用来获
取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。

```
/order/1 HTTP GET ：得到 id = 1 的 order   
/order/1 HTTP DELETE：删除 id = 1的 order   
/order/1 HTTP PUT：更新id = 1的 order   
/order HTTP POST：新增 order
```

- 使用 REST 与 HiddenHttpMethodFilter 过滤器共同使用

```xml
	<!-- 4、使用Rest风格的URI，将页面普通的post请求转为指定的delete或者put请求 -->
	<filter>
		<filter-name>HiddenHttpMethodFilter</filter-name>
		<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>HiddenHttpMethodFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<filter>
		<filter-name>HttpPutFormContentFilter</filter-name>
		<filter-class>org.springframework.web.filter.HttpPutFormContentFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>HttpPutFormContentFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```

## @RequestParam 绑定请求参数值

- 使用 @RequestParam 可以把请求参数传递给请求方法
 – value：参数名
 – required：是否必须。默认为 true, 表示请求参数中必须包含对应的参数，若不存在，将抛出异常
 
```java
	@RequestMapping(value = "/testRequestParam")
	public String testRequestParam(
			@RequestParam(value = "username") String un,
			@RequestParam(value = "age", required = false, defaultValue = "0") int age) {
		System.out.println("testRequestParam, username: " + un + ", age: "
				+ age);
		return SUCCESS;
	}
```

## @RequestHeader 绑定请求报头的属性值

```java
	@RequestMapping("/testRequestHeader")
	public String testRequestHeader(
			@RequestHeader(value = "Accept-Language") String al) {
		System.out.println("testRequestHeader, Accept-Language: " + al);
		return SUCCESS;
	}
```

## @CookieValue 绑定请求中的 Cookie 值

```java
	@RequestMapping("/testCookieValue")
	public String testCookieValue(@CookieValue("JSESSIONID") String sessionId) {
		System.out.println("testCookieValue: sessionId: " + sessionId);
		return SUCCESS;
	}
```


## POJO 对象绑定请求参数值

- Spring MVC 会按请求参数名和 POJO 属性名进行自动匹配，自动为该对象填充属性值。支持级联属性。如：dept.deptId、dept.address.tel 等

```java
	/**
	 * 1. 有 @ModelAttribute 标记的方法, 会在每个目标方法执行之前被 SpringMVC 调用! 
	 * 2. @ModelAttribute 注解也可以来修饰目标方法 POJO 类型的入参, 其 value 属性值有如下的作用:
	 * 1). SpringMVC 会使用 value 属性值在 implicitModel 中查找对应的对象, 若存在则会直接传入到目标方法的入参中.
	 * 2). SpringMVC 会一 value 为 key, POJO 类型的对象为 value, 存入到 request 中. 
	 */
	@ModelAttribute
	public void getUser(@RequestParam(value="id",required=false) Integer id, 
			Map<String, Object> map){
		System.out.println("modelAttribute method");
		if(id != null){
			//模拟从数据库中获取对象
			User user = new User(1, "Tom", "123456", "tom@atguigu.com", 12);
			System.out.println("从数据库中获取一个对象: " + user);
			
			map.put("user", user);
		}
	}
	
	/** 运行流程:
	 * 1. 执行 @ModelAttribute 注解修饰的方法: 从数据库中取出对象, 把对象放入到了 Map 中. 键为: user
	 * 2. SpringMVC 从 Map 中取出 User 对象, 并把表单的请求参数赋给该 User 对象的对应属性.
	 * 3. SpringMVC 把上述对象传入目标方法的参数. 
	 * 
	 * 注意: 在 @ModelAttribute 修饰的方法中, 放入到 Map 时的键需要和目标方法入参类型的第一个字母小写的字符串一致!
	 */
	@RequestMapping("/testModelAttribute")
	public String testModelAttribute(@ModelAttribute("user") User user){
		return SUCCESS;
	}
```


## ModelAndView

- 既包含视图信息，也包含模型数据信息。

- 添加模型数据:
 – MoelAndView addObject(String attributeName, ObjectattributeValue)
 – ModelAndView addAllObject(Map<String, ?> modelMap)

- 设置视图:
 – void setView(View view)
 – void setViewName(String viewName)
 
## Map 及 Model

```java

@ModelAttribute("user")
public User getUser(){
	User user = new User();
	user.setAge(10);
	
	return user;
}
email: ${requestScope.user.email}

@RequestMapping("/handle")
public String handle(Map<String, Object> map){
	map.put("time", new Date());
	User user = (User)map.get("user")
	user.setEmail("aqqje@123.com");
	
	return "success";
}

time: ${requestScope.time}


## @SessionAttributes

- @SessionAttributes 除了可以通过属性名指定需要放到会话中的属性外(实际上使用的是 value 属性值),还可以通过模型属性的对象类型指定哪些模型属性需要放到会话中(实际上使用的是 types 属性值)
 
- 注意: 该注解只能放在类的上面. 而不能修饰放方法. 

- 避免@SessionAttributes引发的异常

```java
@SessionAttributes("user")
@Controller
public class UserController{
	// 该 方法会往隐含模型中添加一个名为 user 的模型属性
	@ModelAttribute("user")
	public User getUser(){
		User user = new User();
		return user;
	}
}


## 希望直接响应通过 SpringMVC 渲染的页面，可以使用 mvc:viewcontroller 标签实现

```xml
	<!-- 配置直接转发的页面 -->
	<!-- 可以直接相应转发的页面, 而无需再经过 Handler 的方法.  -->
	<mvc:view-controller path="/success" view-name="success"/>
```

## 配置国际化资源文件

```xml

i18n.username=Username
i18n.password=Password

<!-- 配置国际化资源文件 -->
	<bean id="messageSource"
		class="org.springframework.context.support.ResourceBundleMessageSource">
		<property name="basename" value="i18n"></property>	
	</bean>
```

## 重定向  forward: 或 redirect:

– redirect:success.jsp：会完成一个到 success.jsp 的重定向的操作
– forward:success.jsp：会完成一个到 success.jsp 的转发操作


## 处理静态资源

可以在 SpringMVC 的配置文件中配置 <mvc:default-servlethandler/> 的方式解决静态资源的问题

```xml

	方法一：
    <!-- 配置静态资源的请求映射关系 -->
    <mvc:resources location="/resources/" mapping="/resources/**"></mvc:resources>
	
	方法二：
	<!-- 将springmvc不能处理的请求交给tomcat -->
	<mvc:default-servlet-handler/>
```

## 处理 JSON

- 1加入 jar 包
 - jackson-annotation-x.x.x.jar
 - jackson-core-x.x.x.jar
 - jackson-databind-2.2.2.jar

- 2编写目标方法，使其返回 JSON 对应的对象或集合

- 3在方法上添加 @ResponseBody 注解

- 字符集编码的过滤器(必须放在所有过渡器之前)

```xml
<!-- 字符集编码的过滤器 -->
	<filter>
		<filter-name>EncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>EncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```
	
## 文件上传

- 配置 MultipartResolver

```java
Controller:

	//执行新增动作
	@RequestMapping(value="/code", method=RequestMethod.POST)
	public ModelAndView add(@RequestParam("codefile") MultipartFile codefile,
			@RequestParam("intro") String intro,
			HttpServletRequest request,
			HttpSession session){
		ModelAndView mv = new ModelAndView();
		
		User loginUser = (User)session.getAttribute("loginUser");
		String path = request.getServletContext().getRealPath("/") + "resources\\codefile\\";
		CodeService codeService = new CodeService();
		Map<String, Object> result = codeService.addCode(codefile, path, loginUser, intro);
		Boolean isSuccess = (Boolean)result.get("isSuccess");
		String message = (String)result.get("message");
		if(isSuccess){
			mv.setViewName("redirect:/codes");
		}else{
			mv.setViewName("bizzerror");
			mv.addObject("message", message);
		}
		
		
		return mv;
	}
	
service: 

	public Map<String, Object> addCode(MultipartFile codefile, String path,
			User loginUser, String intro) {
		Map<String, Object> result = new HashMap<String, Object>();
		//文件上传和新增代码的业务逻辑
		//将文件存到服务器的指定位置
		//1. 路径存在性
		//2. 文件名重复的问题
		try {
			
			if(codefile.getSize() > 0){
				String filename = generateFilename(codefile.getOriginalFilename());
				if(filename.endsWith(".zip") || filename.endsWith(".rar")){
					File file = new File(path, filename);
					if(!file.getParentFile().exists()){
						file.getParentFile().mkdir();
					}
					//将上传的文件保存在服务器上的指定目录
					codefile.transferTo(file);
					
					//向数据库插入一条新的Code数据
					//(id=null, codename="codefile.getname()", 
					//  filepath="文件存放好之后的位置", 
					//  intro=输入的intro
					//  owner="当前登录用户", 
					//  addTime="当前系统时间")
					Code code = new Code();
					code.setCodename(codefile.getOriginalFilename());
					code.setFilepath("resources/codefile/" + filename );
					code.setIntro(intro);
					code.setOwner(loginUser);
					code.setAddTime(new Timestamp(System.currentTimeMillis()));
					CodeDao codeDao = new CodeDao();
					
					codeDao.add(code);
					
					
					result.put("isSuccess", true);
					result.put("message", "上传成功！");
				}else{
					//类型错误，报错
					result.put("isSuccess", false);
					result.put("message", "必须上传.zip或者.rar文件");
				}
			}else{
				//空文件，报错
				result.put("isSuccess", false);
				result.put("message", "文件不可为空");
			}
			return result;
		} catch (Exception e) {
			e.printStackTrace();
			
			result.put("isSuccess", false);
			result.put("message", "上传失败");
			return result;
		}
		
		
	}
```

## 拦截器

- 自定义的拦截器必须实现HandlerInterceptor接口

	– preHandle()：这个方法在业务处理器处理请求之前被调用，在该方法中对用户请求 request 进行处理。
	  如果程序员决定该拦截器对请求进行拦截处理后还要调用其他的拦截器，或者是业务处理器去进行处理，则返回true；
	  如果程序员决定不需要再调用其他的组件去处理请求，则返回false。
	– postHandle()：这个方法在业务处理器处理完请求后，
	  但是DispatcherServlet 向客户端返回响应前被调用，
	  在该方法中对用户请求request进行处理。
	– afterCompletion()：这个方法在 DispatcherServlet 完全处理完请求后被调用，
	  可以在该方法中进行一些资源清理的操作。
  
- springmvc 配置文件中配置自定义的拦截器   
```java

java:

/**
 * 登录检查拦截器
 * @author Administrator
 *
 */
public class LoginInterceptor extends HandlerInterceptorAdapter{
	public static List<String> URLS = null;
	static{
		URLS = new ArrayList<String>();
		URLS.add("/codes");
		URLS.add("/admin");
		URLS.add("/code");
	}
	/**
	 * 执行请求之前进行拦截
	 */
	@Override
	public boolean preHandle(HttpServletRequest request,
			HttpServletResponse response, Object handler) throws Exception {
		String url = request.getServletPath();
		String method = request.getMethod();
		if(method.equalsIgnoreCase("GET") && URLS.contains(url)){
			if(request.getSession().getAttribute("loginUser") == null){
				//未登录状态
				//重定向到未登录错误页面
				response.sendRedirect("not_login");
				return false;
			}else{
				//已登录
				return true;
			}
		}else{
			return true;
		}
	}
	
}

xml:

	<!-- 配置拦截列表 -->
    <mvc:interceptors>
    	<!-- 配置单个拦截器 -->
    	<mvc:interceptor>
    		<mvc:mapping path="/*"/>
    		<bean id="loginInterceptor" class="com.javaee.scms.interceptors.LoginInterceptor"></bean>
    	</mvc:interceptor>

```

## 异常处理


- Spring MVC 通过 HandlerExceptionResolver 处理程序
的异常，包括 Handler 映射、数据绑定以及目标方法执行
时发生的异常。

-SpringMVC 提供的 HandlerExceptionResolver 的实现类



```java
	public class ExceptionHandler implements HandlerExceptionResolver{

	@Override
	public ModelAndView resolveException(HttpServletRequest request,
			HttpServletResponse response, Object handler, Exception exception) {
		ModelAndView mv = new ModelAndView();
		
		if(exception instanceof SystemException){
			StringBuilder message = new StringBuilder();
			message.append("<p>" + exception.getCause().getMessage() + "</p>");
			message.append("<p>请您与管理员联系，您也可以返回<a href='" + request.getContextPath() + "/home'>首页</a></p>");
			mv.addObject("message", message.toString());
			mv.setViewName("syserror");
		}if(exception instanceof BizzException){
			StringBuilder message = new StringBuilder();
			message.append("<p>" + exception.getMessage() + "</p>");
			message.append("<p>请您与管理员联系，您也可以返回<a href='" + request.getContextPath() + "/home'>首页</a></p>");
			mv.addObject("message", message.toString());
			mv.setViewName("bizzerror");
		}else{
			StringBuilder message = new StringBuilder();
			message.append("<p>发生了未知的异常</p>");
			message.append("<p>请您与管理员联系，您也可以返回<a href='" + request.getContextPath() + "/home'>首页</a></p>");
			mv.addObject("message", message.toString());
			mv.setViewName("syserror");
		}
		
		return mv;
	}

}
```