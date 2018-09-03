---
title: webpack
date: 2018-09-03 10:10:53
tags:
- webpack
categories:
- 打包工具
---

用webpack进行项目打包。本文出自`https://juejin.im/entry/58c2023dda2f600d8722043a`
<!-- more -->

## 概念——开发与发布
在开发阶段需要测试，看程序是否跑通。而测试工具在发布时是不需要的，为了分别管理，npm在package.json提供了这个字段
>devDependencies ——仅开发依赖
>dependencies ——依赖包

## 开发与部署
部署会用到和开发阶段不同的webpack配置文件，将输出目录换了另一个。
>app 放文件的地方
>build文件夹 是经过webpack打包，自动生成文件的去处。
>dist文件夹 保存发布版本
在.app中写东西，打包到build中调试，再发布到dist文件夹

## 配置webpack
> webpack.config.js 目标build
> webpack.production.config.js 目标dist 
需要手写。
```
// webpack.config.js
var path = require("path");

module.exports = {
  entry:  path.join(__dirname, '/app/main.js'),
  output: {
    path: path.join(__dirname, '/build'),//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
};
```
__dirname 是当前运行的js所在的目录

>模块的依赖书写方式：
>require——commonJS
>import/export——ES6 module import（需要babel转换器）

## 配置属性
1.entry 入口
  entry值的写法
  ```
  entry: path.resolve(__dirname, 'app');
  entry: path.join(__dirname, 'app');
  entry: __dirname + '/app';(linux, mac下正确， windows下错误)
  ```
path.resolve() 做一些解析，将参数从右到坐拼接，直到遇到一个绝对路径  `/表示路径起点——绝对路径的标志，通常为脚本运行所处的位置`
path.join() 仅仅进行路径拼接

2.context——entry的根目录
可以通过context来定义entry的根目录
```
{
  context: path.join(__dirname, 'app'),
  entry: 'entry'
}
```

3.output `https://www.jianshu.com/p/dcb28b582318`
规定如何将打包后的文件写在磁盘里
output.path 仅仅告诉webpack结果存储到哪儿
output.publicPath 被许多webpack的插件用于在生产模式下更新 内嵌到css,html文件里的url值

4.webpack-dev-server 服务器工具
```
$ npm install --save-dev webpack-dev-server
```
server内部调用webpack，好处是提供了额外的功能，如热更新“Live Reload”以及热替换“Hot Module Replacement”
[HMR] hot模块（局部刷新）
[WDS] webpack-dev-server模块
使用react-hot-loader
```
npm install --save-dev react-hot-loader@3.0.0-beta.6
```
使用babel，若babel配置太多，就新建.babelrc
```
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react babel-preset-stage-2
```
