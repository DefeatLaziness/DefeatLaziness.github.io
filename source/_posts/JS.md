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

## 5. 继承

```js
// 原型与实例的关系
// 1.可通过instanceof来确定
console.log([实例] instanceof [原型])
// true: 代表原型在实例的原型链上出现过; 反之

// 2.可通过isPrototypeOf()来确定， 由原型调用
console.log([原型].prototype.isPrototypeOf([实例]))
// true: 代表实例的原型链上包含此原型; 反之
```

- 子类需要覆盖父类的方法或增加父类没有的方法， 需要在原型赋值后再执行操作

  ```js
  function SuperType() { 
   this.property = true; 
  } 
  SuperType.prototype.getSuperValue = function() { 
   return this.property; 
  }; 
  function SubType() { 
   this.subproperty = false; 
  } 
  // 继承 SuperType 
  SubType.prototype = new SuperType(); 
  // 新方法
  SubType.prototype.getSubValue = function () { 
   return this.subproperty; 
  }; 
  // 覆盖已有的方法
  SubType.prototype.getSuperValue = function () { 
   return false; 
  }; 
  let instance = new SubType(); 
  console.log(instance.getSuperValue()); // false
  ```

- 以对象字面量的方式创建原型方法会破坏之前的原型

  ```js
  function SuperType() { 
   this.property = true; 
  } 
  SuperType.prototype.getSuperValue = function() { 
   return this.property; 
  }; 
  function SubType() { 
   this.subproperty = false; 
  }
  // 继承 SuperType 
  SubType.prototype = new SuperType(); 
  // 通过对象字面量添加新方法，这会导致上一行无效
  SubType.prototype = { 
   getSubValue() { 
   return this.subproperty; 
   }, 
   someOtherMethod() { 
   return false; 
   } 
  }; 
  let instance = new SubType(); 
  console.log(instance.getSuperValue()); // 出错！
  ```

### 5.1 原型链问题

- 原型链中的引用值会在所有实例间共享

  ```js
  function SuperType() { 
   this.colors = ["red", "blue", "green"]; 
  } 
  function SubType() {} 
  // 继承 SuperType 
  SubType.prototype = new SuperType(); 
  let instance1 = new SubType(); 
  instance1.colors.push("black"); 
  console.log(instance1.colors); // "red,blue,green,black" 
  let instance2 = new SubType(); 
  console.log(instance2.colors); // "red,blue,green,black"
  ```

- 原型链的第二个问题是，子类型在实例化时不能给父类型的构造函数传参。事实上，我们无法在不 影响所有对象实例的情况下把参数传进父类的构造函数。再加上之前提到的原型中包含引用值的问题， 就导致原型链基本不会被单独使用。

### 5.2 继承方式

1. 盗用构造函数（也叫对象伪装/经典继承）

   ```js
   // 实现思路：在子类构造函数中调用父类构造函数
   function SuperType() { 
    this.colors = ["red", "blue", "green"]; 
   } 
   function SubType() { 
    // 使用apply()/call()继承 SuperType 
    SuperType.call(this); 
   } 
   let instance1 = new SubType(); 
   instance1.colors.push("black"); 
   console.log(instance1.colors); // "red,blue,green,black" 
   let instance2 = new SubType(); 
   console.log(instance2.colors); // "red,blue,green" 
   
   // 优点: 在子类构造函数中向父类构造函数传参
   function SuperType(name){ 
    this.name = name; 
   } 
   function SubType() { 
    // 继承 SuperType 并传参
    SuperType.call(this, "Nicholas"); 
    // 实例属性
    this.age = 29; 
   } 
   let instance = new SubType(); 
   console.log(instance.name); // "Nicholas"; 
   console.log(instance.age); // 29
   
   // 缺点：
   // 1.只能继承父类构造函数的属性，不能继承方法
   // 2.无法实现构造函数的复用（每次用每次都要重新调用）
   ```

2. 组合继承（也叫伪经典继承）

   ```js
   // 实现思路：使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性
   function SuperType(name){ 
    this.name = name; 
    this.colors = ["red", "blue", "green"]; 
   } 
   SuperType.prototype.sayName = function() { 
    console.log(this.name); 
   }; 
   function SubType(name, age){ 
    // 继承属性
    SuperType.call(this, name); 
    this.age = age; 
   } 
   // 继承方法
   SubType.prototype = new SuperType(); 
   SubType.prototype.sayAge = function() { 
    console.log(this.age); 
   }; 
   let instance1 = new SubType("Nicholas", 29); 
   instance1.colors.push("black"); 
   console.log(instance1.colors); // "red,blue,green,black" 
   instance1.sayName(); // "Nicholas"; 
   instance1.sayAge(); // 29 
   let instance2 = new SubType("Greg", 27); 
   console.log(instance2.colors); // "red,blue,green" 
   instance2.sayName(); // "Greg"; 
   instance2.sayAge(); // 27
   
   // 优点: 弥补了原型链和盗用构造函数的不足，是 JavaScript 中使用最多的继承模式。而且组合继承也保留了 instanceof 操作符和 isPrototypeOf()方法识别合成对象的能力。
   // 缺点: 最主要的效率问题就是父类构造函数始终会被调用两次：一次在是创建子类原型时调用，另一次是在子类构造函数中调用。
   ```

