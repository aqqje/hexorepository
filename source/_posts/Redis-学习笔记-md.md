---
title: Redis 学习笔记.md
date: 2018-09-06 12:13:04
tags: Redis
---

# Redis 学习笔记

## 一、简介

### 1、L**NoSQL非关系型数据库** 

```tex
not only sql, 是一种非关系型的数据存储，key/value键值对存储。现有Nosql DB 产品： Redis/MongoDB/Memcached/Hbase/Cassandra/ Tokyo Cabinet/Voldemort/Dynomite/Riak/ CouchDB/Hypertable/Flare/Tin/Lightcloud/ KiokuDB/Scalaris/Kai/ThruDB, 等等~~~
```

### 2、**为什么需要NoSQL非关系型数据库？** 

```tex
- High performance - 对数据库高并发读写的需求

- Huge Storage - 对海量数据的高效率存储和访问的需求

- High Scalability && High Availability- 对数据库的高可扩展性和高可用性的需求
```

### 3、**Redis** 

```tex
Redis是一种面向“键/值”对类型数据的分布式NoSQL数据库系统，特点是高性能，持久存储，适应高并发的应用场景。和Memcached类似，它支持存储的value类型相对更多，包括string(字符串)、list(链表)、set(集合)和zset(有序集合)。 这些数据类型支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的，支持各种不同方式的排序。redis 与memcached一样，为了保证效率，数据都是缓存在内存中。区别的是redis会周期性的把更新的数据写入磁盘或者把修改 操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

redis目前提供五种数据类型：String,Hash,List,Set,sorted set

Redis的存储分为内存存储、磁盘存储和log文件三部分，配置文件中有三个参数对其进行配置

save seconds updates :指出在多长时间内，有多少次更新操作，就将数据同步到数据文件。
appendonly yes/no :是否在每次更新操作后进行日志记录。如果不开启，可能会在断电时导致一段时间内的数据丢失。因为redis本身同步数据 文件是按上面的save条件来同步的，所以有的数据会在一段时间内只存在于内存中。
appendfsync no/always/everysec ：数据缓存同步至磁盘的方式。no表示等操作系统进行数据缓存同步到磁盘，always表示每次更新操作后手动调用fsync()将数据写到磁盘，everysec表示每秒同步一次。
```



## 二、安装

### 1、下载

 [【win7&&win10 】](https://github.com/MicrosoftArchive/redis/releases)

![](/images/redis/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180815231718.png)

#### A、mis文件

```tex
一路 next 即可
```



![](/images/redis/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180815232412.png)

```tex
测试
```





![1534346808153](C:\Users\ADMINI~1\AppData\Local\Temp\1534346808153.png)

```tex
设置为windows下的服务

- redis-server --service-install redis.windows-service.conf --loglevel verbose
```

![](/images/redis/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180815233103.png)

```tex
观察服务列表
```



![](/images/redis/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180815233252.png)



```tex
完成
```



```tex
常用的redis服务命令。

卸载服务：redis-server --service-uninstall

开启服务：redis-server --service-start

停止服务：redis-server --service-stop

 
```





#### B、zip文件

```tex
- 直接解压

- 手动配置环境变量

- 运行 cmd： redis-server.exe redis.windows.conf
```

![](/images/redis/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180815233827.png)



```tex
完成
```



## 三、参考

[参考【https://www.cnblogs.com/sunxuchu/p/5463403.html】](https://www.cnblogs.com/sunxuchu/p/5463403.html)

[参考【https://www.cnblogs.com/lezhifang/p/7027903.html】](https://www.cnblogs.com/lezhifang/p/7027903.html)

[参考【https://www.cnblogs.com/sunxuchu/p/5463403.html】](https://www.cnblogs.com/sunxuchu/p/5463403.html)