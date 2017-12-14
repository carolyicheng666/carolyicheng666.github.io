---
title: JavaScript 高阶函数之 map、filter、reduce
date: 2017-12-14 11:10:28
tags: [JavaScript]
categories: '编程'
mathjax: true
---



{% cq %}
月色如水，虽不如阳光般耀眼，但我的眼中却只有比那月色更可爱的你  
心跳不知不觉中加速，嘴角也绷不住那溢出来的幸福笑意  
月色确实醉人，却是因为你才显得更美

**月色真美**
{% endcq %}


<!-- more -->


map()
---

{% note danger %}
map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。
{% endnote %}

**语法：**
``` javascript
let new_array = arr.map(function callback(currentValue, index, array) { 
    // Return element for new_array 
}[, thisArg])
```
*currentValue*表示arr当前元素的值  
*index*表示arr当前元素的索引  
*array*表示arr数组本身  
*thisArg*可选参数，表示执行回调函数时使用的 this 值  
返回值是一个新数组，每个元素都是回调函数的结果

举个例子吧，比如说现在我们有这样一个需求：假设现在有一个事先准备好的数组，我们要产生一个新的数组，并且这个新的数组里每一个元素的值是之前准备好的数组元素值的平方，简单的说就是要实现 $f(x)=x^2$ 这样一个效果  
我们先来看看原始的做法：
``` javascript
var f = function (x) {
  return x * x;
};

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var result = [];
for (var i=0; i<arr.length; i++) {
  result.push(f(arr[i]));
}
```
使用 map() 的做法：
``` javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(function(x) {
  return x * x;
});
```
对比看看，使用 map() 函数大大简化了我们的代码，它把运算抽象到它内部的回调函数中，可能在上面的例子中优势并没有体现出来，但是一旦处理起复杂问题时，它的优势会变得非常明显




filter
---

{% note danger %}
filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 
{% endnote %}

**语法：**
``` javascript
var new_array = arr.filter(function callback(element, index, array) { 
    // Return true to keep the element
}[, thisArg])
```
filter() 的所有参数及含义与 map() 的一致，唯一不同的是：filter() 把传入的函数依次作用于每个元素，然后根据返回值是 true 还是 false 决定保留还是丢弃该元素。刚入门的朋友可能会有疑惑，我举个例子吧：假设有一个对象数组，我想筛选出某一属性值作为新的数组，这种情况 filter() 是无法做到的，请看下面的代码
``` javascript
const arr = [
  { a: 1, b: 10 },
  { a: 2, b: 15 },
  { a: 3, b: 20 },
  { a: 4, b: 25 },
];
// map 能正确执行，而 filter 不能正确执行
var result = arr.map(function(x) {
  return x.b;
});
```
照例还是举个例子：假设我们需要从一个数组筛选出其中所有的素数，并合并成一个新的数组  
原始的写法：
``` javascript
function get_primes(x) {
  if (x === 1) return false;
  for (let i = 2; i <= Math.sqrt(x); i++) {
    if (x % i === 0) return false;
  }
  return true;
}

var arr = [], result = [];
for (let i = 1; i < 100; i++) {
  arr.push(i);
}

for (let i = 0; i < arr.length; i++) {
  if (get_primes(arr[i])) {
    result.push(arr[i]);
  }
}
console.log(result);
```
使用 filter() 的写法：
``` javascript
var arr = [];
for (let i = 1; i < 100; i++) {
  arr.push(i);
}

var result = arr.filter(function(e) {
  if (e === 1) return false;
  for (let i = 2; i <= Math.sqrt(e); i++) {
    if (e % i === 0) return false;
  }
  return true;
});
console.log(result);
```



reduce
---

{% note danger %}
reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。
{% endnote %}

**语法：**
``` javascript
var result = arr.reduce(function callback(accumulator, currentValue, currentIndex, array) {
  // Return accumulator
}[, initialValue]);
```
*accumulator*表示最后的返回值，它也是上一次调用回调时返回的累积值。有两种情况：第一种情况如果设置了 initialValue ，则第一次的 accumulator 值就为 initialValue，而 currentValue 的值就为数组的第一个值；第二种情况如果没有设置 initialValue ，则第一次的 accumulator 值为数组的第一个值，而 currentValue 的值就为数组的第二个值  
*currentValue*表示当前数组元素的值  
*currentIndex*表示当前数组元素的索引  
*array*表示arr数组本身  
*initialValue*可选参数，用作第一个调用回调函数的第一个参数的值，如果没有提供初始值，则将使用数组中的第一个元素。**特别注意**，在没有初始值的空数组上调用 reduce 将报错

这个函数就比较好理解了，我们来举个例子：求一个数组所有元素的积  
原始写法：
``` javascript
function product(x, y) {
  return x * y;
}

var arr = [1, 2, 3, 4, 5];
var result = 1;
for (var i=0; i<arr.length; i++) {
  result *= arr[i];
}
```
reduce() 写法：
``` javascript
var arr = [1, 2, 3, 4, 5];
var result = arr.reduce(function(x, y) {
  return x * y;
});
```


综合实例
---

1、将一个数字字符串变成一个整数数字，不能使用parseInt
``` javascript
var result = s.split('').map(function(x) { return +x }).reduce(function (x, y) { return x*10+y });
```

2、计算一个对象数组students中分数在10分以上的分数总和
``` javascript
const students = [
  { name: "Nick", grade: 10 },
  { name: "John", grade: 15 },
  { name: "Julia", grade: 19 },
  { name: "Nathalie", grade: 9 },
];
const aboveTenSum = students
  .map(student => student.grade)
  .filter(grade => grade >= 10)
  .reduce((prev, next) => prev + next, 0);
console.log(aboveTenSum);
```
