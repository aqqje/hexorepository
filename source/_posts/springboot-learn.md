---
title: springboot-learn
date: 2018-09-09 16:27:03
tags: spring boot
---







# 一、Spring Boot 入门

## 1、 Spring Boot 简介

> 简化Spring应用开发框架
>
> 整个Spring技术栈大整合
>
> J2EE开发一站式解决方案 



## 2. 微服务

2014，martin fowler

微服务：架构风格（服务微化）

一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；

单体应用：ALL IN ONE

微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元；

[详细参照微服务文档](https://martinfowler.com/articles/microservices.html#MicroservicesAndSoa)

## 3、环境准备

- jdk1.8： Spring Boot 推荐jdk1.7及以上版本;
- maven3.x: maven 3.3 以上版本
- IntelliJIDEA2017 及 STS
- Spring Boot 1.5.9.RELEASE及以上版本



## 4、Spring Boot HelloWorld

- 实现功能： 

  浏览器发送hello请求， 服务器接受请求并处理，响应 Hello World 字符串



### A、 创建一个maven工程(jar)

- 省略...

### B、导入Spring Boot 相关的依赖

```xml
<!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>

    <!-- Add typical dependencies for a web application -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

### C、写一个主程序；启动Spring Boot应用

```java
/**
 * SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */

@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {
        // Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```

### D、编写相关的Controller、Service

```java
@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping(name = "/hello")
    public String hello(){
        return "Hello World";
    }
}
```

### E、运行主程序测试

- 省略...

### F、简化部署

```xml
<!-- 将应用打包成一个可执行的jar包；-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

- Maven Projects -> spring-boot-helloworld -> Lifecycle -> package
- 将这个应用打成jar包，直接使用java -jar的命令进行执行；

## 5、Hello World探究

### A、POM文件

#### a、父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>

他的父项目是
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>1.5.9.RELEASE</version>
  <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
他来真正管理Spring Boot应用里面的所有依赖版本；

```

Spring Boot的版本仲裁中心；

以后我们导入依赖默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）

#### b、启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter**-***web***：

```
spring-boot-starter：spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；
```



Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器

### B、主程序类，主入口类

```java
/**
 *  @SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {

        // Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}

```

@**SpringBootApplication**:    Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；



```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
      @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

@**SpringBootConfiguration**:Spring Boot的配置类；

```
	标注在某个类上，表示这是一个Spring Boot的配置类；

	@**Configuration**:配置类上来标注这个注解；

		配置类 -----  配置文件；配置类也是容器中的一个组件；@Component
```



@**EnableAutoConfiguration**：开启自动配置功能；

```
	以前我们需要配置的东西，Spring Boot帮我们自动配置；@**EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；
```

```java
@AutoConfigurationPackage
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
```

```
  	@**AutoConfigurationPackage**：自动配置包

	@**Import**(AutoConfigurationPackages.Registrar.class)：

	Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；
```

==将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；==

```
@**Import**(EnableAutoConfigurationImportSelector.class)；

	给容器中导入组件？

	**EnableAutoConfigurationImportSelector**：导入哪些组件的选择器；

	将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

	会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组件，并配置好这些组件；	
```

有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；

```
	SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader)；
```



==Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；==以前我们需要自己配置的东西，自动配置类都帮我们；

J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9.RELEASE.jar；

## 6、使用Spring Initializer快速创建Spring Boot项目

### A、IDEA：使用 Spring Initializer快速创建项目

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；

选择我们需要的模块；向导会联网创建Spring Boot项目；

默认生成的Spring Boot项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源； js css  images；
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties：Spring Boot应用的配置文件；可以修改一些默认设置；

### B、STS使用 Spring Starter Project快速创建项目

- 省略...



# 二、配置文件

## 1、配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

•application.properties

•application.yml



配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好；



YAML（YAML Ain't Markup Language）

```
YAML  A Markup Language：是一个标记语言

YAML   isn't Markup Language：不是一个标记语言；
```

标记语言：

```
以前的配置文件；大多都使用的是  **xxxx.xml**文件；

YAML：**以数据为中心**，比json、xml等更适合做配置文件；

YAML：配置例子
```

```yaml
server:
  port: 8081
```

```
XML：
```

```xml
<server>
	<port>8081</port>
</server>
```

## 2、YAML语法：

### A、基本语法

k:(空格)v：表示一对键值对（空格必须有）；

以**空格**的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
    port: 8081
    path: /hello
```

属性和值也是大小写敏感；



### B、值的写法

#### a、字面量：普通的值（数字，字符串，布尔）

```
k: v：字面直接来写；

	字符串默认不用加上单引号或者双引号；

	""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

			name:   "zhangsan \n lisi"：输出；zhangsan 换行  lisi

	''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

			name:   ‘zhangsan \n lisi’：输出；zhangsan \n  lisi
```



#### b、对象、Map（属性和值）（键值对）：

```
k: v：在下一行来写对象的属性和值的关系；注意缩进

	对象还是k: v的方式
```

```yaml
friends:
		lastName: zhangsan
		age: 20
```

行内写法：

```yaml
friends: {lastName: zhangsan,age: 18}
```



#### c、数组（List、Set）：

用- 值表示数组中的一个元素

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```



#### d、[yaml参考语法](https://bitbucket.org/asomov/snakeyaml/wiki/Home)

## 3、配置文件值注入

- 配置文件

```xml
person:
  name: aqqje
  age: 20
  boss: true
  data: 2018/7/29
  map: {k1: v1, k2: v2}
  list: [AQQJE, LIULI, WLN]
  dog:
    name: xq
    age: 2
```



- javaBean

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定；
 *      prefix = "person"：配置文件中哪个下面的所有属性进行一一映射
 *
 * 只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能；
 *
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean boss;
    private Date data;

    private Map map;
    private List list;
    private Dog dog;
```

- 导入配置文件处理器，编写配置提示

```xml
<!--导入配置文件处理器，配置文件进行绑定就会有提示-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>
```

#### A、 properties配置文件在idea中默认utf-8可能会乱码

- 调整

  settings -> File Encodings -> Default encoding for properties files: UTF-8 &&  **√** Transparent native-to-ascii conversion

#### B、配置文件注入值数据校验

```java
@Component
//@ConfigurationProperties(prefix = "person")
@Validated //@Validated 开启JSR303数据校验
public class Person {
    /**
     * <bean class="Person">
     *      <property name="lastName" value="字面量/${key}从环境变量、配置文件中获取值/#{SpEL}"></property>
     * <bean/>
     */
    // @Value("刘明")
    //lastName必须是邮箱格式
    @Email
    @Value("${person.name}")
    private String name;
    // @Value("#{11 * 2}")
    private Integer age;
    private Boolean boss;
    private Date data;

    //@Value("${person.map}")
    private Map map;
    private List list;
    private Dog dog;
```



#### C、@Value获取值和@ConfigurationProperties获取值比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |



- 配置文件yml还是properties他们都能获取到值；
- 如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value；
- 如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties；

#### D、@PropertySource&@ImportResource&@Bean

- @**PropertySource**：加载指定的配置文件

```java
@PropertySource(value = {"classpath:person.properties"})
```

- @**ImportResource**：导入Spring的配置文件，让配置文件里面的内容生效

> 1、Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别；
>
> 2、想让Spring的配置文件生效，加载进来；@**ImportResource**标注在一个***配置类***(主程序类、主入口类)上
>
> 3、@ImportResource(locations = {"classpath:beans.xml"})
> 导入Spring的配置文件让其生效

- SpringBoot推荐给容器中添加组件的方式；推荐使用全注解的方式

> 1、配置类**@Configuration**------>Spring配置文件
>
> 2、使用**@Bean**给容器中添加组件

myConfig.java:

```java
/**
 * @Configuration：指明当前类是一个配置类；就是来替代之前的Spring配置文件
 *
 * 在配置文件中用<bean><bean/>标签添加组件
 *
 */
@Configuration
public class myConfig {
    //将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService(){
        System.out.println("配置类@Bean给容器中添加组件了...");
        return new HelloService();
    }
}
```

## 4、配置文件占位符

### A、随机数

```java
${random.value}、${random.int}、${random.long}
${random.int(10)}、${random.int[1024,65536]}
```

### B、占位符获取之前配置的值，如果没有可以是用:指定默认值

```properties
person.name=刘明${random.uuid}
person.age=${random.int}
person.boss=false
person.data=2018/7/29
person.map.k1=v1
person.map..k2=v2
person.list=a, b, c
person.dog.name=${person.hello: hello}_dog
person.dog.age=3
```

## 5、Profile

### A、多Profile文件

我们在主配置文件编写的时候，文件名可以是   application-{profile}.properties/yml

默认使用application.properties的配置；

### B、yml支持多文档块方式

```properties
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 11
spring:
  profiles: dev
---
server:
  port: 7021
spring:
  profiles: prod
```

### C、激活指定profile

> 1、在配置文件中指定  spring.profiles.active=dev
>
> 2、命令行：
>
> ```
> java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --			spring.profiles.active=dev；
> 
> 可以直接在测试的时候，配置传入命令行参数
> 
> ```
>
> 3、虚拟机参数；
>
> ```
>  -Dspring.profiles.active=dev
> 
> ```

## 6、配置文件加载位置

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

–file:./config/

–file:./

–classpath:/config/

–classpath:/

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；



<u>***还可以通过spring.config.location来改变默认的配置文件位置***</u>

**项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；**

java -jar demo-0.0.1-SNAPSHOT.jar --spring.config.location=C:/application.properties



## 7、外部配置加载顺序

**==SpringBoot也可以从以下位置加载配置； 优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置==**

- 1.命令行参数

所有的配置都可以在命令行上进行指定

java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087  --server.context-path=/abc

多个配置用空格分开； --配置项=值



- 2.来自java:comp/env的JNDI属性
- 3.Java系统属性（System.getProperties()）
- 4.操作系统环境变量
- 5.RandomValuePropertySource配置的random.*属性值



**由jar包外向jar包内进行寻找，优先加载带profile**

- <u>*6.jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件*</u>
- <u>*7.jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件*</u>



**再来加载不带profile**

- <u>*8.jar包外部的application.properties或application.yml(不带spring.profile)配置文件*</u>
- <u>*9.jar包内部的application.properties或application.yml(不带spring.profile)配置文件*</u>



- 10.@Configuration注解类上的@PropertySource
- 11.通过SpringApplication.setDefaultProperties指定的默认属性

所有支持的配置加载来源；

[参考官方文档](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config)



## 8、自动配置原理

配置文件到底能写什么？怎么写？自动配置原理；

[配置文件能配置的属性参照](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties)



### A、**自动配置原理：**

#### a、SpringBoot启动的时候加载主配置类，开启了自动配置功能 ==@EnableAutoConfiguration==

#### b、@EnableAutoConfiguration 作用：**

- 利用EnableAutoConfigurationImportSelector给容器中导入一些组件？

- 可以查看selectImports()方法的内容；

- List<String> configurations = getCandidateConfigurations(annotationMetadata,      attributes);获取候选的配置

  - ```java
    SpringFactoriesLoader.loadFactoryNames()
    扫描所有jar包类路径下  META-INF/spring.factories
    把扫描到的这些文件的内容包装成properties对象
    从properties中获取到EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加在容器中
    
    ```

    ​

**==将 类路径下  META-INF/spring.factories 里面配置的所有EnableAutoConfiguration的值加入到了容器中；==**

```properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.jest.JestAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mobile.DeviceResolverAutoConfiguration,\
org.springframework.boot.autoconfigure.mobile.DeviceDelegatingViewResolverAutoConfiguration,\
org.springframework.boot.autoconfigure.mobile.SitePreferenceAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.reactor.ReactorAutoConfiguration,\
org.springframework.boot.autoconfigure.security.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.FallbackWebSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.OAuth2AutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.social.SocialWebAutoConfiguration,\
org.springframework.boot.autoconfigure.social.FacebookAutoConfiguration,\
org.springframework.boot.autoconfigure.social.LinkedInAutoConfiguration,\
org.springframework.boot.autoconfigure.social.TwitterAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.web.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.ServerPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.web.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.WebSocketAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration
```

每一个这样的  xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；

#### b、每一个自动配置类进行自动配置功能；

#### c、以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

```java
@Configuration   //表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties(HttpEncodingProperties.class)  //启动指定类的ConfigurationProperties功能；将配置文件中对应的值和HttpEncodingProperties绑定起来；并把HttpEncodingProperties加入到ioc容器中

@ConditionalOnWebApplication //Spring底层@Conditional注解（Spring注解版），根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效；    判断当前应用是否是web应用，如果是，当前配置类生效

@ConditionalOnClass(CharacterEncodingFilter.class)  //判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；

@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true)  //判断配置文件中是否存在某个配置  spring.http.encoding.enabled；如果不存在，判断也是成立的
//即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
public class HttpEncodingAutoConfiguration {
  
  	//他已经和SpringBoot的配置文件映射了
  	private final HttpEncodingProperties properties;
  
   //只有一个有参构造器的情况下，参数的值就会从容器中拿
  	public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
		this.properties = properties;
	}
  
    @Bean   //给容器中添加一个组件，这个组件的某些值需要从properties中获取
	@ConditionalOnMissingBean(CharacterEncodingFilter.class) //判断容器没有这个组件？
	public CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
		filter.setEncoding(this.properties.getCharset().name());
		filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
		filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
		return filter;
	}
```

根据当前不同的条件判断，决定这个配置类是否生效？

一但这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；







#### d、所有在配置文件中能配置的属性都是在xxxxProperties类中封装者‘；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(prefix = "spring.http.encoding")  //从配置文件中获取指定的值和bean的属性进行绑定
public class HttpEncodingProperties {

   public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");
```





### B、 **精髓：**

#### a、SpringBoot启动会加载大量的自动配置类

#### b、我们看我们需要的功能有没有SpringBoot默认写好的自动配置类

#### c、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）

#### d、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值



> 1、xxxxAutoConfigurartion：自动配置类；
>
> 2、给容器中添加组件；
>
> 3、xxxxProperties:封装配置文件中相关属性；





### C、细节

#### A、@Conditional派生注解（Spring注解版原生的@Conditional作用）

```
作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；	

```

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

**自动配置类必须在一定的条件下才能生效；**

我们怎么知道哪些自动配置类生效；

**==我们可以通过启用  debug=true属性；来让控制台打印自动配置报告==**，这样我们就可以很方便的知道哪些自动配置类生效；

```java
=========================
AUTO-CONFIGURATION REPORT
=========================


Positive matches:（自动配置类启用的）
-----------------

   DispatcherServletAutoConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.web.servlet.DispatcherServlet'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
      - @ConditionalOnWebApplication (required) found StandardServletEnvironment (OnWebApplicationCondition)
        
    
Negative matches:（没有启动，没有匹配成功的自动配置类）
-----------------

   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'javax.jms.ConnectionFactory', 'org.apache.activemq.ActiveMQConnectionFactory' (OnClassCondition)

   AopAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'org.aspectj.lang.annotation.Aspect', 'org.aspectj.lang.reflect.Advice' (OnClassCondition)
        
```

# 三、日志

## 1、日志框架

 小张；开发一个大型系统；

> 1、System.out.println("")；将关键数据打印在控制台；去掉？写在一个文件？
>
> 2、框架来记录系统的一些运行时信息；日志框架 ；  zhanglogging.jar；
>
> 3、高大上的几个功能？异步模式？自动归档？xxxx？  zhanglogging-good.jar？
>
> 4、将以前框架卸下来？换上新的框架，重新修改之前相关的API；zhanglogging-prefect.jar；
>
> 5、JDBC---数据库驱动；
>
> ```
> 	写了一个统一的接口层；日志门面（日志的一个抽象层）；logging-abstract.jar；
> 
> 	给项目中导入具体的日志实现就行了；我们之前的日志框架都是实现的抽象层；
> 
> ```
>
>

		

**市面上的日志框架；**

JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

| 日志门面  （日志的抽象层）                                   | 日志实现                                             |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| ~~JCL（Jakarta  Commons Logging）~~    SLF4j（Simple  Logging Facade for Java）    **~~jboss-logging~~** | Log4j  JUL（java.util.logging）  Log4j2  **Logback** |

左边选一个门面（抽象层）、右边来选一个实现；

日志门面：  SLF4J；

日志实现：Logback；



SpringBoot：底层是Spring框架，Spring框架默认是用JCL；‘

```
**SpringBoot选用 SLF4j和logback**

```



## 2、SLF4j使用

### A、如何在系统中使用SLF4j   https://www.slf4j.org

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法；

给系统里面导入slf4j的jar和  logback的实现jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

图示；

![](https://www.slf4j.org/images/concrete-bindings.png)



每一个日志的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件；**

### B、遗留问题

#### a、（slf4j+logback）: Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis、xxxx 

```
***统一日志记录，即使是别的框架和我一起统一使用slf4j进行输出？***

```

![](https://www.slf4j.org/images/legacy.png)  

#### b、如何让系统中所有的日志都统一到slf4j

> 1、将系统中其他日志框架先排除出去
>
> 2、用中间包来替换原有的日志框架
>
> 3、我们导入slf4j其他的实现



## 3、SpringBoot日志关系

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>s
```

SpringBoot使用它来做日志功能；

```xml
	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-logging</artifactId>
		</dependency>
```

总结：

```
1）、SpringBoot底层也是使用slf4j+logback的方式进行日志记录

2）、SpringBoot也把其他的日志都替换成了slf4j；

3）、中间替换包？

```

```java
@SuppressWarnings("rawtypes")
public abstract class LogFactory {

    static String UNSUPPORTED_OPERATION_IN_JCL_OVER_SLF4J = "http://www.slf4j.org/codes.html#unsupported_operation_in_jcl_over_slf4j";

    static LogFactory logFactory = new SLF4JLogFactory();
```



```
4）、如果我们要引入其他框架？一定要把这个框架的默认日志依赖移除掉？

		Spring框架用的是commons-logging；

```

```xml
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<exclusions>
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
```

**==SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可；==**

## 4、日志使用；

### 1、默认配置

SpringBoot默认帮我们配置好了日志；

```java
	//记录器
	Logger logger = LoggerFactory.getLogger(getClass());
	@Test
	public void contextLoads() {
		//System.out.println();

		//日志的级别；
		//由低到高   trace<debug<info<warn<error
		//可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
		logger.trace("这是trace日志...");
		logger.debug("这是debug日志...");
		//SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
		logger.info("这是info日志...");
		logger.warn("这是warn日志...");
		logger.error("这是error日志...");


	}
```



```
    日志输出格式：
		%d表示日期时间，
		%thread表示线程名，
		%-5level：级别从左显示5个字符宽度
		%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
		%msg：日志消息，
		%n是换行符
    -->
    %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n


```

SpringBoot修改日志的默认配置

```properties
logging.level.com.aqqje=trace


#logging.path=
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
#logging.file=G:/springboot.log

# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
logging.path=/spring/log

#  在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
```

| logging.file | logging.path | Example  | Description                        |
| ------------ | ------------ | -------- | ---------------------------------- |
| (none)       | (none)       |          | 只在控制台输出                     |
| 指定文件名   | (none)       | my.log   | 输出日志到my.log文件               |
| (none)       | 指定目录     | /var/log | 输出到指定目录的 spring.log 文件中 |

### 2、指定配置

给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用他默认配置的了

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

logback.xml：直接就被日志框架识别了；

**logback-spring.xml**：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
  	可以指定某段配置只在某个环境下生效
</springProfile>

```

如：

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
			%d表示日期时间，
			%thread表示线程名，
			%-5level：级别从左显示5个字符宽度
			%logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
			%msg：日志消息，
			%n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
```



如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误

 `no applicable action for [springProfile]`

## 5、切换日志框架

可以按照slf4j的日志适配图，进行相关的切换；

slf4j+log4j的方式；

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <artifactId>logback-classic</artifactId>
      <groupId>ch.qos.logback</groupId>
    </exclusion>
    <exclusion>
      <artifactId>log4j-over-slf4j</artifactId>
      <groupId>org.slf4j</groupId>
    </exclusion>
  </exclusions>
</dependency>

<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
</dependency>

```





切换为log4j2

```xml
   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-logging</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

------

# 四、Web开发

## 1、简介



使用SpringBoot；

**1）、创建SpringBoot应用，选中我们需要的模块；**

**2）、SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来**

**3）、自己编写业务代码；**



**自动配置原理？**

这个场景SpringBoot帮我们配置了什么？能不能修改？能修改哪些配置？能不能扩展？xxx

```
xxxxAutoConfiguration：帮我们给容器中自动配置组件；
xxxxProperties:配置类来封装配置文件的内容；

```



## 2、SpringBoot对静态资源的映射规则；

```java
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties implements ResourceLoaderAware {
  //可以设置和静态资源有关的参数，缓存时间等
```



```java
	WebMvcAuotConfiguration：
		@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			Integer cachePeriod = this.resourceProperties.getCachePeriod();
			if (!registry.hasMappingForPattern("/webjars/**")) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler("/webjars/**")
								.addResourceLocations(
										"classpath:/META-INF/resources/webjars/")
						.setCachePeriod(cachePeriod));
			}
			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
          	//静态资源文件夹映射
			if (!registry.hasMappingForPattern(staticPathPattern)) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler(staticPathPattern)
								.addResourceLocations(
										this.resourceProperties.getStaticLocations())
						.setCachePeriod(cachePeriod));
			}
		}

        //配置欢迎页映射
		@Bean
		public WelcomePageHandlerMapping welcomePageHandlerMapping(
				ResourceProperties resourceProperties) {
			return new WelcomePageHandlerMapping(resourceProperties.getWelcomePage(),
					this.mvcProperties.getStaticPathPattern());
		}

       //配置喜欢的图标
		@Configuration
		@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
		public static class FaviconConfiguration {

			private final ResourceProperties resourceProperties;

			public FaviconConfiguration(ResourceProperties resourceProperties) {
				this.resourceProperties = resourceProperties;
			}

			@Bean
			public SimpleUrlHandlerMapping faviconHandlerMapping() {
				SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
				mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
              	//所有  **/favicon.ico 
				mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
						faviconRequestHandler()));
				return mapping;
			}

			@Bean
			public ResourceHttpRequestHandler faviconRequestHandler() {
				ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
				requestHandler
						.setLocations(this.resourceProperties.getFaviconLocations());
				return requestHandler;
			}

		}

```



==1）、所有 /webjars/** ，都去 classpath:/META-INF/resources/webjars/ 找资源；==

```
webjars：以jar包的方式引入静态资源；

```

http://www.webjars.org/

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180203181751.png)

localhost:8080/webjars/jquery/3.3.1/jquery.js

```xml
<!--引入jquery-webjar-->在访问的时候只需要写webjars下面资源的名称即可
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>3.3.1</version>
		</dependency>
```



==2）、"/**" 访问当前项目的任何资源，都去（静态资源的文件夹）找映射==

```
"classpath:/META-INF/resources/", 
"classpath:/resources/",
"classpath:/static/", 
"classpath:/public/" 
"/"：当前项目的根路径

```

localhost:8080/abc ===  去静态资源文件夹里面找abc

==3）、欢迎页； 静态资源文件夹下的所有index.html页面；被"/**"映射；==

```
localhost:8080/   找index页面

```

==4）、所有的 **/favicon.ico  都是在静态资源文件下找；==



## 3、模板引擎

JSP、Velocity、Freemarker、Thymeleaf

![](https://github.com/aqqje/SpringBootLearn/raw/master/images/template-engine.png)



SpringBoot推荐的Thymeleaf；

语法更简单，功能更强大；



### A、引入thymeleaf；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
    2.1.6
</dependency>
切换thymeleaf版本
<properties>
		<thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
		<!-- 布局功能的支持程序  thymeleaf3主程序  layout2以上版本 -->
		<!-- thymeleaf2   layout1-->
		<thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
  </properties>
```



### B、Thymeleaf使用

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = Charset.forName("UTF-8");

	private static final MimeType DEFAULT_CONTENT_TYPE = MimeType.valueOf("text/html");

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";
  	//
```

只要我们把HTML页面放在classpath:/templates/，thymeleaf就能自动渲染；

使用：

##### a、导入thymeleaf的名称空间

```xml
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

#### b、使用thymeleaf语法；

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功！</h1>
    <!--th:text 将div里面的文本内容设置为 -->
    <div th:text="${hello}">这是显示欢迎信息</div>
</body>
</html>
```

### C、语法规则

#### a、th:text；改变当前元素里面的文本内容；

```
th：任意html属性；来替换原生属性的值

```

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//2018-02-04_123955.png)



#### b、表达式？

```properties
Simple expressions:（表达式语法）
    Variable Expressions: ${...}：获取变量值；OGNL；
    		1）、获取对象的属性、调用方法
    		2）、使用内置的基本对象：
    			#ctx : the context object.
    			#vars: the context variables.
                #locale : the context locale.
                #request : (only in Web Contexts) the HttpServletRequest object.
                #response : (only in Web Contexts) the HttpServletResponse object.
                #session : (only in Web Contexts) the HttpSession object.
                #servletContext : (only in Web Contexts) the ServletContext object.
                
                ${session.foo}
            3）、内置的一些工具对象：
#execInfo : information about the template being processed.
#messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
#uris : methods for escaping parts of URLs/URIs
#conversions : methods for executing the configured conversion service (if any).
#dates : methods for java.util.Date objects: formatting, component extraction, etc.
#calendars : analogous to #dates , but for java.util.Calendar objects.
#numbers : methods for formatting numeric objects.
#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
#objects : methods for objects in general.
#bools : methods for boolean evaluation.
#arrays : methods for arrays.
#lists : methods for lists.
#sets : methods for sets.
#maps : methods for maps.
#aggregates : methods for creating aggregates on arrays or collections.
#ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

    Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
    	补充：配合 th:object="${session.user}：
   <div th:object="${session.user}">
    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
    </div>
    
    Message Expressions: #{...}：获取国际化内容
    Link URL Expressions: @{...}：定义URL；
    		@{/order/process(execId=${execId},execType='FAST')}
    Fragment Expressions: ~{...}：片段引用表达式
    		<div th:insert="~{commons :: main}">...</div>
    		
Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
Special tokens:
    No-Operation: _ 
```

## 4、SpringMVC自动配置

https://docs.spring.io/spring-boot/docs/1.5.10.RELEASE/reference/htmlsingle/#boot-features-developing-web-applications

### A. Spring MVC auto-configuration

Spring Boot 自动配置好了SpringMVC

以下是SpringBoot对SpringMVC的默认配置:**==（WebMvcAutoConfiguration）==**

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

  - 自动配置了ViewResolver（视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？））
  - ContentNegotiatingViewResolver：组合所有的视图解析器的；
  - ==如何定制：我们可以自己给容器中添加一个视图解析器；自动的将其组合进来；==

- Support for serving static resources, including support for WebJars (see below).静态资源文件夹路径,webjars

- Static `index.html` support. 静态首页访问

- Custom `Favicon` support (see below).  favicon.ico

  ​

- 自动注册了 of `Converter`, `GenericConverter`, `Formatter` beans.

  - Converter：转换器；  public String hello(User user)：类型转换使用Converter
  - `Formatter`  格式化器；  2017.12.17===Date；

```java
		@Bean
		@ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在文件中配置日期格式化的规则
		public Formatter<Date> dateFormatter() {
			return new DateFormatter(this.mvcProperties.getDateFormat());//日期格式化组件
		}
```

```
==自己添加的格式化器转换器，我们只需要放在容器中即可==

```

- Support for `HttpMessageConverters` (see below).

  - HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User---Json；

  - `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；

    ==自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component）==

    ​

- Automatic registration of `MessageCodesResolver` (see below).定义错误代码生成规则

- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

  ==我们可以配置一个ConfigurableWebBindingInitializer来替换默认的；（添加到容器）==

  ```
  初始化WebDataBinder；
  请求数据=====JavaBean；
  
  
  ```

**org.springframework.boot.autoconfigure.web：web的所有自动场景；**

If you want to keep Spring Boot MVC features, and you just want to add additional [MVC configuration](https://docs.spring.io/spring/docs/4.3.14.RELEASE/spring-framework-reference/htmlsingle#mvc) (interceptors, formatters, view controllers etc.) you can add your own `@Configuration` class of type `WebMvcConfigurerAdapter`, but **without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter` or `ExceptionHandlerExceptionResolver` you can declare a `WebMvcRegistrationsAdapter` instance providing such components.

If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`.

### B、扩展SpringMVC

```xml
    <mvc:view-controller path="/hello" view-name="success"/>
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```

**==编写一个配置类（@Configuration），是WebMvcConfigurerAdapter类型；不能标注@EnableWebMvc==**;

既保留了所有的自动配置，也能用我们扩展的配置；

```java
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       // super.addViewControllers(registry);
        //浏览器发送 /aqqje 请求来到 success
        registry.addViewController("/aqqje").setViewName("success");
    }
}
```

原理：

```
1）、WebMvcAutoConfiguration是SpringMVC的自动配置类

2）、在做其他自动配置时会导入；@Import(**EnableWebMvcConfiguration**.class)

```

```java
    @Configuration
	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration {
      private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();

	 //从容器中获取所有的WebMvcConfigurer
      @Autowired(required = false)
      public void setConfigurers(List<WebMvcConfigurer> configurers) {
          if (!CollectionUtils.isEmpty(configurers)) {
              this.configurers.addWebMvcConfigurers(configurers);
            	//一个参考实现；将所有的WebMvcConfigurer相关配置都来一起调用；  
            	@Override
             // public void addViewControllers(ViewControllerRegistry registry) {
              //    for (WebMvcConfigurer delegate : this.delegates) {
               //       delegate.addViewControllers(registry);
               //   }
              }
          }
	}
```

```
3）、容器中所有的WebMvcConfigurer都会一起起作用；

4）、我们的配置类也会被调用；

效果：SpringMVC的自动配置和我们的扩展配置都会起作用；

```

### C、全面接管SpringMVC；

SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了

**我们需要在配置类中添加@EnableWebMvc即可；**

```java
/**
 *  WebMvcConfigurerAdapter 过时
 *  使用1、WebMvcConfigurationSupport 2、WebMvcConfigurer可以来扩展SpringMVC的功能
 *  @EnableWebMvc 不要接管SpringMVC
 */

@Configuration
public class MyMvcConfig extends WebMvcConfigurationSupport {
    @Override
    protected void addViewControllers(ViewControllerRegistry registry) {
        // super.addViewControllers(registry);
        //浏览器发送 /aqqje 请求来到 success
        super.addViewControllers(registry);
        registry.addViewController("/aqqje").setViewName("success");
    }
}
```

原理：

为什么@EnableWebMvc自动配置就失效了；



#### a、@EnableWebMvc的核心

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
```

##### b、WebMvcConfigurationSupport

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
```

#### c、WebMvcConfigurationSupport 问题

```java
@Configuration
@ConditionalOnWebApplication
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class,
		WebMvcConfigurerAdapter.class })
