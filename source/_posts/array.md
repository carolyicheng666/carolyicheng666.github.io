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

# 检测方法

- ## Array.isArray()
  isArray() 方法用于检测是否是数组。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  if (Array.isArray(nums)) {
    console.log(true)
  } else {
    console.log(false)
  }
  ```

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

- ## indexOf()
  indexOf() 方法接收两个参数，分别是要查找的项和查找起点位置的索引，其中第二个参数可选，不设置时则默认从数组开头进行查找，方法返回要查找的项在起点位置之后首次出现时位置的索引值，如果没有找到，则返回-1。查找时，查找的项要严格相等（ === ）。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  console.log(nums.indexOf(4)) // 3
  console.log(nums.indexOf(4, 4)) // 5
  ```

- ## lastIndexOf()
  lastIndexOf() 方法与 indexOf() 方法使用方式一致，唯一不同的是 lastIndexOf() 方法是从数组的末尾开始向前查找。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  console.log(nums.lastIndexOf(4)) // 5
  console.log(nums.lastIndexOf(4, 4)) // 3
  ```

# 迭代方法
迭代方法一共有5个，每个方法都接收两个参数，分别是要在每一项运行的函数和运行该函数的作用域对象，第二个参数可选。第一个参数中传入的函数会接收三个参数，分别是数组项的值、该数组项的索引值和数组本身，其中后两个参数可选。

- ## every()
  every() 方法对数组中的每一项运行给定函数，如果函数对每一项都返回true，则返回true。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  var everyResult = nums.every(function(item, index, array) {
    return item > 2
  })
  console.log(everyResult) // false
  ```

- ## some()
  some() 方法对数组中的每一项运行给定函数，如果函数对任意一项返回true，则返回true。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  var someResult = nums.some(function(item, index, array) {
    return item > 2
  })
  console.log(someResult) // true
  ```

- ## filter()
  filter() 方法对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  var filterResult = nums.filter(function(item, index, array) {
    return item > 2
  })
  console.log(filterResult) // [ 3, 4, 5, 4, 3 ]
  ```

- ## map()
  map() 方法对数组中的每一项运行给定函数，返回每次调用的结果组成的数组。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  var mapResult = nums.map(function(item, index, array) {
    return item * 2
  })
  console.log(mapResult) // [ 2, 4, 6, 8, 10, 8, 6, 4, 2 ]
  ```

- ## forEach()
  forEach() 方法对数组中的每一项运行给定函数，这个方法没有返回值，本质上与使用for循环迭代数组一样。
  ``` javascript
  var nums = [1, 2, 3, 4, 5, 4, 3, 2, 1]
  nums.forEach(function(item, index, array) {
    // do something
  })
  ```

# 归并方法

- ## reduce()
  reduce() 方法会迭代数组的所有项，然后构建一个最终返回的值。方法接收两个参数。分别是一个在每一项上调用的函数和作为归并的初始值，第二个参数可选。第一个参数函数接收四个参数，分别是数组的前一个值、当前值、项的索引值和数组本身。如果设置了初始值，则函数第一次执行时，第一个参数就是这个初始值；如果没有设置初始值，则函数第一次执行是，第一个参数就是数组的第一项。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  var sum = nums.reduce(function(prev, cur, index, array) {
    return prev + cur
  })
  console.log(sum) // 15
  ```

- ## reduceRight()
  reduceRight() 方法使用方式与 reduce() 一致，唯一的区别是 reduceRight() 方法是从数组最后一项向前遍历。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  var sum = nums.reduceRight(function(prev, cur, index, array) {
    return prev + cur
  })
  console.log(sum) // 15
  ```

# ES6 新增

- ## Array.from()
  Array.from() 方法从一个类似数组或可迭代对象中创建一个新的数组实例。这个方法接收两个参数，分别是想要转换成数组的伪数组对象或可迭代对象
  、新数组中的每个元素要执行的回调函数，第二个参数可选，其实第二个参数相当于一个 map() 方法。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  console.log(Array.from(nums, function(item, index, array) {
    return item * 2
  }))
  ```

- ## Array.of()
  Array.of() 方法创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。这个方法接收任意数量的参数，返回按参数顺序组成的数组。
  ``` javascript
  console.log(Array.of(7)) // [ 7 ]
  console.log(Array.of(1, 2, 3)) // [ 1, 2, 3 ]
  ```

- ## copyWithin()
  copyWithin() 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，而不修改其大小。这个方法接收三个参数，分别是复制到某位置的索引值、开始复制的起始位置（包括）和结束位置（不包括），后两个参数可选，不设置起始位置表示从数组开始进行复制，不设置结束位置表示一致复制到数组末尾。这个方法会该表原数组。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  console.log(nums.copyWithin(0, 3, 4)) // [ 4, 2, 3, 4, 5 ]
  console.log(nums.copyWithin(1, 3)) // [ 4, 4, 5, 4, 5 ]
  console.log(nums) // [ 4, 4, 5, 4, 5 ]
  ```

- ## fill() 
  fill() 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。参数释义及使用方式和上面的 copyWithin() 一致。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  console.log(nums.fill(0, 2, 4)) // [ 1, 2, 0, 0, 5 ]
  console.log(nums.fill(5, 1)) // [ 1, 5, 5, 5, 5 ]
  console.log(nums.fill(6)) // [ 6, 6, 6, 6, 6 ]
  ```

- ## entries()
  entries() 方法返回一个新的Array Iterator对象，该对象包含数组中每个索引的[key,value]。可以调用 next() 方法遍历迭代器取得原数组的[key,value]。
  ``` javascript
  var arr = ["a", "b", "c"]
  var iterator = arr.entries()
  for(let e of iterator) {
    console.log(e)
  }
  // [ 0, 'a' ]
  // [ 1, 'b' ]
  // [ 2, 'c' ]
  ```

- ## keys()
  keys() 方法返回一个新的Array Iterator对象，它包含数组中每个索引的键。
  ``` javascript
  var arr = ["a", "b", "c"]
  var iterator = arr.keys()
  for(let key of iterator) {
    console.log(key)
  }
  // 0
  // 1
  // 2
  ```

- ## values()
  values() 方法返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值。这个方法目前存在兼容性的问题，建议使用上面的 entries() 方法获取。
  ``` javascript
  var arr = ["a", "b", "c"]
  var iterator = arr.values()
  for(let value of iterator) {
    console.log(value)
  }
  ```

- ## find()
  find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  var found = nums.find(function(item) {
    return item > 3
  })
  console.log(found) // 4
  ```

- ## findIndex()
  findIndex() 方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  var found = nums.findIndex(function(item) {
    return item > 3
  })
  console.log(found) // 3
  ```

- ## includes()
  includes() 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。这个方法接收两个参数，分别是指定值和搜索开始的数组索引值，第二个参数可选。第二个参数如果为负值，则按升序从这个负值加上数组长度的索引开始搜索。默认为 0。
  ``` javascript
  var nums = [1, 2, 3, 4, 5]
  console.log(nums.includes(3)) // true
  console.log(nums.includes(10)) // false
  ```
