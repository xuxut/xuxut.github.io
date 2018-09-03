---
title: hexo博客同时部署到github和Coding
date: 2018-09-03 18:09:01
tags:
- hexo 
categories:
- 博客开发
---

>引言：
>之前博客只部署到了github上，具体过程可参考[基于hexo+github博客的搭建](https://xuxut.github.io/2018/02/22/02hexo/)，我尝试同时部署到coding上。

1.打开[Coding.net官网](https://coding.net/)，注册个人账号。这里我直接关联的github账号。

2.新建项目
注意：项目名与注册用的账户名一致。

3.将根目录下的_config.yml的repo写成
```
repo:
    github: https://github.com/账户名/账户名.github.io.git,master
    coding:
      https://git.coding.net/帐户名/账户名.git,master
```

4.部署即可
```
$ hexo g
$ hexo d
```

5.打开Coding.net的项目管理界面，打开代码->Pages服务，选择部署来源为master分支，然后保存即可。

6.访问地址：账户名.coding.me


