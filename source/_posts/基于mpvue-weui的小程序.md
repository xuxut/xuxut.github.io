---
title: 基于mpvue+weui+mpvue-router-patch的小程序
date: 2018-09-04 14:15:55
tags:
- mpvue 
- weui
categories:
- ui框架
---
>原文文档出自http://kuangpf.com/mpvue-weui/#/README
>项目出自https://github.com/RebeccaHanjw/weapp-wechat-zhihu

>其他的小程序UI框架:`https://github.com/weilanwl/ColorUI`

最近用基于vue的mpvue框架以及类似ui库的东西（http://kuangpf.com/mpvue-weui/#/grid）搭建了一个类似知乎的小程序。

<!-- more -->

### 开发步骤
```
$ vue init mpvue/mpvue-quickstart my-project
$ cd my-project
$ npm install
$ npm run dev
```
将项目放在微信web开发工具中即可。（生成的dist文件夹才是转换的微信小程序）

### 使用mpvue-weui
在`/src/main.js`中引入`weui.css`，然后就可以使用了
提供一个链接[weui.css](https://github.com/KuangPF/mpvue-weui/blob/master/static/weui/weui.css)

### 使用mpvue-router-patch
https://www.npmjs.com/package/mpvue-router-patch
```
$ npm install mpvue-router-patch
```
在src文件夹下`main.js`引入如下代码：
```
import Vue from 'vue'
import MpvueRouterPatch from 'mpvue-router-patch'
 
Vue.use(MpvueRouterPatch)
```
跳转到某个页面
```
$router.push(location, onComplete?, onAbort?, onSuccess?)
```
跳转到应用内的某个页面，wx.navigateTo、wx.switchTab 及 wx.reLaunch 均通过该方法实现，location 参数支持字符串及对象两种形式，跳转至 tabBar 页面或重启至某页面时必须以对象形式传入
```
// 字符串
router.push('/pages/news/detail')
 
// 对象
router.push({ path: '/pages/news/detail' })
 
// 带查询参数，变成 /pages/news/detail?id=1
router.push({ path: '/pages/news/detail', query: { id: 1 } })
 
// 切换至 tabBar 页面
router.push({ path: '/pages/news/list', isTab: true })
 
// 重启至某页面，无需指定是否为 tabBar 页面，但 tabBar 页面无法携带参数
router.push({ path: '/pages/news/list', reLaunch: true })
```



