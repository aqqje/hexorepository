---
title: response 笔记
date: 2019-2-12 20:56:18
categories:
	- javaee
tags:
	- javaee
---

# CAS-note

## 1.什么是单点登录

## 2.什么是CAS

## 3.CAS服务端部署

### 3.1 cas-server.war

 - CAS服务端就是一个war包(\cas\cas-server-x.x.x-release\cas-server-x.x.x\modules\cas-server-webapp-x.x.x.war)

### 3.2 tomcat部署

 - 将 cas-server.war 放在一个纯净的 tomcat 的 \apache-tomcat-CAS\webapps目录下，运行 tomcat 即可。

### 3.3 运行测试

 - http://localhost:8080/cas/login

### 3.4 修改配置文件

 - 修改 tomcat 端口 为 9100

 \apache-tomcat-CAS\conf

```xml
<Connector port="9100" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />

```

 - 修改CAS配置文件
 \apache-tomcat-CAS\webapps\cas\WEB-INF\cas.properties

```xml
server.name=http://localhost:9100
```

- 去除https认证
CAS默认使用的是HTTPS协议，如果使用HTTPS协议需要SSL安全证书（需向特定的机构申请和购买） 。如果对安全要求不高或是在开发测试阶段，可使用HTTP协议。我们这里讲解通过修改配置，让CAS使用HTTP协议。

\apache-tomcat-CAS\webapps\cas\WEB-INF\deployerConfigContext.xml

```xml
<bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"
p:httpClient-ref="httpClient"/>
```

这里需要增加参数p:requireSecure="false"，requireSecure属性意思为是否需要安全验证，即HTTPS，false为不采用

- 修改cas的/WEB-INF/spring-configuration/ticketGrantingTicketCookieGenerator.xml

```xml
<bean id="ticketGrantingTicketCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"
      p:cookieSecure="true"
      p:cookieMaxAge="-1"
      p:cookieName="CASTGC"
      p:cookiePath="/cas" />

```
参数p:cookieSecure="true"，同理为HTTPS验证相关，TRUE为采用HTTPS验证，FALSE为不采用https验证。
参数p:cookieMaxAge="-1"，是COOKIE的最大生命周期，-1为无生命周期，即只在当前打开的窗口有效，关闭或重新打开其它窗口，仍会要求验证。可以根据需要修改为大于0的数字，比如3600等，意思是在3600秒内，打开任意窗口，都不需要验证。
我们这里将cookieSecure改为false ,  cookieMaxAge 改为3600

- 修改cas的WEB-INF/spring-configuration/warnCookieGenerator.xml

```xml
<bean id="warnCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"
p:cookieSecure="true"
p:cookieMaxAge="-1"
p:cookieName="CASPRIVACY"
p:cookiePath="/cas" />

```

我们这里将cookieSecure改为false ,  cookieMaxAge 改为3600

- 修改默认用户密码

apache-tomcat-CAS\webapps\cas\WEB-INF\deployerConfigContext.xml

```xml
<bean id="primaryAuthenticationHandler"
          class="org.jasig.cas.authentication.AcceptUsersAuthenticationHandler">
        <property name="users">
            <map>
                <entry key="casuser" value="Mellon"/>
            </map>
        </property>
    </bean>
```

### 3.5 充许退出登录后,跳转至指定页面，修改cas系统的配置文件cas-servlet.xml下的 cas.logout.followServiceRedirects 的 false 为 true


```xml
<bean id="logoutAction" class="org.jasig.cas.web.flow.LogoutAction"
        p:servicesManager-ref="servicesManager"
        p:followServiceRedirects="${cas.logout.followServiceRedirects:true}"/>
```

## 4 CAS-DOME

### 4.1 创建一个maven(war)工程 cas-client-dome1

