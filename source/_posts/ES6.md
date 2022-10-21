---
title: ES6
date: 2022-10-21 15:19:01
summary: ES6
tags: [ES6]
categories: [ES6]
---

# ES6

## 1. 转译

[Babel](https://babeljs.io/) 是一个广泛使用的 ES6 转码器，可以将 ES6 代码转为 ES5 代码，从而在老版本的浏览器执行

```shell
npm install --save-dev @babel/core
```

1. 配置文件.babelrc

   ```shell
   # 最新转码规则
   $ npm install --save-dev @babel/preset-env
   
   # react 转码规则
   $ npm install --save-dev @babel/preset-react
   ```

   ```js
   // .babelrc
   {
      "presets": [
        "@babel/env",
        "@babel/preset-react"
      ],
      "plugins": []
    }
   ```

2. 命令行转译

   ```shell
   $ npm install --save-dev @babel/cli
   
   # 基本用法
   # 转码结果输出到标准输出
   $ npx babel example.js
   
   # 转码结果写入一个文件
   # --out-file 或 -o 参数指定输出文件
   $ npx babel example.js --out-file compiled.js
   # 或者
   $ npx babel example.js -o compiled.js
   
   # 整个目录转码
   # --out-dir 或 -d 参数指定输出目录
   $ npx babel src --out-dir lib
   # 或者
   $ npx babel src -d lib
   
   # -s 参数生成source map文件
   $ npx babel src -d lib -s
   ```

3. 直接运行es6脚本

   ```shell
   $ npm install --save-dev @babel/node
   
   # 假设test.js文件
   # console.log((x => x * 2)())
   
   # 运行
   npx babel-node test.js
   2
   ```

4. @babel/register模块

   `@babel/register`模块改写`require`命令，为它加上一个钩子。此后，每当使用`require`加载`.js`、`.jsx`、`.es`和`.es6`后缀名的文件，就会先用 Babel 进行转码。

   ```js
   // index.js
   require('@babel/register');
   require('./es6.js');
   ```

   

   ```shell
   $ npm install --save-dev @babel/register
   
   # usage
   $ node index.js
   
   # 需要注意的是，@babel/register只会对require命令加载的文件转码，而不会对当前文件转码。另外，由于它是实时转码，所以只适合在开发环境使用。
   ```

5. polyfill

   Babel 默认只转换新的 JavaScript 句法（syntax），而不转换新的 API，比如`Iterator`、`Generator`、`Set`、`Map`、`Proxy`、`Reflect`、`Symbol`、`Promise`等全局对象，以及一些定义在全局对象上的方法（比如`Object.assign`）都不会转码。

   举例来说，ES6 在`Array`对象上新增了`Array.from`方法。Babel 就不会转码这个方法。如果想让这个方法运行，可以使用`core-js`和`regenerator-runtime`(后者提供generator函数的转码)，为当前环境提供一个垫片。

   ```shell
   $ npm install --save-dev core-js regenerator-runtime
   ```

   ```js
   import 'core-js';
   import 'regenerator-runtime/runtime';
   // 或者
   require('core-js');
   require('regenerator-runtime/runtime');
   ```

6. 浏览器环境

   Babel 也可以用于浏览器环境，使用[@babel/standalone](https://babeljs.io/docs/en/next/babel-standalone.html)模块提供的浏览器版本，将其插入网页。

```js
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
// Your ES6 code
</script>
```

注意，网页实时将 ES6 代码转为 ES5，对性能会有影响。生产环境需要加载已经转码完成的脚本。

Babel 提供一个[REPL 在线编译器](https://babeljs.io/repl/)，可以在线将 ES6 代码转为 ES5 代码。转换后的代码，可以直接作为 ES5 代码插入网页运行。

## 2. let和const

### let

- 声明的变量仅在let命令所在的代码块内有效

- 不存在变量提升

- 暂时性死区

  ```js
  var tmp = 123;
  
  // ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
  if (true) {
    tmp = 'abc'; // ReferenceError
    let tmp; // let 绑定在if里面的块级作用域，那么在声明前使用就会报错
  }
  ```

  缺点：

  ```js
  typeof x; // ReferenceError   typeof不再是一个百分百安全的操作
  let x;
  
  // compare
  typeof y // undefined,但不会报错
  ```

- 不允许重复声明

### 块级作用域

https://wangdoc.com/es6/let.html#%E6%9A%82%E6%97%B6%E6%80%A7%E6%AD%BB%E5%8C%BA

