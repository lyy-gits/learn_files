

# nosql概述

## 为什么要用Nosql

> 1、单机MySQL的年代

![image-20210303104359217](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303104359217.png)

90年代，一个基本的网站访问量一般不会太大，单个数据库完全足够！

那个时候，更多的去使用静态网页html，服务器根本没有太大的压力

思考一下，这种情况下：整个网站的瓶颈是什么？

1、数据量如果太大，一个机器放不下了

2、数据的索引mysql（B+Tree），一个机器内存也放不下

3、访问量（读写混合），一个服务器承受不了

只要开始出现以上的三种情况之一，那么就必须要晋级

> 2、Memcached（缓存）+MySQL+垂直拆分（读写分离）

网站80%的情况都是在读，每次都要去查询数据库的话就十分的麻烦！希望减轻数据库的压力，可以使用缓存来保证效率。

发展过程：优化数据结构和索引—>文件缓存（IO）—>Memcached（当时最热门的技术）

![image-20210303105531134](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303105531134.png)

> 3、分库分表+水平拆分+MySQL集群

技术和业务在发展的同时，对人的要求也越来越高

==本质：数据库（读，写）==

早些年MyLSAM：表锁，十分影响效率，高并发下就会出现严重的锁问题

转战Ioondb：行锁

慢慢的开始使用分库分表来解决写的压力，MySQL在那个年代退出了表分区，这个并没有多少公司使用

MySQL的集群，很好的满足那个年代的所有需求

![image-20210303110948323](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303110948323.png)

> 4、如今最近的年代

2010（按键手机Android 1.0 HTC）-2020十年之间，世界已经发生了翻天覆地的变化；（定位，也是一种数据，音乐，热榜！）

MySQL等关系型数据库就不够用了！数据量很多，变化很快

MySQL有的使用它来存储一些比较大的文件，博客，图片，数据库表很大，效率就低了。如果有一种数据库来专门处理这种数据，MySQL压力就变得十分小（研究如何处理这些问题）大数据的IO压力下，表几乎没法更改

> 目前一个基本的互联网项目

![image-20210303112902818](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303112902818.png)

> 为什么要用NoSQL

用户的个人信息，社交网络，地理位置，用户自己产生的数据，用户日志等等爆发式增长

这时候我们就需要NoSQL数据库，NoSQL可以很好的处理以上的情况

## 什么是NoSQL

> NoSQL

NoSQL=Not Only SQL (不仅仅是SQL)

关系型数据库：表格，行，列

泛指非关系型数据库，随着web2.0互联网的诞生！传统的关系型数据库很难对付web2.0时代，尤其是超大规模的高并发的社区！暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，redis是发展最快的，而且是我们当下必须掌握的技术

很多的数据类型用户的个人信息，社交网络，地理位置。这些数据类型的存储不需要一个固定的格式，不需要多余的操作就可以横向扩展的。使用键值对来控制

> NoSQL特点

解耦

1、方便扩展（数据之间没有关系，很好扩展）

2、大数据量高性能（Redis一秒写8万次，读取11万，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高）

3、数据类型是多样型的（不需要事先设计数据库，随取随用，如果是数据量十分大的表，很多人就无法设计）

4、传统RDBMS和NoSQL

```shell
传统的RDBMS关系型数据库
- 结构化组织（表、列）
- SQL
- 数据和关系都存储在单独的表中
- 数据操作，数据定义语言
- 严格的一致性
- 基础的事务
....
```

```shell
NoSQL
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储，文档存储，图形数据库（社交关系）
- 最终一致性
- CAP定理（一致性、可用性、分区容错）和BASE（异地多活）
- 高性能，高可用，高可扩展性
......
```

> 了解：3V+3高

大数据时代的3V：主要是描述问题的

+ 海量Volume
+ 多样Variety
+ 实时Velocity

大数据时代的3高：主要是对程序的要求

+ 高并发
+ 高可扩（随时水平拆分）
+ 高性能（保证用户体验和性能）

真正在公司中的实践：NoSQL+RDBMS一起使用才是最强的，阿里巴巴的架构演进

## 阿里巴巴演进分析

思考问题：这么多东西难道都是在一个数据库中的吗？

![image-20210303155728250](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303155728250.png)

大量公司做的都是相同的业务；（竞品协议）

随着这样的竞争，业务是越来越完善，然后对于开发者的要求也是越来越高

![image-20210303155940740](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303155940740.png)

```shell
# 1、商品的基本信息
    名称、价格、商家信息；
    关系型数据库就可以解决  MySQL/Oracle（淘宝早年就去IOE了 -王坚：推荐文章：阿里云的这群疯子）
    淘宝内部的MySQL不是大家用的MySQL
    
# 2、商品的描述、评论（文字比较多）
	文档型数据库，MongoDB
	
# 3、图片
	分布式文件系统 FastDFS
	- 淘宝自己的 TFS
	- Gooale的 GFS
	- Hadoop  HDFS
	- 阿里云的 oss
	
# 4、商品的关键字（搜索）
	- 搜索引擎 solr elasticsearch
	- ISerach 多隆
	
# 5、商品热门的波段信息
	- 内存数据库
	- Redis Tair Memache...
	
# 6、商品的交易，外部的支付接口
	- 三方应用
```

要知道一个简单的网页背后的技术一定不是大家所想的那么简单

大型互联网应用问题：

+ 数据类型较多
+ 数据源繁多，经常重构
+ 数据要改造，大面积改动

解决问题：

![image-20210303161553781](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303161553781.png)

![image-20210303162102006](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303162102006.png)

## NoSQL的四大分类

### KV键值对：

+ 新浪：**Redis**
+ 美团：Redis+Tair
+ 阿里、百度：Redis+memcache

### 文档型数据库（bson格式和json一样）：

+ **MongoDB**（一般必须要掌握）
  - MongoDB是一个基于分布式文件存储的数据库，C++编写，主要用来处理大量的文档
  - MongoDB是一个介于关系型数据库和非关系型数据库中间的产品。MongoDB是非关系型数据库中功能最丰富，最像关系型数据库的

+ ConthDB

### 列存储数据库

+ **Hbase**
+ 分布式文件系统

### 图形关系数据库

![image-20210303163455606](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303163455606.png)

+ 不是存储的图形，存储的是关系，比如：朋友社交网络，广告推荐
+ **Neo4j**，InfoGrid

> 四者对比

![image-20210303165612872](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303165612872.png)

# Redis入门

## 概述

> Redis是什么？

Redis（==Re==mote ==Di==ctionary ==S==erver )，即远程字典服务

是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C语言)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/数据库/103728)，并提供多种语言的API

![image-20210303173218881](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303173218881.png)

redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步

免费和开源。是当下最热门的NoSQL技术之一，也被人们称之为结构化数据库

> Redis能干什么

1、内存存储、持久化，内存中是断电即失，持久化很重要（RDB、AOF）

2、效率高，可以用于高速缓存

3、发布订阅系统

4、地图信息分析

5、计时器、计数器

> 特性

1、多样的数据类型

2、持久化

3、集群

4、事务

> 学习中需要用到的东西

1、官网：https://redis.io/

2、中文网：http://redis.cn/

3、下载地址：通过官网下载即可

![image-20210303174412268](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303174412268.png)

注意：windows在GitHub上下载（停更）

==Redis推荐都是在Linux服务器上搭建==

## Windows安装

1、下载安装包：https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100

2、下载完毕后得到压缩包，解压（redis十分小，只有5M）

![image-20210303190245090](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303190245090.png)

3、开启redis，双击运行服务即可

redis-server.exe

![image-20210303190528067](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303190528067.png)

4、使用redis客户端来连接redis

redis-cli.exe

![image-20210303190939250](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303190939250.png)

**位置：**http://redis.cn/topics/introduction

![image-20210303191438718](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303191438718.png)

## Linux安装

1、下载安装包	https://redis.io/

2、解压Redis的安装包！程序一般放在/opt目录下

![image-20210303193029865](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303193029865.png)

3、进入解压后的文件，可以看到redis的配置文件

![image-20210303194038398](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303194038398.png)

