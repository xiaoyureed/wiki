---
title: javascript-note-js
tags:
---

<!--more-->

# js 的动态执行代码

https://www.jianshu.com/p/905151465a60

# 语法

## undefined 和 null

```js
// undefined & null
// 在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined (即未定义的变量)。 转为数值时为NaN
// null值则是表示空对象指针 (即空指针), 转为数字是 0
Number(null)
// 0

5 + null
// 5

Number(undefined)
// NaN

5 + undefined
// NaN

// null表示"没有对象"，即该处不应该有值, 是故意设置的空, 比如作为函数参数
// undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义, 是意外出现的空
//      如: 变量被声明了，但没有赋值时，就等于 undefined
//          调用函数时，应该提供的参数没有提供
//          对象没有赋值的属性，该属性的值为 undefined
//          函数没有返回值时，默认返回 undefined。
//          
```

## 判空

```js
// 五种空值和假值，分别为 undefined，null，false，""，0
// 
// error
let a = 0;
console.log(a || '/');//本意是只要 a 为 null 或者 Undefined 的时候，输出 '/'，但实际上只要是我们以上的五种之一就输出 '/'

// 不够简单
let a = 0;
if (a === null || a === undefined) {
  console.log('/');
} else {
  console.log(a);
}

// 更简单
// 空值合并操作符（??）:当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数
let a = 0;
console.log(a??'a=== null/undefined'); // 0

```

## 比较浮点数

```js
// math.js，decimal.js,D.js 三方库

// 或者
function withinErrorMargin (left, right) {
    return Math.abs(left - right) < Number.EPSILON//极小的常量——Number.EPSILON, 如果误差能够小于 Number.EPSILON，我们就可以认为结果是可靠的
}
withinErrorMargin(0.1+0.2, 0.3)
```

## Symbol

表示独一无二的标识, 用来定义对象的唯一属性名

```js
// 可以接受一个字符串作为参数，为新创建的 Symbol 提供描述
let sy = Symbol("KK");
console.log(sy);   // Symbol(KK)
typeof(sy);        // "symbol"
 
// 相同参数 Symbol() 返回的值不相等
let sy1 = Symbol("kk"); 
sy === sy1;       // false

let syObject = {};
syObject[sy] = "kk";
 
syObject[sy];  // "kk"
syObject.sy;   // undefined, 因为.运算符后面是字符串，所以取到的是字符串 sy 属性，而不是 Symbol 值 sy 属性

// Symbol 值作为属性名时，该属性是公有属性不是私有属性，可以在类的外部访问。
// 但是不会出现在 for...in 、 for...of 的循环中，也不会被 Object.keys() 、 Object.getOwnPropertyNames() 返回。
// 如果要读取到一个对象的 Symbol 属性，可以通过 Object.getOwnPropertySymbols() 和 Reflect.ownKeys() 取到
```