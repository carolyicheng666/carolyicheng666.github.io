---
title: JavaScript 创建对象
date: 2018-07-20 10:21:03
tags: [JavaScript]
categories: '编程'
---

{% cq %}
无可取代的东西要等到失去之后才懂得珍惜呢

**干物妹！小埋**
{% endcq %}

<!-- more -->

{% note danger %}
用Object构造函数或字面量创建对象有个明显的缺点：如果创建一定量的对象会产生大量重复性的代码。
{% endnote %}

为了解决这个问题，本文就来介绍JavaScript中常见的几种创建对象的模式。

# 工厂模式

工厂模式抽象了创建具体对象的过程。

``` javascript
/* 工厂模式 */
function createPerson(name, age, job) {
  var o = new Object()
  o.name = name
  o.age = age
  o.job = job
  o.sayName = function() {
    return this.name
  }
  return o
}

var person1 = createPerson("Tom", 20, "Teacher")
var person2 = createPerson("Bob", 18, "Doctor")

console.log(person1.sayName())  // Tom
console.log(person2.sayName())  // Bob
```

每次调用createPerson函数创建对象，这个对象都会包含三个属性（name，age，job）和一个方法（sayName）。工厂模式虽然解决了创建多个相似对象的问题，对没有解决对象识别的问题（不知道对象的类型），比如上面的例子中person1和person2都不是createPerson的实例。

# 构造函数模式

构造函数可以用来创建特定类型的对象，能够解决上述对象识别的问题。

``` javascript
/* 构造函数模式 */
function Person(name, age, job) {
  this.name = name
  this.age = age
  this.job = job
  this.sayName = function() {
    return this.name
  }
}

var person1 = new Person("Tom", 20, "Teacher")
var person2 = new Person("Bob", 18, "Doctor")

console.log(person1.sayName())  // Tom
console.log(person2.sayName())  // Bob
```

这个例子中person1和person2既是Object的实例，同时也是Person的实例。但构造函数模式同样也有缺点：每个方法都要在每个实例上创建一遍，而像上面的sayName()这种类似的方法完全没必要多次创建。

# 原型模式

构造函数的问题可以利用原型模式的特点，让所有对象实例共享原型对象所包含的属性和方法来解决。

``` javascript
/* 原型模式 */
function Person() {

}

Person.prototype = {
  constructor: Person,
  name: "Tom",
  age: 20,
  job: "Dortor",
  friends: ["John", "Lina"],
  sayName: function() {
    return this.name
  }
}

var person1 = new Person()
var person2 = new Person()

person1.name = "Bob"
console.log(person1.sayName())  // Bob
console.log(person2.sayName())  // Tom

person1.friends.push("Van")
console.log(person1.friends)  // [ 'John', 'Lina', 'Van' ]
console.log(person2.friends)  // [ 'John', 'Lina', 'Van' ]
```

虽然原型模式避免了构造函数模式带来的问题，但其共享的特性对于包含引用类型值的属性（比如上例中的friends）来说，无疑是致命的。当然还有一个小问题是初始化时，所有的实例默认都带有相同的默认值。

# 组合构造函数模式和原型模式（推荐使用）

我们可以结合构造函数模式和原型模式的优点，使用构造函数来定义实例属性，使用原型来定义方法和共享的属性。

``` javascript
/* 组合使用构造函数和原型模式 */
function Person(name, age, job) {
  this.name = name
  this.age = age
  this.job = job
  this.friends = ["John", "Lina"]
}

Person.prototype = {
  constructor: Person,
  sayName: function() {
    return this.name
  }
}

var person1 = new Person("Tom", 20, "Teacher")
var person2 = new Person("Bob", 18, "Doctor")

console.log(person1.friends === person2.friends)  // false
console.log(person1.sayName === person2.sayName)  // true

person1.friends.push("Van")
console.log(person1.friends)  // [ 'John', 'Lina', 'Van' ]
console.log(person2.friends)  // [ 'John', 'Lina' ]
```

这种模式是目前使用最广泛、认同度最高的一种创建自定义类型的方法。

# 动态原型模式

这种模式将所有信息都封装在了构造函数中，通过构造函数初始化原型，保持了同时使用构造函数和原型的优点。

``` javascript
/* 动态原型模式 */
function Person(name, age, job) {
  // 属性
  this.name = name
  this.age = age
  this.job = job
  this.friends = ["John", "Lina"]
  // 方法
  if (typeof this.sayName != "function") {
    Person.prototype.sayName = function() {
      return this.name
    }
  }
}

var person1 = new Person("Tom", 20, "Teacher")
var person2 = new Person("Bob", 18, "Doctor")

console.log(person1.sayName())  // Tom
console.log(person2.sayName())  // Bob

person1.friends.push("Van")
console.log(person1.friends)  // [ 'John', 'Lina', 'Van' ]
console.log(person2.friends)  // [ 'John', 'Lina' ]
```

这里巧妙的使用了if判断sayName()方法是否存在，使其只会在初次调用构造函数时才会执行。

# 寄生构造函数模式

基本思想是创建一个函数，这个函数仅仅是封装创建对象的代码，然后再返回新创建的对象。

``` javascript
/* 寄生构造函数模式 */
function Person(name, age, job) {
  var o = new Object()
  o.name = name
  o.age = age
  o.job = job
  o.sayName = function() {
    return this.name
  }
  return o
}

var person1 = new Person("Tom", 20, "Teacher")
var person2 = new Person("Bob", 18, "Doctor")

console.log(person1.sayName())  // Tom
console.log(person2.sayName())  // Bob
```

这种模式除了使用new操作符并把使用的包装函数叫做构造函数之外，其实和工厂模式一模一样。这里的构造函数返回的对象与在构造函数外部创建的对象没有什么不同，所以依然不能确定对象类型。

# 稳妥构造函数模式

稳妥构造函数遵循寄生构造函数类似的模式，但有两点不同：
- 新创建对象的实例方法不引用this
- 不适用new操作符调用构造函数

``` javascript
/* 稳妥构造函数模式 */
function Person(name, age, job) {
  var o = new Object()
  o.sayName = function() {
    return name
  }
  return o
}

var person1 = new Person("Tom", 20, "Teacher")
var person2 = new Person("Bob", 18, "Doctor")

console.log(person1.sayName())  // Tom
console.log(person2.sayName())  // Bob
```

在这种模式下创建的是稳妥对象，比如上例中除了调用sayName()方法外，没有别的方式可以访问其数据成员。不过这种模式和寄生模式类似，不能确定对象类型。