4、基本的环境安装

```shell
yum -y install gcc-c++
#验证
gcc -v

make

make install
```

![image-20210303194442476](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303194442476.png)

![image-20210303194508328](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303194508328.png)

5、redis的默认安装路径 `usr/local/bin`

![image-20210303194735848](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303194735848.png)

6、将redis配置文件复制到当前目录下

![image-20210303195007816](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303195007816.png)

7、redis默认不是后台启动的，修改配置文件

![image-20210303195252703](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303195252703.png)

8、启动redis服务

![image-20210303195504581](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303195504581.png)

9、使用redis-cli进行连接测试

![image-20210303195706539](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210303195706539.png)

10、查看redis进程是否开启

![image-20210304101427331](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304101427331.png)

11、如何关闭Redis服务？`shutdown`

![image-20210304101607981](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304101607981.png)

12、再次查看进程是否存在

![image-20210304101654918](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304101654918.png)

13、后面我们会使用单机多redis启动集群测试

## 测试性能

**redis-benchmark**是一个压力测试工具

官方自带的性能测试工具

redis-benchmark命令参数

![image-20210304102052974](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304102052974.png)

测试：

```shell
#测试：100个并发连接  100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![image-20210304102507171](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304102507171.png)

如何查看这些分析？

![image-20210304103138989](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304103138989.png)

![image-20210304103318350](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304103318350.png)

![image-20210304103349477](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304103349477.png)

## 基础知识

redis默认有16个数据库

![image-20210304103622301](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304103622301.png)

默认使用的是第0个

可以使用select进行切换数据库

```shell
127.0.0.1:6379> select 3	#切换数据库
OK
127.0.0.1:6379[3]> dbsize	#数据库大小
(integer) 0
```

![image-20210304103914131](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304103914131.png)

```shell
127.0.0.1:6379[3]> keys *  # 查看数据库所有的key
1) "name"
```

清除当前数据库`fkushdb`

```shell
127.0.0.1:6379[3]> flushdb
OK
127.0.0.1:6379[3]> keys *
(empty array)
```

清除全部数据库的内容`flushall`

```shell
127.0.0.1:6379[3]> FLUSHALL
OK
127.0.0.1:6379[3]> keys *
(empty array)
```

思考：为什么redis的端口是6379

> Redis是单线程的

明白Redis是很快的，官方表示，Redis是基于内存操作的，CPU不是Redis性能瓶颈，Redis的瓶颈是根据机器的内存和网络宽带，既然可以使用单线程来实现，就使用单线程！

Redis是C语言写的，官方提供的数据为100000+的QPS，完全这个不比同样是使用key-value的memcached差

**Redis为什么单线程还这么快**

1、误区1：高性能的服务器一定是多线程的

2、误区2：多线程（CPU上下文会切换）一定比单线程效率高

CPU>内存>硬盘

核心：redis是将所有的数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作），对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个CPU上的，在内存情况下这个就是最佳的方案

# 五大数据类型

> 官网文档

 Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作==数据库==、==缓存==和==消息中间件==. 它支持多种类型的数据结构，如 [字符串（strings）](http://redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://redis.cn/commands/geoadd.html) 索引半径查询. Redis 内置了 [复制（replication）](http://redis.cn/topics/replication.html)， [LUA脚本（Lua scripting）](http://redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://redis.cn/topics/lru-cache.html)， [事务（transactions）](http://redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://redis.cn/topics/sentinel.html) 和自动 [分区（Cluster）](http://redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）.

## Redis-key

```bash
# keys * 查看所有的key
# set key 存数据
# exists * 判断当前key是否存在
# move key 1 移除key到数据库1
# expire key 30 设置key的过期时间为30s
# ttl key 查看key的剩余时间
# type key 查看key的类型
127.0.0.1:6379> keys *	#查看所有的key
(empty array)
127.0.0.1:6379> set name lyy   #set key
OK
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set age 1
OK
127.0.0.1:6379> keys *
1) "name"
2) "age"
127.0.0.1:6379> EXISTS name		#判断当前的key是否存在
(integer) 1
127.0.0.1:6379> EXISTS name1
(integer) 0
127.0.0.1:6379> move name 1		#移除当前的key到数据库1
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name lyy
OK
127.0.0.1:6379> keys *
1) "name"
2) "age"
127.0.0.1:6379> get name
"lyy"
127.0.0.1:6379> EXPIRE name 10 	#设置key的过期时间，单位秒
(integer) 1
127.0.0.1:6379> ttl name		# 查看当前key的剩余时间
(integer) 7
127.0.0.1:6379> ttl name
(integer) 5
127.0.0.1:6379> ttl name
(integer) 2
127.0.0.1:6379> ttl name
(integer) 0
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> type name 		# 查看当前key的类型
string
127.0.0.1:6379> type age
string
```

遇到不会的命令可以在官网查看帮助文档

![image-20210304111230227](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304111230227.png)

## String（字符串）

```shell
# append key value	追加字符串，如果当前key不存在，就相当于set key
# strlen key	查看value长度
# incr key 	自增1
# decr key	自减1
# incrby key 5  指定自增步长为5
# decrby key 5  指定自减步长为5
# getrange key 0 -1 	截取全部字符串（指定字符串）
# setrange key 6 value	替换指定位置的字符串
# setex key 30 value	设置key的过期时间为30s
# setnx key value		不存在即创建，存在即创建失败
# mset	同时设置多个值
# mget	同时获取多个值
# msetnx key value key value	具有原子性，要么一起成功要么一起失败
# mset user：1：name zhangsan user：1：age 18 	对象存储   设置user：1的名字为张三，年龄为18
# getset key value		#如果key存在则先获取原来的值再set新的值，如果key不存在则返回nil空值
#######################################################################################
127.0.0.1:6379> set key1 v1
OK
127.0.0.1:6379> get key1
"v1"
127.0.0.1:6379> EXISTS key1
(integer) 1
127.0.0.1:6379> APPEND key1 "hello"		# 追加字符串，如果当前key不存在，就相当于set key
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
127.0.0.1:6379> STRLEN key1		# 查看value的长度
(integer) 10
#######################################################################################
# i++
# 步长 i+=
127.0.0.1:6379> set views 0		#初始浏览量为0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views		# 自增1  浏览量+1
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> decr views		# 自减1  浏览量-1
(integer) 1
127.0.0.1:6379> decr views
(integer) 0
127.0.0.1:6379> decr views
(integer) -1
127.0.0.1:6379> get views
"-1"
127.0.0.1:6379> INCRBY views 10		#可以设置步长，指定增量
(integer) 9	
127.0.0.1:6379> DECRBY views 5		#指定减量
(integer) 4
#######################################################################################
# 字符串范围 range
127.0.0.1:6379> set key1 hello,lyy  	# 设置key1的值
OK
127.0.0.1:6379> get key1
"hello,lyy"
127.0.0.1:6379> GETRANGE key1 0 3		# 截取字符串 [0,3]
"hell"
127.0.0.1:6379> GETRANGE key1 0 -1		# 获取全部的字符串 和 get key 一样
"hello,lyy"

# 替换
127.0.0.1:6379> get keys 
(nil)
127.0.0.1:6379> get key2
"abcd"
127.0.0.1:6379> SETRANGE key2 1 xx		# 替换指定位置开始的字符串
(integer) 4
127.0.0.1:6379> get key2
"axxd"
127.0.0.1:6379> 
#######################################################################################
# setex（set with expire）# 设置过期时间
# setnx（set if not exist）# 不存在设置（在分布式锁中会常常使用）
127.0.0.1:6379> setex key3 30 hello  # 设置key3的值为hello，30秒后过期
OK
127.0.0.1:6379> ttl key3
(integer) 21
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> SETNX mykey redis	# 如果mykey不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "key2"
2) "key1"
3) "mykey"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> SETNX mykey mongodb	#如果mykey存在，创建失败
(integer) 0
127.0.0.1:6379> get mykey
"redis"
#######################################################################################
mset
mget

127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3	#同时设置多个值
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
127.0.0.1:6379> mget k1 k2 k3	#同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4	#msetnx是一个原子性操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379> get k4
(nil)