//容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class,
		ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
```

#### d、@EnableWebMvc将WebMvcConfigurationSupport组件导入进来；

#### e、导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

## 5、如何修改SpringBoot的默认配置

模式：

```
1）、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

2）、在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置

3）、在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置

```

## 6、RestfulCRUD

### A、默认访问首页

```java
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
//@EnableWebMvc   不要接管SpringMVC
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       // super.addViewControllers(registry);
        //浏览器发送 /aqqje 请求来到 success
        registry.addViewController("/aqqje").setViewName("success");
    }

    //所有的WebMvcConfigurerAdapter组件都会一起起作用
    @Bean //将组件注册在容器
    public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
        WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter() {
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
            }
        };
        return adapter;
    }
}

```

### B、国际化

**a、编写国际化配置文件；**

b、使用ResourceBundleMessageSource管理国际化资源文件

c、在页面使用fmt:message取出国际化内容



步骤：

a、编写国际化配置文件，抽取页面需要显示的国际化消息

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180211130721.png)



b、SpringBoot自动配置好了管理国际化资源文件的组件；

```java
@ConfigurationProperties(prefix = "spring.messages")
public class MessageSourceAutoConfiguration {
    
    /**
	 * Comma-separated list of basenames (essentially a fully-qualified classpath
	 * location), each following the ResourceBundle convention with relaxed support for
	 * slash based locations. If it doesn't contain a package qualifier (such as
	 * "org.mypackage"), it will be resolved from the classpath root.
	 */
	private String basename = "messages";  
    //我们的配置文件可以直接放在类路径下叫messages.properties；
    
    @Bean
	public MessageSource messageSource() {
		ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
		if (StringUtils.hasText(this.basename)) {
            //设置国际化资源文件的基础名（去掉语言国家代码的）
			messageSource.setBasenames(StringUtils.commaDelimitedListToStringArray(
					StringUtils.trimAllWhitespace(this.basename)));
		}
		if (this.encoding != null) {
			messageSource.setDefaultEncoding(this.encoding.name());
		}
		messageSource.setFallbackToSystemLocale(this.fallbackToSystemLocale);
		messageSource.setCacheSeconds(this.cacheSeconds);
		messageSource.setAlwaysUseMessageFormat(this.alwaysUseMessageFormat);
		return messageSource;
	}
```



c、配置 application.properties

```properties
spring.messages.basename=i18n.login.login
```





d、去页面获取国际化的值；

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180211134506.png)



```html
<!DOCTYPE html>
<html lang="en"  xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<link href="asserts/css/bootstrap.min.css" th:href="@{/webjars/bootstrap/4.0.0/css/bootstrap.css}" rel="stylesheet">
		<!-- Custom styles for this template -->
		<link href="asserts/css/signin.css" th:href="@{/asserts/css/signin.css}" rel="stylesheet">
	</head>

