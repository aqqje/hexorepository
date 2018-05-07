---
title: spring learn
date: 2018-05-07 23:05:40
tags: java, ssm
---

Spring Learn Note
<!-- more -->

## Spring 是什么

简单描述：

- Spring 是一个开源框架.
= Spring 为简化企业级应用开发而生. 使用 Spring 可以使简单的 JavaBean 实现以前只有 EJB 才能实现的功能.
- Spring 是一个 IOC(DI) 和 AOP 容器框架.

具体描述 Spring:

- 轻量级：Spring 是非侵入性的 - 基于 Spring 开发的应用中的对象可以不依赖于 Spring 的 API
- 依赖注入(DI --- dependency injection、IOC)
= 面向切面编程(AOP --- aspect oriented programming)
= 容器: Spring 是一个容器, 因为它包含并且管理应用对象的生命周期
= 框架: Spring 实现了使用简单的组件配置组合成一个复杂的应用. 在 Spring 中可以使用 XML 和 Java 注解组合这些对象
- 一站式：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库 （实际上 Spring 自身也提供了展现层的 SpringMVC 和 持久层的 Spring JDBC）

## Spring HelloWord 

1. 创建 javaBean 类

```java
public class HelloWord {

    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public void hello() {
        System.out.println("Hello:" + name);
    }
}
```
2. 新建 Spring IoC 容器 applicationContext.xml,并配置相应的 Bean

```xml
    <bean id="helloWord" class="aqqje.com.beans.HelloWord">
        <property name="name" value="aqqje" />
    </bean>
```
3. 编写测试类

```java
// 创建 spring IoC 的容器对象
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
// 从容器中获取 bean
// 利用 id 定位容器中的 bean 推荐使用
HelloWord hw = (HelloWord)context.getBean("helloWord");
// 调用 hello()
// 利用 id 定位容器中的 bean 推荐使用
//HelloWord hw = (HelloWord)context.getBean("helloWord");
// 利用 类型返回容器中的 bean 要求：容器只一个该类型的 bean
//HelloWord hw = (HelloWord)context.getBean(HelloWord.class);
hw.hello();
```

## Spring IoC 容器 Bean 的配置

- id : Bean 的名字
     1. 在 IOC 容器中必须是唯一的
	 2. 若 id 没有指定，Spring 自动将权限定性类名作为 Bean 的名字
	 3. id 可以指定多个名字，名字之间可用逗号、分号、或空格分隔

- class: Bean 本身的类名
	 1.必须为全类名
	 
	 
- ref: 指向引用 Bean 的名字

	1.可以引用外部的 Bean 
	2.可以创建内部 Bean 
	3.内部创建的 Bean 只能内部使用, 不能其他外部 Bean 引用
	
```xml
<property name="persion">
	&lt;!&ndash; 内部Bean, 注意：不能被外部 Bean 引用, 只能在内部使用  &ndash;&gt;
	<bean class="aqqje.com.beans.Persion">
		<constructor-arg value="aqqje" type="java.lang.String"></constructor-arg>
		<constructor-arg type="java.lang.String">
			<value><![CDATA[<男>]]></value>
		</constructor-arg>
		<constructor-arg type="double">
			<value>20000.0</value>
		</constructor-arg>
	</bean>
</property>
```

- <constructor-arg>: 构造器注入
		1. value 属性为 bean 的构造器的实参值，为值的内容字符形式
		2. 定位(index="0" 或 type="aqqje.com.beans.Persion" 两者可以混用) ,即指定 value 的类型
		
- 字面值：可用字符串表示的值
	     1.可以使用 <value> 元素标签 或 value 属性进行注入
         2.基本数据类型及其封装类, Stirng 等类型都可以采取字面值注入的方式
         3.若字面值包含特殊字符，可以使用<![CDATA[]]>把字面值包裹
		 
```xml
<bean id="persion2" class="aqqje.com.beans.Persion">
	<constructor-arg value="aqqje" type="java.lang.String"></constructor-arg>
	<constructor-arg type="java.lang.String">
		<value><![CDATA[<男>]]></value>
	</constructor-arg>
	<constructor-arg type="double">
		<value>500.0</value>
	</constructor-arg>
</bean>
```		 

- <null/> : 可以使用专用的 <null/> 元素标签为 Bean 的字符串或其它对象类型的属性注入null 值

```xml
<property name="persion"><null/></property>
```

- 级联：spring 支持级联属性: 级联属性赋值 注意：属性需要先初始化，才能级联属性赋值, 否则将抛出异常

```xml
<property name="persion.name" value="love" />
```

- 集合属性 ：List && Set || Map
	1.使用 list 节点为 List 类型属性赋值
	