# 对象
set user：1 {name：zhangsan，age：3} #设置一个user：1对象值为json字符来保存一个对象

#这里的key是一个巧妙的设计：user:{id}:{filed},【user：1：name】是一个key，来看成一个整体，如此设计在Redis中是完全ok的

127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"
#######################################################################################
getset # 先get然后再set
127.0.0.1:6379> getset db redis	#如果不存在值，则返回nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb	#如果存在值，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
#######################################################################################
```

String类似的使用场景：value除了是字符串还可以是数字

+ 计数器
+ 统计多单位的数量
+ 粉丝数
+ 对象缓存存储

## List（列表）

基本的数据类型，列表

![image-20210304143508042](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210304143508042.png)

在redis里面，我们可以把list玩成，栈、队列、阻塞队列

**所有的list命令都是用l开头的，不区分大小写命令**

```bash
# lpush key value	将一个或多个值插入到列表头部（头部指的是左边left）
# lrange key 0 -1 	获取key的值
# rpush key value	讲一个或多个值插入到列表尾部（右边）
# lpop key 	移除列表第一个元素
# rpop key	移除列表最后一个元素
# lindex key 0/1/2	通过下标获取列表中的值
# llen key	获取列表的长度
# lrem key count value	移除list集合中指定个数的value
# ltrim key start stop 	通过下标截取指定的长度，list已经改变，剩下的是前面截取的部分
# lpoplpush	源列表 目标列表	移除列表最后一个元素到一个新的列表
# lset key index value		将列表中指定下标的值替换为另一个值  key不存在会报错，index对应的元素不存在会报错
# linsert key before/after hello lyy	将lyy放在hello的前面或后面
#######################################################################################
127.0.0.1:6379> LPUSH list one		# 将一个值或者多个值，插入到列表头部
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LRANGE list 0-1		# 获取list值
(error) ERR wrong number of arguments for 'lrange' command
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LRANGE list 0 1		# 通过区间获取具体的值
1) "three"
2) "two"
127.0.0.1:6379> RPUSH list right	# 将一个值或者多个值，插入到列表右边
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"
#######################################################################################
LPOP 
RPOP
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"
127.0.0.1:6379> LPOP list 	# 移除list的第一个元素（队头）
"three"
127.0.0.1:6379> RPOP list	# 移除list最后一个元素（队尾）
"right"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
#######################################################################################
Lindex  下标
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> LINDEX list 1		# 通过下标获得list中的某一个值
"one"
127.0.0.1:6379> LINDEX list 0
"two"
#######################################################################################
Llen # 获取长度
127.0.0.1:6379> LPUSH list one
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LLEN list		#返回列表的长度
(integer) 3
#######################################################################################
移除指定的值
Lrem
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> LREM list 1 three	# 移除list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> LREM list 2 three
(integer) 2
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
#######################################################################################
trim 修剪
127.0.0.1:6379> LPUSH mylist hello
(integer) 1
127.0.0.1:6379> LPUSH mylist hello1
(integer) 2
127.0.0.1:6379> LPUSH mylist hello2
(integer) 3
127.0.0.1:6379> LPUSH mylist hello3
(integer) 4
127.0.0.1:6379> LTRIM mylist 1 2	# 通过下标截取指定的长度，这个list已经被改变，只剩下截取的元素
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello2"
2) "hello1"
#######################################################################################
rpoplpush  #移除列表的最后一个元素到新的列表中
127.0.0.1:6379> RPUSH mylist hello
(integer) 1
127.0.0.1:6379> RPUSH mylist hello1
(integer) 2
127.0.0.1:6379> RPUSH mylist hello2
(integer) 3
127.0.0.1:6379> rpoplpush mylist myotherlist  # 移除最后一个元素到新的list
"hello2"
127.0.0.1:6379> LRANGE mylist 0 -1		# 查看原来列表
1) "hello"
2) "hello1"
127.0.0.1:6379> LRANGE myotherlist 0 -1		# 查看目标列表中，确实存在该值
1) "hello2"

#######################################################################################
lset 将列表中指定下标的值替换为另外一个值
127.0.0.1:6379> EXISTS list		# 判断列表是否存在
(integer) 0
127.0.0.1:6379> LSET list 0 item		# 如果不存在列表更新会报错
(error) ERR no such key
127.0.0.1:6379> lpush list v1
(integer) 1
127.0.0.1:6379> LRANGE list 0 0
1) "v1"
127.0.0.1:6379> LSET list 0 item	# 如果存在，更新当前下标的值
OK
127.0.0.1:6379> LRANGE list 0 0
1) "item"
127.0.0.1:6379> LRANGE list 1 v2	# 不存在，则会报错
(error) ERR value is not an integer or out of range
#######################################################################################
linsert		# 将某个具体的value插入到某个元素的前面或者后面
127.0.0.1:6379> RPUSH list hello
(integer) 1
127.0.0.1:6379> RPUSH list hello1
(integer) 2
127.0.0.1:6379> LINSERT list before hello1 other	#插入hello1前面
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1
1) "hello"
2) "other"
3) "hello1"
127.0.0.1:6379> LINSERT list after hello1 new		# 插入hello1后面
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "hello"
2) "other"
3) "hello1"
4) "new"

#######################################################################################
```

> 小结

+ 实际上是一个链表，before Node after，left，right都可以插入值
+ 如果key不存在，创建新的链表
+ 如果key存在，新增内容
+ 如果移除了所有值，空链表，也代表不存在
+ 在两边插入或者改动值，效率最高，中间元素，相对来说效率会低一点

消息排队  消息队列 （Lpush  Rpop）栈（Lpush  Lpop）

## Set（集合）

set中的值是不能重复的

```bash
# sadd key value  set集合中添加元素
# smembers key	查看set的所有值
# sismember key value	查看某个值是不是在set集合中
# scard key		获取set集合中元素的个数
# srem key value		移除set集合中指定的元素
# srandmember key count		随机抽出指定个数的元素/随机抽取元素
# spop key		随机删除集合中的元素
# smove 源集合 目的集合 key	将集合中的指定值移到另外一个集合
# sdiff k1 k2	两个集合的差集
# sinter k1 k2  两个集合的交集
# sunion k1 k2  两个集合的并集
#######################################################################################
sadd 添加元素
smembers  查看值
sismember  判断值是否存在set中
127.0.0.1:6379> sadd myset hello   	# set集合中添加元素
(integer) 1
127.0.0.1:6379> sadd myset lyy
(integer) 1
127.0.0.1:6379> sadd myset redis
(integer) 1
127.0.0.1:6379> SMEMBERS myset		# 查看set的所有值
1) "hello"
2) "redis"
3) "lyy"
127.0.0.1:6379> SISMEMBER myset hello	# 判断某一个值是不是在set集合中
(integer) 1
127.0.0.1:6379> SISMEMBER myset ee
(integer) 0
#######################################################################################
scard 获取元素个数
127.0.0.1:6379> SCARD myset		# 获取set集合中的内容元素个数
(integer) 3
#######################################################################################
srem  移除
127.0.0.1:6379> srem myset hello	# 移除set中的指定元素
(integer) 1
127.0.0.1:6379> scard myset
(integer) 3
127.0.0.1:6379> SMEMBERS myset
1) "dmh"
2) "redis"
3) "lyy"
#######################################################################################
set 无序不重复集合   抽随机
127.0.0.1:6379> SMEMBERS myset
1) "dmh"
2) "redis"
3) "lyy"
127.0.0.1:6379> SRANDMEMBER myset	# 随机抽选出一个元素
"redis"
127.0.0.1:6379> SRANDMEMBER myset
"lyy"
127.0.0.1:6379> SRANDMEMBER myset
"lyy"
127.0.0.1:6379> SRANDMEMBER myset 2		# 随机抽选出指定个数的元素
1) "lyy"
2) "redis"
127.0.0.1:6379> SRANDMEMBER myset
"dmh"
#######################################################################################
删除指定的key，随机删除key
127.0.0.1:6379> SMEMBERS myset
1) "dmh"
2) "redis"
3) "lyy"
127.0.0.1:6379> spop myset		# 随机删除一些set集合中的元素
"lyy"
127.0.0.1:6379> spop myset
"dmh"
127.0.0.1:6379> SMEMBERS myset
1) "redis"
#######################################################################################
将一个指定的值，移动到另外一个set集合
127.0.0.1:6379> sadd myset hello
(integer) 1
127.0.0.1:6379> sadd myset world
(integer) 1
127.0.0.1:6379> sadd myset lyy
(integer) 1
127.0.0.1:6379> sadd myset2 set2
(integer) 1
127.0.0.1:6379> smove myset myset2 lyy  # 将一个指定的值，移动到另外一个set集合
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "hello"
127.0.0.1:6379> SMEMBERS myset2
1) "lyy"
2) "set2"
#######################################################################################
微博，B站，共同关注（交集）
数字集合类
 - 差集   SDIFF
 - 交集	sinter
 - 并集	sunion
 127.0.0.1:6379> sadd key1 a
