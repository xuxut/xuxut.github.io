---
title: vue重构项目遇到的问题以及vue项目打包
date: 2018-03-04 13:32:49
tags: 
- javascript
- vue
categories:
- 前端 
---

 本文描述的是在用vue重构项目时所遇到的一些问题。在此记录一下。
<!-- more -->
## 1.如果图片的路径是变量，需要用require("路径变量")
## 2.子组件之间的通信
### (1)在全局定义window.bus。
>通知：bus.$emit("事件名称",数据)
>接收：bus.$on("事件名称",function(msg){})
### (2)或者使用本地存储技术
>localStorage/sessionStorage.setItem("名称",数据)
>localStorage/sessionStorage.getItem("名称")
>localStorage/sessionStorage.removeItem("名称")
## 3.怎样刷新页面，局部刷新？？
<h3>this.$router.push("/路径")不支持刷新</h3>
>window.location.reload()是刷新整个页面，我想只刷新某部分的router-view
>this.$router.go(0);刷新整个页面
## 4.关于通信问题。
(1)父与子：
① props
![](./父与子.jpg)	
② $refs
![](./父与子ref.jpg) 
③ $parent
![](./父与子$parent.jpg)
(2)子与父：
子：$emit.事件
 ![](./子与父子.jpg)
父：标签中 @事件=""
 ![](./子与父父.jpg)

(3)子与子bus
子1（传递$emit）:
 ![](./子与子子1.jpg)
子2（绑定$on）：
 ![](./子与子子2.jpg)

# vue环境搭建
![](./搭建.jpg)

# vue项目打包
1.config/index.js
![](./index.jpg)
2.build/utils.js
![](./util.jpg)
3.根目录下运行npm run build
4.完成后文件保存在根目录下的dist，需要打开服务器，才能打开index.html

# vue-router的导航钩子
1.全局导航钩子（前置守卫、后置钩子）
（1）全局前置守卫
```
const router = new VueRouter({...})
router.beforeEach((to,from,next)=>{
	//do something
})

to:Route，代表要进入的目标，它是一个路由对象
from:Route，代表当前正要离开的路由，同样也是一个路由对象
next:Function，这是一个必须需要调用的方法，而具体的执行效果则依赖next方法调用的参数
```
![](./vue-router-nav.jpg)
（2）全局后置钩子:不同于前置守卫，后置钩子没有next函数，也不会改变导航本身。
```
router.afterEach((to,from)=>{
	//do something
})
```
2.路由独享的钩子：在路由配置上直接定义的：
```
cont router = new VueRouter({
     routes:[
	{
		path:'/file',
		component:File,
		beforeEnter:((to,from,next)=>{
			//do something
		})
	}
     ]
})
```
3.组件内的导航钩子
4.父组件向子组件传递props（异步请求回来的数据）时，在子组件的生命周期挂载后，数据为undefined,用定时器延迟来接收即可。
5.改变对象或数组不会触发DOM更新，解决办法：
```
eg: const obj = that.keywordList[key]
		obj.checked = !obj.checked
		that.leywordList.splice(index, 1, obj)
```
6.若需要动态添加元素的CSS,则<style></style>不能写scoped属性