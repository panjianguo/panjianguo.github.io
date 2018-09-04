---
title: redis学习笔记
date: 2018-01-18 11:10:33
tags:
categories: 技术
---
# 一、 redis简介
## redis是什么？
> redis是REmote Dictionary Server(远程字典服务器)的缩写，它以字典结构存储数据，并允许其他应用通过TCP协议读取字典中的内容。
Redis是一个开源的、高性能的、基于键值对的缓存与存储系统，通过提供多种键值数据类型来适应不同场景下的缓存与存储需求。同时 Redis 的诸多高层级功能使其可以胜任消息队列、任务队列等不同的角色。
# redis有哪些特性
1) redis通过键值对的方式存储，键值数据类型如下：
+ 字符串类型
+ 散列类型
+ 列表类型
+ 集合类型
+ 有序集合类型
2) 内存存储与持久化，redis的数据都存储在内存中，对于硬盘存储存在明显的速度优势。程序退出后内存中的数据会丢失,但是Redis提供了对持久化的支持，即可以将内存中的数据异步写入到硬盘中，同时不影响继续提供服务。
3) 功能丰富
4) 简单稳定。redis有100多个命令，但常用的就十几个，redis用C语言编写，并且开源。

<!--more-->

# 二、redis安装
## mac 安装redis
1) brew install redis
2) 安装目录：/usr/local/Cellar/redis/3.2.8
3) 添加至开机启动项
```
$ ln -f /usr/local/Cellar/redis/2.8.13/homebrew.mxcl.redis.plist ~/Library/LaunchAgents/
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
```
4) 启动redis
- 启动 redis-server
5) redis默认端口及配置文件
- REDISPORT=6379
- EXEC=/usr/local/bin/redis-server
- CLIEXEC=/usr/local/bin/redis-cli
- PIDFILE=/var/run/redis_${REDISPORT}.pid
- CONF=/etc/redis/${REDISPORT}.conf
6) 关闭redis
- 关闭redis redis-cli shutdown 或者 kill redis

# 三、redis通用命令
## redis命令
1) redis-cli -h 127.0.0.1 -p 6379
2) redis-cli PING
3) redis-cli 不带参数进入redis交互模式
4) 配置redis日志级别
- redis-server /path/to/redis.conf --loglevel warning
- redis> CONFIG SET loglevel warning

## 取值与赋值
1) set key value
2) get key
3) incr key 递增数字 DECR key 减少数字（下面的递增递减省略）
4) incrby key integer 增加指定的整数
5) incrbyfloat key float 增加指定的双精度浮点数
6) append key value 向尾部追加值
7) strlen key 获取字符串长度
8) mset key1 value1 key2 value2 ... 设置多个键值对
9) mget key1 key2 ... 根据多个键值对获取多个值
10) 位操作：（感觉不常用）
- GETBIT key offset **GETBIT命令可以获得一个字符串类型键指定位置的二进制位的值（0或1），索引从0开始，如果需要获取的二进制位的索引超出了键值的二进制位的实际长度则默认位值是0**
- SETBIT key offset value **SETBIT 命令可以设置字符串类型键指定位置的二进制位的值，返回值是该位置的旧值，如果要设置的位置超过了键值的二进制位的长度，SETBIT命令会自动将中间的二进制位设置为0，同理设置一个不存在的键的指定二进制位的值会自动将其前面的位赋值为0**
- BITCOUNT key [start] [end] **BITCOUNT命令可以获得字符串类型键中值是1的二进制位个数可以通过参数来限制统计的字节范围**
- BITOP operation destkey key [key …] **BITOP命令可以对多个字符串类型键进行位运算，并将结果存储在destkey参数指定的键中。BITOP命令支持的运算操作有AND、OR、XOR和NOT**

# redis类型
## 1.散列类型
### （1）介绍
> Redis 是采用字典结构以键值对的形式存储数据的，而散列类型（hash）的键值也是一种字典结构，其存储了字段（field）和字段值的映射，但字段值只能是字符串，不支持其他数据类型，换句话说，散列类型不能嵌套其他的数据类型。一个散列类型键可以包含至多232−1个字段。

> **提示 除了散列类型，Redis 的其他数据类型同样不支持数据类型嵌套。比如集合类型的每个元素都只能是字符串，不能是另一个集合或散列表等。**

> 散列类型适合存储对象：使用对象类别和 ID 构成键名，使用字段表示对象的属性，而字段值则存储属性值。

### （2）命令
1) HSET key field value
2) HGET key field
3) HMSET key field value [field value …]
4) HMGET key field [field …]
5) HGETALL key
6) HEXISTS key field 判断字段是否存在
7) HSETNX key field value 当前字段不存在时赋值
8) HINCRBY key field increment 增加数字
9) HDEL key field [field …] 删除字段
10) HKEYS key 只获取对象的所有字段名
11) HVALUES key 只获取对象的所有字段值


参考自 李子骅. “Redis入门指南（第2版）”。

