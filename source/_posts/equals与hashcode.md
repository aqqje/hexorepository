---
title: 为什么要重写equals()与hashCode()方法？
date: 2019-1-12 20:56:18
categories:
	- javaee
tags:
	- javaee
---

#### 为什么要重写equals()与hashCode()方法？

### 一、两者的职能是什么？

- **equals**

很明显，该方法就是用来判断两个对象是否是同一个对象。在Object类源码中，其底层是使用了“==”来实现，也就是说通过比较两个对象的内存地址是否相同判断是否是同一个对象。

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

实际应用中，该方法不能满足的我们的需求。因为我们认为两个对象即使不是指向的同一块内存，只要这两个对象的各个字段属性值都相同，那么就认为这两个对象是同一个对象。**所以就需要重写equals（）方法，即如果两个对象指向内存地址相同或者两个对象各个字段值相同，那么就是同一个对象。**

- **hashcod**

Object类源码中，hashCode()是一个native方法，哈希值的计算利用的是内存地址。

```java
public native int hashCode();
```

利用哈希表也能起到一定的判重的作用，但是现实是可能存在哈希冲突,哈希表具有优越的查询性能，并且存在哈希冲突。**

### 二、两者之间有什么联系？

1. 如果两个对象相同（即用equals比较返回true），那么它们的hashCode值一定要相同！！！！；
2. 如果两个对象不同（即用equals比较返回true），那么它们的hashCode值可能相同也可能不同；
3. 如果两个对象的hashCode相同（存在哈希冲突），那么它们可能相同也可能不同(即equals比较可能是false也可能是true) 
4. 如果两个对象的hashCode不同，那么他们肯定不同(即用equals比较返回false)
### 三、为什么要重写？

对于对象集合的判重，如果一个集合含有10000个对象实例，仅仅使用equals()方法的话，那么对于一个对象判重就需要比较10000次，随着集合规模的增大，时间开销是很大的。但是同时使用哈希表的话，就能快速定位到对象的大概存储位置，并且在定位到大概存储位置后，后续比较过程中，如果两个对象的hashCode不相同，也不再需要调用equals（）方法，从而大大减少了equals()比较次数。 
所以从程序实现原理上来讲的话，既需要equals()方法，也需要hashCode()方法。那么既然重写了equals（），那么也要重写hashCode()方法，以保证两者之间的配合关系。 

基于以上分析，我们可以在Java集合框架中得到验证。由于HashSet是基于HashMap来实现的，所以这里只看HashMap的put方法即可。源码如下

```java
public V put(K key, V value) {
        if (table == EMPTY_TABLE) {
            inflateTable(threshold);
        }
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key);
        //这里通过哈希值定位到对象的大概存储位置
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            //if语句中，先比较hashcode，再调用equals()比较
            //由于“&&”具有短路的功能，只要hashcode不同，也无需再调用equals方法
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```

我们在实际应用过程中，如果仅仅重写了equals（），而没有重写hashCode()方法，会出现什么情况？ 
字段属性值完全相同的两个对象因为hashCode不同，所以在hashmap中的table数组的下标不同，从而这两个对象就会同时存在于集合中，所以重写equals()就一定要重写hashCode()方法。

对于“为什么重写equals()就一定要重写hashCode()方法？”这个问题应该是有个前提，就是你需要用到HashMap,HashSet等Java集合。用不到哈希表的话，其实仅仅重写equals()方法也可以吧。而工作中的场景是常常用到Java集合，所以Java官方建议 重写equals()就一定要重写hashCode()方法。 

[原文：为什么重写equals()就一定要重写hashCode()方法？](https://blog.csdn.net/sixingmiyi39473/article/details/78306296)

### 四、==与equals的区别

- **==操作符专门用来比较两个变量的值是否相等，也就是用于比较变量所对应的内存中所存储的数值是否相同**，要比较两个基本类型的数据或两个引用变量是否相等，只能用==操作符。

- **equals方法是用于比较两个独立对象的内容是否相同，就好比去比较两个人的长相是否相同，它比较的两个对象是独立的。**

[参考: Java的==与equals之辨，简单解释，很清楚.](https://www.cnblogs.com/findumars/p/3746878.html)