	<body class="text-center">
		<form class="form-signin" action="dashboard.html">
			<img class="mb-4" th:src="@{/asserts/img/bootstrap-solid.svg}" src="asserts/img/bootstrap-solid.svg" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<label class="sr-only" th:text="#{login.username}">Username</label>
			<input type="text" class="form-control" placeholder="Username" th:placeholder="#{login.username}" required="" autofocus="">
			<label class="sr-only" th:text="#{login.password}">Password</label>
			<input type="password" class="form-control" placeholder="Password" th:placeholder="#{login.password}" required="">
			<div class="checkbox mb-3">
				<label>
          		<input type="checkbox" value="remember-me"/> [[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm">中文</a>
			<a class="btn btn-sm">English</a>
		</form>

	</body>

</html>
```

效果：根据浏览器语言设置的信息切换了国际化；



原理：

```
国际化Locale（区域信息对象）；LocaleResolver（获取区域信息对象）；

```

```java
		@Bean
		@ConditionalOnMissingBean
		@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
		public LocaleResolver localeResolver() {
			if (this.mvcProperties
					.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
				return new FixedLocaleResolver(this.mvcProperties.getLocale());
			}
			AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
			localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
			return localeResolver;
		}
默认的就是根据请求头带来的区域信息获取Locale进行国际化
```

e、点击链接切换国际化

```java
/**
 * 可以在连接上携带区域信息
 */
public class MyLocaleResolver implements LocaleResolver {
    
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String l = request.getParameter("l");
        Locale locale = Locale.getDefault();
        if(!StringUtils.isEmpty(l)){
            String[] split = l.split("_");
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {

    }
}


 @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
}


```

### C、登陆

开发期间模板引擎页面修改以后，要实时生效

a、禁用模板引擎的缓存

```
# 禁用缓存
spring.thymeleaf.cache=false 


```

b、页面修改完成以后ctrl+f9：重新编译；



登陆错误消息的显示

```html
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```



### D、拦截器进行登陆检查

a、编写拦截器

```java
/**
 * 登录检查拦截器
 */
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object user = request.getSession().getAttribute("loginUser");
        if(user == null ){
            // 未登录，返回登录
            request.setAttribute("msg", "没权限，请登录！");
            request.getRequestDispatcher("index.html").forward(request, response);
            return false;
        }else{
            // 放行
            return true;
        }

    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}

```



b、注册拦截器

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    // 注册拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")
                .excludePathPatterns("/", "login.html", "/index.html", "/user/login");
    }

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/aqqje").setViewName("success");
    }

    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
        WebMvcConfigurer config = new WebMvcConfigurer(){
            /**
             * 第一种：配置 templates 首页【抑制spring boot的自动加载index页面为首页】,
             * 必须加载到Spring Boot 容器中才可以生效
             */
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("login");
                registry.addViewController("/index.html").setViewName("login");
                registry.addViewController("/login.html").setViewName("login");
                registry.addViewController("/main.html").setViewName("dashboard");
            }
        };
        return config;
    }

    // 注册国际化解析器
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocalResolver();
    }
}

```

c、配置静态资源（默认路径不用配置）

方式一：application.properties

```properties
#spring.mvc.static-path-pattern=/**表示所有的访问都经过静态资源路径；
spring.mvc.static-path-pattern=/**
#web.upload-path=D:/temp/study13/
#spring.resources.static-locations配置静态资源路径
spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,\
  classpath:/static/,classpath:/public/
```

方式二：MyMvcConfig.java

```java
//注册其他静态资源路径    
@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/my/**").addResourceLocations("classpath:/my/");
    }
```





### E、CRUD-员工列表

实验要求：

a、RestfulCRUD：CRUD满足Rest风格；

URI：  /资源名称/资源标识       HTTP请求方式区分对资源CRUD操作

|      | 普通CRUD（uri来区分操作） | RestfulCRUD       |
| ---- | ------------------------- | ----------------- |
| 查询 | getEmp                    | emp---GET         |
| 添加 | addEmp?xxx                | emp---POST        |
| 修改 | updateEmp?id=xxx&xxx=xx   | emp/{id}---PUT    |
| 删除 | deleteEmp?id=1            | emp/{id}---DELETE |

b、实验的请求架构;

| 实验功能                             | 请求URI | 请求方式 |
| ------------------------------------ | ------- | -------- |
| 查询所有员工                         | emps    | GET      |
| 查询某个员工(来到修改页面)           | emp/1   | GET      |
| 来到添加页面                         | emp     | GET      |
| 添加员工                             | emp     | POST     |
| 来到修改页面（查出员工进行信息回显） | emp/1   | GET      |
| 修改员工                             | emp     | PUT      |
| 删除员工                             | emp/1   | DELETE   |

c、员工列表：

#### thymeleaf公共页面元素抽取

```html
1、抽取公共片段
<div th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</div>

2、引入公共片段
<div th:insert="~{footer :: copy}"></div>
~{templatename::selector}：模板名::选择器
~{templatename::fragmentname}:模板名::片段名

3、默认效果：
insert的公共片段在div标签中
如果使用th:insert等属性进行引入，可以不用写~{}：
行内写法可以加上：[[~{}]];[(~{})]；
```



三种引入公共片段的th属性：

**th:insert**：将公共片段整个插入到声明引入的元素中

**th:replace**：将声明引入的元素替换为公共片段

**th:include**：将被引入的片段的内容包含进这个标签中



```html
<footer th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

引入方式
<div th:insert="footer :: copy"></div>
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>

效果
<div>
    <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
</div>

<footer>
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

<div>
&copy; 2011 The Good Thymes Virtual Grocery
</div>
```



引入片段的时候传入参数： 

```html
<nav class="col-md-2 d-none d-md-block bg-light sidebar" id="sidebar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active"
                   th:class="${activeUri=='main.html'?'nav-link active':'nav-link'}"
                   href="#" th:href="@{/main.html}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                        <polyline points="9 22 9 12 15 12 15 22"></polyline>
                    </svg>
                    Dashboard <span class="sr-only">(current)</span>
                </a>
            </li>

<!--引入侧边栏;传入参数-->
<div th:replace="commons/bar::#sidebar(activeUri='emps')"></div>
```

### F、CRUD-员工添加

添加页面

```html
<form>
    <div class="form-group">
        <label>LastName</label>
        <input type="text" class="form-control" placeholder="zhangsan">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input type="email" class="form-control" placeholder="zhangsan@aqqje.com">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="1">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender"  value="0">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <select class="form-control">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
            <option>5</option>
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input type="text" class="form-control" placeholder="zhangsan">
    </div>
    <button type="submit" class="btn btn-primary">添加</button>
</form>
```

提交的数据格式不对：生日：日期；

2017-12-12；2017/12/12；2017.12.12；

日期的格式化；SpringMVC将页面提交的值需要转换为指定的类型;

2017-12-12---Date； 类型转换，格式化;

默认日期是按照/的方式；

### G、CRUD-员工修改

修改添加二合一表单

```html
<!--需要区分是员工修改还是添加；-->
<form th:action="@{/emp}" method="post">
    <!--发送put请求修改员工数据-->
    <!--
1、SpringMVC中配置HiddenHttpMethodFilter;（SpringBoot自动配置好的）
2、页面创建一个post表单
3、创建一个input项，name="_method";值就是我们指定的请求方式
-->
    <input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
    <input type="hidden" name="id" th:if="${emp!=null}" th:value="${emp.id}">
    <div class="form-group">
        <label>LastName</label>
        <input name="lastName" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${emp.lastName}">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input name="email" type="email" class="form-control" placeholder="zhangsan@aqqje.com" th:value="${emp!=null}?${emp.email}">
    </div>
    <div class="form-group">
        <label>Gender</label><br/>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="1" th:checked="${emp!=null}?${emp.gender==1}">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="0" th:checked="${emp!=null}?${emp.gender==0}">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <!--提交的是部门的id-->
        <select class="form-control" name="department.id">
            <option th:selected="${emp!=null}?${dept.id == emp.department.id}" th:value="${dept.id}" th:each="dept:${depts}" th:text="${dept.departmentName}">1</option>
        </select>
    </div>
    <div class="form-group">
        <label>Birth</label>
        <input name="birth" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}">
    </div>
    <button type="submit" class="btn btn-primary" th:text="${emp!=null}?'修改':'添加'">添加</button>
</form>
```

### H、CRUD-员工删除

```html
<tr th:each="emp:${emps}">
    <td th:text="${emp.id}"></td>
    <td>[[${emp.lastName}]]</td>
    <td th:text="${emp.email}"></td>
    <td th:text="${emp.gender}==0?'女':'男'"></td>
    <td th:text="${emp.department.departmentName}"></td>
    <td th:text="${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}"></td>
    <td>
        <a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.id}">编辑</a>
        <button th:attr="del_uri=@{/emp/}+${emp.id}" class="btn btn-sm btn-danger deleteBtn">删除</button>
    </td>
</tr>


<script>
    $(".deleteBtn").click(function(){
        //删除当前员工的
        $("#deleteEmpForm").attr("action",$(this).attr("del_uri")).submit();
        return false;
    });
</script>
```



## 7、错误处理机制

### A、SpringBoot默认的错误处理机制

默认效果：

```
	1）、浏览器，返回一个默认的错误页面

```

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180226173408.png)

  浏览器发送请求的请求头：

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180226180347.png)

```
	2）、如果是其他客户端，默认响应一个json数据

```

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180226173527.png)

	

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180226180504.png)

原理：

```
可以参照ErrorMvcAutoConfiguration；错误处理的自动配置；

```

  	给容器中添加了以下组件

```
1、DefaultErrorAttributes：

```

```java
帮我们在页面共享信息；
@Override
	public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes,
			boolean includeStackTrace) {
		Map<String, Object> errorAttributes = new LinkedHashMap<String, Object>();
		errorAttributes.put("timestamp", new Date());
		addStatus(errorAttributes, requestAttributes);
		addErrorDetails(errorAttributes, requestAttributes, includeStackTrace);
		addPath(errorAttributes, requestAttributes);
		return errorAttributes;
	}
```



```
2、BasicErrorController：处理默认/error请求

```

```java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {
    
    @RequestMapping(produces = "text/html")//产生html类型的数据；浏览器发送的请求来到这个方法处理
	public ModelAndView errorHtml(HttpServletRequest request,
			HttpServletResponse response) {
		HttpStatus status = getStatus(request);
		Map<String, Object> model = Collections.unmodifiableMap(getErrorAttributes(
				request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
		response.setStatus(status.value());
        
        //去哪个页面作为错误页面；包含页面地址和页面内容
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		return (modelAndView == null ? new ModelAndView("error", model) : modelAndView);
	}

	@RequestMapping
	@ResponseBody    //产生json数据，其他客户端来到这个方法处理；
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		Map<String, Object> body = getErrorAttributes(request,
				isIncludeStackTrace(request, MediaType.ALL));
		HttpStatus status = getStatus(request);
		return new ResponseEntity<Map<String, Object>>(body, status);
	}
```



```
3、ErrorPageCustomizer：

```

```java
	@Value("${error.path:/error}")
	private String path = "/error";  系统出现错误以后来到error请求进行处理；（web.xml注册的错误页面规则）
```



```
4、DefaultErrorViewResolver：

```

```java
@Override
	public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status,
			Map<String, Object> model) {
		ModelAndView modelAndView = resolve(String.valueOf(status), model);
		if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
			modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);
		}
		return modelAndView;
	}

	private ModelAndView resolve(String viewName, Map<String, Object> model) {
        //默认SpringBoot可以去找到一个页面？  error/404
		String errorViewName = "error/" + viewName;
        
        //模板引擎可以解析这个页面地址就用模板引擎解析
		TemplateAvailabilityProvider provider = this.templateAvailabilityProviders
				.getProvider(errorViewName, this.applicationContext);
		if (provider != null) {
            //模板引擎可用的情况下返回到errorViewName指定的视图地址
			return new ModelAndView(errorViewName, model);
		}
        //模板引擎不可用，就在静态资源文件夹下找errorViewName对应的页面   error/404.html
		return resolveResource(errorViewName, model);
	}
```



```
步骤：

	一但系统出现4xx或者5xx之类的错误；ErrorPageCustomizer就会生效（定制错误的响应规则）；就会来到/error请求；就会被**BasicErrorController**处理；

	1）响应页面；去哪个页面是由**DefaultErrorViewResolver**解析得到的；

```

```java
protected ModelAndView resolveErrorView(HttpServletRequest request,
      HttpServletResponse response, HttpStatus status, Map<String, Object> model) {
    //所有的ErrorViewResolver得到ModelAndView
   for (ErrorViewResolver resolver : this.errorViewResolvers) {
      ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
      if (modelAndView != null) {
         return modelAndView;
      }
   }
   return null;
}
```

### B、如果定制错误响应：

#### a、如何定制错误的页面；**

```
		**1）、有模板引擎的情况下；error/状态码;** 【将错误页面命名为  错误状态码.html 放在模板引擎文件夹里面的 error文件夹下】，发生此状态码的错误就会来到  对应的页面；

		我们可以使用4xx和5xx作为错误页面的文件名来匹配这种类型的所有错误，精确优先（优先寻找精确的状态码.html）；		

		页面能获取的信息；

			timestamp：时间戳

			status：状态码

			error：错误提示

			exception：异常对象

			message：异常消息

			errors：JSR303数据校验的错误都在这里

		2）、没有模板引擎（模板引擎找不到这个错误页面），静态资源文件夹下找；

		3）、以上都没有错误页面，就是默认来到SpringBoot默认的错误提示页面；

```



#### b、如何定制错误的json数据；

```
	1）、自定义异常处理&返回定制json数据；

```

```java
@ControllerAdvice
public class MyExceptionHandler {

    @ResponseBody
    @ExceptionHandler(UserNotExistException.class)
    public Map<String,Object> handleException(Exception e){
        Map<String,Object> map = new HashMap<>();
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        return map;
    }
}
//没有自适应效果...
```



```
	2）、转发到/error进行自适应响应效果处理

```

```java
 @ExceptionHandler(UserNotExistException.class)
    public String handleException(Exception e, HttpServletRequest request){
        Map<String,Object> map = new HashMap<>();
        //传入我们自己的错误状态码  4xx 5xx，否则就不会进入定制错误页面的解析流程
        /**
         * Integer statusCode = (Integer) request
         .getAttribute("javax.servlet.error.status_code");
         */
        request.setAttribute("javax.servlet.error.status_code",500);
        map.put("code","user.notexist");
        map.put("message",e.getMessage());
        //转发到/error
        return "forward:/error";
    }
```

#### c、将我们的定制数据携带出去；

出现错误以后，会来到/error请求，会被BasicErrorController处理，响应出去可以获取的数据是由getErrorAttributes得到的（是AbstractErrorController（ErrorController）规定的方法）；

```
1、完全来编写一个ErrorController的实现类【或者是编写AbstractErrorController的子类】，放在容器中；

2、页面上能用的数据，或者是json返回能用的数据都是通过errorAttributes.getErrorAttributes得到；

		容器中DefaultErrorAttributes.getErrorAttributes()；默认进行数据处理的；

```

自定义ErrorAttributes

```java
//给容器中加入我们自己定义的ErrorAttributes
@Component
public class MyErrorAttributes extends DefaultErrorAttributes {

    @Override
    public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes, boolean includeStackTrace) {
        Map<String, Object> map = super.getErrorAttributes(requestAttributes, includeStackTrace);
        map.put("company","aqqje");
        return map;
    }
}
```

最终的效果：响应是自适应的，可以通过定制ErrorAttributes改变需要返回的内容，

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180228135513.png)



## 8、配置嵌入式Servlet容器

SpringBoot默认使用Tomcat作为嵌入式的Servlet容器；

![](C:/Users/Administrator/Desktop/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180301142915.png)



问题？

### A、如何定制和修改Servlet容器的相关配置；

1、修改和server有关的配置（ServerProperties【也是EmbeddedServletContainerCustomizer】）；

```properties
server.port=8081
server.context-path=/crud

server.tomcat.uri-encoding=UTF-8

//通用的Servlet容器设置
server.xxx
//Tomcat的设置
server.tomcat.xxx
```

2、编写一个**EmbeddedServletContainerCustomizer**：嵌入式的Servlet容器的定制器；来修改Servlet容器的配置

```java
@Bean  //一定要将这个定制器加入到容器中
public EmbeddedServletContainerCustomizer embeddedServletContainerCustomizer(){
    return new EmbeddedServletContainerCustomizer() {

        //定制嵌入式的Servlet容器相关的规则
        @Override
        public void customize(ConfigurableEmbeddedServletContainer container) {
            container.setPort(8083);
        }
    };
}
```

### B、注册Servlet三大组件【Servlet、Filter、Listener】

由于SpringBoot默认是以jar包的方式启动嵌入式的Servlet容器来启动SpringBoot的web应用，没有web.xml文件。

注册三大组件用以下方式

ServletRegistrationBean

```java
//注册三大组件
@Bean
public ServletRegistrationBean myServlet(){
    ServletRegistrationBean registrationBean = new ServletRegistrationBean(new MyServlet(),"/myServlet");
    return registrationBean;
}

```

FilterRegistrationBean

