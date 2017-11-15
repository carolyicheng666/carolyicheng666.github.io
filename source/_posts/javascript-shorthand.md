---
title: JavaScript的一些简写技巧
date: 2017-11-15 15:23:36
tags: [JavaScript, ES6]
categories: '编程'
---

{% cq %}
逃避有两种：我们把有目的的逃避叫做飞翔，把没有目的的逃避叫做漂浮。

**空之境界**
{% endcq %}

<!-- more -->



三目运算符
---

``` javascript
const x = 20;
let answer;
if (x > 10) {
  answer = 'greater than 10';
} else {
  answer = 'less than 10';
}
```
简写为：
``` javascript
const answer = x > 10 ? 'greater than 10' : 'less than 10';
```



变量赋值
---

这里原文有点问题，下面代码能执行的条件是如果 `variable1` 不存在必须先声明，我们知道，在 `javascript` 中已存在或声明后的变量如果不被赋值，那么它的值应该是 `undefined` ，所以下面的代码简化前后输出 `variable2` 的值一定不相等，应该去掉 `variable1 !== undefined` 这一判断项，或者直接改成注释中内容即可：
``` javascript
if (variable1 !== null || variable1 !== undefined || variable1 !== '') { // if (variable)
  let variable2 = variable1;
} else {
  let variable2 = 'new';
}
```
简写为：
``` javascript
const variable2 = variable1  || 'new';
```



声明变量
---

在函数开始之前声明变量是很好的习惯。在同时声明多个变量时，用下面的方法可以节省大量的时间和空间：
``` javascript
let x;
let y;
let z = 3;
```
简写为：
``` javascript
let x, y, z=3;
```



if语句
---

``` javascript
if (likeJavaScript === true) { ... }
if (a !== true) { ... }
```
简写为：
``` javascript
if (likeJavaScript) { ... }
if (!a) { ... }
```



循环语句
---

如果不依赖外部库（如 `jQuery` 或 `lodash`），下面的小技巧会非常有用：
``` javascript
for (let i = 0; i < allImgs.length; i++)
```
简写为：
``` javascript
for (let index of allImgs)
```
forEach遍历数组简写：
``` javascript
function logArrayElements(element, index, array) {
  console.log("a[" + index + "] = " + element);
}
[2, 5, 9].forEach(logArrayElements);
// logs:
// a[0] = 2
// a[1] = 5
// a[2] = 9
```



十进制数
---

``` javascript
for (let i = 0; i < 10000; i++) {}
```
简写为：
``` javascript
for (let i = 0; i < 1e4; i++) {}

// All the below will evaluate to true
1e0 === 1;
1e1 === 10;
1e2 === 100;
1e3 === 1000;
1e4 === 10000;
1e5 === 100000;
```



对象属性
---

`ES6` 提供了一个很简单的办法，来分配属性的对象。如果属性名与 `key` 名相同，则可以使用简写：
``` javascript
const obj = { x:x, y:y };
```
简写为：
``` javascript
const obj = { x, y };
```



箭头函数
---

``` javascript
function sayHello(name) {
  console.log('Hello', name);
}

setTimeout(function() {
  console.log('Loaded')
}, 2000);

list.forEach(function(item) {
  console.log(item);
});
```
简写为：
``` javascript
sayHello = name => console.log('Hello', name);

setTimeout(() => console.log('Loaded'), 2000);

list.forEach(item => console.log(item));
```



隐式返回
---

返回值是我们通常用来返回函数最终结果的关键字。只有一条语句的箭头函数，可以隐式返回结果（函数必须省略大括号 `{}` ，以便省略返回关键字）。
要返回多行语句（例如对象文本），需要使用 `()` 而不是 `{}` 来包裹函数体。这样可以确保代码以单个语句的形式进行求值。
``` javascript
function calcCircumference(diameter) {
  return Math.PI * diameter
}
```
简写为：
``` javascript
calcCircumference = diameter => (
  Math.PI * diameter;
)
```



参数默认值
---

可以使用 `if` 语句来定义函数参数的默认值。`ES6` 中规定了可以在函数声明中定义默认值。
``` javascript
function volume(l, w, h) {
  if (w === undefined)
    w = 3;
  if (h === undefined)
    h = 4;
  return l * w * h;
}
```
简写为：
``` javascript
volume = (l, w = 3, h = 4 ) => (l * w * h);
```



字符串模板
---

过去我们习惯了使用 `+` 将多个变量转换为字符串，但是有没有更简单的方法呢？
`ES6` 提供了相应的方法，我们可以使用反引号和 `${}` 将变量合成一个字符串。
``` javascript
const welcome = 'You have logged in as ' + first + ' ' + last + '.'

const db = 'http://' + host + ':' + port + '/' + database;
```
简写为：
``` javascript
const welcome = `You have logged in as ${first} ${last}`;

const db = `http://${host}:${port}/${database}`;
```



解构赋值
---

解构赋值是一种表达式，用于从数组或对象中快速提取属性值，并赋给定义的变量。
在代码简写方面，解构赋值能达到很好的效果。
``` javascript
const observable = require('mobx/observable');
const action = require('mobx/action');
const runInAction = require('mobx/runInAction');

const store = this.props.store;
const form = this.props.form;
const loading = this.props.loading;
const errors = this.props.errors;
const entity = this.props.entity;
```
简写为：
``` javascript
import { observable, action, runInAction } from 'mobx';