### 4.2 引入依赖

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.aqqje.dome</groupId>
  <artifactId>cas-client-dome1</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  <dependencies>
		<!-- cas -->  
		<dependency>  
		    <groupId>org.jasig.cas.client</groupId>  
		    <artifactId>cas-client-core</artifactId>  
		    <version>3.3.3</version>  
		</dependency>  		
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>  
			<scope>provided</scope>
		</dependency>
	</dependencies> 
	
	<build>  
	  <plugins>
	      <plugin>  
	          <groupId>org.apache.maven.plugins</groupId>  
	          <artifactId>maven-compiler-plugin</artifactId>  
	          <version>3.7.0</version>  
	          <configuration>  
	              <source>1.8</source>  
	              <target>1.8</target>  
	          </configuration>  
	      </plugin>  
	      <plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<configuration>
					<!-- 指定端口 -->
					<port>9001</port>
					<!-- 请求路径 -->
					<path>/</path>
				</configuration>
	  	  </plugin>
	  </plugins>  
    </build>
</project>
```

### 4.3 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">	
    <!-- 用于单点退出，该过滤器用于实现单点登出功能，可选配置 -->  
    <listener>  
     <listener-class>org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>  
    </listener>  
    <!-- 该过滤器用于实现单点登出功能，可选配置。 -->  
    <filter>  
        <filter-name>CAS Single Sign Out Filter</filter-name>  
        <filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>  
    </filter>  
    <filter-mapping>  
        <filter-name>CAS Single Sign Out Filter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
    <!-- 该过滤器负责用户的认证工作，必须启用它 -->  
    <filter>  
        <filter-name>CASFilter</filter-name>       
        <filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>  
        <init-param>  
            <param-name>casServerLoginUrl</param-name>  
            <param-value>http://localhost:9100/cas/login</param-value>  
            <!--这里的server是服务端的IP -->  
        </init-param>  
        <init-param>  
            <param-name>serverName</param-name>  
            <param-value>http://localhost:9001</param-value>
        </init-param>  
    </filter>  
    <filter-mapping>  
        <filter-name>CASFilter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
    <!-- 该过滤器负责对Ticket的校验工作，必须启用它 -->  
    <filter>  
        <filter-name>CAS Validation Filter</filter-name>  
        <filter-class>org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>  
        <init-param>  
            <param-name>casServerUrlPrefix</param-name>  
            <param-value>http://localhost:9100/cas</param-value>  
        </init-param>  
        <init-param>  
            <param-name>serverName</param-name>  
            <param-value>http://localhost:9001</param-value>
        </init-param>  
    </filter>  
    <filter-mapping>  
        <filter-name>CAS Validation Filter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
    <!-- 该过滤器负责实现HttpServletRequest请求的包裹， 比如允许开发者通过HttpServletRequest的getRemoteUser()方法获得SSO登录用户的登录名，可选配置。 -->  
    <filter>  
        <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>  
        <filter-class>  
            org.jasig.cas.client.util.HttpServletRequestWrapperFilter</filter-class>  
    </filter>  
    <filter-mapping>  
        <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
    <!-- 该过滤器使得开发者可以通过org.jasig.cas.client.util.AssertionHolder来获取用户的登录名。 比如AssertionHolder.getAssertion().getPrincipal().getName()。 -->  
    <filter>  
        <filter-name>CAS Assertion Thread Local Filter</filter-name>       
        <filter-class>org.jasig.cas.client.util.AssertionThreadLocalFilter</filter-class>  
    </filter>  
    <filter-mapping>  
        <filter-name>CAS Assertion Thread Local Filter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
</web-app>
 
```

### 4.4 编写index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>cas-client-dome1</title>
</head>
<body>
 ==== cas-client-dome1 ====
 <%=request.getRemoteUser()%>
 <a href="http://localhost:9100/cas/logout?service=http://www.aqqje.com">退出登录</a>
