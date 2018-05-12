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

## 工厂方法创建 Bean

- 1) 静态工厂方法:直接调用某一个类的静态方法就可以返回一个 bean 实例

```java 
public class StaticFactory {
    private static Map<String, Object> cars = new HashMap<>();

    static{
        cars.put("audi", new Car("adui", 100000));
        cars.put("ford", new Car("ford", 400000));
    }

    public static Car getCar(String carName){
        return (Car)cars.get(carName);
    }
}
````

```xml 
    <!--
        通过静态工厂方法来配置 bean， 注意不是配置静态工厂实例，而是配置 bean 实例

        class 属性：指向静态工厂方法的全类名
        factory-method: 指向静态工厂方法的名字
        constructor-arg: 如果工厂方法需要传入参数，则使用 constructor-arg 来配置参数
     -->
    <bean id="car" class="aqqje.com.factory.StaticFactory" factory-method="getCar">
        <constructor-arg value="ford"/>
    </bean>
```

- 2) 实例工厂方法:

```java 
public class InstaceFactory {

    private  Map<String, Object> cars;
    public InstaceFactory(){
        cars = new HashMap<>();
        cars.put("audi", new Car("adui", 100000));
        cars.put("ford", new Car("ford", 400000));
    }

    public Car getCar(String CarName){
        return (Car)cars.get(CarName);
    }
}

```

```xml
    <!--
        factory-bean ：指向实例工厂方法的全类名
        factory-method: 指向实例工厂方法的名字
        constructor-arg: 如果工厂方法需要传入参数，则使用 constructor-arg 来配置参数
     -->

    <bean id="instaceFactory" class="aqqje.com.factory.InstaceFactory" />

    <bean id="car1" factory-bean="instaceFactory" factory-method="getCar">
        <constructor-arg value="audi"/>
    </bean>
```

## 组件装配

- <context:component-scan> 自动注册 AutowiredAnnotationBeanPostProcessor 实例

	可以使用 autuwire 属性指定自行装配的方式（不推荐）
        byName: 根据 bean 的名字和当前 Bean 的 setter 风格的属性名进行自动装配，若有匹配的，则进行自行装配， 若没有就不装配
        byType: 根据 bean 的类型和当前 Bean 的 的属性的类型进行自动装配， 注意：byType 使用则该只能是出现 1 次， 若有 2 个及以上的则抛出异常

 @Autowired 注解自动装配具有兼容类型的单个 Bean属性<br/>
	- 1.构造器, 普通字段(即使是非 public), 一切具有参数的方法都可以应用@Authwired 注解<br/>
	- 2.默认情况下, 所有使用 @Authwired 注解的属性都需要被设置. 当 Spring 找不到匹配的 Bean 装配属性时, 会抛出异常, 若某一属性允许不被设置, 可以设置 @Authwired 注解的 required 属性为 false<br/>
	- 3.默认情况下, 当 IOC 容器里存在多个类型兼容的 Bean 时, 通过类型的自动装配将无法工作. 此时可以在 @Qualifier 注解里提供 Bean 的名称. Spring 允许对方法的入参标注 @Qualifiter 已指定注入 Bean 的名称<br/>
	- 4.@Authwired 注解也可以应用在数组类型的属性上, 此时 Spring 将会把所有匹配的 Bean 进行自动装配.<br/>
	- 5.@Authwired 注解也可以应用在集合属性上, 此时 Spring 读取该集合的类型信息, 然后自动装配所有与之兼容的 Bean. <br/>
	- 6.@Authwired 注解用在 java.util.Map 上时, 若该 Map 的键值为 String, 那么 Spring 将自动装配与之 Map 值类型兼容的 Bean, 此时 Bean 的名称作为键值<br/>

- Spring 还支持 @Resource 和 @Inject 注解，
	
	这两个注解和 @Autowired 注解的功用类似
	@Resource 注解要求提供一个 Bean 名称的属性，若该属性为空，则自动采用标注处的变量或方法名作为 Bean 的名称
	@Inject 和 @Autowired 注解一样也是按类型匹配注入的 Bean， 但没有 reqired 属性
	建议使用 @Autowired 注解
	
## 基于注解方式的 aop

- 加入 jar 包：
    
    与 aop 相关:
    - com.springsource.org.aopalliance-1.0.0.jar<br/>
    - com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar<br/>
    - spring-aspects-4.3.14.RELEASE.jar<br/>
    - spring-aop-4.3.14.RELEASE.jar<br/>

    常用:<br/>
    commons-logging-1.2.jar<br/>
    spring-beans-4.3.14.RELEASE.jar<br/>
    spring-context-4.3.14.RELEASE.jar<br/>
    spring-core-4.3.14.RELEASE.jar<br/>
    spring-expression-4.3.14.RELEASE.jar<br/>
    
- 在 spring IoC 容器加入 aop 命令空间并加入如下配置:

 <!-- 使用 aspectj 起作用：自动匹配相应的类生成代理对象 -->
 <aop:aspectj-autoproxy />

- 把横切关注点的代码抽象到切面的类中
 - 使用 @Component 声明该类是 IoC 容器的一个 Bean 
 -  使用 @Aspect 声明该类是一个切面
    
    在切面类声明各种通知：
    - @Before: 前置通知, 在方法执行之前执行
    - @After: 后置通知, 在方法执行之后执行 [无论是否异常]
    - @AfterRunning: 返回通知, 在方法返回结果之后执行[返回参数 throwing 的值与方法的异常参数名要一致,方法的异常类型可以指定，指定有则执行，无则不执行] 
    - @AfterThrowing: 异常通知, 在方法抛出异常之后[异常参数 returning 的值与方法的返回参数名要一致] 
    - @Around: 环绕通知, 围绕着方法执行[该相当一个完整的代理过程 与 ProceedingJoinPoint 参数共同使用，并且方法有返回值]

- 可以在通知方法中声明一个类型为 JoinPoint 的参数，然后就能访问链接细节，如方法名称和参数值

```java 
        @Around(value="declareJointPointExpression()")
            public Object arounMethod(ProceedingJoinPoint joinPoint){
                String methodName = joinPoint.getClass().getName();
                List<Object> args = Arrays.asList(joinPoint.getArgs());
                Object result = null;
                try {
                    // 前置通知
                    System.out.println("The method " + methodName + " with " + args);
                    // 执行方法
                    result = joinPoint.proceed();
                    // 返回通知
                    System.out.println("The method " + methodName + " end " + result);
                } catch (Throwable throwable) {
                    throwable.printStackTrace();
                    // 异常通知
                    System.out.println("The method " + methodName + " ocrrous thorw " + throwable);
                }
                // 后置通知
                System.out.println("The method " + methodName + " end " + result);
                return result;
            }