(integer) 1
127.0.0.1:6379> sadd key1 b
(integer) 1
127.0.0.1:6379> sadd key1 c
(integer) 1
127.0.0.1:6379> sadd key2 c
(integer) 1
127.0.0.1:6379> sadd key2 d
(integer) 1
127.0.0.1:6379> sadd key2 e
(integer) 1
127.0.0.1:6379> sdiff key1 key2		# 差集  第一个集合中独有的元素
1) "a"
2) "b"
127.0.0.1:6379> sinter key1 key2	# 交集	共同好友可以这样实现
1) "c"
127.0.0.1:6379> sunion key1 key2	# 并集
1) "c"
2) "b"
3) "a"
4) "e"
5) "d"
#######################################################################################
```

微博，A用户将所有关注的人放在一个set集合中，将粉丝也放在一个集合中

共同关注，共同爱好，二度好友（六度分割理论）推荐好友

## Hash（哈希）

Map集合，key-<key-value>    key=map   这时候这个值是一个map集合  本质和string类型没有太大区别，还是一个简单的key-value

set myhash field lyy

```shell
# hset key field value	set一个具体key-value
# hget key field	获取一个字段值
# hmset key field1 value field2 value  set多个key-value
# hmget key field field1	获取多个字段值
# hgetall key	获取全部数据
# hdel key field   删除hash指定key字段
# hlen key	获取hash表中字段的数量
# hexixts key field		判断hash表中指定的字段是否存在
# hkeys key		获取所有字段
# hvals key		获取所有value
# hincrby key field 5	指定增量为5
# hincrby key field -5	减5
# hsetnx key field value	不存在则创建，存在则报错
#######################################################################################
127.0.0.1:6379> hset myhash field1 kuangshen	# set一个具体key-value
(integer) 1
127.0.0.1:6379> hget myhash field1	# 获取一个字段值
"kuangshen"
127.0.0.1:6379> hmset myhash field1 hello field2 world  # set 多个key-value
OK
127.0.0.1:6379> hmget myhash field1 field2		# 获取多个字段值
1) "hello"
2) "world"
127.0.0.1:6379> hgetall myhash	# 获取全部的数据
1) "field1"
2) "hello"
3) "field2"
4) "world"
127.0.0.1:6379> hdel myhash field1		# 删除hash指定key字段，对应的value值也就消失了
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
#######################################################################################
hlen
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
3) "field1"
4) "hello"
127.0.0.1:6379> hlen myhash		#获取hash表的字段数量
(integer) 2
#######################################################################################
hexists
127.0.0.1:6379> HEXISTS myhash field1	# 判断hash中指定字段是否存在
(integer) 1
127.0.0.1:6379> HEXISTS myhash field3
(integer) 0
#######################################################################################
# 只获得所有所有field
# 只获得所有value
127.0.0.1:6379> hkeys myhash	# 只获得所有所有field
1) "field2"
2) "field1"
127.0.0.1:6379> hvals myhash	# 只获得所有value
1) "world"
2) "hello"
#######################################################################################
incr
decr
127.0.0.1:6379> hset myhash field3 5		# 指定增量
(integer) 1
127.0.0.1:6379> HINCRBY myhash field3 1
(integer) 6
127.0.0.1:6379> HINCRBY myhash field3 -1
(integer) 5
127.0.0.1:6379> hsetnx myhash field4 hello	# 如果不存在则可以设置
(integer) 1
127.0.0.1:6379> hsetnx myhash field4 lyy	# 如果存在则不能设置
(integer) 0
#######################################################################################
```

hash变更的数据user name age，尤其是用户信息之类的，经常变动的信息  hash更适合于对象的存储，string更适合字符串存储

```shell
127.0.0.1:6379> hset user:1 name lyy
(integer) 1
127.0.0.1:6379> hget user:1 name
"lyy"
```

## Zset(有序集合)

在set的基础上增加了一个值，set k1 v1 zset k1 score1 v1

```shell
# zadd myset 1 one	添加一个值
# zrange myset 0 -1 	获取全部元素
# zrangebyscore salary -inf +inf 	从小到大显示全部用户
# zrangebyscore salary -inf +inf withscores 从小到大显示全部用户，并且附带成绩
# zrevrange salary 0 -1 withscores		从大到小显示全部用户
# zrangebyscores salary -inf 5000 withscores	薪资小于5000的员工升序排序
# zrem salary xiaohong  	移除有序集合中的指定元素
# zcard salary	获取有序集合中的个数
# zcount myset 1 3	获取指定区间的成员数量
#######################################################################################
# zadd 添加
#zrem 移除
#zrange 获取集合范围
#zcount 数量
#zrangebyscore 从小到大排序
#zrevrange	从大到小排序
127.0.0.1:6379> zadd myset 1 one	# 添加一个值
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 three	# 添加多个值
(integer) 1
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
#######################################################################################
排序如何实现
127.0.0.1:6379> zadd salary 2500 xiaohong		# 添加三个用户
(integer) 1
127.0.0.1:6379> zadd salary 5000 zhangsan
(integer) 1
127.0.0.1:6379> zadd salary 500 lyy
(integer) 1
# ZRANGEBYSCORE key min max
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf	# 显示全部的用户，从小到大
1) "lyy"
2) "xiaohong"
3) "zhangsan"
127.0.0.1:6379> ZREVRANGE salary 0 -1	# 从大到小进行排序
1) "zhangsan"
2) "lyy"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores	# 显示全部的用户，从小到大 并且附带成绩
1) "lyy"
2) "500"
3) "xiaohong"
4) "2500"
5) "zhangsan"
6) "5000"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 2500 withscores 	#显示工资小于2500员工的升序排序
1) "lyy"
2) "500"
3) "xiaohong"
4) "2500"
#######################################################################################
# 移除rem中的元素
127.0.0.1:6379> ZRANGE salary 0 -1
1) "lyy"
2) "xiaohong"
3) "zhangsan"
127.0.0.1:6379> zrem salary xiaohong	# 移除有序集合中的指定元素
(integer) 1
127.0.0.1:6379> zrange salary 0 -1
1) "lyy"
2) "zhangsan"
127.0.0.1:6379> ZCARD salary	# 获取有序集合中的个数
(integer) 2
#######################################################################################
127.0.0.1:6379> zadd myset 1 hello
(integer) 1
127.0.0.1:6379> zadd myset 2 world 3 lyy
(integer) 2
127.0.0.1:6379> ZCOUNT myset 1 3	# 获取指定区间的成员数量
(integer) 3
127.0.0.1:6379> ZCOUNT myset 1 2
(integer) 2
#######################################################################################
```

其余api，在官方文档中查找

http://redis.cn/commands.html#sorted_set

![image-20210308112051065](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210308112051065.png)

案例思路：set 排序 存储班级成绩表，工资表排序

普通消息：1、重要消息；2、带权重进行判断

排行榜应用实现，取Top N测试

# 三种特殊数据类型

## geospatial 地理位置

Redis的Geo在Redis3.2版本就推出了，这个功能可以推算地理位置的信息，两地之间的距离，方圆几里的人

可以查询一些测试数据

只有六个命令

![image-20210308142823975](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210308142823975.png)

> #### geoadd

```shell
# 添加地理位置
#规则：两级无法直接添加，我们一般会下载城市数据，直接通过java程序一次性导入