```java
@Bean
public FilterRegistrationBean myFilter(){
    FilterRegistrationBean registrationBean = new FilterRegistrationBean();
    registrationBean.setFilter(new MyFilter());
    registrationBean.setUrlPatterns(Arrays.asList("/hello","/myServlet"));
    return registrationBean;
}
```

ServletListenerRegistrationBean

```java
@Bean
public ServletListenerRegistrationBean myListener(){
    ServletListenerRegistrationBean<MyListener> registrationBean = new ServletListenerRegistrationBean<>(new MyListener());
    return registrationBean;
}
```



SpringBoot帮我们自动SpringMVC的时候，自动的注册SpringMVC的前端控制器；DIspatcherServlet；

DispatcherServletAutoConfiguration中：

```java
@Bean(name = DEFAULT_DISPATCHER_SERVLET_REGISTRATION_BEAN_NAME)
@ConditionalOnBean(value = DispatcherServlet.class, name = DEFAULT_DISPATCHER_SERVLET_BEAN_NAME)
public ServletRegistrationBean dispatcherServletRegistration(
      DispatcherServlet dispatcherServlet) {
   ServletRegistrationBean registration = new ServletRegistrationBean(
         dispatcherServlet, this.serverProperties.getServletMapping());
    //默认拦截： /  所有请求；包静态资源，但是不拦截jsp请求；   /*会拦截jsp
    //可以通过server.servletPath来修改SpringMVC前端控制器默认拦截的请求路径
    
   registration.setName(DEFAULT_DISPATCHER_SERVLET_BEAN_NAME);
   registration.setLoadOnStartup(
         this.webMvcProperties.getServlet().getLoadOnStartup());
   if (this.multipartConfig != null) {
      registration.setMultipartConfig(this.multipartConfig);
   }
   return registration;
}

```

2）、SpringBoot能不能支持其他的Servlet容器；

### C、替换为其他嵌入式Servlet容器

![](C:/Users/Administrator/Desktop/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180302114401.png)

默认支持：

Tomcat（默认使用）

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   引入web模块默认就是使用嵌入式的Tomcat作为Servlet容器；
</dependency>
```

Jetty

```xml
<!-- 引入web模块 -->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   <exclusions>
      <exclusion>
         <artifactId>spring-boot-starter-tomcat</artifactId>
         <groupId>org.springframework.boot</groupId>
      </exclusion>
   </exclusions>
</dependency>

<!--引入其他的Servlet容器-->
<dependency>
   <artifactId>spring-boot-starter-jetty</artifactId>
   <groupId>org.springframework.boot</groupId>
</dependency>
```

Undertow

```xml
<!-- 引入web模块 -->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   <exclusions>
      <exclusion>
         <artifactId>spring-boot-starter-tomcat</artifactId>
         <groupId>org.springframework.boot</groupId>
      </exclusion>
   </exclusions>
</dependency>

<!--引入其他的Servlet容器-->
<dependency>
   <artifactId>spring-boot-starter-undertow</artifactId>
   <groupId>org.springframework.boot</groupId>
</dependency>
```

### D、嵌入式Servlet容器自动配置原理；



EmbeddedServletContainerAutoConfiguration：嵌入式的Servlet容器自动配置？

```java
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
@Configuration
@ConditionalOnWebApplication
@Import(BeanPostProcessorsRegistrar.class)
//导入BeanPostProcessorsRegistrar：Spring注解版；给容器中导入一些组件
//导入了EmbeddedServletContainerCustomizerBeanPostProcessor：
//后置处理器：bean初始化前后（创建完对象，还没赋值赋值）执行初始化工作
public class EmbeddedServletContainerAutoConfiguration {
    
    @Configuration
	@ConditionalOnClass({ Servlet.class, Tomcat.class })//判断当前是否引入了Tomcat依赖；
	@ConditionalOnMissingBean(value = EmbeddedServletContainerFactory.class, search = SearchStrategy.CURRENT)//判断当前容器没有用户自己定义EmbeddedServletContainerFactory：嵌入式的Servlet容器工厂；作用：创建嵌入式的Servlet容器
	public static class EmbeddedTomcat {

		@Bean
		public TomcatEmbeddedServletContainerFactory tomcatEmbeddedServletContainerFactory() {
			return new TomcatEmbeddedServletContainerFactory();
		}

	}
    
    /**
	 * Nested configuration if Jetty is being used.
	 */
	@Configuration
	@ConditionalOnClass({ Servlet.class, Server.class, Loader.class,
			WebAppContext.class })
	@ConditionalOnMissingBean(value = EmbeddedServletContainerFactory.class, search = SearchStrategy.CURRENT)
	public static class EmbeddedJetty {

		@Bean
		public JettyEmbeddedServletContainerFactory jettyEmbeddedServletContainerFactory() {
			return new JettyEmbeddedServletContainerFactory();
		}

	}

	/**
	 * Nested configuration if Undertow is being used.
	 */
	@Configuration
	@ConditionalOnClass({ Servlet.class, Undertow.class, SslClientAuthMode.class })
	@ConditionalOnMissingBean(value = EmbeddedServletContainerFactory.class, search = SearchStrategy.CURRENT)
	public static class EmbeddedUndertow {

		@Bean
		public UndertowEmbeddedServletContainerFactory undertowEmbeddedServletContainerFactory() {
			return new UndertowEmbeddedServletContainerFactory();
		}

	}
```

1）、EmbeddedServletContainerFactory（嵌入式Servlet容器工厂）

```java
public interface EmbeddedServletContainerFactory {

   //获取嵌入式的Servlet容器
   EmbeddedServletContainer getEmbeddedServletContainer(
         ServletContextInitializer... initializers);

}
```

![](C:/Users/Administrator/Desktop/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180302144835.png)

2）、EmbeddedServletContainer：（嵌入式的Servlet容器）

![](C:/Users/Administrator/Desktop/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180302144910.png)



3）、以**TomcatEmbeddedServletContainerFactory**为例

```java
@Override
public EmbeddedServletContainer getEmbeddedServletContainer(
      ServletContextInitializer... initializers) {
    //创建一个Tomcat
   Tomcat tomcat = new Tomcat();
    
    //配置Tomcat的基本环节
   File baseDir = (this.baseDirectory != null ? this.baseDirectory
         : createTempDir("tomcat"));
   tomcat.setBaseDir(baseDir.getAbsolutePath());
   Connector connector = new Connector(this.protocol);
   tomcat.getService().addConnector(connector);
   customizeConnector(connector);
   tomcat.setConnector(connector);
   tomcat.getHost().setAutoDeploy(false);
   configureEngine(tomcat.getEngine());
   for (Connector additionalConnector : this.additionalTomcatConnectors) {
      tomcat.getService().addConnector(additionalConnector);
   }
   prepareContext(tomcat.getHost(), initializers);
    
    //将配置好的Tomcat传入进去，返回一个EmbeddedServletContainer；并且启动Tomcat服务器
   return getTomcatEmbeddedServletContainer(tomcat);
}
```

4）、我们对嵌入式容器的配置修改是怎么生效？

```
ServerProperties、EmbeddedServletContainerCustomizer


```



**EmbeddedServletContainerCustomizer**：定制器帮我们修改了Servlet容器的配置？

怎么修改的原理？

5）、容器中导入了**EmbeddedServletContainerCustomizerBeanPostProcessor**

```java
//初始化之前
@Override
public Object postProcessBeforeInitialization(Object bean, String beanName)
      throws BeansException {
    //如果当前初始化的是一个ConfigurableEmbeddedServletContainer类型的组件
   if (bean instanceof ConfigurableEmbeddedServletContainer) {
       //
      postProcessBeforeInitialization((ConfigurableEmbeddedServletContainer) bean);
   }
   return bean;
}

private void postProcessBeforeInitialization(
			ConfigurableEmbeddedServletContainer bean) {
    //获取所有的定制器，调用每一个定制器的customize方法来给Servlet容器进行属性赋值；
    for (EmbeddedServletContainerCustomizer customizer : getCustomizers()) {
        customizer.customize(bean);
    }
}

private Collection<EmbeddedServletContainerCustomizer> getCustomizers() {
    if (this.customizers == null) {
        // Look up does not include the parent context
        this.customizers = new ArrayList<EmbeddedServletContainerCustomizer>(
            this.beanFactory
            //从容器中获取所有这葛类型的组件：EmbeddedServletContainerCustomizer
            //定制Servlet容器，给容器中可以添加一个EmbeddedServletContainerCustomizer类型的组件
            .getBeansOfType(EmbeddedServletContainerCustomizer.class,
                            false, false)
            .values());
        Collections.sort(this.customizers, AnnotationAwareOrderComparator.INSTANCE);
        this.customizers = Collections.unmodifiableList(this.customizers);
    }
    return this.customizers;
}

ServerProperties也是定制器
```

步骤：

1）、SpringBoot根据导入的依赖情况，给容器中添加相应的EmbeddedServletContainerFactory【TomcatEmbeddedServletContainerFactory】

2）、容器中某个组件要创建对象就会惊动后置处理器；EmbeddedServletContainerCustomizerBeanPostProcessor；

只要是嵌入式的Servlet容器工厂，后置处理器就工作；

3）、后置处理器，从容器中获取所有的**EmbeddedServletContainerCustomizer**，调用定制器的定制方法



###5）、嵌入式Servlet容器启动原理；

什么时候创建嵌入式的Servlet容器工厂？什么时候获取嵌入式的Servlet容器并启动Tomcat；

获取嵌入式的Servlet容器工厂：

1）、SpringBoot应用启动运行run方法

2）、refreshContext(context);SpringBoot刷新IOC容器【创建IOC容器对象，并初始化容器，创建容器中的每一个组件】；如果是web应用创建**AnnotationConfigEmbeddedWebApplicationContext**，否则：**AnnotationConfigApplicationContext**

3）、refresh(context);**刷新刚才创建好的ioc容器；**

```java
public void refresh() throws BeansException, IllegalStateException {
   synchronized (this.startupShutdownMonitor) {
      // Prepare this context for refreshing.
      prepareRefresh();

      // Tell the subclass to refresh the internal bean factory.
      ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

      // Prepare the bean factory for use in this context.
      prepareBeanFactory(beanFactory);

      try {
         // Allows post-processing of the bean factory in context subclasses.
         postProcessBeanFactory(beanFactory);

         // Invoke factory processors registered as beans in the context.
         invokeBeanFactoryPostProcessors(beanFactory);

         // Register bean processors that intercept bean creation.
         registerBeanPostProcessors(beanFactory);

         // Initialize message source for this context.
         initMessageSource();

         // Initialize event multicaster for this context.
         initApplicationEventMulticaster();

         // Initialize other special beans in specific context subclasses.
         onRefresh();

         // Check for listener beans and register them.
         registerListeners();

         // Instantiate all remaining (non-lazy-init) singletons.
         finishBeanFactoryInitialization(beanFactory);

         // Last step: publish corresponding event.
         finishRefresh();
      }

      catch (BeansException ex) {
         if (logger.isWarnEnabled()) {
            logger.warn("Exception encountered during context initialization - " +
                  "cancelling refresh attempt: " + ex);
         }

         // Destroy already created singletons to avoid dangling resources.
         destroyBeans();

         // Reset 'active' flag.
         cancelRefresh(ex);

         // Propagate exception to caller.
         throw ex;
      }

      finally {
         // Reset common introspection caches in Spring's core, since we
         // might not ever need metadata for singleton beans anymore...
         resetCommonCaches();
      }
   }
}
```

4）、  onRefresh(); web的ioc容器重写了onRefresh方法

5）、webioc容器会创建嵌入式的Servlet容器；**createEmbeddedServletContainer**();

**6）、获取嵌入式的Servlet容器工厂：**

EmbeddedServletContainerFactory containerFactory = getEmbeddedServletContainerFactory();

```
从ioc容器中获取EmbeddedServletContainerFactory 组件；**TomcatEmbeddedServletContainerFactory**创建对象，后置处理器一看是这个对象，就获取所有的定制器来先定制Servlet容器的相关配置；

```

7）、**使用容器工厂获取嵌入式的Servlet容器**：this.embeddedServletContainer = containerFactory      .getEmbeddedServletContainer(getSelfInitializer());

8）、嵌入式的Servlet容器创建对象并启动Servlet容器；

**先启动嵌入式的Servlet容器，再将ioc容器中剩下没有创建出的对象获取出来；**

**==IOC容器启动创建嵌入式的Servlet容器==**



## 9、使用外置的Servlet容器

嵌入式Servlet容器：应用打成可执行的jar

```
	优点：简单、便携；

	缺点：默认不支持JSP、优化定制比较复杂（使用定制器【ServerProperties、自定义EmbeddedServletContainerCustomizer】，自己编写嵌入式Servlet容器的创建工厂【EmbeddedServletContainerFactory】）；

```



外置的Servlet容器：外面安装Tomcat---应用war包的方式打包；

### A、步骤

#### a、必须创建一个war项目；（利用idea创建好目录结构）

#### b、将嵌入式的Tomcat指定为provided；

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-tomcat</artifactId>
   <scope>provided</scope>
</dependency>
```

#### c、必须编写一个**SpringBootServletInitializer**的子类，并调用configure方法

```java
public class ServletInitializer extends SpringBootServletInitializer {

   @Override
   protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
       //传入SpringBoot应用的主程序
      return application.sources(SpringBoot04WebJspApplication.class);
   }

}
```

#### d、启动服务器就可以使用；

### B、原理

jar包：执行SpringBoot主类的main方法，启动ioc容器，创建嵌入式的Servlet容器；

war包：启动服务器，**服务器启动SpringBoot应用**【SpringBootServletInitializer】，启动ioc容器；



servlet3.0（Spring注解版）：

8.2.4 Shared libraries / runtimes pluggability：

> 规则：

- 服务器启动（web应用启动）会创建当前web应用里面每一个jar包里面ServletContainerInitializer实例：
- ServletContainerInitializer的实现放在jar包的META-INF/services文件夹下，有一个名为javax.servlet.ServletContainerInitializer的文件，内容就是ServletContainerInitializer的实现类的全类名
- 还可以使用@HandlesTypes，在应用启动的时候加载我们感兴趣的类；



> 流程：

- 1、启动Tomcat

- 2、org\springframework\spring-web\4.3.14.RELEASE\spring-web-4.3.14.RELEASE.jar!\META-INF\services\javax.servlet.ServletContainerInitializer：

  Spring的web模块里面有这个文件：**org.springframework.web.SpringServletContainerInitializer**-、

- 3、SpringServletContainerInitializer将@HandlesTypes(WebApplicationInitializer.class)标注的所有这个类型的类都传入到onStartup方法的Set<Class<?>>；为这些WebApplicationInitializer类型的类创建实例；

- 4、每一个WebApplicationInitializer都调用自己的onStartup；

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180302221835.png)

- 5、相当于我们的SpringBootServletInitializer的类会被创建对象，并执行onStartup方法
- 6、SpringBootServletInitializer实例执行onStartup的时候会createRootApplicationContext；创建容器

```java
protected WebApplicationContext createRootApplicationContext(
      ServletContext servletContext) {
    //1、创建SpringApplicationBuilder
   SpringApplicationBuilder builder = createSpringApplicationBuilder();
   StandardServletEnvironment environment = new StandardServletEnvironment();
   environment.initPropertySources(servletContext, null);
   builder.environment(environment);
   builder.main(getClass());
   ApplicationContext parent = getExistingRootWebApplicationContext(servletContext);
   if (parent != null) {
      this.logger.info("Root context already created (using as parent).");
      servletContext.setAttribute(
            WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, null);
      builder.initializers(new ParentContextApplicationContextInitializer(parent));
   }
   builder.initializers(
         new ServletContextApplicationContextInitializer(servletContext));
   builder.contextClass(AnnotationConfigEmbeddedWebApplicationContext.class);
    
    //调用configure方法，子类重写了这个方法，将SpringBoot的主程序类传入了进来
   builder = configure(builder);
    
    //使用builder创建一个Spring应用
   SpringApplication application = builder.build();
   if (application.getSources().isEmpty() && AnnotationUtils
         .findAnnotation(getClass(), Configuration.class) != null) {
      application.getSources().add(getClass());
   }
   Assert.state(!application.getSources().isEmpty(),
         "No SpringApplication sources have been defined. Either override the "
               + "configure method or add an @Configuration annotation");
   // Ensure error pages are registered
   if (this.registerErrorPageFilter) {
      application.getSources().add(ErrorPageFilterConfiguration.class);
   }
    //启动Spring应用
   return run(application);
}
```

7）、Spring的应用就启动并且创建IOC容器

