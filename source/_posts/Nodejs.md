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

## 5. 模块化

```js
// 使用require加载模块的时候，会执行模块内的代码

// 模块在第一次加载后会被缓存 === 多次调用require()不会导致模块的代码被多次执行。

// index.js
const express = require('express')
const router = require('./router')
const app = express()

app.use(router)

app.listen(80, () => {
  console.log('express server is running at http:127.0.0.1:80')
})

// router.js
const express = require('express')
const router = express.Router()

router.get('/user', (req, res) => {
  res.send({ name: 'kim' })
})

router.post('/user', (req, res) => {
  res.send('请求成功！')
})

module.exports = router
```

## 6. Express的基本使用

```js
const express = require('express')

const app = express()

// 对外托管静态资源
// 参数1 '/public' 为访问的路径前缀	e.g.: http://localhost/public/index.html
// 参数2 express.static('../resource/day1') 的路径以当前命令执行的目录为准
// e.g.: a文件夹内有需要暴露的index.html文件, b文件夹内有需要执行的js文件
// 若执行目录与a同级，则路径应为 './a'
// 若执行目录位于b内，则路径应为 '../a'

app.use('/public', express.static('../resource/day1'))

app.get('/user', (req, res) => {
  res.send({ name: '张三', age: 18, sex: '男' })
})

app.post('/user', (req, res) => {
  res.send('请求成功！')
})

app.get('/', (req, res) => {
// req.query 可以获取url的查询参数 e.g.: http://127.0.0.1?name=kim
  res.send(req.query) // {name: 'kim'}
})

app.get('/user/:id', (req, res) => {
// req.params 可以获取url的动态参数 e.g.: http://127.0.0.1/user/1
  res.send(req.params) // {id: '1'}
})

app.listen(80, () => {
  console.log('express server is running at http:127.0.0.1:80')
})
```

## 7.中间件

```js
// 定义全局中间件	
// 多次调用app.use()定义中间件，会按照定义的先后顺序来触发中间件函数，因此须在定义路由前定义

app.use((req, res, next) => {
    console.log('这是一个简单的全局中间件')
    next()
})

// 局部中间件
// 不使用app.use()定义的中间件都叫局部中间件
// mw 为中间件函数
app.get('/', mw, (req, res) => { res.send('请求成功') })

// 定义多个局部中间件
app.get('/', mw1, mw2, (req, res) => { res.send('请求成功') })
app.get('/', [mw1, mw2], (req, res) => { res.send('请求成功') })

// 中间件注意事项
// 1. 在路由之前注册中间件
// 2. 接收到客户端的请求后，可以连续调用多个中间件进行处理
// 3. 执行完中间件的业务代码后，要调用next()函数
// 4. 为了防止代码逻辑混乱，在调用next()函数后不要再写代码
// 5. 连续调用多个中间件时，多个中间件之间共享req, res对象
```

```js
// 中间件的分类
// 1.应用级别的中间件
// 通过app.use()/app.get()/app.post()，绑定到app实例上的中间件，叫做应用级别的中间件。

// 2.路由级别的中间件
// 绑定到express.Router()实例上的中间件，叫做路由级别的中间件。它的用法和应用级别的中间件没有任何区别。
const express = require('express')
const router = express.Router()

router.use(mw)
router.get('/', mw, (req,res) => {})

// 3.错误级别的中间件
// 专门用来捕获项目中发生的异常错误，防止项目崩溃
// 必须主车在所有路由之后
app.use((err, req, res, next) => {
    console.log('发生错误：' + err.message)
    res.send('Error!' + err.message)
})

// 4.Express内置中间件
// express.static	快速托管静态资源的内置中间件（无兼容性）

// express.json	解析JSON格式的请求体数据（有兼容性，仅4.16.0+版本可用）
app.use(express.json())
// 在服务端，可以通过req.body获取JSON格式的表单数据和url-encoded格式的数据
// express.urlencoded	解析URL-encoded格式的请求体数据（有兼容性，仅4.16.0+版本可用）
app.use(express.urlencoded({ extended: false }))

// 5.第三方中间件 
// 非Express官方内置的，由第三方开发出来的中间件
```

### 自定义中间件

