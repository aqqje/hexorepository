---
title: 如何创建Class的实例
date: 2018-03-07 16:37:13
tags: java
categories: java
---
{% note info %}
如何获取Class的实例（4种）
{% endnote %}
<!-- more -->

```java
//Class实例四:通过类的加载器
ClassLoader cl = new Arithmetic().getClass().getClassLoader();
Class clazz3 = cl.loadClass("com.javaee_02.exer1.Arithmetic");
Arithmetic ar3 = (Arithmetic)clazz3.newInstance();
System.out.println(clazz3.getName());

//Class实例三:通过Class的静态方法获取
Class clazz2 = Class.forName("com.javaee_02.exer1.Arithmetic");
Arithmetic ar2 = (Arithmetic)clazz2.newInstance();

//Class实例二:通过运行时类的对象获取
Arithmetic ar1 = new Arithmetic();
Class clazz1 = ar1.getClass();

//Class实例一:调用运行时类本身的.class属性
Class<Arithmetic> clazz = Arithmetic.class;
Arithmetic ar = (Arithmetic)clazz.newInstance();
```