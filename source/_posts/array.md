---
title: JavaScript 数组方法
date: 2018-08-03 21:51:04
tags: [JavaScript]
categories: '编程'
---

{% cq %}
越是美好的回忆，有时可能会令人越痛苦  
幸福和美好的回忆并不一定是救赎

**可塑性记忆**
{% endcq %}

<!-- more -->

# 转换方法

- ## join()
  除了继承而来的 toLocaleString()、toString() 和 valueOf() 方法，数组还有自己的 join() 方法。join() 方法只接收一个参数，即用作分隔符的字符串，然后返回包含所有数组项的字符串。如果不给 join() 方法传入任何参数，或者传入 undefined ，则使用逗号作为分隔符，与前面继承而来的三个方法返回结果一致。
  ``` javascript
  var colors = ["red", "green", "blue"]
  console.log(colors.join()) // red,green,blue
  console.log(colors.join("|")) // red|green|blue
  ```

# 栈方法

- ## push()
  push() 方法接收任意数量的参数，把它们逐个添加到数组的末尾，并返回修改后数组的长度。
  ``` javascript
  var colors = ["red", "green", "blue"]
  var count = colors.push("black", "yellow")
  console.log(count) // 5
  console.log(colors) // [ 'red', 'green', 'blue', 'black', 'yellow' ]
  ```

- ## pop()
  pop() 方法从数组末尾移除最后一项，减少数组的长度，然后返回移除的项。
  ``` javascript
  var colors = ["red", "green", "blue"]
  var item = colors.pop()
  console.log(item) // 'blue'
  console.log(colors) // [ 'red', 'green' ]
  ```

# 队列方法

- ## shift()
  shift() 方法从数组头部移除第一项，减少数组的长度，然后返回移除的项。
  ``` javascript
  var colors = ["red", "green", "blue"]
  var item = colors.shift()
  console.log(item) // 'red'
  console.log(colors) // [ 'green', 'blue' ]
  ```

- ## unshift()
  unshift() 方法接收任意数量的参数，并在数组的前端添加项，并返回修改后数组的长度。这里是以队列方式添加，注意与push()添加的区别。
  ``` javascript
  var colors = ["red", "green", "blue"]
  var count = colors.unshift("black", "yellow")
  console.log(count) // 5
  console.log(colors) // [ 'black', 'yellow', 'red', 'green', 'blue' ]
  ```

# 重排序方法

- ## reserve()
  reserve() 方法用于反转数组的顺序。
  ``` javascript
  var nums = [1, 3, 2, 5, 4]
  console.log(nums.reverse()) // [ 4, 5, 2, 3, 1 ]
  ```

- ## sort()
  sort() 方法默认情况下会按升序排列数组项，但是 sort() 方法为了实现排序会调用每个数组项的 toString() 转型方法，然后比较得到的字符串，所以可能会产生意想不到的效果，例如：
  ``` javascript
  var nums = [0, 1, 5, 10, 15]
  console.log(nums.sort()) // [ 0, 1, 10, 15, 5 ]
  ```
  为了解决上面的问题，sort() 方法可以接收一个比较函数作为参数，使排序结果达到我们的预期。
  ``` javascript
  var nums = [0, 1, 10, 5, 15]
  console.log(nums.sort(compare)) // [ 0, 1, 5, 10, 15 ]
  function compare(a, b) {
    return a-b
  }
  ```
  compare() 方法如果返回正数，说明a要位于b之后；如果返回负数，说明a要位于b之前；如果返回0，说明a与b相等。

# 操作方法

- ## concat()
  concat() 方法可以基于当前数组中的所有项创建一个新数组。这个方法会先创建当前数组的一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组，参数可以是一个或多个数组。如果传递的参数不是数组，这些值就会被简单地添加到结果数组的末尾。
  ``` javascript
  var colors = ["red", "green", "blue"]
  console.log(colors.concat("yellow", ["black", "brown"])) // [ 'red', 'green', 'blue', 'yellow', 'black', 'brown' ]
  ```

- ## slice()
  slice() 方法接收一个或两个参数，即要返回项的起始位置和结束位置。如果只传一个参数，返回从该参数指定位置（包含该位置的项）开始到当前数组末尾的新数组；如果有两个参数，返回起始和结束位置之间（包含起始位置但不包含结束位置的项）的新数组。slice() 方法不会影响原始数组。
  ``` javascript
  var colors = ["red", "green", "blue", "yellow", "black"]
  console.log(colors.slice(1)) // [ 'green', 'blue', 'yellow', 'black' ]
  console.log(colors.slice(1, 4)) // [ 'green', 'blue', 'yellow' ]
  ```

- ## splice()
  splice() 方法有三个用法：删除、插入和替换。这个方法始终返回一个数组，包含从原始数组中删除的项，如果没有删除任何项，则返回一个空数组。
  用作删除时，传入两个参数，分别是要删除的第一项的位置和要删除的项数。
  ``` javascript
  var colors = ["red", "green", "blue", "yellow", "black"]
  console.log(colors.splice(0, 2)) // [ 'red', 'green' ]
  console.log(colors) // [ 'blue', 'yellow', 'black' ]
  ```
  用作插入和替换时，传入三个参数，分别是起始位置、要删除的项数（用作插入时设置成0）和要插入（替换）的项，这里的项可以是一个或多个。
  ``` javascript
  var colors = ["red", "green", "blue", "yellow", "black"]
  console.log(colors.splice(2, 1, "orange", "purple")) // [ 'blue' ]
  console.log(colors) // [ 'red', 'green', 'orange', 'purple', 'yellow', 'black' ]
  ```

# 位置方法
# 迭代方法
# 归并方法