```java
public ConfigurableApplicationContext run(String... args) {
   StopWatch stopWatch = new StopWatch();
   stopWatch.start();
   ConfigurableApplicationContext context = null;
   FailureAnalyzers analyzers = null;
   configureHeadlessProperty();
   SpringApplicationRunListeners listeners = getRunListeners(args);
   listeners.starting();
   try {
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(
            args);
      ConfigurableEnvironment environment = prepareEnvironment(listeners,
            applicationArguments);
      Banner printedBanner = printBanner(environment);
      context = createApplicationContext();
      analyzers = new FailureAnalyzers(context);
      prepareContext(context, environment, listeners, applicationArguments,
            printedBanner);
       
       //刷新IOC容器
      refreshContext(context);
      afterRefresh(context, applicationArguments);
      listeners.finished(context, null);
      stopWatch.stop();
      if (this.logStartupInfo) {
         new StartupInfoLogger(this.mainApplicationClass)
               .logStarted(getApplicationLog(), stopWatch);
      }
      return context;
   }
   catch (Throwable ex) {
      handleRunFailure(context, listeners, analyzers, ex);
      throw new IllegalStateException(ex);
   }
}
```

**==启动Servlet容器，再启动SpringBoot应用==**



# 五、Docker

## 1、简介

**Docker**是一个开源的应用容器引擎；是一个轻量级容器技术；

Docker支持将软件编译成一个镜像；然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者可以直接使用这个镜像；

运行中的这个镜像称为容器，容器启动是非常快速的。

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180303145450.png)



![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180303145531.png)

## 2、核心概念

- 1、docker主机(Host)：安装了Docker程序的机器（Docker直接安装在操作系统之上）；
- 2、docker客户端(Client)：连接docker主机进行操作；
- 3、docker仓库(Registry)：用来保存各种打包好的软件镜像；
- 4、docker镜像(Images)：软件打包好的镜像；放在docker仓库中；
- 5、docker容器(Container)：镜像启动后的实例称为一个容器；容器是独立运行的一个或一组应用

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180303165113.png)

使用Docker的步骤：

1）、安装Docker

2）、去Docker仓库找到这个软件对应的镜像；

3）、使用Docker运行这个镜像，这个镜像就会生成一个Docker容器；

4）、对容器的启动停止就是对软件的启动停止；

## 3、安装Docker

#### A、安装linux虚拟机



##### a、VMWare、VirtualBox（安装）；

##### b、导入虚拟机文件centos7-aqqje.ova；

##### c、双击启动linux虚拟机;使用  root/ 123456登陆

##### d、使用客户端连接linux服务器进行命令操作；

##### e、设置虚拟机网络；

```
	桥接网络===选好网卡====接入网线；

```

##### f、设置好网络以后使用命令重启虚拟机的网络

```shell
service network restart
```

##### g、查看linux的ip地址

```shell
ip addr
```

##### h、使用客户端连接linux；

#### B、在linux虚拟机上安装docker

步骤：

```react
1、检查内核版本，必须是3.10及以上
uname -r
2、安装docker
yum install docker
3、输入y确认安装
4、启动docker
[root@localhost ~]# systemctl start docker
[root@localhost ~]# docker -v
Docker version 1.12.6, build 3e8e77d/1.12.6
5、开机启动docker
[root@localhost ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
6、停止docker
systemctl stop docker
```

## 4、Docker常用命令&操作

### A、镜像操作

| 操作 | 命令                                            | 说明                                                     |
| ---- | ----------------------------------------------- | -------------------------------------------------------- |
| 检索 | docker  search 关键字  eg：docker  search redis | 我们经常去docker  hub上检索镜像的详细信息，如镜像的TAG。 |
| 拉取 | docker pull 镜像名:tag                          | :tag是可选的，tag表示标签，多为软件的版本，默认是latest  |
| 列表 | docker images                                   | 查看所有本地镜像                                         |
| 删除 | docker rmi image-id                             | 删除指定的本地镜像                                       |

https://hub.docker.com/

### B、容器操作

软件镜像（QQ安装程序）----运行镜像----产生一个容器（正在运行的软件，运行的QQ）；

步骤：

```react
1、搜索镜像
[root@localhost ~]# docker search tomcat
2、拉取镜像
[root@localhost ~]# docker pull tomcat
3、根据镜像启动容器
docker run --name mytomcat -d tomcat:latest
4、docker ps  
查看运行中的容器
5、 停止运行中的容器
docker stop  容器的id
6、查看所有的容器
docker ps -a
7、启动容器
docker start 容器id
8、删除一个容器
 docker rm 容器id
9、启动一个做了端口映射的tomcat
[root@localhost ~]# docker run -d -p 8888:8080 tomcat
-d：后台运行
-p: 将主机的端口映射到容器的一个端口    主机端口:容器内部的端口

10、为了演示简单关闭了linux的防火墙
service firewalld status ；查看防火墙状态
service firewalld stop：关闭防火墙
11、查看容器的日志
docker logs container-name/container-id

更多命令参看
https://docs.docker.com/engine/reference/commandline/docker/
可以参考每一个镜像的文档

```



### C、安装MySQL示例

```react
docker pull mysql
```



错误的启动

```react
[root@localhost ~]# docker run --name mysql01 -d mysql
42f09819908bb72dd99ae19e792e0a5d03c48638421fa64cce5f8ba0f40f5846

mysql退出了
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
42f09819908b        mysql               "docker-entrypoint.sh"   34 seconds ago      Exited (1) 33 seconds ago                            mysql01
538bde63e500        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       compassionate_
goldstine
c4f1ac60b3fc        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       lonely_fermi
81ec743a5271        tomcat              "catalina.sh run"        About an hour ago   Exited (143) About an hour ago                       sick_ramanujan


//错误日志
[root@localhost ~]# docker logs 42f09819908b
error: database is uninitialized and password option is not specified 
  You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD；这个三个参数必须指定一个
```

正确的启动

```react
[root@localhost ~]# docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
b874c56bec49fb43024b3805ab51e9097da779f2f572c22c695305dedd684c5f
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
b874c56bec49        mysql               "docker-entrypoint.sh"   4 seconds ago       Up 3 seconds        3306/tcp            mysql01
```

做了端口映射

```react
[root@localhost ~]# docker run -p 3306:3306 --name mysql02 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
ad10e4bc5c6a0f61cbad43898de71d366117d120e39db651844c0e73863b9434
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
ad10e4bc5c6a        mysql               "docker-entrypoint.sh"   4 seconds ago       Up 2 seconds        0.0.0.0:3306->3306/tcp   mysql02
```



几个其他的高级操作

```react
docker run --name mysql03 -v /conf/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
把主机的/conf/mysql文件夹挂载到 mysqldocker容器的/etc/mysql/conf.d文件夹里面
改mysql的配置文件就只需要把mysql配置文件放在自定义的文件夹下（/conf/mysql）

docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
指定mysql的一些配置参数

```



# 六、SpringBoot与数据访问

## 1、JDBC

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
```



```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://192.168.15.22:3306/jdbc
    driver-class-name: com.mysql.jdbc.Driver
```

效果：

```
默认是用org.apache.tomcat.jdbc.pool.DataSource作为数据源；

数据源的相关配置都在DataSourceProperties里面；

```

自动配置原理：

org.springframework.boot.autoconfigure.jdbc：

#### A、参考DataSourceConfiguration，根据配置创建数据源，默认使用Tomcat连接池；可以使用spring.datasource.type指定自定义的数据源类型；

#### B、SpringBoot默认可以支持；

```
org.apache.tomcat.jdbc.pool.DataSource、HikariDataSource、BasicDataSource、


```

#### C、自定义数据源类型

```java
/**
 * Generic DataSource configuration.
 */
@ConditionalOnMissingBean(DataSource.class)
@ConditionalOnProperty(name = "spring.datasource.type")
static class Generic {

   @Bean
   public DataSource dataSource(DataSourceProperties properties) {
       //使用DataSourceBuilder创建数据源，利用反射创建响应type的数据源，并且绑定相关属性
      return properties.initializeDataSourceBuilder().build();
   }

}
```

#### D、**DataSourceInitializer：ApplicationListener**；

> 作用：

- 1、runSchemaScripts();运行建表语句；
- 2、runDataScripts();运行插入数据的sql语句；

默认只需要将文件命名为：

```properties
schema-*.sql、data-*.sql
默认规则：schema.sql，schema-all.sql；
可以使用   
	schema:
      - classpath:department.sql
      指定位置
```

#### E、操作数据库：自动配置了JdbcTemplate操作数据库

## 2、整合Druid数据源

```java
导入druid数据源
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid(){
       return  new DruidDataSource();
    }

    //配置Druid的监控
    //1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String,String> initParams = new HashMap<>();

        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123456");
        initParams.put("allow","");//默认就是允许所有访问
        initParams.put("deny","192.168.15.21");

        bean.setInitParameters(initParams);
        return bean;
    }


    //2、配置一个web监控的filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        Map<String,String> initParams = new HashMap<>();
        initParams.put("exclusions","*.js,*.css,/druid/*");

        bean.setInitParameters(initParams);

        bean.setUrlPatterns(Arrays.asList("/*"));

        return  bean;
    }
}

```

## 3、整合MyBatis

```xml
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.1</version>
		</dependency>
```

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180305194443.png)

> 步骤：

- 1、配置数据源相关属性（见上一节Druid）
- 2、给数据库建表
- 3、创建JavaBean

### A、注解版

```java
//指定这是一个操作数据库的mapper
@Mapper
public interface DepartmentMapper {

    @Select("select * from department where id=#{id}")
    public Department getDeptById(Integer id);

    @Delete("delete from department where id=#{id}")
    public int deleteDeptById(Integer id);

    @Options(useGeneratedKeys = true,keyProperty = "id")
    @Insert("insert into department(departmentName) values(#{departmentName})")
    public int insertDept(Department department);

    @Update("update department set departmentName=#{departmentName} where id=#{id}")
    public int updateDept(Department department);
}
```

问题：

自定义MyBatis的配置规则；给容器中添加一个ConfigurationCustomizer；

```java
@org.springframework.context.annotation.Configuration
public class MyBatisConfig {

    @Bean
    public ConfigurationCustomizer configurationCustomizer(){
        return new ConfigurationCustomizer(){

            @Override
            public void customize(Configuration configuration) {
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
}
```



```java
使用MapperScan批量扫描所有的Mapper接口；
@MapperScan(value = "com.aqqje.springboot.mapper")
@SpringBootApplication
public class SpringBoot06DataMybatisApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringBoot06DataMybatisApplication.class, args);
	}
}
```

### B、配置文件版

```yaml
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml 指定全局配置文件的位置
  mapper-locations: classpath:mybatis/mapper/*.xml  指定sql映射文件的位置
```

更多使用参照

http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/



## 4、整合SpringData JPA

### A、SpringData简介

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180306105412.png)

### B、整合SpringData JPA

JPA:ORM（Object Relational Mapping）；

1）、编写一个实体类（bean）和数据表进行映射，并且配置好映射关系；

```java
//使用JPA注解配置映射关系
@Entity //告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "tbl_user") //@Table来指定和哪个数据表对应;如果省略默认表名就是user；
public class User {

    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键
    private Integer id;

    @Column(name = "last_name",length = 50) //这是和数据表对应的一个列
    private String lastName;
    @Column //省略默认列名就是属性名
    private String email;
```

2）、编写一个Dao接口来操作实体类对应的数据表（Repository）

```java
//继承JpaRepository来完成对数据库的操作
public interface UserRepository extends JpaRepository<User,Integer> {
}

```

3）、基本的配置JpaProperties

```yaml
spring:  
 jpa:
    hibernate:
#     更新或者创建数据表结构
      ddl-auto: update
#    控制台显示SQL
    show-sql: true
```



# 七、启动配置原理

> 几个重要的事件回调机制

- 1、配置在META-INF/spring.factories
  - **ApplicationContextInitializer**
  - *SpringApplicationRunListener**



- 2、只需要放在ioc容器中
  - **ApplicationRunner**
  - **CommandLineRunner**



启动流程：

## **1、创建SpringApplication对象**

```java
initialize(sources);
private void initialize(Object[] sources) {
    //保存主配置类
    if (sources != null && sources.length > 0) {
        this.sources.addAll(Arrays.asList(sources));
    }
    //判断当前是否一个web应用
    this.webEnvironment = deduceWebEnvironment();
    //从类路径下找到META-INF/spring.factories配置的所有ApplicationContextInitializer；然后保存起来
    setInitializers((Collection) getSpringFactoriesInstances(
        ApplicationContextInitializer.class));
    //从类路径下找到ETA-INF/spring.factories配置的所有ApplicationListener
    setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    //从多个配置类中找到有main方法的主配置类
    this.mainApplicationClass = deduceMainApplicationClass();
}
```

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180306145727.png)

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180306145855.png)

## 2、运行run方法

```java
public ConfigurableApplicationContext run(String... args) {
   StopWatch stopWatch = new StopWatch();
   stopWatch.start();
   ConfigurableApplicationContext context = null;
   FailureAnalyzers analyzers = null;
   configureHeadlessProperty();
    
   //获取SpringApplicationRunListeners；从类路径下META-INF/spring.factories
   SpringApplicationRunListeners listeners = getRunListeners(args);
    //回调所有的获取SpringApplicationRunListener.starting()方法
   listeners.starting();
   try {
       //封装命令行参数
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(
            args);
      //准备环境
      ConfigurableEnvironment environment = prepareEnvironment(listeners,
            applicationArguments);
       		//创建环境完成后回调SpringApplicationRunListener.environmentPrepared()；表示环境准备完成
       
      Banner printedBanner = printBanner(environment);
       
       //创建ApplicationContext；决定创建web的ioc还是普通的ioc
      context = createApplicationContext();
       
      analyzers = new FailureAnalyzers(context);
       //准备上下文环境;将environment保存到ioc中；而且applyInitializers()；
       //applyInitializers()：回调之前保存的所有的ApplicationContextInitializer的initialize方法
       //回调所有的SpringApplicationRunListener的contextPrepared()；
       //
      prepareContext(context, environment, listeners, applicationArguments,
            printedBanner);
       //prepareContext运行完成以后回调所有的SpringApplicationRunListener的contextLoaded（）；
       
       //s刷新容器；ioc容器初始化（如果是web应用还会创建嵌入式的Tomcat）；Spring注解版
       //扫描，创建，加载所有组件的地方；（配置类，组件，自动配置）
      refreshContext(context);
       //从ioc容器中获取所有的ApplicationRunner和CommandLineRunner进行回调
       //ApplicationRunner先回调，CommandLineRunner再回调
      afterRefresh(context, applicationArguments);
       //所有的SpringApplicationRunListener回调finished方法
      listeners.finished(context, null);
      stopWatch.stop();
      if (this.logStartupInfo) {
         new StartupInfoLogger(this.mainApplicationClass)
               .logStarted(getApplicationLog(), stopWatch);
      }
       //整个SpringBoot应用启动完成以后返回启动的ioc容器；
      return context;
   }
   catch (Throwable ex) {
      handleRunFailure(context, listeners, analyzers, ex);
      throw new IllegalStateException(ex);
   }
}
```

## 3、事件监听机制

配置在META-INF/spring.factories

**ApplicationContextInitializer**

```java
public class HelloApplicationContextInitializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {
    @Override
    public void initialize(ConfigurableApplicationContext applicationContext) {
        System.out.println("ApplicationContextInitializer...initialize..."+applicationContext);
    }
}

```

**SpringApplicationRunListener**

```java
public class HelloSpringApplicationRunListener implements SpringApplicationRunListener {

    //必须有的构造器
    public HelloSpringApplicationRunListener(SpringApplication application, String[] args){

    }

    @Override
    public void starting() {
        System.out.println("SpringApplicationRunListener...starting...");
    }

    @Override
    public void environmentPrepared(ConfigurableEnvironment environment) {
        Object o = environment.getSystemProperties().get("os.name");
        System.out.println("SpringApplicationRunListener...environmentPrepared.."+o);
    }

    @Override
    public void contextPrepared(ConfigurableApplicationContext context) {
        System.out.println("SpringApplicationRunListener...contextPrepared...");
    }

    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("SpringApplicationRunListener...contextLoaded...");
    }

    @Override
    public void finished(ConfigurableApplicationContext context, Throwable exception) {
        System.out.println("SpringApplicationRunListener...finished...");
    }
}

```

配置（META-INF/spring.factories）

```properties
org.springframework.context.ApplicationContextInitializer=\
com.aqqje.springboot.listener.HelloApplicationContextInitializer

org.springframework.boot.SpringApplicationRunListener=\
com.aqqje.springboot.listener.HelloSpringApplicationRunListener
```





只需要放在ioc容器中

**ApplicationRunner**

```java
@Component
public class HelloApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner...run....");
    }
}
```



**CommandLineRunner**

```java
@Component
public class HelloCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner...run..."+ Arrays.asList(args));
    }
}
```



# 八、自定义starter

starter：

```
1、这个场景需要使用到的依赖是什么？

2、如何编写自动配置

```

```java
@Configuration  //指定这个类是一个配置类
@ConditionalOnXXX  //在指定条件成立的情况下自动配置类生效
@AutoConfigureAfter  //指定自动配置类的顺序
@Bean  //给容器中添加组件

@ConfigurationPropertie结合相关xxxProperties类来绑定相关的配置
@EnableConfigurationProperties //让xxxProperties生效加入到容器中

自动配置类要能加载
将需要启动就加载的自动配置类，配置在META-INF/spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
```

```
3、模式：

```

启动器只用来做依赖导入；

专门来写一个自动配置模块；

启动器依赖自动配置；别人只需要引入启动器（starter）

mybatis-spring-boot-starter；自定义启动器名-spring-boot-starter



步骤：

1）、启动器模块

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.aqqje.starter</groupId>
    <artifactId>aqqje-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--启动器-->
    <dependencies>

        <!--引入自动配置模块-->
        <dependency>
            <groupId>com.aqqje.starter</groupId>
            <artifactId>aqqje-spring-boot-starter-autoconfigurer</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>

</project>
```

2）、自动配置模块

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>

   <groupId>com.aqqje.starter</groupId>
   <artifactId>aqqje-spring-boot-starter-autoconfigurer</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <packaging>jar</packaging>

   <name>aqqje-spring-boot-starter-autoconfigurer</name>
   <description>Demo project for Spring Boot</description>

   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>1.5.10.RELEASE</version>
      <relativePath/> <!-- lookup parent from repository -->
   </parent>

   <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
      <java.version>1.8</java.version>
   </properties>

   <dependencies>

      <!--引入spring-boot-starter；所有starter的基本配置-->
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter</artifactId>
      </dependency>

   </dependencies>



</project>

```



```java
package com.aqqje.starter;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "aqqje.hello")
public class HelloProperties {

    private String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}

```

```java
package com.aqqje.starter;

public class HelloService {

    HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHellaqqje(String name){
        return helloProperties.getPrefix()+"-" +name + helloProperties.getSuffix();
    }
}

```

```java
package com.aqqje.starter;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConditionalOnWebApplication //web应用才生效
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;
    @Bean
    public HelloService helloService(){
        HelloService service = new HelloService();
        service.setHelloProperties(helloProperties);
        return service;
    }
}

```

# 九、Spring Boot与缓存 

## 1、简介



> - 1、Spring从3.1开始定义了org.springframework.cache.Cache和org.springframework.cache.CacheManager接口来统一不同的缓存技术；并支持使用JCache（JSR-107）注解简化我们开发； 
> - 2、Cache接口为缓存的组件规范定义，包含缓存的各种操作集合；
> - 3、Cache接口下Spring提供了各种xxxCache的实现；如RedisCache，EhCacheCache , ConcurrentMapCache等；

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180809230305.png)



## 2、几个重要概念&缓存注解 

| Cache          | 缓存接口，定义缓存操作。实现有：RedisCache、EhCacheCache、ConcurrentMapCache等 |
| -------------- | ------------------------------------------------------------ |
| CacheManager   | 缓存管理器，管理各种缓存（Cache）组件                        |
| @Cacheable     | 主要针对方法配置，能够根据方法的请求参数对其结果进行缓存     |
| @CacheEvict    | 清空缓存                                                     |
| @CachePut      | 保证方法被调用，又希望结果被缓存。                           |
| @EnableCaching | 开启基于注解的缓存                                           |
| keyGenerator   | 缓存数据时key生成策略                                        |
| serialize      | 缓存数据时value序列化策略                                    |



![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180810001615.png)

## 3、Cache SpEL available metadata 

| 名字          | 位置               | 描述                                                         | 示例                 |
| ------------- | ------------------ | ------------------------------------------------------------ | -------------------- |
| methodName    | root object        | 当前被调用的方法名                                           | #root.methodName     |
| method        | root object        | 当前被调用的方法                                             | #root.method.name    |
| target        | root object        | 当前被调用的目标对象                                         | #root.target         |
| targetClass   | root object        | 当前被调用的目标对象类                                       | #root.targetClass    |
| args          | root object        | 当前被调用的方法的参数列表                                   | #root.args[0]        |
| caches        | root object        | 当前方法调用使用的缓存列表（如@Cacheable(value={"cache1",   "cache2"})），则有两个cache | #root.caches[0].name |
| argument name | evaluation context | 方法参数的名字. 可以直接 #参数名 ，也可以使用 #p0或#a0 的形式，0代表参数的索引； | #iban 、 #a0 、  #p0 |
| result        | evaluation context | 方法执行后的返回值（仅当方法执行之后的判断有效，如‘unless’，’cache put’的表达式 ’cache evict’的表达式beforeInvocation=false） | #result              |



## 4.初体验

> - 1、引入spring-boot-starter-cache模块
> - 2、@EnableCaching开启缓存
> - 3、使用缓存注解
> - 4、切换为其他缓存





```java
 ===============SpringBootCacheApplication.java============

@MapperScan("com.aqqje.cache.mapper")
@EnableCaching // 开启缓存
@SpringBootApplication
public class SpringBootCacheApplication {
    
    
================EmployeeService.java===================

package com.aqqje.cache.service;

import com.aqqje.cache.bean.Employee;
import com.aqqje.cache.mapper.EmployeeMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.*;
import org.springframework.stereotype.Service;

@Service
@CacheConfig(cacheNames="emp"/*,cacheManager = "employeeCacheManager"*/) //抽取缓存的公共配置
public class EmployeeService {

