---
title: java注解
date: 2018-03-07 17:47:35
tags: java
categories: java
---
{% note info %}
java注解，三个基本的注解，自定义的注解，元注解
{% endnote %}
## 注解(Annotation)@since 1.5

## 三个基本的注解
>@Override:限定重写父类方法，该注释只能用于方法
>@Deprecated:用于表示某个程序元素（类， 方法等）已过时
>@SupperssWarnings:抑制编译器警钟警告

## 自定义的注解

使用@interface声明一个注解

```java
public @interface MyAnnotation{
	String value() default "hello";
}
```

## 元注解

>@Retention:只能用于修饰一个注解定义，用于指定该注解可以保留多长时间
[RetentionPolicy]类型成员变量，使用@Retention必须指定Value;
value:
 - RetetionPolicy.SOURCE:编译器直接丢弃这种策略的注释
 - RetetionPolicy.CLASS:编译器将把注释记录在class文件中，运行java程序时，<span id="inline-yellow">jvm不会保留注解</span>，这是默认值
 - RetetionPolicy.RUNTIME:编译器将把注释记录在class文件中，运行java程序时，<span id="inline-yellow">jvm会保留注解</span>，程序可以通过反射获取该 注释

>@Target:用于修饰注解定义，用于指定被修饰的注解能用于修饰那些程序元素，@Target也包含一个名为value的成员变量

>@Document：用于指定被该元注解修饰的注解类将javadoc工具提取成文档
说明：依赖@Retention(RetentionPolicy.RUNTIME)

>@Inheried:被该注解修饰的注解将具有继承性，修饰的注解的类的子类将自动具有该注解
