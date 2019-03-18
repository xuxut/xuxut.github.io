---
title: 发布自己的npm包
date: 2019-03-18 17:09:40
tags:
- react
- npm
---

从0开始发布自己的npm包--react的音乐插件
1. 在自己的github上新建一个仓库，将仓库克隆到本地
2. npm包初始化，定义包名，描述，作者名~~， 生成`package.json`文件
```
$ npm init
```
添加常见的命令
```
package.json
srcipts:[
  "start": "webpack-dev-server --open --mode development",
  "build": "webpack --mode production"
]
```
3. >>>安装需要的react包
>>>使用webpack作构建，webpack-dev-server做本地开发服务器，安装相应依赖
>>>最基本的组件只需要jsx，所以安装babel-loader
```
$ npm i react react-dom -D
$ npm i webpack webpack-cli webpack-dev-server -D
$ npm i html-webpack-plugin -D
$ npm i babel-loader -D
```
4. 新建测试目录和组件目录
```
├── example // 示例代码，在自己测试的时候可以把测试文件放到 src 里
│   └── src // 示例源代码
│       ├── index.html // 示例 html
│       └── app.js // 添加到 react-dom 的文件
├── package.json 
├── src // 组件源代码
│   └── index.js // 组件源代码文件
├── .babelrc
├── .editorconfig // 不必须的，但是建议有
├── .gitignore // 如果要放到 github 上，这个是需要有的
└── webpack.config.js
```
首先定义组件
```javascript
// src/index.js
import React from 'react'

const Demo = () => {
  <h1>音乐组件</h1>
}

export default Demo

```
使用组件，放入容器
```javascript
import React from 'react'
import { render } from 'react-dom'
import ReactDemo from '../../src'

const App = () => {
  return <ReactDemo /> 
}

render(<App />, document.getElementById('root'))
```
然后创建容器，装载react元素
```html
<!-- example/src/index.html -->
<html>
  <title>我的第一个音乐组件</title>
  <meta charSet="utf-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0, shrink-to-fit=no"/>
  <body>
    <div id="root"></div>
  </body>
</html>
```

5. 配置webpack.config.js
   loader的作用是将模块转换为javascript模块，因为webpack只识别jsvascript模块
```
const path = require('path')
// 用户不必手动写html页面，自动注入编译打包好的脚本文件
const HtmlWebpackPlugin = require('html-webpack-plugin')
const htmlWebpackPlugin = new HtmlWebpackPlugin({
  template: path.join(__dirname, "./example/src/index.html"),
  filename: './index.html'
})
// 自动打开浏览器: webpack-dev-server也可以自动打开
// const OpenBrowserPlugin = require('open-browser-webpack-plugin')

module.exports = {
  entry: path.join(__dirname, "./example/src/app.js"),
  output: {
    path: path.join(__dirname, "example/dist"),
    filename: 'bundle.js'
  },
  module: {
    rules: [{
      test: /\.(js|jsx)$/,
      use: "babel-loader",
      exclude: /node_modules/
    }]
  },
  plugins: [htmlWebpackPlugin],
  resolve: {
    // 自动扩展文件后缀名，意味着require时不必加这个后缀
    extensions: ['.js', '.jsx']
  },
  devServer: {
    port: 8001
  }
}
```
6.然后再配置一下 babel，咱们的 babel 主要做两件事，将 jsx 编译成 es5，然后再加一个通用的 env，所以 .babelrc 配置如下：
```
{
    "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```


