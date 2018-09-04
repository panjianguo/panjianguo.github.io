---
title: docker使用
date: 2018-09-03 15:30:32
tags:
categories: 服务器
---

# docker 简介

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）。几乎没有性能开销，可以很容易地在机器和数据中心中运行。最重要的是，他们不依赖于任何语言、框架包括系统。

<!--more-->

# 一、CentOS6.x安装docker

1) 更新yum源
```
yum update -y
```
2) 通过安装docker-io软件包来安装Docker
```
yum -y install docker-io
```
3) 启动docker进程
```
service docker on
```
4) 让docker在服务器启动时启动
```
chkconfig docker on
```

# 二、Docker 使用
1） 查询和拉取镜像

```
docker search mysql:5.7
docker pull mysql:5.7
```
2) 列出镜像

```
[root@vultr /]# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
hub.c.163.com/library/mysql   5.7                 573ca163b053        11 months ago       407.1 MB
mysql                         5.7                 573ca163b053        11 months ago       407.1 MB
```
3) 运行docker

```
docker run -d -p 3306:3306 --name mymysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7

-d 表示后台云信docker镜像
-p 表示端口号，前一个8080是指我们访问tomcat时的端口号，
后一个8080是tomcat启动的一个容器在docker中运行的端口号，
指定端口号为了更明确的访问tomcat。 
tomcat:last last是指定的tomcat的标签，相同的镜像可以指定不同的标签以做区分。
```

4) 列出正在运行的容器

```
[root@vultr /]# docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
94ee2b979517        mysql:5.7           "docker-entrypoint.s   2 days ago          Up 2 days           0.0.0.0:3306->3306/tcp   mysqlserver
```
5) 进入容器
```
docker exec -it 94ee2b979517 /bin/bash
```

6) 停止运行容器

```
docker stop 44d9be4dca95
```

7) 移除容器和移除镜像
```
docker rm 44d9be4dca95 // 移除容器
docker rmi 4b8387148088 // 移除镜像
```

# 三、docker 安装mysql
1.下载镜像，mysql 5.7
```
docker pull mysql:5.7
```
2.创建mysql容器，并后台启动
```
docker run -d -p 3306:3306 -e MYSQL_USER="woniu" -e MYSQL_PASSWORD="123456" -e MYSQL_ROOT_PASSWORD="123456" --name mysqltest1 mysql:5.7 --character-set-server=utf8 --collation-server=utf8_general_ci

```
参数说明：
-e MYSQL_USER="root"  ：添加root用户

-e MYSQL_PASSWORD="123456"：设置添加的用户密码

-e MYSQL_ROOT_PASSWORD="123456"：设置root用户密码

--character-set-server=utf8：设置字符集为utf8

--collation-server=utf8_general_cli：设置字符比较规则为utf8_general_cli

3.挂载外部配置和数据安装
1) 创建目录和配置文件my.cnf
```
mkdir /docker
mkdir /docker/mysql
mkdir /docker/mysql/conf
mkdir /docker/mysql/data
 
创建my.cnf配置文件
touch /docker/mysql/conf/my.cnf
 
my.cnf添加如下内容：
[mysqld]
user=mysql
character-set-server=utf8
default_authentication_plugin=mysql_native_password
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
```

2) 创建容器，并后台启动

```
docker run -d -p 3306:3306 --privileged=true -v /docker/mysql/conf/my.cnf:/etc/my.cnf -v /docker/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysqltest2 mysql:5.7
```
参数说明：

--privileged=true：容器内的root拥有真正root权限，否则容器内root只是外部普通用户权限

-v /docker/mysql/conf/my.cnf:/etc/my.cnf：映射配置文件

-v /docker/mysql/data:/var/lib/mysql：映射数据目录