```
    
- 最典型的切入点表达式时根据方法的签名来匹配各种方法:
  - execution * com.atguigu.spring.ArithmeticCalculator.*(..): 匹配 ArithmeticCalculator 中声明的所有方法,第一个 * 代表任意修饰符及任意返回值. 第二个 * 代表任意方法. .. 匹配任意数量的参数. 若目标类与接口与该切面在同一个包中, 可以省略包名.
  - execution public * ArithmeticCalculator.*(..): 匹配 ArithmeticCalculator 接口的所有公有方法.
  - execution public double ArithmeticCalculator.*(..): 匹配 ArithmeticCalculator 中返回 double 类型数值的方法
  - execution public double ArithmeticCalculator.*(double, ..): 匹配第一个参数为 double 类型的方法, .. 匹配任意数量任意类型的参数
  - execution public double ArithmeticCalculator.*(double, double): 匹配参数类型为 double, double 类型的方法.
 
- 重用切面关注点表达式:
  - 定义一个方法, 用于声明切入点表达式. 一般地, 该方法中再不需要添入其他的代码.
  - 使用 @Pointcut 来声明切入点表达式.
  - 后面的其他通知直接使用方法名来引用当前的切入点表达式.
   
 
- @Order(int order) 该声明切点类的执行顺序, 参数 order 值越小其执行顺序就越高 
 
## 基于 IoC 容器配置方式

  - 配置切面 Bean
  - 配置 AOP
  - 配置切面表达式
  - 配置切面及通知
  
```xml 
<!-- 配置 Bean -->
    <bean id="arithmeticCalculator" class="aqqje.com.aspect.xml.ArithmeticCalculatorImpl" />
    <!-- 配置切面 Bean -->
    <bean id="loggingAspect" class="aqqje.com.aspect.xml.LoggingAspect"/>

    <!-- 配置 AOP -->
    <aop:config>
        <!-- 配置切点表达式 -->
        <aop:pointcut id="arithmeticCalculatorPoincut" expression="execution(* aqqje.com.aspect.xml.ArithmeticCalculator.*(..))"/>
        <!-- 配置通知及切面 -->
        <aop:aspect ref="loggingAspect" order="1">
            <aop:before method="berforeMethod" pointcut-ref="arithmeticCalculatorPoincut"/>
            <aop:after method="afterMethod" pointcut-ref="arithmeticCalculatorPoincut"/>
            <aop:after-returning method="afterReturnMethod" pointcut-ref="arithmeticCalculatorPoincut" returning="result"/>
            <aop:after-throwing method="afterThorwMethod" pointcut-ref="arithmeticCalculatorPoincut" throwing="e"/>
        </aop:aspect>
    </aop:config>