```js
// index.js
const express = require('express')
const bodyParser = require('./custom_body_parse')
const app = express()

app.use(bodyParser)

app.post('/', (req, res) => {
  res.send(req.body)
})

app.listen(80, () => {
  console.log('server is running at http://127.0.0.1')
})


// custom_body_parse.js
const qs = require('querystring')

function bodyParser(req, res, next) {
  let str = ''

  // 监听req的data事件，可能多块拼接
  req.on('data', (chunk) => {
    str += chunk
  })

  // 监听req的end事件
  req.on('end', () => {
    const body = qs.parse(str)
    req.body = body
    next()
  })
}

module.exports = bodyParser

```

## 8. 跨域解决

```js
// 使用cors中间
// npm i cors
const cors = require('cors')
// 在定义路由前定义cors中间件
app.use(cors())

// 手动配置
// 1.Access-Control-Allow-Origin
// url参数为允许访问服务器的地址，*代表所有
res.setHeader('Access-Control-Allow-Origin', url)

// 2.Access-Control-Allow-Headers
// 第二个参数为配置的信息
res.setHeader('Access-Control-Allow-Headers', 'Content-Type, X-Custom-Header')

// 2.Access-Control-Allow-Methods
// 第二个参数为指明除了GET、POST、HEAD之外的请求，*代表所有请求
res.setHeader('Access-Control-Allow-Methods', 'PUT, DELETE')
```

## 9. Mysql

 ### 1. 数据库分类

传统型数据库（关系型数据库、SQL数据库）：MySQL、Oracle、SQL Server

新型数据库（非关系型数据库、NoSQL数据库）：Mongodb

### 2. SQL语句

```sql
-- 关键字对大小写不敏感

-- 查
-- SELECT 列名称 FROM 表名称
-- 其中 * 代表所有列

-- 增
-- insert into table_name(column1, column2...) values (value1, value2...)

-- 改
-- update table_name set colName = newVal where colName = val

-- 删
-- delete from table_name where colName = val

-- e.g.
-- select * from my_db_01.users
-- select username, password from my_db_01.users
-- insert into my_db_01.users (usernameusers, password) values ('tony stark', '098123')
-- update my_db_01.users set password = '888888' where id = 4
-- update my_db_01.users set password = 'admin123', status = 1 where id = 2
-- delete from my_db_01.users where id = 4
```

### 3. 在项目中使用mysql

1. 安装操作MySQL数据库的第三方模块(mysql)

   ```shell
   npm i mysql
   ```

   

2. 通过mysql模块连接到MySQL数据库

3. 通过mysql模块执行SQL语句

```js
const mysql = require('mysql')

const db = mysql.createPool({
  host: '127.0.0.1',
  user: 'root',
  password: 'admin123',
  database: 'my_db_01',
})

// 插入数据
const user = { username: 'Spider-Ma', password: 'pcc321' }
// ? 代表占位符
const sqlStrForInsert = 'insert into users (username, password) values (?, ?)'
db.query(sqlStrForInsert, [user.username, user.password], (err, result) => {
  if (err) return console.log(err.message)
  if (result.affectedRows === 1) console.log('插入数据成功')
})

// 插入数据便捷写法
const user = { username: 'Spider', password: 'pcc321' }
// ? 代表占位符
const sqlStrForInsert = 'insert into users set ?'
db.query(sqlStrForInsert, user, (err, result) => {
  if (err) return console.log(err.message)
  if (result.affectedRows === 1) console.log('插入数据成功')
})

// 删除数据便捷写法
// ? 代表占位符
const sqlStrForDelete = 'delete from users where id=?'
db.query(sqlStrForDelete, 5, (err, result) => {
  if (err) return console.log(err.message)
  if (result.affectedRows === 1) console.log('删除数据成功')
})

// 更新数据便捷写法
const user = { id: 5, username: 'aba', password: '000' }
// ? 代表占位符
const sqlStrForUpdate = 'update users set ? where id=?'
db.query(sqlStrForUpdate, [user, user.id], (err, result) => {
  if (err) return console.log(err.message)
  if (result.affectedRows === 1) console.log('更新数据成功')
})

// 标记删除
// 使用delete语句会真正地把数据从数据库中删除，为了保险起见，将用户状态禁用了则代表删除了
const sqlStrForMarkDel = 'update users set status = 1 where id=?'
db.query(sqlStrForMarkDel, 8, (err, result) => {
  if (err) return console.log(err.message)
  if (result.affectedRows === 1) console.log('标记删除成功！')
})

// 查询数据
db.query('select * from users', (err, result) => {
  if (err) return console.log(err.message)

  console.log(result)
})

```

## 10. Web开发模式

