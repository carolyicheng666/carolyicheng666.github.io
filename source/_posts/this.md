---
title: JavaScript 之 this
date: 2018-02-07 17:06:04
tags: [JavaScript]
categories: '编程'
---



{% cq %}
不管前方的路有多苦  
只要走的方向正确  
不管多么崎岖不平  
都比站在原地更接近幸福  

**千与千寻**
{% endcq %}



<!-- more -->

对于和我一样 JavaScript 基础不是很扎实的朋友，在看别人写的代码的时候，一定总是弄不清 this 的指向，工作和学习的效率也相应大打折扣。

最近稍微研究了一下，基本总结为两句话：**this不在箭头函数中，this 永远指向最后调用它的那个对象；this在箭头函数中，this 始终指向函数定义时的 this**

其实研究透了，说起来也挺简单，下面举几个例子对比一下，大致也就明白了：

**首先声明**：在严格模式下，全局对象是 undefined；在非严格模式下，全局对象才是 window。下面的例子全是在非严格模式下。

**例子一**：这个是最简单的例子，直接看调用a方法时，前面没有调用的对象，那就是全局对象 window 调用的，也可以写成 window.a()
``` javascript
var name = "Zhang San";
function a() {
  var name = "Li Si";
  console.log(this.name);
}
a(); // ZhangSan
```

**例子二**：同样直接看调用，两个 fn() 最后都是 a 调用的，所以它们的 this 指向的都是 a ，例子一的说明部分已经讲过了
``` javascript
var name = "Zhang San";
var a = {
  name: "Li Si",
  fn: function () {
    console.log(this.name);
  }
}
a.fn(); // Li Si
window.a.fn(); // Li Si
```

**例子三**：这类的例子应该是最困扰大家的一种了，我们只要记住是在**调用**时即可。本例先声明了 f ，后在调用时 f() 前面并没有调用的对象，那么可以参考例子一的解释，即此时 this 指向全局对象 window
``` javascript
var name = "Zhang San";
var a = {
  name: "Li Si",
  fn: function () {
    console.log(this.name);
  }
}

var f = a.fn;
f(); // Zhang San
```

**例子四**：这个问题大家肯定也经常遇到，在调用一些原生函数时，不知道调用它的对象是什么，这个也没办法，要一点一点积累经验。比如本例中的setTimeout()究竟是哪个对象调用的呢？“红宝书”里有写过：超时调用的代码都是在全局作用域中执行的，因此函数中 this 的值在非严格模式下指向 window 对象，在严格模式下是 undefined 。回到本例中来，window 对象中没有f1()，所以才会报出错误 `this.f1 is not a function`
``` javascript
var name = "Zhang San";
var a = {
  name : "Li Si",
  f1: function () {
    console.log(this.name);
  },
  f2: function () {
    setTimeout( function () {
      this.f1();
    }, 100);
  }
};
a.f2(); // this.f1 is not a function
```

那么这个代码我们要怎样改造，才能成功输出 `Li Si` 呢？  
有两种方案，第一种也是最常见的一种：使用一个变量将进入 setTimeout() 之前的 this 给保存下来：
``` javascript
var name = "Zhang San";
var a = {
  name : "Li Si",
  f1: function () {
    console.log(this.name);
  },
  f2: function () {
    var self = this;
    setTimeout( function () {
      self.f1();
    }, 100);
  }
};
a.f2();
```

第二种方案是使用箭头函数，可以回头看看我一开始的总结，这里需要注意一点：箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined。所以我们可以这么改造：（千万不要把 f2 也写成箭头函数）
``` javascript
var name = "Zhang San";
var a = {
  name : "Li Si",
  f1: function () {
    console.log(this.name);
  },
  f2: function () {
    setTimeout(() => {
      this.f1();
    }, 100);
  }
};
a.f2();
```