</body>
</html>
```
- request.getRemoteUser()为获取远程登录名。
- http://localhost:9100/cas/logout 为单点登出地址。

### 4.5 再创建一个maven工程，过程同上，端口为9002

## 5.CAS-Clinet与SpringSecutiy整合

## 6.实战

### 6.1 CAS使用 user 表里做验证，修改cas服务端中web-inf下deployerConfigContext.xml ，添加如下配置

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"  
			  p:driverClass="com.mysql.jdbc.Driver"  
			  p:jdbcUrl="jdbc:mysql://127.0.0.1:3306/pinyougoudb?characterEncoding=utf8"  
			  p:user="root"  
			  p:password="123456" /> 
<bean id="passwordEncoder" 
class="org.jasig.cas.authentication.handler.DefaultPasswordEncoder"  
		c:encodingAlgorithm="MD5"  
		p:characterEncoding="UTF-8" />  
<bean id="dbAuthHandler"  
		  class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler"  
		  p:dataSource-ref="dataSource"  
		  p:sql="select password from tb_user where username = ?"  
		  p:passwordEncoder-ref="passwordEncoder"/>  
 
```

然后在配置文件开始部分找到如下配置

```xml
 <bean id="authenticationManager" class="org.jasig.cas.authentication.PolicyBasedAuthenticationManager">
        <constructor-arg>
            <map>               
                <entry key-ref="proxyAuthenticationHandler" value-ref="proxyPrincipalResolver" />
                <entry key-ref="primaryAuthenticationHandler" value-ref="primaryPrincipalResolver" />
            </map>
        </constructor-arg>      
        <property name="authenticationPolicy">
            <bean class="org.jasig.cas.authentication.AnyAuthenticationPolicy" />
        </property>
</bean>

```

其中

```xml
<entry key-ref="primaryAuthenticationHandler" value-ref="primaryPrincipalResolver" />

```

一句是使用固定的用户名和密码，我们在下面可以看到这两个bean ,如果我们使用数据库认证用户名和密码，需要将这句注释掉。

添加下面这一句配置

```xml
<entry key-ref="dbAuthHandler" value-ref="primaryPrincipalResolver"/>
```


将以下三个jar包放入webapps\cas\WEB-INF\lib下  


- c3p0-0.9.1.2.jar
- cas-server-support-jdbc-4.0.0.jar
- mysql-connector-java-5.1.32.jar

### 6.2 修改 CAS 默认登录界面

- 编辑casLoginView.jsp 内容,添加指令

```xml
<%@ page pageEncoding="UTF-8" %>
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

- 修改form标签

```xml
<form:form method="post" id="fm1" commandName="${commandName}" htmlEscape="true" class="sui-form">
......
</form:form>

```

- 修改用户名框

```xml
<form:input id="username" tabindex="1" 
	accesskey="${userNameAccessKey}" path="username" autocomplete="off" htmlEscape="true" 
	placeholder="邮箱/用户名/手机号" class="span2 input-xfat" />
```

- 修改密码框


```xml
 <form:password  id="password" tabindex="2" path="password" 
      accesskey="${passwordAccessKey}" htmlEscape="true" autocomplete="off" 
	  placeholder="请输入密码" class="span2 input-xfat"   />
```


- 修改登陆按钮

```xml
<input type="hidden" name="lt" value="${loginTicket}" />
<input type="hidden" name="execution" value="${flowExecutionKey}" />
<input type="hidden" name="_eventId" value="submit" />
<input class="sui-btn btn-block btn-xlarge btn-danger" accesskey="l" value="登陆" type="submit" />
```

### 6.3 错误提示

- 在表单内加入错误提示框

```xml
<form:errors path="*" id="msg" cssClass="errors" e
lement="div" htmlEscape="false" />
```

- 测试：输入错误的用户名和密码，提示是英文。这个提示信息是在WEB-INF\classes目录下的messages.properties文件中

```xml
authenticationFailure.AccountNotFoundException=Invalid credentials.
authenticationFailure.FailedLoginException=Invalid credentials.
```

- 设置国际化为zn_CN  ,修改cas-servlet.xml

```xml
<bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver" p:defaultLocale="zh_CN" />
```

- 我们需要将此信息拷贝到messages_zh_CN.properties下，并改为中文提示（转码）

```xml
authenticationFailure.AccountNotFoundException=\u7528\u6237\u4E0D\u5B58\u5728.
authenticationFailure.FailedLoginException=\u5BC6\u7801\u9519\u8BEF.

```