#经度纬度写反
127.0.0.1:6379> geoadd china:city 40.22 116.23 beijing
(error) ERR invalid longitude,latitude pair 40.220000,116.230000

# 参数 key 值（纬度、经度、名称）
# 有效的经度从-180度到180度。
# 有效的纬度从-85.05112878度到85.05112878度
127.0.0.1:6379> geoadd china:city 116.23 40.22 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.48 31.40 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.54 29.40 chongqing 113.88 22.55 shenzhen
(integer) 2
127.0.0.1:6379> geoadd china:city 120.21 30.20 hangzhou 108.93 34.23 xian
(integer) 2
```

> #### geopos

```shell
127.0.0.1:6379> GEOPOS china:city beijing   # 获取指定的城市的经度和纬度
1) 1) "116.23000055551528931"
   2) "40.2200010338739844"
127.0.0.1:6379> GEOPOS china:city chongqing beijing
1) 1) "106.54000014066696167"
   2) "29.39999880018641676"
2) 1) "116.23000055551528931"
   2) "40.2200010338739844"
```

> #### geodist

两个人之间的距离

单位：默认是米

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺

```shell
127.0.0.1:6379> GEODIST china:city beijing chongqing km   # 查看北京到重庆的直线距离
"1491.6716"
127.0.0.1:6379> GEODIST china:city beijing shanghai
"1088785.4302"
127.0.0.1:6379> GEODIST china:city beijing shanghai km		# 查看北京到上海的直线距离
"1088.7854"
```

> #### georadius已给定的经纬度为中心，找出某一半径内的元素

我附近的人？（获得所有附近的人的地址，定位）通过半径来查询

获得指定数量的人，200个人

所有数据都应该录入：china:city    才会让结果更加清晰

```shell
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km  # 以110,30这个经纬度为中心，寻找方圆1000km内的城市
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km
1) "chongqing"
2) "xian"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withcoord  # 显示他人的定位信息
1) 1) "chongqing"
   2) 1) "106.54000014066696167"
      2) "29.39999880018641676"
2) 1) "xian"
   2) 1) "108.92999857664108276"
      2) "34.23000121926852302"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist  # 显示到中心距离的位置
1) 1) "chongqing"
   2) "340.8679"
2) 1) "xian"
   2) "481.1540"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist withcoord count 1  # 筛选出指定的结果
1) 1) "chongqing"
   2) "340.8679"
   3) 1) "106.54000014066696167"
      2) "29.39999880018641676"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist withcoord count 2
1) 1) "chongqing"
   2) "340.8679"
   3) 1) "106.54000014066696167"
      2) "29.39999880018641676"
2) 1) "xian"
   2) "481.1540"
   3) 1) "108.92999857664108276"
      2) "34.23000121926852302"
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist withcoord count 3
1) 1) "chongqing"
   2) "340.8679"
   3) 1) "106.54000014066696167"
      2) "29.39999880018641676"
2) 1) "xian"
   2) "481.1540"
   3) 1) "108.92999857664108276"
      2) "34.23000121926852302"
```

> #### georadiusbymember

```shell
# 找出位于指定元素周围的其他元素
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km	
1) "beijing"
2) "xian"
127.0.0.1:6379> GEORADIUSBYMEMBER china:city shanghai 400 km
1) "hangzhou"
2) "shanghai"

```

> #### GEOHASH 命令 - 返回一个或多个位置元素的 Geohash 表示

该命令将返回11个字符的Geohash字符串

```shell
# 将二维的经纬度转换为一维的字符串，如果两个字符串距离越接近，那么距离越接近
127.0.0.1:6379> GEOHASH china:city beijing chongqing
1) "wx4sucu47r0"
2) "wm5z22h53v0"
```

> #### GEO底层的实现原理其实就是Zset，我们可以使用Zset命令来操作geo

```shell
127.0.0.1:6379> ZRANGE china:city 0 -1  # 查看地图中全部元素
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
127.0.0.1:6379> zrem china:city beijing	# 移除指定元素
(integer) 1
127.0.0.1:6379> ZRANGE china:city 0 -1
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
```

## Hyperloglog

> 什么是基数？

A {1,3,5,7,8,7}	B{1,3,5,7,8}

基数（一个集合内不重复的数）=5  可以接受误差

> 简介

Redis2.8.9版本就更新了Hyperloglog数据结构

Redis Hyperloglog 基数统计的算法

**优点**

+ 占用的内存是固定的，2^64不同元素的技术，只需要费12KB内存，如果要从内存角度比较的话，Hyperloglog首选

**网页的uv （一个人访问一个网站多次，但是还是算作一个人）**

传统 方式，set（set集合中元素不允许重复）保存用户的id，就可以统计set中的元素数量作为标准判断

这个方式如果保存大量的用户id，就会比较麻烦。我们的目的是为了计数，而不是保存用户id

0.81%错误率，统计uv任务可以忽略不计的

> 测试使用

```shell
127.0.0.1:6379> PFADD mykey a b c	 # 创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> pfadd mykey2 c d e f	# 创建mykey2
(integer) 1
127.0.0.1:6379> PFCOUNT mykey	# 统计mykey元素的基数数量
(integer) 3
127.0.0.1:6379> PFCOUNT mykey2
(integer) 4
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2	# 合并两组 mykey+mykey2 => mykey3 并集
OK
127.0.0.1:6379> PFCOUNT mykey3	# 查看并集的数量
(integer) 6

```

如果允许容错，那么一定可以使用Hyperloglog

如果允许容错，就使用set或者自己的数据类型即可

## Bitmaps

> 位存储

统计用户信息，b站：活跃、不活跃      登录、未登录        打卡    两个状态的，都可以使用bitmaps

统计疫情感染人数：0 1 0 1

bitmaps 位图，数据结构   都是操作二进制位来进行记录，就只有0和1两个状态

365天=365bit  1字节=8bit   46个字节左右

> 测试

使用bitmap来记录周一到周日的打卡

周一：1（打卡）   周二：0（未打卡）

![image-20210308160121353](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210308160121353.png)

查看某一天是否有打卡

```shell
127.0.0.1:6379> getbit sign 3 	# 查看周三打卡情况
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0

```

统计操作,统计打卡天数

```shell
127.0.0.1:6379> BITCOUNT sign  # 统计这周的打卡记录
(integer) 3
```

# 事务

Redis事务本质：一组命令的集合！一个事务中的所有命令都会被序列化，在事务执行的过程中，会按照顺序执行

一次性、顺序性、排他性！执行一系列的命令

==redis事务没有隔离级别的概念==

所有的命令在事务中，并没有直接被执行，只有发起执行命令的时候才会执行！Exec

==Reid单条命令是保证原子性的，但是事务不保证原子性==。（要么同时成功，要么同时失败，原子性）
Mysql：ACID

**redis的事务：**

+ 开启事务（multi）
+ 命令入队（）
+ 执行事务（exec）

锁：Redis可以实现乐观锁

> 正常执行事务

```shell
127.0.0.1:6379> multi		# 开启事务
OK
# 命令入队
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> exec 	# 执行事务
1) OK
2) OK
3) "v2"
4) OK
```

> 放弃事务

```shell
127.0.0.1:6379> multi		# 开启事务
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k4 v4
QUEUED
127.0.0.1:6379(TX)> DISCARD		# 取消事务
OK
127.0.0.1:6379> get k4		# 事务队列中命令都不会被执行
(nil)
```

> 编译型异常（代码有问题，命令有误）事务中所有的命令都不会被执行

```shell
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> getset k3		# 错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> set k4 v4 
QUEUED
127.0.0.1:6379(TX)> set k5 v5
QUEUED
127.0.0.1:6379(TX)> exec		# 执行事务报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k5		# 所有的命令都不会被执行
(nil)
```

> 运行时异常，如果事务队列中存在语法性，那么执行命令的时候，其他命令是可以正常执行的，错误命令抛出异常

```shell
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> INCR k1		# 会执行的时候失败
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> exec  
1) (error) ERR value is not an integer or out of range 	# 虽然第一条命令报错了，但是依旧正常执行成功了
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
```

> 监控   Watch（面试常问）watch本身就是乐观锁
>
> 与CAS的区别：http://c.biancheng.net/view/4544.html

**悲观锁：**

+ 很悲观，认为什么时候都会出问题，无论做什么都会加锁

**乐观锁：**

+ 很乐观，认为什么时候都不会出现问题，所以不会上锁！更新数据的时候去判断一下在此期间是否有人修改过这个数据，version
+ 获取version
+ 更新的时候比较version（一般是在数据表中加上一个数据版本号 version 字段，表示数据被修改的次数。当数据被修改时，version 值会+1。当线程A要更新数据值时，在读取数据的同时也会读取 version 值，在提交更新时，若刚才读取到的 version 值与当前数据库中的 version 值相等时才更新，否则重试更新操作，直到更新成功。）

> Redis测监视测试

正常执行成功

```shell
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money 	# 监视money对象
OK
127.0.0.1:6379> MULTI	# 事务正常结束，数据期间没有发生变动，这个时候就正常执行成功
OK
127.0.0.1:6379(TX)> DECRBY money 20
QUEUED
127.0.0.1:6379(TX)> INCRBY out 20
QUEUED
127.0.0.1:6379(TX)> exec
1) (integer) 80
2) (integer) 20

