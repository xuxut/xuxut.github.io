---
title: 'node搭建服务器'
date: 2018-03-22 09:32:38
tags:
- node
categories:
- 后端
---

使用node搭建一个简单的服务器
<!-- more -->
1.新建文件夹，在里面再创建一个文件夹server
2.在server里面
```
npm init    创建一个package.json
npm i --save express(npm i -S express)  安装express
```
3.创建app.js(index.js)
```
const express = require("express");
let app = new express();
app.listen(3000);
```
4.打开服务器
安装：npm i -g supervisor
```
cmd:supervisor app|node app
```
地址栏localhost:3000
5.全局安装隧道服务
```
cmd:npm i -g ngrok
任意位置:ngrok http 3000
```
或者网页安装（http://ngrok.com/download）
C:/ngrok.exe
之后在C盘web-01里面将ngrok打开，并解压到桌面
```
C:/ cmd:ngrok.exe http 3000
```
6.server/
```
npm i -S wechat
```
7.app.js
```
const wechat = require("wechat");
let config = {
	appid:'',
	token:'',
	encodingAESKey:''
}
```
8.微信公众平台-登录个人订阅号账号
开发-基本配置
复制AppId,粘贴为config的AppID值
9.开发-基本配置
服务器(未启用)-修改配置
token:weixin
encodingAESKey:随机生成
10.复制ngrok生成的隧道地址，填写到公众平台的url处
11.app.js
```
app.get('/',wechat(config,(req,res)=>{
	let message = req.weixin 
	console.log(message)

}))
```

12.开发-基本配置
服务器配置（未启用）-修改配置
提交

13.启用开发者模式
服务器配置（已启用）

14.在公众号中输入消息，后台可以看到。
15.在公众号中输入消息，可以自动回复。
返回响应：res.reply("收到")

16.模拟编辑模式的关键词半匹配回复。
```
const express = require("express");
const wechat = require("wechat");

let config = {
	appid:'wx8c42dec7e3b4dc1f',
	token:'weixin',
	encodingAESKey:'utC2otCu4vW0F86XpHIQZ9kx71I9fcltwDvbGFNk3Il'
};

let app = new express();

app.post('/',wechat(config,(req,res)=>{
	let message = req.weixin 
	console.log(message.Content)
	res.reply("收到")

}))

app.listen(3000);
```
```
const express = require("express");
const wechat = require("wechat");
const mysql=require("mysql");
const pool = mysql.createPool({
	user:'root',
	connectionLimit:5
})


let config = {
	appid:'wx8c42dec7e3b4dc1f',
	token:'weixin',
	encodingAESKey:'utC2otCu4vW0F86XpHIQZ9kx71I9fcltwDvbGFNk3Il'
};

let app = new express();

app.post('/',wechat(config,(req,res)=>{
	let message = req.weixin.Content
	console.log(message)
	//if(message.includes('JavaScript')){
	//	res.reply('JS...')
	//}else if(message.includes('HTML')){
	//	res.reply('HTML...')
	//}else if(message.includes('CSS')){
	//	res.reply('CSS')
	//}

	let sql = `SELECT * FROM db.chat WHERE ? LIKE CONCAT('%'+question+'%')`;
	pool.query(sql,[message],(err,result)=>{
		//返回answer
		if(result.length==1){
			res.reply(results[0].answer)
		}else{
			res.reply('啊？')
		}
	})

}))

app.listen(3000);


```