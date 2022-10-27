---
title: JS
date: 2022-10-25 10:20:26
summary: JS
tags: [JS]
categories: [JS]
---

# JS

## 	1.什么是Javascript

​		1995年	→	目的：代替服务端语言处理输入验证

​		Javascript的核心：ECMAScript 、DOM、BOM	

## 	2.语言基础

### 		变量

```js
// 1. var存在变量提升

// 2. 声明的是函数作用域

// 3. 可以通过省略var操作符来定义全局变量。但不推荐
// 他人无法判断此时省略var是不是有意为之，不易理解；且在严格模式下，会导致抛出ReferenceError
message = 'hi'
```

				#### 			let 和 const

```js
// 1. 不存在变量提升（暂时性死区）

// 2. 声明的是块级作用域

// 3. 不能冗余声明，会报错

// 区别：const在声明的时候必须同时初始化变量，且不能修改const声明的变量

// 注意：const声明的限制只适用于它指向的变量的引用。
const person = {}
person.name = 'kim' // 不会报错	
```

### 		数据类型

```js
// 标签函数

let a = 6,
    b = 9
function zipTag(strings, ...expressions) {
	return strings[0] + expressions.map((e, i) => `${e}${strings[i + 1]}`).join('')
}
let untaggedResult = `${ a } + ${ b } = ${ a + b }`

let taggedResult = zipTag`${ a } + ${ b } = ${ a + b }`

console.log(untaggedResult)
console.log(taggedResult)
```

```js
// symbol

// Symbol.isConcatSpreadable + Array.prototype.concat()
// 数组对象、类数组对象、非类数组对象在Symbol.isConcatSpreadable为false的时候会被追加到数组末尾； true时，数组对象、类数组对象都被打平到数组实例，非类数组对象则被忽略

let initial = ['foo']; 

let array = ['bar']; 
console.log(array[Symbol.isConcatSpreadable]); // undefined 
console.log(initial.concat(array)); // ['foo', 'bar'] 
array[Symbol.isConcatSpreadable] = false; 
console.log(initial.concat(array)); // ['foo', Array(1)]

let arrayLikeObject = { length: 1, 0: 'baz' }; 
console.log(arrayLikeObject[Symbol.isConcatSpreadable]); // undefined 
console.log(initial.concat(arrayLikeObject)); // ['foo', {...}] 
arrayLikeObject[Symbol.isConcatSpreadable] = true; 
console.log(initial.concat(arrayLikeObject)); // ['foo', 'baz'] 

let otherObject = new Set().add('qux'); 
console.log(otherObject[Symbol.isConcatSpreadable]); // undefined 
console.log(initial.concat(otherObject)); // ['foo', Set(1)] 
otherObject[Symbol.isConcatSpreadable] = true; 
console.log(initial.concat(otherObject)); // ['foo']
```

```js
// yield

function* test(x) {
  var y = 2 * (yield (x + 1))
  var z = yield (y + 3)
	return (x + y + z)
}

var a = test(5)
a.next() // {value: 6, done: false}
a.next() // {value: NaN, done: false}
a.next() // {value: NaN, done: true}

var b = test(5)
b.next() // {value: 6, done: false}
// 注意next()内的传参 = 上一次yield expression的值
b.next(12) // {value: 8, done: false}
// y = 2 * 12
b.next(13) // {value: 42, done: true}
// z = 13
```

### 操作符

```js
// ++ / -- 
// 前： 代表先执行运算再赋值
// 后： 代表先赋值再运算

// example
let num = 3
let num1 = num-- + 1 
console.log(num1) // 4

let num2 = 3
let num3 = --num2 + 1
console.log(num3) // 3
```

```js
// 计算二进制的负值
// example

// 十进制18的二进制表示方法
// 0000 0000 0000 0000 0000 0000 0001 0010

// 反转（计算补数）
// 1111 1111 1111 1111 1111 1111 1110 1101

//给补数加1（下面的就是-18的二进制表示方法）
// 1111 1111 1111 1111 1111 1111 1110 1110

// 无符号整数比有符号整数的范围更大，因为第32位被用来表示数值了
```

```js
// Math.pow()对应的操作符为 **(指数操作符)

// Math.pow(3, 2) == 3 ** 2

// 指数赋值操作符 **=
let a = 3
a **= 2
console.log(a) // 9
```

```js
// 关系操作符	< > <= >=

// 注意：两个类型为String的比较时，不会转换为Number再比较，而是依次比较每一位的unicode编码大小
console.log("23" < "3") // true
console.log("23" < 3) // false

// 注意：NaN与任何值比较都返回false
console.log(NaN < 3) // false
console.log(NaN >= 3) // false
```

## 3. 变量、作用域与内存

### 3.1 原始值与引用值

简单数据类型（String、Number、Boolean、Undefined、Null、Symbol）是按值传递；

复杂数据类型（Object）是按引用传递；

函数的参数是按值传递的。

### 3.2 垃圾回收

1：标记清理

垃圾回收程序运行的时候，会标记内存中存储的所有变量（记住，标记方法有很多种）。然后，它会将所有在上下文中的变量，以及被在上下文中的变量引用的变量的标记去掉。在此之后再被加上标记的变量就是待删除的了，原因是任何在上下文中的变量都访问不到它们了。随后垃圾回收程序做一次内存清理，销毁带标记的所有值并收回它们的内存。

2: 引用计数

对每个值都记录它被引用的次数。声明变量并给它赋一个引用值时，这个值的引用数为 1。如果同一个值又被赋给另一个变量，那么引用数加 1。类似地，如果保存对该值引用的变量被其他值给覆盖了，那么引用数减 1。当一个值的引用数为 0 时，就说明没办法再访问到这个值了，因此可以安全地收回其内存了。垃圾回收程序下次运行的时候就会释放引用数为 0 的值的内存。

缺点：循环引用；	解决办法：改用标记清理

![image-20221026181255471.png](https://s2.loli.net/2022/10/26/GP3lcAO79Qzapju.png)

## 4.基本引用类型

### 4.1 Date

```js
// Date类型的valueof()方法不返回字符串，返回的是被重写后的时期的毫秒数。
// 因此操作符可以直接使用Date类型的值来比较日期先后

let date1 = new Date(2019, 0, 1) // 2019年1月1日
let date2 = new Date(2019, 1, 1) // 2019年2月1日

console.log(date1 < date2) // true
console.log(date1 > date2) // false


// Date类型的getTimezoneOffset()可返回以分钟计的UTC与本地时区的偏移量

let now = new Date()
console.log(now.getTimezoneOffset()) // -480 代表比UTC时间快480分钟
```

### 4.2 原始值包装类型Boolean

```js
// 所有对象在布尔表达式中在会转为true
let falseObj = new Boolean(false)
let result = falseObj && true
console.log(result) // true
```

### 4.3 原始值包装类型的String

let stringValue = 'hello world'

|             |                          参数为正数                          |                          参数为负数                          |
| :---------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|   slice()   | （startIndex, endIndex）<br />stringValue.slice(3, 7) → 'lo w' |       正常行为<br />stringValue.slice(3, -4) → ‘lo w'        |
|  substr()   | （startIndex, 返回的字符串长度）<br />stringValue.substr(3, 7) → 'lo worl' | 参数1（字符串长度+该值），参数2（转为0）<br />stringValue.substr(3， -4) → ‘empty string’ |
| substring() | （startIndex, endIndex）<br />stringValue.slice(3, 7) → 'lo w' |       都转为0<br />stringValue.substr(3， -4) → ‘hel’        |

159