    @Autowired
    EmployeeMapper employeeMapper;

    // 查询

    /**
     * 将方法的运行结果进行缓存；以后再要相同的数据，直接从缓存中获取，不用调用方法；
     * CacheManager管理多个Cache组件的，对缓存的真正CRUD操作在Cache组件中，每一个缓存组件有自己唯一一个名字；
     *

     *
     * 原理：
     *   1、自动配置类；CacheAutoConfiguration
     *   2、缓存的配置类
     *   org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.JCacheCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.EhCacheCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.HazelcastCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.InfinispanCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.CouchbaseCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.CaffeineCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.GuavaCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.SimpleCacheConfiguration【默认】
     *   org.springframework.boot.autoconfigure.cache.NoOpCacheConfiguration
     *   3、哪个配置类默认生效：SimpleCacheConfiguration；
     *
     *   4、给容器中注册了一个CacheManager：ConcurrentMapCacheManager
     *   5、可以获取和创建ConcurrentMapCache类型的缓存组件；他的作用将数据保存在ConcurrentMap中；
     *
     *   运行流程：
     *   @Cacheable：
     *   1、方法运行之前，先去查询Cache（缓存组件），按照cacheNames指定的名字获取；
     *      （CacheManager先获取相应的缓存），第一次获取缓存如果没有Cache组件会自动创建。
     *   2、去Cache中查找缓存的内容，使用一个key，默认就是方法的参数；
     *      key是按照某种策略生成的；默认是使用keyGenerator生成的，默认使用SimpleKeyGenerator生成key；
     *          SimpleKeyGenerator生成key的默认策略；
     *                  如果没有参数；key=new SimpleKey()；
     *                  如果有一个参数：key=参数的值
     *                  如果有多个参数：key=new SimpleKey(params)；
     *   3、没有查到缓存就调用目标方法；
     *   4、将目标方法返回的结果，放进缓存中
     *
     *   @Cacheable标注的方法执行之前先来检查缓存中有没有这个数据，默认按照参数的值作为key去查询缓存，
     *   如果没有就运行方法并将结果放入缓存；以后再来调用就可以直接使用缓存中的数据；
     *
     *   核心：
     *      1）、使用CacheManager【ConcurrentMapCacheManager】按照名字得到Cache【ConcurrentMapCache】组件
     *      2）、key使用keyGenerator生成的，默认是SimpleKeyGenerator
     *
     *
     *   几个属性：
     *      cacheNames/value：指定缓存组件的名字;将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存；
     *
     *      key：缓存数据使用的key；可以用它来指定。默认是使用方法参数的值  1-方法的返回值
     *              编写SpEL； #i d;参数id的值   #a0  #p0  #root.args[0]
     *              getEmp[2]
     *
     *      keyGenerator：key的生成器；可以自己指定key的生成器的组件id
     *              key/keyGenerator：二选一使用;
     *
     *
     *      cacheManager：指定缓存管理器；或者cacheResolver指定获取解析器
     *
     *      condition：指定符合条件的情况下才缓存；
     *              ,condition = "#id>0"
     *          condition = "#a0>1"：第一个参数的值》1的时候才进行缓存
     *
     *      unless:否定缓存；当unless指定的条件为true，方法的返回值就不会被缓存；可以获取到结果进行判断
     *              unless = "#result == null"
     *              unless = "#a0==2":如果第一个参数的值是2，结果不缓存；
     *      sync：是否使用异步模式
     * @param id
     * @return
     *
     */
    @Cacheable(/*cacheNames = {"emp"}*/, key = "#a0"/*,keyGenerator = "myKeyGenerator",condition = "#a0>1",unless = "#a0==2"*/) // 根据方法的请求参数对其结果进行缓存
    public Employee findById(Integer id){
        System.out.println("查询" + id + "号员工");
        return employeeMapper.findById(id);
    }

    //更新
    /**
     * @CachePut：既调用方法，又更新缓存数据；同步更新缓存
     * 修改了数据库的某个数据，同时更新缓存；
     * 运行时机：
     *  1、先调用目标方法
     *  2、将目标方法的结果缓存起来
     *
     * 测试步骤：
     *  1、查询1号员工；查到的结果会放在缓存中；
     *          key：1  value：lastName：张三
     *  2、以后查询还是之前的结果
     *  3、更新1号员工；【lastName:zhangsan；gender:0】
     *          将方法的返回值也放进缓存了；
     *          key：传入的employee对象  值：返回的employee对象；
     *  4、查询1号员工？
     *      应该是更新后的员工；
     *          key = "#employee.id":使用传入的参数的员工id；
     *          key = "#result.id"：使用返回后的id
     *             @Cacheable的key是不能用#result
     *      为什么是没更新前的？【1号员工没有在缓存中更新】
     *
     */
    @CachePut(/*value="emp"*/ key = "#a0.id")
    public boolean updateById(Employee employee){

        return 0 != employeeMapper.updateById(employee);
    }

    //删除
    /**
     * @CacheEvict：缓存清除
     *  key：指定要清除的数据
     *  allEntries = true：指定清除这个缓存中所有的数据
     *  beforeInvocation = false：缓存的清除是否在方法之前执行
     *      默认代表缓存清除操作是在方法执行之后执行;如果出现异常缓存就不会清除
     *
     *  beforeInvocation = true：
     *      代表清除缓存操作是在方法运行之前执行，无论方法是否出现异常，缓存都清除
     *
     *
     */
    @CacheEvict(/*value="emp"*/ key="#id" /* allEntries=true*/)
    public boolean deleteById(Integer id){
        return 0 != employeeMapper.deleteById(id);
    }

    //添加
    @CachePut(/*value = "emp"*/, key = "#a0.id")
    public boolean insertEmp(Employee employee){
        return 0 != employeeMapper.insertEmp(employee);
    }

    // @Caching 定义复杂的缓存规则
    @Caching(
            cacheable = {
                    @Cacheable(/*value="emp",*/key = "#lastName")
            },
            put = {
                    @CachePut(/*value="emp",*/key = "#result.id"),
                    @CachePut(/*value="emp",*/key = "#result.email")
            }
    )
    public Employee getEmpByLastName(String lastName){
        return employeeMapper.getEmpByLastName(lastName);
    }

}

```



- KeyGenerator: key 生成器

```java
package com.aqqje.cache.config;

import org.springframework.cache.interceptor.KeyGenerator;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.lang.reflect.Method;
import java.util.Arrays;

@Configuration
public class CacheCofing {

    // 自定义key生成器
    @Bean("mykeyGenerator")
    public KeyGenerator keyGenerator(){
        return new KeyGenerator(){
            @Override
            public Object generate(Object o, Method method, Object... objects) {
                return method.getName()+ "[" + Arrays.asList(objects).toString() + "]";
            }
        };
    }
}

// 如何使用
@Cacheable(cacheNames = {"emp"},keyGenerator = "myKeyGenerator")
```



## 5、整合Redis



### A、引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### B、配置 IP 和端口



```properties
#配置redis的 Ip 和 端口
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

### C、使用Redis



```java
====================================MyRedisConfig=================
    package com.aqqje.cache.config;

import com.aqqje.cache.bean.Employee;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;

import java.net.UnknownHostException;

@Configuration
public class MyRedisConfig {

    /*
    * json序列化器
    * */
    @Bean
    public RedisTemplate<Object, Employee> jsonRedisTemplate(
            RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        RedisTemplate<Object, Employee> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        template.setDefaultSerializer(jackson2JsonRedisSerializer);
        return template;
    }
}

=========================SpringBootCacheApplicationTests=================
package com.aqqje.cache;

import com.aqqje.cache.bean.Employee;
import com.aqqje.cache.mapper.EmployeeMapper;
import com.aqqje.cache.service.EmployeeService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.test.context.junit4.SpringRunner;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringBootCacheApplicationTests {

    @Autowired
    DataSource dataSource;

    @Autowired
    EmployeeService employeeService;

    @Autowired
    RedisTemplate redisTemplate;//k-v都是对象的

    @Autowired
    StringRedisTemplate stringRedisTemplate;  //操作k-v都是字符串的

    @Autowired
    RedisTemplate<Object, Employee> jsonRedisTemplate;

    /**
     * Redis常见的五大数据类型
     *  String（字符串）、List（列表）、Set（集合）、Hash（散列）、ZSet（有序集合）
     *  stringRedisTemplate.opsForValue()[String（字符串）]
     *  stringRedisTemplate.opsForList()[List（列表）]
     *  stringRedisTemplate.opsForSet()[Set（集合）]
     *  stringRedisTemplate.opsForHash()[Hash（散列）]
     *  stringRedisTemplate.opsForZSet()[ZSet（有序集合）]
     */
    @Test
    public void testStringRedisTemplate(){
        // 给redis保存数据
        //stringRedisTemplate.opsForValue().append("msg", "aqqje");
        //String msg = stringRedisTemplate.opsForValue().get("msg");
        //System.out.println(msg);
        //stringRedisTemplate.opsForList().leftPush("mylist", "1");
        stringRedisTemplate.opsForList().leftPush("mylist", "2");
    }

    // 测试保存对象
    @Test
    public void testRedisTemplate(){
        Employee id = employeeService.findById(4);
        //redisTemplate.opsForValue().set("emp-01", id);

        //默认如果保存对象，使用jdk序列化机制，序列化后的数据保存到redis中
        //redisTemplate.opsForValue().set("emp-01",empById);
        //1、将数据以json的方式保存
        //(1)自己将对象转为json
        //(2)redisTemplate默认的序列化规则；改变默认的序列化规则；
        jsonRedisTemplate.opsForValue().set("emp-01", id);
    }
```



### D、自定义RedesCacheManager

```java
====================================MyRedisConfig=================  
@Bean
    public RedisCacheManager empCacheManger(RedisConnectionFactory redisConnectionFactory){
        //初始化一个RedisCacheWriter
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisConnectionFactory);
        //设置CacheManager的值序列化方式为Jackson2JsonRedisSerializer
        Jackson2JsonRedisSerializer<Employee> employeeJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        RedisSerializationContext.SerializationPair<Employee> pair = RedisSerializationContext.SerializationPair.fromSerializer(employeeJackson2JsonRedisSerializer);
        RedisCacheConfiguration defaultCacheConfig=RedisCacheConfiguration.defaultCacheConfig().serializeValuesWith(pair);
        //设置默认超过期时间是30秒
        defaultCacheConfig.entryTtl(Duration.ofSeconds(30));
        //初始化RedisCacheManager
        RedisCacheManager cacheManager = new RedisCacheManager(redisCacheWriter, defaultCacheConfig);
        return cacheManager;
    }
```



### E、多个CacheManager使用

