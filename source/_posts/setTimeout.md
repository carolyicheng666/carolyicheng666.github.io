---
title: 闭包经典面试题
date: 2018-08-29 20:40:11
tags: [JavaScript]
categories: '编程'
---

{% cq %}
今天的风儿甚是喧嚣  

**男子高中生的日常**
{% endcq %}

<!-- more -->

# 题目

Q: 下面的代码会输出什么？

``` javascript
for(var i=0; i<5; i++) {
  setTimeout(function() {
    console.log(i)
  }, i*1000)
}
```

A: 先立即输出一个5，然后每隔1s输出一个5，一共输出5个5。

Q: 那么要正常（每隔1s）输出0，1，2，3，4，程序要如何修改？

# 使用let

``` javascript
for(let i=0; i<5; i++) {
  setTimeout(function() {
    console.log(i)
  }, i*1000)
}
```

# 使用IIFE

``` javascript
for(var i=0; i<5; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, j*1000);
  })(i);
}
```

# 使用闭包

``` javascript
for(var i=0; i<5; i++) {
  setTimeout(function(j) {
    return function() {
      console.log(j)
    }
  }(i), i*1000)
}
```

# 使用setTimeout的第三个参数

``` javascript
for(var i=0; i<5; i++) {
  setTimeout(function(j) {
    console.log(j);
  }, i*1000, i)
}
```

嗯，由于被问到的频率有点高，就总结一下，可能考点是闭包吧，面试时有说过可以用IIFE实现，然后的感觉像是互相在尬聊......  
暂时只想到这几种方法......