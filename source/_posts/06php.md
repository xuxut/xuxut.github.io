---
title: 阿里云上传数据库与后台书写
date: 2018-02-27 19:23:31
tags:
- php
categories:
- 后端
---
有些同学可能在本地服务器测试从后台请求回来的数据是正确的，但是上传到阿里云后就不可以了，原因在本文中已经帮同学们列举出来了。

<!-- more -->
>需要大家知道的一点是，当我们在PHP中使用mysqli_fetch_all($result,MYSQLI_ASSOC)，并且将这段代码放到线上环境时，那么就会发现没有请求回来的结果，所以，此函数是无法使用的。
>通过搜索后知道，连接MySQL存在两套连接驱动（libmysql（老）和mysqlind（新））。阿里云只认libmysql。mysqlind是后来出现的。

>mysqlnd(MySQL Native Driver)是老版本中php的mysql连接库libmysqlclient（ MySQL Client Library）的一个新的替代。mysqlnd从PHP 5.3.0开始就内置在官方发布的php源码包中。
>从 http://www.runoob.com/php/func-mysqli-fetch-all.html 中可以知道，mysqli_fetch_all()函数只在带有 MySQL Native Driver 时可用。
![](./define.jpg "定义和用法")

>这是我的主机php使用的版本，由于是5.2.0，所以连接驱动是libmysql
![](./host.jpg "php版本")

## 需要注意两点：
### 1.需要在php中使用mysqli_fetch_array()代替mysqli_fetch_all()，用法如下：
![](./1.jpg "php代码")

### 2.不可以使用 $arr=[] 来直接定义数组，需要使用 $arr = array() 定义数组。

>关于mysqlind的更多新特性见 http://www.360doc.com/content/14/0811/17/17265359_401083295.shtml


