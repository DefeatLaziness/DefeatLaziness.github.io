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

#### 			var

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

page 75
