---
title: Node.js
date: 2022-11-01 10:46:44
summary: Node
tags: [Node]
categories: [Node]
---

# Node

## 1. 用途

1. 基于Express框架，快速构建Web应用；
2. 基于Electron框架，构建跨平台的桌面应用；
3. 基于restify框架，快速构建API接口项目。

## 2. fs文件系统模块

```js
// fs.readFile(path[, options], callback)
// callback: (err, data) => {}
// 读取成功: err为null, data为数据对象
// 读取失败: err为错误对象, data为undefined
fs.readFile('./1.txt', 'utf8', (err, data) => {
  console.log(err)
  console.log('----------------')
  console.log(data)
})

// fs.writeFile(file, data[, options], callback)
// callback: (err) => {}
// 读取成功: err为null
// 读取失败: err为错误对象
fs.writeFile('./2.txt', 'Hello, jim', (err) => {
  console.log(err)
})
// 注意：
// 1.此方法只能用来创建文件，不能用来创建路径
// 2.重复调用此方法写入同一个文件，新写入的内容会覆盖旧的内容
```

## 3. path 路径模块

```js
const path = require('path')

// path.join()
const pathStr = path.join('/a', '/b/c', '../', 'd')

console.log(pathStr) // \a\b\d

console.log(path.join(__dirname, './1.txt'))
// C:\Users\kim\Desktop\test\1.txt

// path.basename()
const fPath = '/a/b/c/index.html'

const fileFullName = path.basename(fPath)

console.log(fileFullName) // index.html

const fileName = path.basename(fPath, '.html')

console.log(fileName) // index

// path.extname()
const extName = path.extname(fPath)

console.log(extName) // .html
```

## 4. http 模块

```js
const http = require('http')
const server = http.createServer()

server.on('request', (req, res) => {
  // req.url 是请求地址, req.method 是请求方法
  const { url, method } = req
  let content = '<h1>404 Not Found</h1>'

  if (url === '/' || url === '/index') content = '<h1>首页</h1>'
  else if (url === '/about') content = '<h1>关于</h1>'
  
  // 为了防止中文显示乱码的问题，需要设置响应头 Content-Type 的值为 text/html; charset=utf-8
  res.setHeader('Content-Type', 'text/html; charset=utf-8')

  // res.end(content) 向客户端响应内容并结束请求
  res.end(content)
})

server.listen(80, () => {
  console.log('server is running.')
})

```

