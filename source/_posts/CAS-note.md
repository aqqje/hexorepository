---
title: response �ʼ�
date: 2019-2-12 20:56:18
categories:
	- javaee
tags:
	- javaee
---

# CAS-note

## 1.ʲô�ǵ����¼

## 2.ʲô��CAS

## 3.CAS����˲���

### 3.1 cas-server.war

 - CAS����˾���һ��war��(\cas\cas-server-x.x.x-release\cas-server-x.x.x\modules\cas-server-webapp-x.x.x.war)

### 3.2 tomcat����

 - �� cas-server.war ����һ�������� tomcat �� \apache-tomcat-CAS\webappsĿ¼�£����� tomcat ���ɡ�

### 3.3 ���в���

 - http://localhost:8080/cas/login

### 3.4 �޸������ļ�

 - �޸� tomcat �˿� Ϊ 9100

 \apache-tomcat-CAS\conf

```xml
<Connector port="9100" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />

```

 - �޸�CAS�����ļ�
 \apache-tomcat-CAS\webapps\cas\WEB-INF\cas.properties

```xml
server.name=http://localhost:9100
```

- ȥ��https��֤
CASĬ��ʹ�õ���HTTPSЭ�飬���ʹ��HTTPSЭ����ҪSSL��ȫ֤�飨�����ض��Ļ�������͹��� ������԰�ȫҪ�󲻸߻����ڿ������Խ׶Σ���ʹ��HTTPЭ�顣�������ｲ��ͨ���޸����ã���CASʹ��HTTPЭ�顣

\apache-tomcat-CAS\webapps\cas\WEB-INF\deployerConfigContext.xml

```xml
<bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"
p:httpClient-ref="httpClient"/>
```

������Ҫ���Ӳ���p:requireSecure="false"��requireSecure������˼Ϊ�Ƿ���Ҫ��ȫ��֤����HTTPS��falseΪ������

- �޸�cas��/WEB-INF/spring-configuration/ticketGrantingTicketCookieGenerator.xml

```xml
<bean id="ticketGrantingTicketCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"
      p:cookieSecure="true"
      p:cookieMaxAge="-1"
      p:cookieName="CASTGC"
      p:cookiePath="/cas" />

```
����p:cookieSecure="true"��ͬ��ΪHTTPS��֤��أ�TRUEΪ����HTTPS��֤��FALSEΪ������https��֤��
����p:cookieMaxAge="-1"����COOKIE������������ڣ�-1Ϊ���������ڣ���ֻ�ڵ�ǰ�򿪵Ĵ�����Ч���رջ����´��������ڣ��Ի�Ҫ����֤�����Ը�����Ҫ�޸�Ϊ����0�����֣�����3600�ȣ���˼����3600���ڣ������ⴰ�ڣ�������Ҫ��֤��
�������ｫcookieSecure��Ϊfalse ,  cookieMaxAge ��Ϊ3600

- �޸�cas��WEB-INF/spring-configuration/warnCookieGenerator.xml

```xml
<bean id="warnCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"
p:cookieSecure="true"
p:cookieMaxAge="-1"
p:cookieName="CASPRIVACY"
p:cookiePath="/cas" />

```

�������ｫcookieSecure��Ϊfalse ,  cookieMaxAge ��Ϊ3600

- �޸�Ĭ���û�����

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

### 3.5 �����˳���¼��,��ת��ָ��ҳ�棬�޸�casϵͳ�������ļ�cas-servlet.xml�µ� cas.logout.followServiceRedirects �� false Ϊ true


```xml
<bean id="logoutAction" class="org.jasig.cas.web.flow.LogoutAction"
        p:servicesManager-ref="servicesManager"
        p:followServiceRedirects="${cas.logout.followServiceRedirects:true}"/>
```

## 4 CAS-DOME

### 4.1 ����һ��maven(war)���� cas-client-dome1

### 4.2 ��������

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
					<!-- ָ���˿� -->
					<port>9001</port>
					<!-- ����·�� -->
					<path>/</path>
				</configuration>
	  	  </plugin>
	  </plugins>  
    </build>