```java
==========================myRedisConfig.java============================
        /*
    * json序列化器
    * */
    @Bean
    public RedisTemplate<Object, Employee> empJsonRedisTemplate(
            RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        RedisTemplate<Object, Employee> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        template.setDefaultSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    /**
     * employee json 序列化器
     *
     * @Primary：有多个序列化器必须指定其中一个这首要序列化器
     * @param redisConnectionFactory
     * @return
     */
    @Primary
    @Bean
    public RedisCacheManager empCacheManager(RedisConnectionFactory redisConnectionFactory){
        //初始化一个RedisCacheWriter
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisConnectionFactory);
        //设置CacheManager的值序列化方式为Jackson2JsonRedisSerializer
        Jackson2JsonRedisSerializer<Employee> Jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        RedisSerializationContext.SerializationPair<Employee> pair = RedisSerializationContext.SerializationPair.fromSerializer(Jackson2JsonRedisSerializer);
        RedisCacheConfiguration defaultCacheConfig=RedisCacheConfiguration.defaultCacheConfig().serializeValuesWith(pair);
        //设置默认超过期时间是30秒
        defaultCacheConfig.entryTtl(Duration.ofSeconds(30));
        //初始化RedisCacheManager
        RedisCacheManager cacheManager = new RedisCacheManager(redisCacheWriter, defaultCacheConfig);
        return cacheManager;
    }

    @Bean
    public RedisTemplate<Object, Department> deptJsonRedisTemplate(
            RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        RedisTemplate<Object, Department> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Department>(Department.class);
        template.setDefaultSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    /**
     * department json 序列化器
     *
     * @param redisConnectionFactory
     * @return
     */
    @Bean
    public RedisCacheManager deptCacheManager(RedisConnectionFactory redisConnectionFactory){
        //初始化一个RedisCacheWriter
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisConnectionFactory);
        //设置CacheManager的值序列化方式为Jackson2JsonRedisSerializer
        Jackson2JsonRedisSerializer<Department> Jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Department>(Department.class);
        RedisSerializationContext.SerializationPair<Department> pair = RedisSerializationContext.SerializationPair.fromSerializer(Jackson2JsonRedisSerializer);
        RedisCacheConfiguration defaultCacheConfig=RedisCacheConfiguration.defaultCacheConfig().serializeValuesWith(pair);
        //设置默认超过期时间是30秒
        defaultCacheConfig.entryTtl(Duration.ofSeconds(30));
        //初始化RedisCacheManager
        RedisCacheManager cacheManager = new RedisCacheManager(redisCacheWriter, defaultCacheConfig);
        return cacheManager;
    }

==================DepartmentService.java&&EmployeeService.java====================
    // 分别指定对应的 xxxCacheManager
    
    @CacheConfig(cacheNames = {"dept"}, cacheManager = "deptCacheManager" )
    

    @CacheConfig(cacheNames="emp",cacheManager = "empCacheManager") //抽取缓存的公共配置
    
==================SpringBootCacheApplicationTests.java===================
    
    // 使用 xxxCacheManager 对缓存进行操作
    
    @Qualifier("deptCacheManager")
    @Autowired
    RedisCacheManager deptCacheManager;

    @Test
    public void test1(){
        Cache cache = deptCacheManager.getCache("dept");
        Object nativeCache = cache.getNativeCache();
        System.out.println(cache.getName());
    }
```



# 十、Spring Boot与消息



## 1、消息服务简介

### A、提升系统异步通信、扩展解耦能力

### B、消息服务中两个重要概念：       

### C、消息代理（message broker）

### D、目的地（destination）

### E、消息队列主要有两种形式的目的地：

- 1.队列（queue）：点对点消息通信（point-to-point）
- 2.主题（topic）：发布（publish）/订阅（subscribe）消息通信

### F、使用场景

- 异步处理

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//20180904.png)

- 应用解耦

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//2018090401.png)

- 流量削峰

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//2018090402.png)

### G、点对点式:

- 消息发送者发送消息，消息代理将其放入一个队列中，消息接收者从队列中获取消息内容，消息读取后被移出队列
- 消息只有唯一的发送者和接受者，但并不是说只能有一个接收者

### H、发布订阅式:

- –发送者（发布者）发送消息到主题，多个接收者（订阅者）监听（订阅）这个主题，那么就会在消息到达时同时收到消息

### I、JMS（Java Message Service）JAVA消息服务：

- –基于JVM消息代理的规范。ActiveMQ、HornetMQ是JMS实现

### J、AMQP（Advanced Message Queuing Protocol）

- 高级消息队列协议，也是一个消息代理的规范，兼容JMS
- RabbitMQ是AMQP的实现
- JMS与AMQP对比

|              | JMS                                                          | AMQP                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定义         | Java   api                                                   | 网络线级协议                                                 |
| 跨语言       | 否                                                           | 是                                                           |
| 跨平台       | 否                                                           | 是                                                           |
| Model        | 提供两种消息模型：   （1）、Peer-2-Peer   （2）、Pub/sub     | 提供了五种消息模型：   （1）、direct   exchange   （2）、fanout   exchange   （3）、topic   change   （4）、headers   exchange   （5）、system   exchange   本质来讲，后四种和JMS的pub/sub模型没有太大差别，仅是在路由机制上做了更详细的划分； |
| 支持消息类型 | 多种消息类型：   TextMessage   MapMessage   BytesMessage   StreamMessage   ObjectMessage   Message   （只有消息头和属性） | byte[]   当实际应用时，有复杂的消息，可以将消息序列化后发送。 |
| 综合评价     | JMS   定义了JAVA   API层面的标准；在java体系中，多个client均可以通过JMS进行交互，不需要应用修改代码，但是其对跨平台的支持较差； | AMQP定义了wire-level层的协议标准；天然具有跨平台、跨语言特性。 |

### L、Spring支持

- spring-jms提供了对JMS的支持
- spring-rabbit提供了对AMQP的支持
- 需要ConnectionFactory的实现来连接消息代理
- 提供JmsTemplate、RabbitTemplate来发送消息
- @JmsListener（JMS）、@RabbitListener（AMQP）注解在方法上监听消息代理发布的消息
- @EnableJms、@EnableRabbit开启支持



### M、Spring Boot自动配置

- JmsAutoConfiguration
- RabbitAutoConfiguration



## 2、RabbitMQ简介

### A、RabbitMQ

```
是一个由erlang开发的AMQP(Advanved Message Queue Protocol)的开源实现。

```

### B、核心概念

- Message

  消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。

- Publisher

  消息的生产者，也是一个向交换器发布消息的客户端应用程序。

- Exchange

  交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

  Exchange有4种类型：direct(默认)，fanout, topic, 和headers，不同类型的Exchange转发消息的策略有所区别

- Queue

  消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。

- Binding

  绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。

  Exchange 和Queue的绑定可以是多对多的关系。

- Connection

  网络连接，比如一个TCP连接。

- Channel

  信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接。

- Consumer

  消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。

- Virtual Host

  虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个 vhost 本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。vhost 是 AMQP 概念的基础，必须在连接时指定，RabbitMQ 默认的 vhost 是  。

- Broker

  表示消息队列服务器实体

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//201809043.png)



### C、RabbitMQ运行机制

#### a、MQP 中消息路由

- AMQP 中消息的路由过程和 Java 开发者熟悉的 JMS 存在一些差别，AMQP 中增加了 Exchange 和 Binding 的角	色。生产者把消息发布到 Exchange 上，消息最终到达队列并被消费者接收，而 Binding 决定交换器的消息应该发送到那个队列。

#### b、Exchange类型

- Exchange分发消息时根据类型的不同分发策略有区别，目前共四种类型：direct、fanout、topic、headers 。headers 匹配 AMQP 消息的 header 而不是路由键， headers 交换器和 direct 交换器完全一致，但性能差很多，目前几乎用不到了，所以直接看另外三种类型：

- direct Exchange : 一对一

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//201809044.png)

- fanout Exchange：对所有

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//201809045.png)

- topic Exchange：对匹配

  ![](https://github.com/aqqje/SpringBootLearn/raw/master/images//201809046.png)



### D、RabbitMq安装测试(docker 容器)

- 下载运行

```tex
docker pull registry.docker-cn.com/library/rabbitmq:3-management  // 下载rabbitmq镜像

docker run --name rabbit -d -p 5672:5672 -p 15672:15672 rabbitmq:3-management // 运行rabbitmq 其中 【5672为消息通信端口】 【15672为rabbit  web 界面端口】
```

- 消息测试图

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180905220019.png)



- 创建交换器

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180905220752.png)



- 创建消息队列

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180905221013.png)

- 为交换器绑定消息队列

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180905221534.png)



- 测试

![](https://github.com/aqqje/SpringBootLearn/raw/master/images//%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180905221643.png)



## 3、SpringBoot整合rabbitMQ

### A、引入jar包

```xml
 <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

### B、基于注解开启rabbit模式

```java
/**
 * rabbitMq自动配置类：RabbitAutoConfiguration
 *  1.CachingConnectionFactory：自动配置的连接工厂
 *  2.RabbitTemplate：关于 rabbitMq 消息的发送和接收
 *  3.AmqpAdmin：RabbitMQ系统管理功能组件;
 *      创建和删除 Queue，Exchange，Binding
 */

@EnableRabbit // 基于注解开启rabbit模式
@SpringBootApplication
public class SpringbootRabbitmqApplication
```

### C、配置rabbitMQ并使用

> application.properties:

```xml
# 配置连接端口
spring.rabbitmq.host=192.168.99.100 
# 配置 rabbitMq 用户和密码
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```



> SpringbootRabbitmqApplicationTests：

```java
package com.aqqje.rabbitmq;

import com.aqqje.rabbitmq.comfig.bean.Book;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.HashMap;
import java.util.Map;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootRabbitmqApplicationTests {


    @Autowired
    RabbitTemplate rabbitTemplate;

    /**
     * 单播(点对点)
     */
    @Test
    public void contextLoads() {
        //Message需要自己构造一个;定义消息体内容和消息头
        //rabbitTemplate.send(exchage,routeKey,message);

        //object默认当成消息体，只需要传入要发送的对象，自动序列化发送给rabbitmq；
        //rabbitTemplate.convertAndSend(exchage,routeKey,object);
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("msg", "This first messages");
        map.put("data", "Hello Word");
        map.put("flag", true);
        rabbitTemplate.convertAndSend("exchange.direct","aqqje.news", map);
    }

    /**
     * 接收消息
     */
    @Test
    public void receive(){
        Object book = rabbitTemplate.receiveAndConvert("aqqje");
        System.out.println(book.getClass());
        System.out.println(book);

    }

    /**
     * 广播
     */

    @Test
    public void fanout(){
        rabbitTemplate.convertAndSend("exchange.fanout", "", new Book("aqqje", 100));
    }
}
```

### D、配置json序列化器

```java
package com.aqqje.rabbitmq.comfig;

import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 为rabbitMq配置json序列化器
 */
@Configuration
public class MyAmqpConfig {

    @Bean
    public MessageConverter messageConverter(){
        return new Jackson2JsonMessageConverter();
    }
}

```

### E、rabbitmq监听器及amqpadmin对exchanges, queues, binding 的操作（删除与创建）

```java
@EnableRabbit + @RabbitListener = rabbitmq监听器

=============================SpringbootRabbitmqApplication============

@EnableRabbit // 基于注解开启rabbit模式
@SpringBootApplication
public class SpringbootRabbitmqApplication {


==============================BookService=============================

package com.aqqje.rabbitmq.service;

import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    @RabbitListener(queues = "aqqje.emps")
    public void reciver(Message message){
        System.out.println(message.getBody());
        System.out.println(message.toString());
    }
}

```



```java
@Test
    public void testamqpAdmin(){
        // 创建一个交换器
        //amqpAdmin.declareExchange(new DirectExchange("aqqje.direct", true, false));
        //amqpAdmin.declareExchange(new FanoutExchange("aqqje.fanout", true, false));
        //amqpAdmin.declareExchange(new TopicExchange("aqqje.topic", true, false));
        // 创建消息队列
        //amqpAdmin.declareQueue(new Queue("aqqje.emps", true));
        //amqpAdmin.declareQueue(new Queue("aqqje.news", true));
        //amqpAdmin.declareQueue(new Queue("aqqje", true));
        // 为交换器绑定消息队列
        //amqpAdmin.declareBinding(new Binding("aqqje.emps", Binding.DestinationType.QUEUE, "aqqje.direct", "aqqje.emps", null));
        //amqpAdmin.declareBinding(new Binding("aqqje.news", Binding.DestinationType.QUEUE, "aqqje.fanout", "aqqje.news", null));
        //amqpAdmin.declareBinding(new Binding("aqqje", Binding.DestinationType.QUEUE, "aqqje.topic", "aqqje.#", null));
        //amqpAdmin.declareBinding(new Binding("aqqje.emps", Binding.DestinationType.QUEUE, "aqqje.topic", "aqqje.#", null));
        //amqpAdmin.declareBinding(new Binding("aqqje.news", Binding.DestinationType.QUEUE, "aqqje.topic", "aqqje.#", null));
        //amqpAdmin.declareBinding(new Binding("aqqje.news", Binding.DestinationType.QUEUE, "aqqje.topic", "*.aqqje", null));

        //System.out.println("创建完成！");
        
        // 删除指定的交换器和消息队列
        //amqpAdmin.deleteExchange("aqqje.fanout");
        //amqpAdmin.deleteQueue("aqqje.news");
    }
```



# 十一、SpringBoot整合ElasticSearch

## 1、docker安装ElasticSearch

```tex
# 搜索elasticsearch镜像
docker search elasticsearch

# 下载elasticsearch镜像(加速)
docker pull registry.docker-cn.com/library/elasticsearch

# 运行elasticsearch(9200：web端口，9300：各节点通信端口)
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name ES01 registry.docker-cn.com/library/elasticsearch

```

## 2、ElasticSearch入门

- [官方文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/getting-started.html)
- [阮一峰ES教程](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)

## 3、springboot整合ES(jest)

> ```tex
> /**
>  * SpringBoot默认支持两种技术来和ES交互；
>  * 1、Jest（默认不生效）
>  *     需要导入jest的工具包（io.searchbox.client.JestClient）
>  * 2、SpringData ElasticSearch【ES版本有可能不合适】
>  *        版本适配说明：https://github.com/spring-projects/spring-data-elasticsearch
>  *    如果版本不适配：2.4.6
>  *       1）、升级SpringBoot版本
>  *       2）、安装对应版本的ES
>  *
>  *        1）、Client 节点信息clusterNodes；clusterName
>  *        2）、ElasticsearchTemplate 操作es
>  *    3）、编写一个 ElasticsearchRepository 的子接口来操作ES；
>  * 两种用法：https://github.com/spring-projects/spring-data-elasticsearch
>  * 1）、编写一个 ElasticsearchRepository
>  */
> ```

### A、引入  jar 包(与ES版本对应)

```xml
<!-- https://mvnrepository.com/artifact/io.searchbox/jest -->
        <dependency>
            <groupId>io.searchbox</groupId>
            <artifactId>jest</artifactId>
            <version>6.3.1</version>
        </dependency>
```

### B、添加一定的elastic配置



```xml
=====================application.properties=======================

spring.elasticsearch.jest.uris=http://192.168.99.100:9200
```

#### C、测试

```java
package com.aqqje.eclastic;

import com.aqqje.eclastic.bean.Article;
import io.searchbox.client.JestClient;
import io.searchbox.core.Index;
import io.searchbox.core.Search;
import io.searchbox.core.SearchResult;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.io.IOException;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootEclasticApplicationTests {

    @Autowired
    JestClient jestClient;

    @Test
    public void contextLoads() {
        Article article = new Article(2, "aqqje", "china");

        // 创建新的索引
        Index index = new Index.Builder(article).index("aqqje").type("news").id("1").build();

        try {
            // 执行
            jestClient.execute(index);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    /**
     * 全文搜索
     */
    @Test
    public void search(){
        //查询表达式
        String json ="{\n" +
                "    \"query\" : {\n" +
                "        \"match\" : {\n" +
                "            \"type\" : \"china\"\n" +
                "        }\n" +
                "    }\n" +
                "}";

        //更多操作：https://github.com/searchbox-io/Jest/tree/master/jest
        //构建搜索功能
        Search search = new Search.Builder(json).addIndex("aqqje").addType("news").build();
        try {
            SearchResult result = jestClient.execute(search);
            System.out.println(result.getJsonString());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}

```



## 4、springboot整合ES(spring-data)

> 版本要一定的适配，

- [官方文档](https://docs.spring.io/spring-data/elasticsearch/docs/3.0.6.RELEASE/reference/html/)

```xml
==========================application.properties===============================
spring.data.elasticsearch.cluster-name=elasticsearch
spring.data.elasticsearch.cluster-nodes=192.168.99.100:9300
```

```java
==========================BookRepository.java===============================
public interface BookRepository extends ElasticsearchRepository<Book, Integer> {
    //参照
    // https://docs.spring.io/spring-data/elasticsearch/docs/3.0.6.RELEASE/reference/html/
   public List<Book> findByBookNameLike(String bookName);
}
==========================Book.java===============================
@Document(indexName = "aqqje", type = "book")
public class Book {
    private Integer id;
    private String author;
    private String title;

==========================aSpringboot03ElasticApplicationTests.java==========================
    @RunWith(SpringRunner.class)
@SpringBootTest
public class Springboot03ElasticApplicationTests {

	@Autowired
	JestClient jestClient;

	@Autowired
	BookRepository bookRepository;

	@Test
	public void test02(){
//		Book book = new Book();
//		book.setId(1);
//		book.setTitle("西游记");
//		book.setAuthor("吴承恩");
//		bookRepository.index(book);


		for (Book book : bookRepository.findByBookNameLike("游")) {
			System.out.println(book);
		}
		;

	}

```



# 十二、SpringBoot与任务

## 1、异步任务



```java
==================SpringbootTaskApplication.java===================
@EnableAsync  //开启异步任务
@SpringBootApplication
public class SpringbootTaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootTaskApplication.class, args);
    }
}

=====================AsyncService.java=================
@Service
public class AsyncService {

    @Async // 异步加载
    public void hello(){
        try {
            Thread.sleep(3);

        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("数据正在加载中....");
    }
}

=====================AsyncController.java===========================

@RestController
public class AsyncController {

    @Autowired
    AsyncService asyncService;


    @GetMapping("/hello")
    public String asyncHello(){
        asyncService.hello();
        return "hello async";
    }
}

```



## 2、定时任务



```java
>
========================SpringbootTaskApplication.java=====================
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootTaskApplication.class, args);
    }
}

====================ScheduledSerivce.java======================

@Service
public class ScheduledSerivce {

    /**
     * second(秒), minute（分）, hour（时）, day of month（日）, month（月）, day of week（周几）.
     * 0 * * * * MON-FRI
     *  【0 0/5 14,18 * * ?】 每天14点整，和18点整，每隔5分钟执行一次
     *  【0 15 10 ? * 1-6】 每个月的周一至周六10:15分执行一次
     *  【0 0 2 ? * 6L】每个月的最后一个周六凌晨2点执行一次
     *  【0 0 2 LW * ?】每个月的最后一个工作日凌晨2点执行一次
     *  【0 0 2-4 ? * 1#1】每个月的第一个周一凌晨2点到4点期间，每个整点都执行一次；
     */
    // @Scheduled(cron = "0 * * * * MON-SAT")
    //@Scheduled(cron = "0,1,2,3,4 * * * * MON-SAT")
    // @Scheduled(cron = "0-4 * * * * MON-SAT")
    @Scheduled(cron = "0-4 * * * * 1-5")
    public void schedule(){
        System.out.println("hello schedule");
    }
}

```

> 关于 cron 属性的表达式

| 字段 | 允许值                  | 允许的特殊字符    |
| ---- | ----------------------- | ----------------- |
| 秒   | 0-59                    | , -   * /         |
| 分   | 0-59                    | , -   * /         |
| 小时 | 0-23                    | , -   * /         |
| 日期 | 1-31                    | , -   * ? / L W C |
| 月份 | 1-12                    | , -   * /         |
| 星期 | 0-7或SUN-SAT   0,7是SUN | , -   * ? / L C # |

| 特殊字符 | 代表含义                   |
| -------- | -------------------------- |
| ,        | 枚举                       |
| -        | 区间                       |
| *        | 任意                       |
| /        | 步长                       |
| ?        | 日/星期冲突匹配            |
| L        | 最后                       |
| W        | 工作日                     |
| C        | 和calendar联系后计算过的值 |
| #        | 星期，4#2，第2个星期四     |

## 3、邮件任务

> 引入 jar 包

```xml
================pom.xml===================
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

> 配置发件邮箱的基本信息

```xml
===================application.properties====================
# 邮箱帐户
spring.mail.username=1042136232@qq.com
# 授权码
spring.mail.password=ysutsphjlltjbcej
# 发件端口
spring.mail.host=smtp.qq.com
# ssl传输
spring.mail.properties.mail.smtp.ssl.enable=true
#加密连接
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.starttls.required=true
```

> 测试邮箱发件

```java
==========================SpringbootTaskApplicationTests===============
    
package com.aqqje.task;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSenderImpl;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.test.context.junit4.SpringRunner;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;
import java.io.File;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootTaskApplicationTests {

    @Autowired
    JavaMailSenderImpl mailSender;

    /**
     * 发送简单的邮件
     */
    @Test
    public void contextLoads() {
        SimpleMailMessage simpleMailMessage = new SimpleMailMessage();
        simpleMailMessage.setTo("aqqjeqnyh@163.com");
        simpleMailMessage.setFrom("1042136232@qq.com");
        // 邮件标题
        simpleMailMessage.setSubject("spring-boot-mail");
        // 邮件内容
        simpleMailMessage.setText("====aqqje learning spring-boot-mail=====");
        mailSender.send(simpleMailMessage);


    }
    /**
     * 发送复杂的邮件
     */
    @Test
    public void test02() throws MessagingException {
        MimeMessage mimeMessage = mailSender.createMimeMessage();
        /**
         * multipart: true
         *  可发送携带附件的邮件
         */
        MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);
        helper.setTo("aqqjeqnyh@163.com");
        helper.setFrom("1042136232@qq.com");
        // 邮件标题
        helper.setSubject("<b style='color:red'>spring-boot-mail<b/>");
        // 邮件内容
        helper.setText("====aqqje learning spring-boot-mail=====");
        // 添加图片
        helper.addAttachment("1.jpg", new File("C:\\wallpaper\\360\\317441.jpg"));
        helper.addAttachment("1.jpg", new File("C:\\wallpaper\\360\\319354.jpg"));
        mailSender.send(mimeMessage);
    }

}

```

# 十三、SpringBoot与SpringSecurity

```java
package com.aqqje.security;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
/**
 * 1、引入SpringSecurity；
 * 2、编写SpringSecurity的配置类；
 * 		@EnableWebSecurity   extends WebSecurityConfigurerAdapter
 * 3、控制请求的访问权限：
 * 		configure(HttpSecurity http) {
 * 		 	http.authorizeRequests().antMatchers("/").permitAll()
 * 		 		.antMatchers("/level1/**").hasRole("VIP1")
 * 		}
 * 4、定义认证规则：
 * 		configure(AuthenticationManagerBuilder auth){
 * 		 	auth.inMemoryAuthentication()
 * 		 		.withUser("zhangsan").password("123456").roles("VIP1","VIP2")
 * 		}
 * 5、开启自动配置的登陆功能：
 * 		configure(HttpSecurity http){
 * 		 	http.formLogin();
 * 		}
 * 6、注销：http.logout();
 * 7、记住我：Remeberme()；
 */
@SpringBootApplication
public class SprintbootSecurityApplication {

    public static void main(String[] args) {
        SpringApplication.run(SprintbootSecurityApplication.class, args);
    }
}

 
```

## 1、引入SpringSecurity

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity4</artifactId>
</dependency>
```



## 2、编写PasswordEncoder实现类

```java
public class MyPasswordEncoder implements PasswordEncoder {
    @Override
    public String encode(CharSequence charSequence) {
        return charSequence.toString();
    }

    @Override
    public boolean matches(CharSequence charSequence, String s) {
        return s.equals(charSequence.toString());
    }
}

```



## 3、编写SpringSecurity的配置类

```java
package com.aqqje.security.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity
public class MySecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    public void configure(WebSecurity web) throws Exception {
        //super.configure(web);
        //设置静态资源不要拦截
        //web.ignoring().antMatchers("/js/**","/cs/**","/images/**");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //super.configure(http);
        //定制请求的授权规则
        http
            .authorizeRequests()
                // 放行 /** 下的所有请求
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("VIP1")
                .antMatchers("/level2/**").hasRole("VIP2")
                .antMatchers("/level3/**").hasRole("VIP3");
        //开启自动配置的登陆功能，效果，如果没有登陆，没有权限就会来到登陆页面
        http.formLogin().usernameParameter("user").passwordParameter("pwd").loginPage("/userlogin");
        //1、/login来到登陆页
        //2、重定向到/login?error表示登陆失败
        //3、更多详细规定
        //4、默认post形式的 /login代表处理登陆
        //5、一但定制loginPage；那么 loginPage的post请求就是登陆

        //开启自动配置的注销功能。
        http.logout().logoutSuccessUrl("/");
        //1、访问 /logout 表示用户注销，清空session
        //2、注销成功会返回 /login?logout 页面；

        //开启记住我功能
        http.rememberMe().rememberMeParameter("remember");
        //登陆成功以后，将cookie发给浏览器保存，以后访问页面带上这个cookie，只要通过检查就可以免登录
        //点击注销会删除cookie

        //关闭默认的csrf认证
        //http.csrf().disable();
    }

    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //super.configure(auth);
        auth.inMemoryAuthentication()
                //不加.passwordEncoder(new MyPasswordEncoder())
                //就不是以明文的方式进行匹配，会报错
                //.withUser("aqqje").password("123456").roles("VIP1", "VIP2")

                .passwordEncoder(new MyPasswordEncoder())
                .withUser("aqqje").password("123456").roles("VIP1", "VIP2")
                .and()
                .passwordEncoder(new MyPasswordEncoder())
                .withUser("yjgm").password("123456").roles("VIP2", "VIP3")
                .and()
                //.passwordEncoder(new MyPasswordEncoder())。
                //这样，页面提交时候，密码以明文的方式进行匹配。
                .passwordEncoder(new MyPasswordEncoder())
                .withUser("tom").password("123456").roles("VIP1", "VIP3");
    }

}
```

## 4、使用thymeleaf操作数据

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
	  xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<h1 align="center">欢迎光临武林秘籍管理系统</h1>

<div sec:authorize="!isAuthenticated()">
	<h2 align="center">游客您好，如果想查看武林秘籍 <a th:href="@{/userlogin}">请登录</a></h2>
</div>
<hr>
<!--
	isAuthenticated:
	布尔值，指示当前用户是否已通过身份验证（已登录）。
-->
<div sec:authorize="isAuthenticated()">
	<h2>
		<span sec:authentication="name"></span>，您好,您的角色有：
		<span sec:authentication="principal.authorities"></span>
	</h2>
	<form th:action="@{/logout}" th:method="post">
		<input th:type="submit" th:value="注销">
	</form>
</div>

<div sec:authorize="hasRole('VIP1')">
	<h3>普通武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level1/1}">罗汉拳</a></li>
		<li><a th:href="@{/level1/2}">武当长拳</a></li>
		<li><a th:href="@{/level1/3}">全真剑法</a></li>
	</ul>
</div>

<div sec:authorize="hasRole('VIP2')">
	<h3>高级武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level2/1}">太极拳</a></li>
		<li><a th:href="@{/level2/2}">七伤拳</a></li>
		<li><a th:href="@{/level2/3}">梯云纵</a></li>
	</ul>
</div>

<div sec:authorize="hasRole('VIP3')">
	<h3>绝世武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level3/1}">葵花宝典</a></li>
		<li><a th:href="@{/level3/2}">龟派气功</a></li>
		<li><a th:href="@{/level3/3}">独孤九剑</a></li>
	</ul>
</div>
</body>
</html>
```