```

## spring 事务

- 声明式事件
 - 配置事务管理器 DataSourceTransactionManager
 - 启用事务管理 transaction-manager
 - 添加事件注解 Transactional
 
```xml 
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!-- 启用事务管理 -->
    <tx:annotation-driven transaction-manager="transactionManager" />
```

## 事件的传播行为

- 当一个事务方法被另一个事务方法调用时，必须指定事务应该如何传播
- 事务的传播行为可以由传播属性指定：spring 定义了 7 种类传播行为 
- 使用 propagation 指定事件的传播行为
 - Propagation.REQUIRED, 即使用外事务。
 - REQUIRES_NEW， 即使用内事务，外事务挂起
 
## 事务的隔离级别

* 1.使用 propagation 指定事务的传播行为，即当前事务方法被别处一个事务方法调用时
     *  如何使用事务，默认取值为 REQUIRED， 即使用调用方法的事务
     *  REQUIRES_NEW：事务自己的事务，调用的事务方法的事务被挂起。
     *  2. 使用 isolation 指定事务的隔离级别， 最常用的取值为事务READ_COMMITTED
     *  3.默认情况下 spring 的声明式事务所有的运行时异常进行回滚，也可以通过对应的属性进行设置，通常情况下去默认值即可。
     *  4.使用 readOnly 指定指定事务的是否为只读， 表示这个事务只读取数据但不更新数据 ，
     *  这样可以帮助数据库引擎优化事务， 若真的事一个只读取数据库值的方法， 应设置 readOnly = true
     *  5.使用 timiout 指定强制回滚之前事务可以占用的赶时间
     
     
## xml形式配置事务

- 步骤：
 - 1.配置事务管理器
 - 2.配置事务属性
 - 3.配置事务切入点，把事务切入点和事务属性关联起来
 
 ```xml 
     <!-- 配置事务管理器 -->
     <bean id="tranactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
         <property name="dataSource" ref="dataSource" />
     </bean>
 
     <!-- 配置事务属性 -->
     <tx:advice id="txAdvice" transaction-manager="tranactionManager">
         <tx:attributes>
             <tx:method name="purchase" propagation="REQUIRES_NEW"/>
 
         </tx:attributes>
     </tx:advice>
 
 
     <!-- 配置事务切入点，把事务切入点和事务属性关联越来 -->
     <aop:config>
         <aop:pointcut id="txPointCut" expression="execution(* aqqje.com.jdbc.txxml.services.BookStockService.*(..))"/>
         <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
     </aop:config>
 ```
 
 ## Spring 如何在 WEB 应用使用?
 
-  1) 需要外加的 jar 包:
 - spring-web-xxx.RELEASE.jar
 - spring-webmvc-xxx.RELEASE.jar

- 2) spring 的配置文件不变

- 3) 如何创建 IoC 容器?
 - a 非 WEB 应用在 main 方法中直接创建
 - b 应该在 WEB 应用被服务器加载时就创建 IoC 容器:  
      在 ServletContextListener#contextInitalized(ServletContextExvent sce) 方法中创建
 - c  在 WEB 应用的其他组件中如何来访问 IoC 容器? 
      在 ServletContextListener#contextInitalized(ServletContextExvent sce) 方法中创建后, 
      可以把其在 ServletContext(即 application 域)的一个属性中
 - d 实际上, spring 配置文件的名字和位置应该也是可以配置的! 将其配置到时当前 WEB 应用的初始化参数中较为合适

- 4) 在 WEB 环境下使用 spring 
 - 需要在 web.xml 文件中加入如下配置:
 
 ```xml 
 <!-- 配置 Spring 配置文件的名称和位置 -->
 <context-param>
 	<param-name>contextConfigLocation</param-name>
 	<param-value>classpath:applicationContext.xml</param-value>
 </context-param>
 
 <!-- 启动 IOC 容器的 ServletContextListener -->
 <listener>
 	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
 </listener>
 ```
 
 
 
 
 
 
