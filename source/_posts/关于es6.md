---
title: 关于es6
date: 2018-09-06 09:41:32
tags:
- javascript 
categories:
- es6
---

箭头函数的那些事儿
由于大括号会被解析为代码块，所以当返回一个对象时，需要在对象外面加一个括号，否则会报错
```javascript
var getObj = time => ({type:'哈哈哈',time})
```

