---
title: git和vscode安装文档
date: 2019-02-11 9:45:45
tags: 
- git
- vscode
categories:
- 工具
---

## node
`https://nodejs.org/zh-cn/download/`

## git
`https://git-scm.com/downloads`

<!-- more -->
## npm-cnpm-yarn-tyarn
```
npm -v

npm install cnpm -g --registry=https://registry.npm.taobao.org
cnpm -v

cnpm install yarn tyarn -g
yarn -v
tyarn -v
```

## visual studio code
`https://visualstudio.microsoft.com/zh-hans/downloads/`

1. 插件：
```
Auto Close Tag
Auto Complete Tag
Auto Rename Tag
Beautify
Chinese (Simplified) Language
ES7 React/Redux/GraphQL/React-Native snippets
Git History
GitLens -Git supercharged
HTML CSS Support
HTML Snippets
npm Intellisense
Path Intellisense
React/Redux/react-router Snippets
Reactjs code snippets
```

2. 设置：setting.json
```json
{
    "workbench.editor.enablePreview": false,
    "git.ignoreMissingGitWarning": true,
    "editor.fontSize": 18,
    "window.zoomLevel": 1,
    "git.path": "D:\\git\\Git\\bin\\git.exe",
    "emmet.includeLanguages": {
        "javascript": "javascriptreact"
    },
    "javascript.implicitProjectConfig.experimentalDecorators": true,
    "git.confirmSync": false,
}
```

3. 添加git的环境变量
右键`我的电脑`->`属性`->`高级系统设置`->`环境变量`
administrator下的Path
添加`D:\git\Git\bin`

4. 常用快捷键
ctrl+p 打开某个文件
ctrl+g 去某一行
ctrl+shift+p 使用git

5. 左侧应用
```
左侧搜索全局
使用git
插件安装
```

## 项目地址
`https://code.aliyun.com/`