const { store, form, loading, errors, entity } = this.props;
```
也可以自定义变量名：
``` javascript
const { store, form, loading, errors, entity:contact } = this.props;
```



多行字符串
---

``` javascript
const lorem = 'Lorem ipsum dolor sit amet, consectetur\n\t'
    + 'adipisicing elit, sed do eiusmod tempor incididunt\n\t'
    + 'ut labore et dolore magna aliqua. Ut enim ad minim\n\t'
    + 'veniam, quis nostrud exercitation ullamco laboris\n\t'
    + 'nisi ut aliquip ex ea commodo consequat. Duis aute\n\t'
    + 'irure dolor in reprehenderit in voluptate velit esse.\n\t'
```
简写为：
``` javascript
const lorem = `Lorem ipsum dolor sit amet, consectetur
    adipisicing elit, sed do eiusmod tempor incididunt
    ut labore et dolore magna aliqua. Ut enim ad minim
    veniam, quis nostrud exercitation ullamco laboris
    nisi ut aliquip ex ea commodo consequat. Duis aute
    irure dolor in reprehenderit in voluptate velit esse.`
```



字符串展开
---

展开运算符是在 `ES6` 中引入的，使用展开运算符可以替换某些数组函数。
``` javascript
// joining arrays
const odd = [1, 3, 5];
const nums = [2 ,4 , 6].concat(odd);

// cloning arrays
const arr = [1, 2, 3, 4];
const arr2 = arr.slice()
```
简写为：
``` javascript
// joining arrays
const odd = [1, 3, 5 ];
const nums = [2 ,4 , 6, ...odd];
console.log(nums); // [ 2, 4, 6, 1, 3, 5 ]

// cloning arrays
const arr = [1, 2, 3, 4];
const arr2 = [...arr];
```
和 `concat()` 功能不同的是，用户可以使用扩展运算符在任何一个数组中插入另一个数组。
``` javascript
const odd = [1, 3, 5 ];
const nums = [2, ...odd, 4 , 6];
```
也可以将展开运算符和 `ES6` 解构符号结合使用：
``` javascript
const { a, b, ...z } = { a: 1, b: 2, c: 3, d: 4 };
console.log(a) // 1
console.log(b) // 2
console.log(z) // { c: 3, d: 4 }
```



强制参数
---

默认情况下，如果不向函数参数传值，那么 `JavaScript` 会将函数参数设置为未定义，其它一些语言则会发出警告或错误。要执行参数分配，可以使用 `if` 语句抛出未定义的错误，或者可以利用“强制参数”。
``` javascript
function foo(bar) {
  if(bar === undefined) {
    throw new Error('Missing parameter!');
  }
  return bar;
}
```
简化为：
``` javascript
mandatory = () => {
  throw new Error('Missing parameter!');
}

foo = (bar = mandatory()) => {
  return bar;
}
```



Array.find
---

如果你曾经编写过普通 `JavaScript` 中的 `find` 函数，那么你可能使用了 `for` 循环。在 `ES6` 中，介绍了一种名为 `find()` 的新数组函数，可以实现 `for` 循环的简写。
``` javascript
const pets = [
  { type: 'Dog', name: 'Max'},
  { type: 'Cat', name: 'Karl'},
  { type: 'Dog', name: 'Tommy'},
]

function findDog(name) {
  for(let i = 0; i<pets.length; ++i) {
    if(pets[i].type === 'Dog' && pets[i].name === name) {
      return pets[i];
    }
  }
}
```
简写为：
``` javascript
pet = pets.find(pet => pet.type ==='Dog' && pet.name === 'Tommy');
console.log(pet); // { type: 'Dog', name: 'Tommy' }
```



Object[key]
---

虽然将 foo.bar 写成 foo ['bar'] 是一种常见的做法，但是这种做法构成了编写可重用代码的基础。
``` javascript
function validate(values) {
  if(!values.first)
    return false;
  if(!values.last)
    return false;
  return true;
}

console.log(validate({first:'Bruce',last:'Wayne'})); // true
```
上面的函数完美的完成验证工作。但是当有很多表单，则需要应用验证，此时会有不同的字段和规则。如果可以构建一个在运行时配置的通用验证函数，会是一个好选择。
``` javascript
// object validation rules
const schema = {
  first: {
    required:true
  },
  last: {
    required:true
  }
}

// universal validation function
const validate = (schema, values) => {
  for(field in schema) {
    if(schema[field].required) {
      if(!values[field]) {
        return false;
      }
    }
  }
  return true;
}


console.log(validate(schema, {first:'Bruce'})); // false
console.log(validate(schema, {first:'Bruce',last:'Wayne'})); // true
```
现在有了这个验证函数，我们就可以在所有窗体中重用，而无需为每个窗体编写自定义验证函数。



双位操作符
---

位操作符是 `JavaScript` 初级教程的基本知识点，但是我们却不常使用位操作符。因为在不处理二进制的情况下，没有人愿意使用 1 和 0。
但是双位操作符却有一个很实用的案例。你可以使用双位操作符来替代 `Math.floor()` 。双否定位操作符的优势在于它执行相同的操作运行速度更快。
``` javascript
Math.floor(4.9) === 4  //true
```
简写为：
``` javascript
~~4.9 === 4  //true
```



参考文献
---
[<span style="color: red;">原文链接</span>](https://www.sitepoint.com/shorthand-javascript-techniques/)