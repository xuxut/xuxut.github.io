---
title: 使用fetch实现文件下载
date: 2019-1-23 16:00:56
tags: 
- fetch 
- 下载 
category: 
- react
blogexcerpt: 当我们要实现一个下载的功能时，浏览器会通过文件的MIME类型来决定是直接打开文件，还是下载文件。比如：当需要下载的文件是图片或者视频时，浏览器会自动打开文件。最近遇到了一个需要下载视频的需求，在这里记录一下过程。
---

>文档地址：[https://www.yuque.com/ant-design/course/wvbsue](https://www.yuque.com/ant-design/course/wvbsue)

当我们要实现一个下载的功能时，浏览器会通过文件的MIME类型来决定是直接打开文件，还是下载文件。比如：当需要下载的文件是图片或者视频时，浏览器会自动打开文件。最近遇到了一个需要下载视频的需求，在这里记录一下过程。

项目是antd design pro搭建的，视频的下载这个过程是使用fetch去模拟a标签的下载过程。代码示例如下：
```javascript
    import fetch from 'dva/fetch'
    fetch('http://somehost/somefile.zip')
        // 1.
        .then(res => res.blob())
        .then(blob => {
            // 2.
            var a = document.createElement('a');
            var url = window.URL.createObjectURL(blob);
            var filename = 'myfile.zip';
            a.href = url;
            a.download = filename;
            // 3.
            a.click();
            // 4.
            window.URL.revokeObjectURL(url);
        }))
```
1. fetch 一个接口获取其内容并转成 [blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob) 对象。
2. 将 blob 对象使用 [createObjectURL](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL) 方法转化成 ObjectURL，等同于一个下载地址链接。
3. 创建一个 a 标签，并赋予 ObjectURL 且执行一次 click。
4. 通过 [revokeObjectURL](https://developer.mozilla.org/en-US/docs/Web/API/URL/revokeObjectURL) 回收 ObjectURL。