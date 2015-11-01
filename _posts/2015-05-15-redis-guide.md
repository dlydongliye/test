---
layout: post
title: Redis入门指南
category : web
tagline: "Supporting tagline"
tags : [Redis]
---
{% include JB/setup %}
# Redis入门指南前两章
---

####Redis安装

* 官网下载压缩包，解压。

* cd到文件夹，命令行```make```。

* ```sudo make install```。将若干文件转移到bin中，以后可以直接使用，如redis-server等。

* ```sudo make test```。将会对所有的redis命令进行测试。

<!--break-->

####启动和停止Redis

#####启动
* 开发环境直接启动：```redis-serve```默认端口号6379 ``` redis-server --port 6380```修改端口号

* 初始化脚本启动redis: Redis随系统自动运行，源代码中有utils文件夹，其中有个redis_init_script脚本文件。修改如下：

>配置初始化脚本。将文件复制到/etc/init.d目录，目录名为redis_6379，修改第六行redispost变量的值为6379。这数字表示端口号。

>创建/etc/redis（存放配置文件）和/var/redis/端口号（存放持久化文件）两个文件夹

>修改配置文件redis.conf。将配置文件模板复制到/etc/redis目录，取名为 6379.conf。修改如下：daemonize改为yes; pidfile改为/var/run/redis_6379.pid; port改为端口号;dir改为/var/redis/6379;

>```sudo update-rc.d redis_6379 defaults```

#####停止

考虑到Redis可能正在将内存数据写到硬盘，强制停止可能会导致数据丢失。所以```redis-cli shutdown```

####Redis命令行客户端redis-cli

#####发送命令

开启客户端：```redis-cli -h 127.0.0.1 -p 6379``` 其中 -h,-p默认不选；

提供ping测试链接状态，正常返回pong。```redis-cli ping```

#####命令返回值

* 状态回复，例如ok,pong等； 

* 错误回复，以（error）开头；

* 整数回复，以（integer）开头;

* 字符串回复，以""包裹；

* 多行字符串回复，每行以序号开头；

####多数据库

每个数据库对外都是以一个从０开始的递增，默认有１６个，默认选择０号数据库，可以通过```select 1```。Redis不支持对数据库名进行修改，不可以为每个数据库设置密码，即要么全部可访问，要么一个也不可以，但是这些数据库不是完全隔离，如：FLUSHALL可以去除所有数据。不适宜存放不同应用数据，适宜：０存放生产环境数据，１存放测试环境数据。