```

测试多线程修改值，使用watch可以当做redis的乐观锁操作

```shell
127.0.0.1:6379> WATCH money		# 监视 money
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> DECRBY money 10
QUEUED
127.0.0.1:6379(TX)> INCRBY out 10
QUEUED
127.0.0.1:6379(TX)> exec	# 执行之前，另外一个线程修改了money的值，会导致事务执行失败
(nil)

```

``unwatch是在watch和multi之间使用的，discard是在multi和exec之间使用的``

unwatch参考文档：http://doc.redisfans.com/transaction/unwatch.html

如果修改失败，获取最新的值就好

![image-20210309112541826](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309112541826.png)

# redis.conf详解

启动的时候，通过配置文件来启动

> 单位

![image-20210309121936728](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309121936728.png)

1、配置文件 unit单位 对大小写不敏感

> 包含

![image-20210309122116828](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309122116828.png)

将多个配置文件配置过来

> 网络

```shell
bind 127.0.0.1  # 绑定的ip，仅本地访问，若想要其他都能访问可以通配或者指定ip
protected-mode yes  # 保护模式
port 6379	# 端口设置
```

> 通用配置（GENERAL）

```shell
daemonize yes	# 以守护进程的方式运行，默认是no，手动开启

pidfile /var/run/redis_6379.pid 	# 如果以后台方式运行，我们就需要指定一个pid进程文件

# 日志
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)生产环境使用
# warning (only very important / critical messages are logged)
loglevel notic

logfile "" 	# 日志的文件位置名，如果是空则是标准输出

databases 16	# 数据库的数量，默认是16个数据库

always-show-logo no		# 是否显示LOGO
```

> 快照

持久化，在规定的时间内，执行了多少次操作，则会持久化到文件   .rdb     .rof

redis是内存数据库，如果没有持久化，那么数据断电即失

```shell
# 如果3600秒内，如果至少有1个key进行了修改，我们就进行持久化操作
save 3600 1
# 如果300秒内，如果至少有100个key进行了修改，我们就进行持久化操作
save 300 100
# 如果60秒内，如果至少有10000个key进行了修改，我们就进行持久化操作
save 60 10000
# 我们之后学习持久化，会自己定义这个测试


stop-writes-on-bgsave-error yes	# 持久化如果出错，是否还需要继续工作

rdbcompression yes	# 是否压缩rdb文件，需要消耗一些cpu资源

rdbchecksum yes		# 保存rdb文件的时候，进行错误的检查校验

dir ./		# rdb文件保存的目录
```

> REPLICATION 复制，后面讲解主从复制进行讲解

> SECURITY  安全

```shell
# redis默认是没有密码的
127.0.0.1:6379> CONFIG GET requirepass	# 查看密码
1) "requirepass"
2) ""
# 设置密码的两种方法
# 一、在配置文件中设置，在SECURITY模块下插入 requirepass 123456
# 二、直接用命令行设置
	CONFIG SET requirepass "123456"
	# 重新测试获取redis密码看是否有权限，没有权限需要用auth进行登录
	127.0.0.1:6379> AUTH 123456
OK
```

> 限制CLIENTS

```bash
maxclients 10000	# 设置能连接上redis的最大客户端数量

maxmemory <bytes>	# redis 配置最大的内存容量

maxmemory-policy noeviction		# 内存到达上限之后的处理策略

1、volatile-lru：只对设置了过期时间的key进行LRU（默认值） 
2、allkeys-lru ： 删除lru算法的key   
3、volatile-random：随机删除即将过期key   
4、allkeys-random：随机删除   
5、volatile-ttl ： 删除即将过期的   
6、noeviction ： 永不过期，返回错误
```

> APPEND ONLY MODE模式  aof配置

```shell
appendonly no	# 默认是不开启aof模式的，默认是使用rdb方式持久化的，在不大部分所有的情况下，rdb完全够用
appendfilename "appendonly.aof"		# 持久化的文件的名字   

# appendfsync always	# 每次修改都会sync 速度较慢，消耗性能
appendfsync everysec	# 每秒执行一次 sync  可能会丢失这1s的数据
# appendfsync no		# 不执行 sync 这个时候操作系统自己同步数据，速度是最快的
```

# Redis持久化

Redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。所以Redis提供了持久化功能

## RDB（Redis DataBase）

> 什么是RDB

在主从复制中，rdb就是备用，在从机上面

![image-20210309143324122](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309143324122.png)

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。默认的就是RDB，一般情况下不需要修改这个配置

有时候在生产环境会将这个文件进行备份

==rdb保存的文件是 dump.rdb== 都是在配置文件中快照模块里进行配置

![image-20210309144322004](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309144322004.png)

![image-20210309144525356](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309144525356.png)

> 触发机制

1、save的规则满足的情况下，会自动触发rdb规则

2、执行flushall命令，也会触发我们的rdb规则

3、退出redis，也会产生rdb文件

备份就自动生成一个dump.rdb文件

![image-20210309152907125](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309152907125.png)

> 如何恢复rdb文件

1、只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候会自动检查dump.rdb恢复其中的数据

2、查看需要存放的位置

```shell
127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/bin"		# 如果在这个目录下存在dump.rdb文件，启动就会自动恢复其中的数据
```

**优点：**

1、适合大规模的数据恢复

2、对数据的完整性要求不高

**缺点：**

1、需要一定的时间间隔进行操作，如果redis意外宕机，最后一次修改的数据就丢失了

2、fork子进程时，会占用一定的内存空间

## AOF（Append Only File）

将我们的所有命令都记录下来，history，恢复时把这个文件全部再执行一遍

> 是什么

![image-20210309153921465](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309153921465.png)

以日志的形式来记录每个写操作，将redis执行过的所有指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

==Aof保存的是appendonly.aof文件==

> append

![image-20210309154341092](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309154341092.png)

默认是不开启的，需要手动进行配置，仅需将appendonly改为yes就开启了aof

![image-20210309154410221](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309154410221.png)

重启redis即可以生效

![image-20210309154705351](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309154705351.png)

如果aof文件有错误，redis启动失败无法连接

![image-20210309155130110](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309155130110.png)

需要修复aof文件，redis提供了一个工具：`redis-check-aof`

![image-20210309155337112](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309155337112.png)

如果文件正常，重启直接恢复  ==注意：这里只是将被破坏的key直接清理掉了==

> 重写规则说明

aof默认就是文件的无限追加，文件会越来越大

![image-20210309160257450](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309160257450.png)

如果aof文件大于64m，太大了，会fork一个新的进程来将文件进行重写

> 优点和缺点

```shell
appendonly no	# 默认是不开启aof模式的，默认是使用rdb方式持久化的，在不大部分所有的情况下，rdb完全够用
appendfilename "appendonly.aof"		# 持久化的文件的名字   