- 服务端渲染

  优点：前端耗时少、有利于SEO

  缺点：占用服务器资源、不利于前后端分离而导致开发效率低

  身份认证：推荐使用Session认证机制

- 前后端分离

  优点：开发体验好、用户体验好、减轻了服务端的渲染压力

  缺点：不利于SEO（可以通过SSR技术解决）

  身份认证：推荐使用JWT认证机制

## 11. Express中使用Session认证

1. 安装express-session中间件

   ```shell
   npm i express-session
   ```

2. 配置express-session中间件

   ```js
   const session = require('express-session')
   app.use(
     session({
       secret: 'kim',
       resave: false,
       saveUninitialized: true,
     })
   )
   ```

3. 将数据存储到session中

   ```js
   // 登录的 API 接口
   app.post('/api/login', (req, res) => {
     // 判断用户提交的登录信息是否正确
     if (req.body.username !== 'admin' || req.body.password !== '000000') {
       return res.send({ status: 1, msg: '登录失败' })
     }
   
     console.log(req.session)
     // TODO_02：请将登录成功后的用户信息，保存到 Session 中
     req.session.user = req.body
     req.session.isLogin = true
     console.log(req.session)
   
     res.send({ status: 0, msg: '登录成功' })
   })
   ```

4. 将数据从session中取出

   ```js
   // 获取用户姓名的接口
   app.get('/api/username', (req, res) => {
     // TODO_03：请从 Session 中获取用户的名称，响应给客户端
     if (!req.session.isLogin) return res.send({ status: 1, msg: 'fail' })
   
     res.send({
       status: 0,
       msg: 'success',
       username: req.session.user.username,
     })
   })
   ```

5. 清空session中的数据

   ```js
   // 退出登录的接口
   app.post('/api/logout', (req, res) => {
     // TODO_04：清空 Session 信息
     req.session.destroy()
     res.send({
       status: 0,
       msg: '退出登录成功！',
     })
   })
   ```

   ## 12. JWT认证机制

   由三部分组成：Header(头部).Payload(有效载荷).Signature(签名)

   - 其中Payload部分才是真正的用户信息，它是用户信息经过加密之后生成的字符串。
   - Header 和 Signature 是安全性相关的部分，只是为了保证token的安全性。
   
   在Express中使用JWT
   
   1. 安装JWT相关包
   
      ```shell
      npm i jsonwebtoken express-jwt
      // jsonwebtoken 生成JWT字符串
      // express-jwt 将JWT字符串还原成json对象
      ```
   
   2. 导入JWT相关包
   
      ```js
      const jwt = require('jsonwebtoken')
      const expressJWT = require('express-jwt')
      ```
   
   3. 定义secret密钥&生成token
   
      ```js
      const secretKey = 'DefeatLaziness~~'
      // 登录接口
      app.post('/api/login', function (req, res) {
        // 将 req.body 请求体中的数据，转存为 userinfo 常量
        const userinfo = req.body
        // 登录失败
        if (userinfo.username !== 'admin' || userinfo.password !== '000000') {
          return res.send({
            status: 400,
            message: '登录失败！',
          })
        }
        // 登录成功
        // TODO_03：在登录成功之后，调用 jwt.sign() 方法生成 JWT 字符串。并通过 token 属性发送给客户端
        // 参数1: 用户的信息对象
        // 参数2：加密的密钥
        // 参数3：配置对象
        const tokenStr = jwt.sign({ username: userinfo.username }, secretKey, {
          expiresIn: '30s',
        })
        res.send({
          status: 200,
          message: '登录成功！',
          token: tokenStr, // 要发送给客户端的 token 字符串
        })
      })
      ```
   
   4. 将JWT字符串还原为JSON对象
   
      ```js
      app.use(expressJWT({ secret: secretKey }).unless({ path: [/^\/api\//] }))
      // 只要配置成功express-jwt中间件，就会把解析出来的用户信息挂载到req.user中
      // 这是一个有权限的 API 接口
      app.get('/admin/getinfo', function (req, res) {
        // TODO_05：使用 req.user 获取用户信息，并使用 data 属性将用户信息发送给客户端
        res.send({
          status: 200,
          message: '获取用户信息成功！',
          data: req.auth, // 要发送给客户端的用户信息
        })
      })
      ```
   
   5. 捕获JWT解析失败
   
      ```js
      app.use((err, req, res, next) => {
        if (err.name === 'UnauthorizedError')
          return res.send({ status: 401, message: '无效的token' })
        res.send({ status: 500, message: '未知错误' })
      })
      ```
   
      