```xml
<property name="persion">
	<!--
	Set 类型类似 List
	 -->
	<list>
		<ref bean="persion1"/>
		<ref bean="persion2"/>
	</list>
</property>

```
	2.使用 map 节点及 map中的 entry 节点配置 Map 属性的成员变量
	
```xml
<property name="persion">
	<map>
		<entry key="aa" value-ref="persion1" />
		<entry key="bb" value-ref="persion2" />
	</map>
</property>
```
- properties 属性:

```properties
user=root
password=root
driverClass=com.mysql.jdbc.Driver
jdbcUrl=jdbc:mysql:///scms
```
```xml
<bean id="dataSource" class="aqqje.com.beans.contollers.DataSource">
	<property name="properties">
		<!-- 使用 props 和 prop 配置 properties 文件 -->
		<props>
			<prop key="driverClass">com.jdbc.mysql.Driver</prop>
			<prop key="jdbcUrl">jdbc.mysql:///test</prop>
			<prop key="user">root</prop>
			<prop key="password">root</prop>
		</props>
	</property>
</bean>
```
	
- 配置单例集合 Bean :  util命名空间 以便多个 Bean 引用 : 注意：需要导入 util jar包

```xml
<util:list id="persions">
	<ref bean="persion1" />
	<ref bean="persion2" />
</util:list>
<!-- 引用单例集合 Bean -->
<property name="persion" ref="persions"/>
```
- p 命名空间：可以使用 p 命名空间对 Bean 的属性进行赋值 注意：需要导入 p jar包 ;特点：比较传统的方式更简洁

```xml
 <bean id="god5" class="aqqje.com.beans.contollers.God"  p:name="Aellen" p:leg="5" p:persion-ref="persions" />
```

## spring Bean 之间的关系

1. 继承：1.1)Bean(子Bean) 可以使用 parent 属性来继承父类的 Bean(父Bean)
		 1.2)Bean(子Bean) 可以覆盖 Bean(父Bean) 的属性

2. 抽象：2.1)Bean(父Bean) 可以使用 abstract 属性来定义该 Bean 为抽象Bean,
		 2.2)若一个 Bean 没有指定 Class 属性, 则该 Bean 必须是一个抽象 Bean
		 2.3)抽象 Bean 的 class 属性可以省略, 如省略 Bean(子Bean) 则必须指定 class 属性,如不省略，Bean(子Bean)则可以省略不指定
		 2.4)注意：抽象 Bean 不可以被 IoC 容器所实例化

3.依赖：1.1)使用 depends-on 属性指定该 Bean 需要 依赖的 Bean , 被依赖的 Bean 必须要存在(不分先后), 否则将抛出异常
		1.2)单单指定 depens-on 属性是不行的, 必须与 p: ref 共同使用
		1.3)如果前置依赖于多个 Bean，则可以通过逗号，空格或的方式配置 Bean 的名称
		1.4)前置依赖的 Bean 会在本 Bean 实例化之前创建好

```xml
<bean id="adress" class="aqqje.com.relation.Adress"  p:city="HuNan" p:street="HengYang"   abstract="true"/>
<!-- <bean id="adress1" class="aqqje.com.relation.Adress" p:city="HuNan" p:street="XiangTang"/>-->
<bean id="adress1"  p:city="HuNan" p:street="XiangTang" parent="adress"/>

<bean id="persion" class="aqqje.com.relation.Persion" p:name="aqqje" p:car-ref="car" depends-on="car" />
<bean id="car" class="aqqje.com.relation.Car" p:brank="China" p:pirce="6100000" />
```


- spring Bean 作用域: 使用 scope 属性里设置 Bean 的作用域

	singleton(单例): 默认值, 在 IoC 容器创建时该 Bean 就被实例化了, 整个 IoC 容器范围内都能共享该 Bean , 生命周期与 IoC 一样长.
	prototype(原型): 在 IoC 容器创建时该 Bean 不会被实例化, 一到需要使用时调用 getBean() 方法就会实例化一个该 Bean 的对象.
	request(请求): 每次 Http 请求都会实例化一个 Bean 对象, 该作用仅使用于 WebApplicationContext 环境
	session(会议): 同一个 Http session 共享一个 bean , 不同的 Http session 使用不同的 bean, 该作用仅使用于 WebApplicationContext 环境
	
```xml
<bean id="car" class="aqqje.com.relation.Car" p:brank="Chian" p:pirce="61000.0" scope="prototype" />
```