# appendfsync always	# 每次修改都会sync 速度较慢，消耗性能
appendfsync everysec	# 每秒执行一次 sync  可能会丢失这1s的数据
# appendfsync no		# 不执行 sync 这个时候操作系统自己同步数据，速度是最快的

#rewrite
```

优点：

+ 每一次修改都同步，文件的完整性会更好
+ 每秒同步一次，可能会丢失一秒的数据
+ 从不同步，效率最高

缺点：

+ 相对于数据文件来说，aof远远大于rdb，修复的速度也比rdb慢
+ Aof运行效率也要比rdb慢，所以redis默认的配置是rdb持久化

**扩展**

1、RDB持久化方式能够在指定的时间间隔内对你的数据进行快照存储

2、AOF持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。

3、只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化

4、同时开启两种持久化方式

+ 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存

  的数据集要比RDB文件保存的数据集要完整。

+ RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢?作者建议不要，

  因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，

  留着作为一个万一的手段。

5、性能建议

+ 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留

  save 900 1这条规则。

+ 如果Enable AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文

  件就可以了，代价一是带来了持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造

  成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值

  64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。

+ 如果不Enable AOF，仅靠Master-Slave Repllcation实现高可用性也可以，能省掉一大笔IO，也减少了

  rewrite时带来的系统波动。代价是如果Master/Slave同时倒掉（断电），会丢失十几分钟的数据，启动脚本也要比较

  两个Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。

# Redis发布订阅

Redis发布订阅（pub/sub）是一种==消息通信模式==：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。微信、微博、关注系统

Redis 客户端可以订阅任意数量的频道。

订阅/发布消息图：

第一个：消息发送者；第二个：频道；第三个：消息订阅者

![image-20210309162231834](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309162231834.png)

下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![image-20210309162705019](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309162705019.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![image-20210309162727098](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309162727098.png)

> 命令

这些命令被广泛用于构建即时通信应用，比如网络聊天室(chatroom)和实时广播、实时提醒等。

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [PSUBSCRIBE pattern [pattern ...\]](https://www.runoob.com/redis/pub-sub-psubscribe.html) <br />订阅一个或多个符合给定模式的频道。 |
| 2    | [PUBSUB subcommand [argument [argument ...\]]](https://www.runoob.com/redis/pub-sub-pubsub.html) <br />查看订阅与发布系统状态。 |
| 3    | [PUBLISH channel message](https://www.runoob.com/redis/pub-sub-publish.html) <br />将信息发送到指定的频道。 |
| 4    | [PUNSUBSCRIBE [pattern [pattern ...\]]](https://www.runoob.com/redis/pub-sub-punsubscribe.html) <br />退订所有给定模式的频道。 |
| 5    | [SUBSCRIBE channel [channel ...\]](https://www.runoob.com/redis/pub-sub-subscribe.html) <br />订阅给定的一个或多个频道的信息。 |
| 6    | [UNSUBSCRIBE [channel [channel ...\]]](https://www.runoob.com/redis/pub-sub-unsubscribe.html) <br />指退订给定的频道。 |

> 测试

订阅端：

```shell
127.0.0.1:6379> subscribe yuanqiyaya	# 订阅一个频道 yuanqiyaya
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "yuanqiyaya"
3) (integer) 1
# 等待读取推送的消息
1) "message"	# 消息
2) "yuanqiyaya"		# 哪个频道的消息
3) "hello"		# 消息的具体内容

1) "message"
2) "yuanqiyaya"
3) "happy"

```

发送端：

```shell
127.0.0.1:6379> publish yuanqiyaya hello	# 发布者发布消息到频道
(integer) 1
127.0.0.1:6379> publish yuanqiyaya happy	# 发布者发布消息到频道
(integer) 1

```

> 原理

Redis是使用C实现的，通过分析Redis源码里的pubsub.c文件，了解发布和订阅机制的底层实现，借此加深对

Redis 的理解。

Redis通过PUBLISH 、SUBSCRIBE 和PSUBSCRIBE等命令实现发布和订阅功能。

【微信公众号】

通过SUBSCRIBE命令订阅某频道后，redis-server里维护了一个字典，字典的键就是一个个频道，而字典的值则是一个链表，链表中保存了所有订阅这个channel 的客户端。SUBSCRIBE 命令的关键，就是将客户端添加到给定 channel 的订阅链表中。

通过PUBLISH命令向订阅者发送消息，redis-server会使用给定的频道作为键，在它所维护的 channel字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。

Pub/Sub从字面上理解就是发布( Publish )与订阅(Subscribe )，在Redis中，你可以设定对某一个key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能。

使用场景：

+ 实时消息系统
+ 实时聊天（频道当做聊天室，将信息回显给所有人即可）
+ 订阅，关注系统都是可以的

稍微复杂的场景我们就会使用消息中间件

# Redis主从复制

## 概念

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower); ==数据的复制是单向的，只能由主节点到从节点。==Master以写为主，Slave以读为主。

==默认情况下，每台Redis服务器都是主节点 ;==

且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。

主从复制的作用主要包括:

1、数据冗余︰主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。

2、故障恢复∶当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复;实际上是一种服务的冗余。

3、负载均衡︰在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载;尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。

4、高可用（集群）基石︰除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。



一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机，一主二从），原因如下:

1、从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大;

2、从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，==单台Redis最大使用内存不应该超过20G。==

电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"多读少写"。

对于这种场景，我们可以使用如下这种架构

![image-20210309165204190](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309165204190.png)

主从复制，读写分离，80%的情况下都是在进行读操作，减缓服务器的压力，架构中经常使用！ 一主二从

## 环境配置

只配置从库，不用配置主库

```shell
127.0.0.1:6379> info replication	# 查看当前库的信息
# Replication
role:master		# 角色  master
connected_slaves:0	# 没有从机
master_failover_state:no-failover
master_replid:7613e6b57e3ffd738242758aadaa1a74f13783ee
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> role 	# role命令也可以查看库信息
1) "master"
2) (integer) 0
3) (empty array)
```

复制三个配置文件，修改对应信息

1、端口号

![image-20210309172106533](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309172106533.png)

2、pid名字

![image-20210309172045436](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309172045436.png)

3、log文件名字

![image-20210309172038764](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309172038764.png)

4、dump.rdb名字

![image-20210309172021913](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309172021913.png)

修改完毕之后启动三个redis，通过进程信息查看

![image-20210309172250548](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309172250548.png)

## 一主二从

==默认情况下，每台Redis服务器都是主节点 ;==一般情况下配置从机即可

一主（79）二从（80、81）

```shell
127.0.0.1:6380> slaveof 127.0.0.1 6379		# slaveof host 6379 找谁当自己的老大
OK
127.0.0.1:6380> info replication
# Replication
role:slave		# 当前角色是从机
master_host:127.0.0.1	# 可以看到主机的信息
master_port:6379
master_link_status:up
master_last_io_seconds_ago:5
master_sync_in_progress:0
slave_repl_offset:14
slave_priority:100
slave_read_only:1
connected_slaves:0
master_failover_state:no-failover
master_replid:6f3b63e5637fa3f9e9f62ccaa3a5e57e4a00e2ee
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:14
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:14