</project>
```

### 4.3 ����web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">	
    <!-- ���ڵ����˳����ù���������ʵ�ֵ���ǳ����ܣ���ѡ���� -->  
    <listener>  
     <listener-class>org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>  
    </listener>  
    <!-- �ù���������ʵ�ֵ���ǳ����ܣ���ѡ���á� -->  
    <filter>  
        <filter-name>CAS Single Sign Out Filter</filter-name>  
        <filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>  
    </filter>  
    <filter-mapping>  
        <filter-name>CAS Single Sign Out Filter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
    <!-- �ù����������û�����֤���������������� -->  
    <filter>  
        <filter-name>CASFilter</filter-name>       
        <filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>  
        <init-param>  
            <param-name>casServerLoginUrl</param-name>  
            <param-value>http://localhost:9100/cas/login</param-value>  
            <!--�����server�Ƿ���˵�IP -->  
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
    <!-- �ù����������Ticket��У�鹤�������������� -->  
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
    <!-- �ù���������ʵ��HttpServletRequest����İ����� ������������ͨ��HttpServletRequest��getRemoteUser()�������SSO��¼�û��ĵ�¼������ѡ���á� -->  
    <filter>  
        <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>  
        <filter-class>  
            org.jasig.cas.client.util.HttpServletRequestWrapperFilter</filter-class>  
    </filter>  
    <filter-mapping>  
        <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>  
        <url-pattern>/*</url-pattern>  
    </filter-mapping>  
    <!-- �ù�����ʹ�ÿ����߿���ͨ��org.jasig.cas.client.util.AssertionHolder����ȡ�û��ĵ�¼���� ����AssertionHolder.getAssertion().getPrincipal().getName()�� -->  
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

### 4.4 ��дindex.jsp

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
 <a href="http://localhost:9100/cas/logout?service=http://www.aqqje.com">�˳���¼</a>
</body>
</html>
```
- request.getRemoteUser()Ϊ��ȡԶ�̵�¼����
- http://localhost:9100/cas/logout Ϊ����ǳ���ַ��

### 4.5 �ٴ���һ��maven���̣�����ͬ�ϣ��˿�Ϊ9002

## 5.CAS-Clinet��SpringSecutiy����

## 6.ʵս

### 6.1 CASʹ�� user ��������֤���޸�cas�������web-inf��deployerConfigContext.xml �������������

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

Ȼ���������ļ���ʼ�����ҵ���������

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

����

```xml
<entry key-ref="primaryAuthenticationHandler" value-ref="primaryPrincipalResolver" />

```

һ����ʹ�ù̶����û��������룬������������Կ���������bean ,�������ʹ�����ݿ���֤�û��������룬��Ҫ�����ע�͵���

���������һ������

```xml
<entry key-ref="dbAuthHandler" value-ref="primaryPrincipalResolver"/>
```


����������jar������webapps\cas\WEB-INF\lib��  


- c3p0-0.9.1.2.jar
- cas-server-support-jdbc-4.0.0.jar
- mysql-connector-java-5.1.32.jar

### 6.2 �޸� CAS Ĭ�ϵ�¼����

- �༭casLoginView.jsp ����,���ָ��

```xml
<%@ page pageEncoding="UTF-8" %>
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

- �޸�form��ǩ

```xml
<form:form method="post" id="fm1" commandName="${commandName}" htmlEscape="true" class="sui-form">
......
</form:form>

```

- �޸��û�����

```xml
<form:input id="username" tabindex="1" 
	accesskey="${userNameAccessKey}" path="username" autocomplete="off" htmlEscape="true" 
	placeholder="����/�û���/�ֻ���" class="span2 input-xfat" />
```

- �޸������


```xml
 <form:password  id="password" tabindex="2" path="password" 
      accesskey="${passwordAccessKey}" htmlEscape="true" autocomplete="off" 
	  placeholder="����������" class="span2 input-xfat"   />
```


- �޸ĵ�½��ť

```xml
<input type="hidden" name="lt" value="${loginTicket}" />
<input type="hidden" name="execution" value="${flowExecutionKey}" />
<input type="hidden" name="_eventId" value="submit" />
<input class="sui-btn btn-block btn-xlarge btn-danger" accesskey="l" value="��½" type="submit" />
```

### 6.3 ������ʾ

- �ڱ��ڼ��������ʾ��

```xml
<form:errors path="*" id="msg" cssClass="errors" e
lement="div" htmlEscape="false" />
```

- ���ԣ����������û��������룬��ʾ��Ӣ�ġ������ʾ��Ϣ����WEB-INF\classesĿ¼�µ�messages.properties�ļ���

```xml
authenticationFailure.AccountNotFoundException=Invalid credentials.
authenticationFailure.FailedLoginException=Invalid credentials.
```

- ���ù��ʻ�Ϊzn_CN  ,�޸�cas-servlet.xml

```xml
<bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver" p:defaultLocale="zh_CN" />
```

- ������Ҫ������Ϣ������messages_zh_CN.properties�£�����Ϊ������ʾ��ת�룩

```xml
authenticationFailure.AccountNotFoundException=\u7528\u6237\u4E0D\u5B58\u5728.
authenticationFailure.FailedLoginException=\u5BC6\u7801\u9519\u8BEF.

```
