---
title: 非权威指南-javaScript严格模式
header-img: back.png
header-color: 'rgb(65,57,50)'
tag-color: 'rgb(65,57,50)'
catalog: true
toc-number: false
date: 2020-02-19 18:30:11
tags: ['javaScript', '严格模式']
---

## 1. 什么是严格模式

严格模式是 ECMAScript5 引入的采用具有限制性 JavaScript 变体的一种方式。它不仅仅是一个子集，它还对 JavaScript 正常的语义做了一些更改：

- 通过抛出错误消除了原来的一些静默错误
- 修复了一些导致 JavaScript 引擎难以优化的缺陷： 有时候相同的代码严格模式可以比非严格模式运行得更快
- 严格模式禁用了在未来的 ECMAScript 标准中可能定义的一些语法

严格模式代码可以和非严格模式代码共存，因此项目脚本可以渐进式地采用严格模式

## 2. 使用严格模式

严格模式可以用于整个脚本或者单个函数

### 2.1 为脚本开启严格模式

为整个脚本开启严格模式只需要在所有语句前放一个特定字符串语句 `use strict`

```js
'use strict'  // "use strict"
const a = 1
...
```

### 2.2 为单个函数开启严格模式

把`use strict` 放在函数体所有语句之前即可

```js
function fn() {
  'use strict'
  ...  
}
```

## 3. 严格模式和非严格模式的区别

### 3.1 将过失错误转换为异常

#### *无法给未声明变量赋值

这样可以防止意外得修改创建全局变量

```js
'use strict'
var test = 1 
tesy = 2	// ReferenceError: tesy is not defined
```

#### *使静默失败的赋值操作抛出异常

静默失败指的是不报错也没有任何效果，例如:

- 给不可写的属性赋值
- 给不可拓展对象的新属性赋值等

```js
'use strict'
// 给不可写属性赋值
var obj1 = {};
Object.defineProperty(obj1, "x", { value: 1, writable: false });
obj1.x = 9; // TypeError: Cannot assign to read only property 'x' of object

// 给不可拓展对象新属性赋值
var fixed = {};
Object.preventExtensions(fixed);
fixed.newProp = "ohai"; // TypeError: Cannot add property newProp, object is not extensible
```

#### *删除不可删除属性时抛出异常

```js
'use strict'
delete Object.prototype  // TypeError
```

#### *要求函数参数名唯一

```js
"use strict";
function fn (a, a){ 	// SyntaxError
    ...
}
```

#### *禁止以0为前导的八进制数字语法

```js
'use strict'
const a = 015
console.log(a)  // SyntaxError
```

#### *禁止设置基本类型的属性

```js
'1'.a = 1  // TypeError
```

### 3.2 简化变量的使用

严格模式简化了代码中变量名字映射到变量定义的方式。很多编译器的优化依赖存储变量 x 的位置，而 JavaScript 有些情况会使得变量名字到定义的映射只在运行时才产生。严格模式移除了大多这种情况的产生，所以编译器可以更好得优化严格模式的代码。‘

#### * 禁用 with

```js
'use strict'
const a = 1
const obj = {
  a: 2
}	
with (obj) {	// SyntaxError
  console.log(a)
}
```

#### * 禁止删除声明变量

```js
'use strict'
var a
delete a  // SyntaxError
```

#### * arguments[i] 的值不会随与之相应的参数的值的改变而变化 

```js
'use strict'
function fn (a) {
  a = 13
  console.log(a, arguments[0]);
}
 fn(1)	// 13 1
```

#### * 禁用 arguments.callee

```js
'use strict'
function fn() {
  return arguments.callee
}
fn()	// TypeError
```

### 3.3 为未来的 ECMAScript 版本铺平道路

#### * 添加了一部分保留字

`implements`,`interface`,`let`, `package`, `private`, `protected`, `public`, `static`和`yield` 

#### * 禁止不在脚本和函数层面声明函数（自己测试可以）