- Spring 表达式语言（简称SpEL）：是一个支持运行时查询和操作对象图的强大的表达式语言。
            语法类似于 EL：SpEL 使用 #{…} 作为定界符，所有在大框号中的字符都将被认为是 SpEL
            SpEL 为 bean 的属性进行动态赋值提供了便利
        
		通过 SpEL 可以实现：
            通过 bean 的 id 对 bean 进行引用
            调用方法以及引用对象中的属性
            计算表达式的值
            正则表达式的匹配

        字面量的表示：
            整数：<property name="count" value="#{5}"/>
            小数：<property name="frequency" value="#{89.7}"/>
            科学计数法：<property name="capacity" value="#{1e4}"/>
            String可以使用单引号或者双引号作为字符串的定界符号：<property name=“name” value="#{'Chuck'}"/> 或 <property name='name' value='#{"Chuck"}'/>
            Boolean：<property name="enabled" value="#{false}"/>
        引用其他对象 && 引用其他对象的属性 && 调用其他方法,还可以链式操作 $$ 支持运算符
            &lt;!&ndash; 通过 value 属性和 SpEL 之间的应用关系 &ndash;&gt;
            <bean id="persion" class="aqqje.com.spel.Persion"  p:name="#{car.brank}" p:car="#{car}" p:adress="#{adress.city}" p:wage="#{car.pirce > 3000 ? '金领' : '白领'}" />

        支持运算符：
            算数运算符：+, -, *, /, %, ^：
            加号还可以用作字符串连接：
            比较运算符： <, >, ==, <=, >=, lt, gt, eq, le, ge
            逻辑运算符号： and, or, not, |
            if-else 运算符：?: (ternary), ?: (Elvis)
            if-else 的变体
            正则表达式：matches

```xml
<bean id="adress" class="aqqje.com.spel.Adress" p:city="#{'HuNan'}" p:street="softSchool"  />
<bean id="car" class="aqqje.com.spel.Car" p:brank="#{'Chinal'}" p:pirce="2000" p:tirecCircumference="#{T(java.lang.Math).PI * 25}" />
<bean id="persion" class="aqqje.com.spel.Persion"  p:name="#{car.brank}" p:car="#{car}" p:adress="#{adress.city}" p:wage="#{car.pirce > 3000 ? '金领' : '白领'}" />
```

- IoC 容器中 Bean 的周期：
            作用：
                Spring IOC 容器可以管理 Bean 的生命周期, Spring 允许在 Bean 生命周期的特定点执行定制的任务.
            初始 && 销毁
                在 Bean 的声明里设置 init-method 和 destroy-method 属性, 为 Bean 指定初始化和销毁方法.

         Bean 的周期过程：
             1.通过构造器或工厂方法创建 Bean 实例
             2.为 Bean 的属性设置值和对其他 Bean 的引用
             3.调用 Bean 的初始化方法
             4.Bean 可以使用了
             5.当容器关闭时, 调用 Bean 的销毁方法

         Bean 后置处理器：
            作用：
                1.在调用初始化方法前后对 Bean 进行额外的处理.
                2.Bean 后置处理器对 IOC 容器里的所有 Bean 实例逐一处理, 而非单一实例. 其典型应用是: 检查 Bean 属性的正确性或根据特定的标准更改 Bean 的属性.

            实现:
                1.自定义 Bean 后置处理器并实现 BeanPostProcessor 接口
                2.初始化方法被调用前后，重写 postProcessAfterInitialixation(...) && postProcessBeforeInitialixation(...)方法
                3.在 spring IoC 容器中配置Bean 后置处理器
            添加 Bean 后置处理器中 Bean 的周期过程：
                1.通过构造器或工厂方法创建 Bean 实例
                2.为 Bean 的属性设置值和对其他 Bean 的引用
                3.*将 Bean 实例传递给 Bean 后置处理器的 postProcessBeforeInitialization 方法*
                4.调用 Bean 的初始化方法
                5.*将 Bean 实例传递给 Bean 后置处理器的 postProcessAfterInitialization方法*
                6.Bean 可以使用了
                7.当容器关闭时, 调用 Bean 的销毁方法
				
```java
// car init
public void init() {
	System.out.println("Car init..");
}
// car destroy
public void destroy() {
	System.out.println("Car destroy....");
}
```

```java
// Bean 后置处理器
public class MyPostProcessor implements BeanPostProcessor {

@Override
public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
	System.out.println("postProcessBeforeInitialization:" + bean + "," + beanName);
	return bean;
}

@Override
public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
	System.out.println("postProcessBeforeInitialization:" + bean + "," + beanName);
	Car car = new Car();
	car.setName("aqqje");
	return car;
}
```

```xml
<bean id="car" class="aqqje.com.lifecycle.Car"
	  p:name="BMWX6" p:price="10000000" init-method="init" destroy-method="destroy"/>

<!--
	bean : bean 实例本身
	beanName: IoC容器配置 的bean 的名字
	返回值： 是实际上返回给用户的那个 Bean, 注意：可以在以上两方法中修改返回的 bean, 甚至返回一个新的 bean

	配置 bean 的后置处理器： 不需要配置 id , IoC 容器自动识别一个 BeanPostProcessor
 -->
<bean class="aqqje.com.lifecycle.MyPostProcessor" />	
```