3. 原型式继承

   ```js
   function object(o) { 
    function F() {} 
    F.prototype = o; 
    return new F(); 
   } 
   // 这个 object()函数会创建一个临时构造函数，将传入的对象赋值给这个构造函数的原型，然后返回这个临时类型的一个实例。本质上，object()是对传入的对象执行了一次浅复制。来看下面的例子：
   let person = { 
    name: "Nicholas", 
    friends: ["Shelby", "Court", "Van"] 
   }; 
   let anotherPerson = object(person); 
   anotherPerson.name = "Greg"; 
   anotherPerson.friends.push("Rob"); 
   let yetAnotherPerson = object(person); 
   yetAnotherPerson.name = "Linda"; 
   yetAnotherPerson.friends.push("Barbie"); 
   console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie" 
   
   // 使用场景： 你有一个对象，想在它的基础上再创建一个新对象。你需要把这个对象先传给 object()，然后再对返回的对象进行适当修改。
   
   // ECMAScript 5 通过增加 Object.create()方法将原型式继承的概念规范化了。这个方法接收两个参数：作为新对象原型的对象，以及给新对象定义额外属性的对象（第二个可选）。在只有一个参数时，Object.create()与这里的 object()方法效果相同：
   
   let person = { 
    name: "Nicholas", 
    friends: ["Shelby", "Court", "Van"] 
   }; 
   let anotherPerson = Object.create(person); 
   anotherPerson.name = "Greg"; 
   anotherPerson.friends.push("Rob"); 
   let yetAnotherPerson = Object.create(person); 
   yetAnotherPerson.name = "Linda"; 
   yetAnotherPerson.friends.push("Barbie"); 
   console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie" 
   
   // Object.create()的第二个参数与 Object.defineProperties()的第二个参数一样：每个新增属性都通过各自的描述符来描述。以这种方式添加的属性会遮蔽原型对象上的同名属性。比如：
   let person = { 
    name: "Nicholas", 
    friends: ["Shelby", "Court", "Van"] 
   }; 
   let anotherPerson = Object.create(person, { 
    name: { 
    value: "Greg" 
    } 
   }); 
   console.log(anotherPerson.name); // "Greg"
   
   // 注意: 原型式继承非常适合不需要单独创建构造函数，但仍然需要在对象间共享信息的场合。但要记住，属性中包含的引用值始终会在相关对象间共享，跟使用原型模式是一样的。
   ```

4. 寄生式继承

   ```js
   // 实现思路: 类似于寄生构造函数和工厂模式：创建一个实现继承的函数，以某种方式增强对象，然后返回这个对象。
   
   function object(o) { 
    function F() {} 
    F.prototype = o; 
    return new F(); 
   } 
   
   function createAnother(original){ 
    let clone = object(original); // 通过调用函数创建一个新对象
    clone.sayHi = function() { // 以某种方式增强这个对象
    console.log("hi"); 
    }; 
    return clone; // 返回这个对象
   }
   
   let person = { 
    name: "Nicholas", 
    friends: ["Shelby", "Court", "Van"] 
   }; 
   let anotherPerson = createAnother(person); 
   anotherPerson.sayHi(); // "hi" 
   
   // 寄生式继承同样适合主要关注对象，而不在乎类型和构造函数的场景。object()函数不是寄生式继承所必需的，任何返回新对象的函数都可以在这里使用。
   // 注意: 通过寄生式继承给对象添加函数会导致函数难以重用，与构造函数模式类似。
   ```

5. 寄生式组合继承

   ```js
   // 解决组合继承的缺点：父类构造函数始终会被调用两次
   
   // 基本思路是不通过调用父类构造函数给子类原型赋值，而是取得父类原型的一个副本。说到底就是使用寄生式继承来继承父类原型，然后将返回的新对象赋值给子类原型。
   
   // 这个函数接收两个参数：子类构造函数和父类构造函数。在这个函数内部，第一步是创建父类原型的一个副本。然后，给返回的prototype 对象设置 constructor 属性，解决由于重写原型导致默认 constructor 丢失的问题。最后将新创建的对象赋值给子类型的原型。
   function inheritPrototype(subType, superType) { 
    let prototype = object(superType.prototype); // 创建对象
    prototype.constructor = subType; // 增强对象 
    subType.prototype = prototype; // 赋值对象
   } 
   
   function SuperType(name) { 
    this.name = name; 
    this.colors = ["red", "blue", "green"]; 
   } 
   SuperType.prototype.sayName = function() { 
    console.log(this.name); 
   }; 
   function SubType(name, age) { 
    SuperType.call(this, name); 
    this.age = age; 
   } 
   inheritPrototype(SubType, SuperType); 
   SubType.prototype.sayAge = function() { 
    console.log(this.age); 
   };
   
   // 这里只调用了一次 SuperType 构造函数，避免了 SubType.prototype 上不必要也用不到的属性，因此可以说这个例子的效率更高。而且，原型链仍然保持不变，因此 instanceof 操作符和isPrototypeOf()方法正常有效。寄生式组合继承可以算是引用类型继承的最佳模式。
   ```

   ## 6. Class

   ### 6.1 new实例化

   1. 在内存中创建一个新对象
   2. 这个新对象的[ [ Prototype ] ]指针被赋值为构造函数的prototype属性
   3. 构造函数内部的this被赋值为这个新对象（即this指向新对象）
   4. 执行构造函数内部的代码（给新对象添加属性）
   5. 如果构造函数返回非空对象，则返回该对象；否则返回刚创建的新对象



