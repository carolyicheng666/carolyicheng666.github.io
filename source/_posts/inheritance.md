---
title: JavaScript 继承
date: 2018-07-17 20:01:25
tags: [JavaScript]
categories: '编程'
---


{% cq %}
只要有想见的人  
就一定不会孤单

**夏目友人帐**
{% endcq %}

<!-- more -->

{% note danger %}
大部分面向对象的语言都支持两种继承方式：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。由于 ECMAScript 的函数没有签名，无法实现接口继承，故其只能支持实现继承，而且实现继承主要是依靠原型链来实现的。
{% endnote %}

# 原型链

基本思想是利用原型将一个引用类型继承另一个引用类型的属性和方法。

``` javascript
/* 原型链继承 */
function superType() {
  this.property = true
  this.colors = ["red", "yellow", "blue"]
}

superType.prototype.getSuperValue = function() {
  return this.property
}

function subType() {
  this.property = false
}

// 继承了 superType
subType.prototype = new superType()

subType.prototype.getSubValue = function() {
  return this.property
}

var instance = new subType()
console.log(instance.getSubValue())  // false
console.log(instance.colors)  // [ 'red', 'yellow', 'blue' ]

instance.colors.push("green")
console.log(instance.colors)  // [ 'red', 'yellow', 'blue', 'green' ]

var instance2 = new subType()
console.log(instance2.colors)  // [ 'red', 'yellow', 'blue', 'green' ]
```

上面的例子简单实现了子类型subType继承超类型superType，但是正如上所示，存在着两个致命的问题：
- 包含引用类型值的原型属性会被所有实例共享
- 在创建子类型实例时，不能向超类型的构造函数中传递参数

# 借用构造函数

基本思想是子类型构造函数的内部调用超类型构造函数。

``` javascript
/* 借用构造函数 */
function superType(name) {
  this.name = name
  this.colors = ["red", "yellow", "blue"]
}

function subType() {
  // 继承了 superType
  superType.call(this, "Bob")
  this.age = 18
}

var instance = new subType()
console.log(instance.name)  // Bob
console.log(instance.age)  // 18
console.log(instance.colors)  // [ 'red', 'yellow', 'blue' ]

instance.colors.push("green")
console.log(instance.colors)  // [ 'red', 'yellow', 'blue', 'green' ]

var instance2 = new subType()
console.log(instance2.colors)  // [ 'red', 'yellow', 'blue' ]
```

同样的，上面的例子简单实现了子类型subType继承超类型superType，但还是存在两个问题：
- 方法都在构造函数中定义，不能做到函数复用
- 在超类型的原型中定义的方法，对子类型是不可见的，结果所有类型都只能使用构造函数模式

# 组合继承

将原型链和借用构造函数的技术组合到一起，发挥两者的长处，即使用原型链实现对原型属性和方法的继承，借用构造函数实现对实例属性的继承。

``` javascript
/* 组合继承 */
function superType(name) {
  this.name = name
  this.colors = ["red", "yellow", "blue"]
}

superType.prototype.sayName = function() {
  return this.name
}

function subType(name, age) {
  // 继承属性
  superType.call(this, name)
  this.age = age
}

// 继承方法
subType.prototype = new superType()
subType.prototype.constructor = subType

subType.prototype.sayAge = function() {
  return this.age
}

var instance = new subType("Bob", 18)
instance.colors.push("green")
console.log(instance.colors)  // [ 'red', 'yellow', 'blue', 'green' ]
console.log(instance.sayName())  // Bob
console.log(instance.sayAge())  // 18

var instance2 = new subType("Tom", 20)
console.log(instance2.colors)  // [ 'red', 'yellow', 'blue' ]
console.log(instance2.sayName())  // Tom
console.log(instance2.sayAge())  // 20
```

组合继承既避免了原型链和借用构造函数的缺陷，又融合了它们的优点，成为JavaScript中最常用的继承模式。

# 原型式继承

基本思想是将一个已有对象通过Object.create()方法生成新的对象，再根据具体需求对新对象加以修改即可。

``` javascript
/* 原型式继承 */
var person = {
  name: "Bob",
  colors: ["red", "yellow", "blue"]
}

var one = Object.create(person)
one.name = "Tom"
one.colors.push("green")
console.log(one.name)  // Tom
console.log(one.colors)  // [ 'red', 'yellow', 'blue', 'green' ]

var two = Object.create(person)
two.name = "John"
two.colors.push("orange")
console.log(two.name)  // John
console.log(two.colors)  // [ 'red', 'yellow', 'blue', 'green', 'orange' ]

console.log(person.name)  // Bob
console.log(person.colors)  // [ 'red', 'yellow', 'blue', 'green', 'orange' ]
```

在不需要创建构造函数，只想让一个对象与另一个对象保持类似的情况下可以使用，但是这种方式依然不能避开原型链中提到的 `包含引用类型值的原型属性会被所有实例共享` 这一致命问题。

# 寄生式继承

寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数内部以某种方式来增强对象，最后返回该对象。

``` javascript
/* 寄生式继承 */
function parasitic(original) {
  var clone = Object(original)
  clone.sayHi = function() {
    return `Hi!`
  }
  return clone
}

var person = {
  name: "Bob",
  colors: ["red", "yellow", "blue"]
}

var otherPerson = parasitic(person)
console.log(otherPerson.sayHi())
```

上面的例子中otherPerson对象通过parasitic()方法，不仅具有了person的所有属性和方法，而且还增强了sayHi()方法，但是这种方式依然和 `借用构造函数` 有着类似的问题：不能做到函数复用。

# 寄生组合式继承

前面说过组合继承是JavaScript最常用的继承模式，不过它仍然存在一个问题：无论什么情况下，都会调用两次超类型的构造函数，一次是在创建子类型原型的时候，另一次是在子类型构造函数的内部。  
所以就有了寄生组合式继承。所谓寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其基本思路是不必为了指定子类型的原型而调用超类型的构造函数，我们只需要超类型原型的一个副本而已。本质上就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

``` javascript
/* 寄生组合式继承 */
function parasitic(subType, superType) {
  var clone = Object(superType.prototype)
  clone.constructor = subType
  subType.prototype = clone
}

function superType(name) {
  this.name = name
  this.colors = ["red", "yellow", "blue"]
}

superType.prototype.sayName = function() {
  return this.name
}

function subType(name, age) {
  superType.call(this, name)
  this.age = age
}

parasitic(subType, superType)

subType.prototype.sayAge = function() {
  return this.age
}

var instance = new subType("Bob", 18)
instance.colors.push("green")
console.log(instance.colors)  // [ 'red', 'yellow', 'blue', 'green' ]
console.log(instance.sayName())  // Bob
console.log(instance.sayAge())  // 18

var instance2 = new subType("Tom", 20)
console.log(instance2.colors)  // [ 'red', 'yellow', 'blue' ]
console.log(instance2.sayName())  // Tom
console.log(instance2.sayAge())  // 20
```

上面的寄生组合式继承的例子，只调用了一次superType构造函数，又避免了在subType.prototype上创建不必要的属性，还保持了原型链不变，应该是最理想的继承方式了。
