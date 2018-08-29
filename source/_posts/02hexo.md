---
title: 基于hexo+github博客的搭建
date: 2018-02-22 13:32:49
categories:
- 博客开发
tags:
- hexo
---
介绍一下基于hexo+github博客的搭建（很基础）。

<!-- more -->

## 1.建立空文件夹
任意地方建立空文件夹（自己找的到）来存放一些必要的子目录文件。
比如（我自己的myblog）：
![](./blank.jpg '目录')

## 2.安装node
### 检测node是否已安装:
打开myblog，按住shift+鼠标右键，输入以下命令
```bash
$ node -v
```
![](./node.jpg '安装node')
若没有，请自行安装

## 3.安装hexo
### 安装hexo之前必须确保已安装node。之后执行以下代码
①安装hexo
```bash
$ npm install -g hexo
```
正在安装中...
![](./hexo.jpg '安装hexo')
安装成功！！！
![](./hexos.jpg '安装hexo成功')

②初始化hexo，生成一些文件
```bash
$ hexo init
```

③安装所需要的组件
```bash
$ npm install
```

④生成静态页面，并部署到页面
```bash
$ hexo g
$ hexo d
```
![](./hg.jpg '生成静态页面')

⑤打开服务器
```bash
$ hexo s 
```
![](./hs.jpg '打开服务器')

⑥浏览器地址栏输入localhost:4000，可以看到本地博客。且在本地修改内容也可以。

## 4.建立GitHub仓库
首先申请GitHub账号，如何申请自己网上搜索。
进入GitHub，
![](./createg.jpg '建立')
![](./newgithub.jpg '建立新仓库')

## 5.将本地博客连入GitHub仓库
①安装git
在空白区域单击右键，若出现以下内容，说明电脑已安装git。
若没有，请自己安装
![](./git.jpg '安装git')

②设置用户名和邮箱
在myblog文件夹中点击右键，点击Git Base Here
输入自己的用户名和邮箱
```bash
$ git config --global user.name "xxx" 
```
```bash
$ git config --global user.email "xxxx@xx.com" 
```
```bash
$ git init
```
![](./uname.jpg '设置用户名和邮箱')

③检查是否有.ssh的文件夹
```bash
$ cd ~/.ssh 
```
若以前没有.ssh文件夹，则显示：
![](./none.jpg '没有.ssh')

④生成密匙
```bash
$ ssh-keygen -t rsa -C "xxxx@xx.com"     
```
此处填写自己的邮箱
连续三个回车
可以看到创建了一个.ssh的文件夹（包含id_rsa和id_rsa.pub两个文件），并生成密匙。
（两个文件存储路径默认在图中所示，不同电脑有所不同）
![](./key.jpg '生成密匙')

④再次检查.ssh文件夹
```bash
$ cd ~/.ssh 
```
![](./ssh.jpg '进入.ssh')

⑤进入ssh文件夹，输入ls，出现id_rsa id_rsa.pub，说明生成密匙成功。
```bash
$ ls 
```
![](./ls.jpg '文件存在')

⑥输入eval "$(ssh-agent -s)"，添加密钥到ssh-agent
```bash
$ eval "$(ssh-agent -s)" 
```
![](./eval.jpg '添加密匙到ssh-agent')

⑦再输入ssh-add ~/.ssh/id_rsa，添加生成的SSH key到ssh-agent
```bash
$ ssh-add ~/.ssh/id_rsa 
```
![](./add.jpg '添加生成的SSH key到ssh-agent')

⑧登录GitHub，打开首页的右下角仓库。
![](./in.jpg '登录')
![](./addkey.jpg '添加密匙')
![](./dkeys.jpg '设置密匙')

⑨测试添加ssh是否成功。
出现(yes/no)，输入yes。
若看到 Hi 后面是你的用户名，说明成功了。
```bash
$ ssh -T git@github.com 
```
![](./success.jpg '成功')



## 6.配置Deployment，在myblog（最开始建立的文件夹）中，找到_config.yml文件，在末尾进行修改。
![](./deployment.jpg '修改配置')
repo值是在仓库里的右下角
![](./github.jpg '地址')

## 7.安装扩展
①打开myblog文件夹，单击右键+shift。点击“在此处执行命令窗口”

②安装扩展（hexo部署）
```bash
$ npm install hexo-deployer-git --save 
```

③生成静态页面，发布到GitHub
```bash
$ hexo g 
$ hexo d 
```
④连接成功后会出现一个弹出框。
输入自己的用户名/邮箱，密码即可
![](./login.jpg '验证用户')
成功了就访问地址 http://用户名.github.io，可以看见页面了

若此处出错，则修改地址，运行以下代码：（在git bash中执行）
![](./error.jpg '出错')
```bash
$ git remote add origin git@github.com:用户名/用户名.github.io.git 
```
打开myblog下的_config.yml，修改repo
![](./re.jpg '地址')

```bash
$ hexo g
$ hexo d 
```
![](./resuccess.jpg '修改地址后成功')
访问地址 http://用户名.github.io