# 在主机中查看
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:1	# 多了从机的配置
slave0:ip=127.0.0.1,port=6380,state=online,offset=28,lag=1
master_failover_state:no-failover
master_replid:6f3b63e5637fa3f9e9f62ccaa3a5e57e4a00e2ee
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:28
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:28
```

如果两个都配置完了，就是有两个从机的

![image-20210309173026822](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309173026822.png)

真实的主从配置应该在配置文件中配置，这样的话是永久的，这里使用的是命令，是暂时的

配置文件中配置主从（在replication模块中）

![image-20210309173621783](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309173621783.png)

> 细节了解

主机可以写，从机不能写只能读，主机中的所有信息和数据，都会自动被从机保存

主机写：

![image-20210309173850545](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309173850545.png)

从节点读取：

![image-20210309173909981](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309173909981.png)

测试：主机断开连接，从机依旧连接到主机的，但是没有写操作，这个时候，主机如果回来了，从机依旧可以直接获取到主机写的信息

如果是使用命令行来配置的主从，这个时候从节点要是重启，会重新变回主节点！只要重新设置为从机，立马就会从主机中获取值

> 复制原理

Slave启动成功连接到 master后会发送一个sync同步命令

Master接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，==master将传送整个数据文件到slave，并完成一次完全同步。==

==全量复制==︰而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

==增量复制== : Master继续将新的所有收集到的修改命令依次传给slave，完成同步

但是只要是重新连接master，一次完全同步（全量复制）将被自动执行！数据一定可以在从节点上看到

> 层层链路

上一个M链接下一个S

![image-20210309185707205](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309185707205.png)

这时候也可以完成主从复制

> 如果没有Master，这个时候能不能选择一个Master出来？手动

谋朝篡位

如果主机断开连接，我们可以使用`slaveof no one`让自己变成主机，其他的节点就可以手动连接到最新的这个主节点（手动）。如果这个时候之前的master恢复，手动重新链接

## 哨兵模式

自动选择master节点

> 概述

主从切换技术的方法是︰当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供了Sentinel (哨兵）架构来解决这个问题。

谋朝篡位的自动版，能够后台监控主机是否故障，如果故障了==根据投票数自动将从库转换为主库。==

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例**。

![image-20210309190707841](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309190707841.png)

这里的哨兵有两个作用

+ 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
+ 当哨兵监测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式。

![image-20210309190935508](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309190935508.png)

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover（重新选举）过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover[故障转移]操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为**客观下线**。

> 测试

目前的状态是一主二从

1、配置哨兵配置文件sentinel.conf

```bash
# sentinel monitor 被监控的名称 127.0.0.1 6379 1
sentinel monitor myredis 127.0.0.1 6379 1
```

后面的数字1，代表主机挂了，slave投票看让谁接替为master，票数最多的就会成为master

2、启动哨兵

```bash
redis-sentinel lconfig/sentinel.conf 	# 启动
22536:X 09 Mar 2021 19:16:51.218 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
22536:X 09 Mar 2021 19:16:51.218 # Redis version=6.2.1, bits=64, commit=00000000, modified=0, pid=22536, just started
22536:X 09 Mar 2021 19:16:51.218 # Configuration loaded
22536:X 09 Mar 2021 19:16:51.219 * Increased maximum number of open files to 10032 (it was originally set to 1024).
22536:X 09 Mar 2021 19:16:51.219 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.1 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 22536
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

22536:X 09 Mar 2021 19:16:51.220 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
22536:X 09 Mar 2021 19:16:51.228 # Sentinel ID is 9fe0d6f2b6d3885512f1f686a44db2b4bbf41740
22536:X 09 Mar 2021 19:16:51.228 # +monitor master myredis 127.0.0.1 6379 quorum 1
22536:X 09 Mar 2021 19:16:51.229 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379
22536:X 09 Mar 2021 19:16:51.236 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
```

如果master节点断开了，这个时候就会从从机中随机选择一个服务器（这里面有一个算法）

![image-20210309192035885](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309192035885.png)

![image-20210309192113868](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309192113868.png)

如果主机此时回来了，只能归到新的master下，当做slave，这就是哨兵模式的规则

> 哨兵模式

优点：

1、哨兵集群，基于主从复制模式，所有的主从配置优点，它全有

2、主从可以切换，故障可以转移，系统的可用性就会更好

3、哨兵模式就是主从模式的升级，从手动更加自动，更加健壮

缺点：

1、redis不好在线扩容，集群容量一旦到达上限，在线扩容就十分麻烦

2、实现哨兵模式的配置其实是很麻烦的，里面有很多选择

> 哨兵模式的全部配置

```shell
#Example sentine1.conf

#哨兵sentine1实例运行的端口默认26379
port 26379

#哨兵sentine1的工作目录
dir /tmp

# 哨兵sentine1监控的redis主节点的ip port
# master-name可以自己命名的主节点名字只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum配置多少个sentine1哨兵统一认为master主节点失联那么这时客观上认为主节点失联了
# sentine7 monitor <master-name> <ip> <redis-port> <quorum>
sentine7 monitor mymaster 127.0.0.1 6379 2

# 当在Redis实例中开启了requirepass foobared 授权密码这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentine1连接主从的密码注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passwOrd

#指定多少毫秒之后主节点没有应答哨兵sentine1 此时 哨兵主观上认为主节点下线默认30秒
# sentine7 down-after-mi1liseconds <master-name> <mil7iseconds>
sentine7 down-after-mi1liseconds mymaster 30000

# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行同步，
这个数字越小，完成failover所需的时间就越长,
但是如果这个数字越大，就意味着越多的slave因为replication而不可用。
可以通过将这个值设为1来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentine1 para7le1-syncs <master-name> <numslaves>
sentine1 para77e1-syncs mymaster 1

#故障转移的超时时间failover-timeout可以用在以下这些方面:
#1．同一个sentine1对同一个master两次failover之间的间隔时间。
#2，当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。
#4.当进行failoverl时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按paralle1-syncs所配置的规则来了
#默认三分钟
#sentine1 failover-timeout <master-name> <mi77iseconds>
sentine1 failover-timeout mymaster 180000

# SCRIPTS EXECUTION
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则:
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGK工LL信号终止，之后重新执行。

#通知型脚本:当sentine1有任何警告级别的事件发生时《比如说redis实例的主观失效和客观失效等等)，将会去调用这个脚本，这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。如果sentjine1.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentine1无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
sentinel notification-script mymaster /var/redis/notify.sh

#客户端重新配置主节点参数脚本
#当一个master由于faiTover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
#以下参数将会在调用脚本时传给脚本:
#<master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
#目前<state>总是“failover",
#<role>是“leader"或者"observer”中的一个。
#参数 from-ip，from-port,to-ip，to-port是用来和旧的master和新的master(即旧的s7ave)通信的
#这个脚本应该是通用的，能被多次调用,不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentine1 c1ient-reconfig-script mymaster /var/redis/reconfig.sh
```

# Redis缓存穿透和雪崩

Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。

![image-20210309194654556](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309194654556.png)

## 缓存穿透（查不到）

> 概念

缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中，于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

> 解决方案

**布隆过滤器**

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力;

![image-20210309194859043](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309194859043.png)

优点：

不需要存储key，节省空间

缺点：

误算率，随着存入的元素数量增加，误算率随之增加。但是如果元素数量太少，则使用散列表足矣

无法删除

**缓存空对象**

当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源;

![image-20210309195034610](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210309195034610.png)

但是这种方法会存在两个问题:

1、如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键;

2、即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。

## 缓存击穿（量太大，缓存过期）

> 概述

这里需要注意和缓存击穿的区别，缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。

当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导使数据库瞬间压力过大。

> 解决方案

**设置热点数据永不过期**

从缓存层面来看，没有设置过期时间，所以不会出现热点key过期后产生的问题。

**加互斥锁**

分布式锁∶使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。

![image-20210310101132698](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210310101132698.png)

缓存中没有数据，第1个进入的线程，获取锁并从数据库去取数据，没释放锁之前，其他并行进入的线程会等待100ms，再重新去缓存取数据。这样就防止都去数据库重复取数据，重复往缓存中更新数据情况出现。

## 缓存雪崩

> 概念

服务的高可用问题

缓存雪崩，是指在某一个时间段，缓存集中过期失效。redis宕机

缓存雪崩是指缓存中数据大批量到过期时间，而查询数据量巨大，引起数据库压力过大甚至down机。和缓存击穿不同的是，缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。

产生雪崩的原因之一，比如在写本文的时候，马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。

![image-20210310101525757](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210310101525757.png)

其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。

双十一：停掉一些服务（保证主要的服务可用）

> 解决方案

**redis高可用**

这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。（异地多活）

**限流降级**

这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

**数据预热**

数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。