# 十四、SpringBoot与分布式

## 1、springBoot(1.5.15)+zookeeper+dubbo

### A、在运程主机安装 zookerper 注册中心（[详情](https://hub.docker.com/_/zookeeper/)）

### B、创建一个生产者-服务【provider-ticket】

> 加入依赖

```xml
 <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>0.1.0</version>
        </dependency>

        <!--引入zookeeper的客户端工具-->
        <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>
```



> 创建ticket服务

```java
===========================TicketService====================
public interface TicketService {
    public String ticket();
}

===============================TicketServiceImpl====================
import com.alibaba.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;

@Component
@Service
public class TicketServiceImpl implements TicketService {

    @Override
    public String ticket() {
        return "《厉害了，我的国》";
    }
}
```

> 将服务注册到注册中心

```properties
dubbo.application.name=provider-ticket

dubbo.registry.address=zookeeper://192.168.99.100:2181

dubbo.scan.base-packages=com.aqqje.ticket.service
```

### C、创建一个消费者-用户【consumer-user】

> 加入依赖

<dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>0.1.0</version>
        </dependency>

```xml
    <!--引入zookeeper的客户端工具-->
    <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
    <dependency>
        <groupId>com.github.sgroschupf</groupId>
        <artifactId>zkclient</artifactId>
        <version>0.1</version>
    </dependency>
```

> 将ticket服务的接口复制到cousumer-user用户下；即 public interface TicketService ;同名同包

> 创建用户消费服务

```java
package com.aqqje.user;

import com.alibaba.dubbo.config.annotation.Reference;
import com.aqqje.ticket.service.TicketService;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Reference
    TicketService ticketService;

    public void hello(){
        String ticket = ticketService.ticket();
        System.out.println("买到票：" + ticket);
    }

}
```

> 注册到注册中心

```properties
dubbo.application.name=consumer-user

dubbo.registry.address=zookeeper://192.168.99.100:2181
```

> 测试

## 2、SpringBoot2 + SpringCloud

> 1.eureka --> 注册中心
>
> 2.provider-ticket --> 购票服务
>
> 2.concumer-user --> 消费者购票

### A、eureka

> 1.配置eureka信息

```xml
server:
  port: 8761
eureka:
  instance:
    hostname: eureka-server
  client:
    register-with-eureka: false # 不把自己注册到注册中心
    fetch-registry: false # 不获取注册中心的信息
    service-url:
      defaultZone: http://localhost:8761/eureka/
```



> 2.开启eurekao服务: @ EurekaService

```java
@EnableEurekaServer
@SpringBootApplication
public class EurekaServerApplication {
```

### B、provider-ticket

> 编写服务类

```java
@Service
public class TicketService {
    public String getTicket(){
        return "《厉害了，我的国！》";
    }
}
```

```java
@RestController
public class TicketController {

    @Autowired
    TicketService ticketService;

    @GetMapping("/ticket")
    public String getTicket(){
        return ticketService.getTicket();
    }
}

```

> 将服务注册到注册中心

```properties
server:
  port: 8082
spring:
  application:
    name: provider-ticket
eureka:
  instance:
    prefer-ip-address: true #注册服务的时候使用服务的ip地址
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

### C、concumer-user

> 开启发现服务功能

```java
@EnableDiscoveryClient // 开启发现服务功能
@SpringBootApplication
public class ConsumerUserApplication {
```

> 使用 RestTemplate 发送 http 服务请求

```
  @LoadBalanced // 开启负载均衡机制
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

```

> 使用 ticket服务

```java
package com.aqqje.user.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class UserService {

    @Autowired
    RestTemplate restTemplate;

    public String ticket(){
        String s = restTemplate.getForObject("http://PROVIDER-TICKET/ticket", String.class);
        return s;
    }
}
```

# 十五、SpringBoot与热部署

> 在开发中我们修改一个Java文件后想看到效果不得不重启应用，这导致大量时间花费，我们希望不重启应用的情况下，程序可以自动部署（热部署）。有以下四种情况，如何能实现热部署。

## 1、模板引擎

- 在Spring Boot中开发情况下禁用模板引擎的cache
- 页面模板改变ctrl+F9可以重新编译当前页面并生效

## 2、Spring Loaded

Spring官方提供的热部署程序，实现修改类文件的热部署

- 下载Spring Loaded（项目地址<https://github.com/spring-projects/spring-loaded>）
- 添加运行时参数；
- javaagent:C:/springloaded-1.2.5.RELEASE.jar –noverify

## 3、JRebel

- 收费的一个热部署软件
- 安装插件使用即可

## 4、Spring Boot Devtools（推荐）

- 引入依赖

```xml
<dependency>  
       <groupId>org.springframework.boot</groupId>  
       <artifactId>spring-boot-devtools</artifactId>   
</dependency> 

```

- IDEA使用ctrl+F9



# 十六、SpringBoot与Spring Actuator【监管】

## 1、文档

- [官方html文档](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/actuator-api//pdf/spring-boot-actuator-web-api.pdf)
- [官方ptf文档](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/actuator-api//pdf/spring-boot-actuator-web-api.pdf)

- [中文文档](https://blog.csdn.net/alinyua/article/details/80009435)

## 2、Actuator配置

```xml
========application.properties=======
```

```properties
#基于springboot2x

# 暴露出指定的端口(*代表所有)
#management.endpoints.web.exposure.include=*
management.endpoints.web.exposure.include=info,health
# 设置所有端口的基础路径
management.endpoints.web.base-path=/
# info 信息
info.app.id=springboot-actuator
info.app.version=1.0.0

# 单个使用端口 management.endpoint.[endpoint].enabled=true
management.endpoint.shutdown.enabled=true

# 通过management.endpoints.enabled-by-default来修改全局端口默认配置,以下示例启用info端点并禁用所有其他端点：
#management.endpoints.enabled-by-default=false
management.endpoint.info.enabled=true
management.endpoint.health.enabled=true

#*在YAML中有特殊的含义，所以如果你想包含（或排除）所有的端点，一定要加引号，如下例所示：
#management:
#endpoints:
#web:
#exposure:
#include: '*'

#禁用HTTP端点
#management.server.port=-1

#自定义管理服务器地址
management.server.port=8081
management.server.address=localhost

#Git提交信息
management.info.git.mode=full
#端点映射到其他路径
management.endpoints.web.path-mapping.info=info11
```

## 3、监控和管理端点 

| 端点名      | 描述                        |
| ----------- | --------------------------- |
| autoconfig  | 所有自动配置信息            |
| auditevents | 审计事件                    |
| beans       | 所有Bean的信息              |
| configprops | 所有配置属性                |
| dump        | 线程状态信息                |
| env         | 当前环境信息                |
| health      | 应用健康状况                |
| info        | 当前应用信息                |
| metrics     | 应用的各项指标              |
| mappings    | 应用@RequestMapping映射路径 |
| shutdown    | 关闭当前应用（默认关闭）    |
| trace       | 追踪信息（最新的http请求）  |

## 4、自定义HealthIndicator类

```java
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;

@Component
public class MyHealthIndicators implements HealthIndicator {

    @Override
    public Health health() {
        //return Health.up().build(); 正常
        return Health.down().withDetail("msg","服务异常").build();
    }
}
```



# 十七、[更多SpringBoot整合示例](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples)



