---
title: hexo博客保存源代码
date: 2018-08-29 18:16:59
tags:
- hexo 
- git
categories:
- 博客开发
---
hexo默认上传代码至github仓库时，会自动将文件进行打包转换为.deploy_git中的代码，这样，我们用别的电脑继续书写博客，就会存在找不到源文件的问题。所以，针对这个问题，我重新开了一个分支来保存源码，步骤已列出，希望给大家带来一些帮助。

<!-- more -->
在根目录中放置一个git文件，以便将代码上传至github
```
$ git init
```
git文件夹中的 config 放至以下内容， `注意url中的地址需要是Use HTTPS，不是SSH。（如果电脑上不需要添加账户密钥就用HTTPS的地址，这样无论在何时都可以更新博客了）` 根目录下的 _config.yml也需要将地址更改为Use HTTPS 
```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	url = https://github.com/用户名/用户名.github.io.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```
在GitHub仓库中创建一个新分支，git创建一个同样的分支并切换到此分支，执行以下代码：
在这里我用的git的上传工具，TortoiseGit。
![](./branch.png '创建新分支')
![](./git.png '创建新分支')
```
$ git pull 
$ git commit
$ git push
```
然后，就可以在github的分支上面看到hexo的源代码了

若推送时，警告说已存在这个仓库，就删掉仓库中的分支，执行以下代码：
```
$ git push origin :需要去掉的分支的名字
```




