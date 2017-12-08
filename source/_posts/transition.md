---
title: transition
date: 2017-12-08 16:04:14
tags: [CSS]
categories: '编程'
---


{% cq %}
被拒绝是悲伤的，但是拒绝的一方也很痛苦

**纯白交响曲**
{% endcq %}

<!-- more -->



简介
---

CSS transitions 提供了一种在更改CSS属性时控制动画速度的方法。 其可以让属性变化成为一个持续一段时间的过程，而不是立即生效的。比如，将一个元素的颜色从白色改为黑色，通常这个改变是立即生效的，使用 CSS transitions 后该元素的颜色将逐渐从白色变为黑色，按照一定的曲线速率变化。这个过程可以自定义。

通常将两个状态之间的过渡称为隐式过渡（implicit transitions），因为开始与结束之间的状态由浏览器决定。

CSS transitions 可以决定哪些属性发生动画效果 (明确地列出这些属性)，何时开始 (设置 delay），持续多久 (设置 duration) 以及如何动画 (定义timing funtion，比如匀速地或先快后慢)。



浏览器兼容性
---

以下浏览器数据支持来自 [Caniuse](https://caniuse.com/#search=transition) ，可以看出支持程度相当好了

PC端浏览器：

| Chrome | Opera | FireFox | IE | Edge | Safari |
|:------:|:-----:|:-------:|:--:|:----:|:------:|
|   26   |  12.1 |   16    | 10 |  YES |   6.1  |

移动端浏览器：

| iOS Safari | Opera Mobile | Opera Mini | Android | Android Chrome | Android Firefox |
|:----------:|:------------:|:----------:|:-------:|:--------------:|:---------------:|
|    7.1     |     12.1     |    12.1    |   4.4   |       62       |       57        |



基本用法
---

``` css
transition: transition-property transition-duration transition-timing-function transition-delay;
```
  
*transition-property*：规定设置过渡效果的 CSS 属性的名称  
*transition-duration*：规定完成过渡效果的时间，单位是秒或毫秒  
*transition-timing-function*：规定速度效果的速度曲线  
*transition-delay*：定义过渡效果延迟时间，单位是秒或毫秒 

下面重点介绍property和timing-function这两个属性：
- transition-property  
这个属性的取值有很多，建议到 [MDN官方文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_animated_properties) 中查阅，经常用到的有color类的，width/height类的等等

- transition-duration  
这个属性严格意义上不能分类，官方单独将五种（不包含step类的）比较特别的单独命名出来，便于大家使用，分别是：  
*linear*：规定以相同速度开始至结束的过渡效果  
*ease*：规定慢速开始，然后变快，然后慢速结束的过渡效果  
*ease-in*：规定以慢速开始的过渡效果  
*ease-out*：规定以慢速结束的过渡效果  
*ease-in-out*：规定以慢速开始和结束的过渡效果  
其实最基本的都是依赖于cubic-bezier(n,n,n,n)，n的取值范围是0到1



实例
---

demo1:
``` html
<a href="javascript:void(0);" class="demo">我是一个demo</a>

.demo {
  position: relative;
  text-decoration: none;
  font-size: 20px;
  color: #333;
}
.demo:before {
  content: "";
  position: absolute;
  left: 0;
  bottom: -2px;
  height: 2px;
  width: 100%;
  background: #4285f4;
  transform: scale(0);
  transition: all 0.3s;
}
.demo:hover:before {
  transform: scale(1);
}
```

demo2:
``` html
<a class="l-border border-line">我是另一个demo</a>

.l-border{
  display: inline-block;
  padding: 16px 32px;
  border: 1px solid #ebebeb;
  -webkit-transition: all 0.6s ease-in;
  transition: all 0.6s ease-in;
}

.l-border:hover{
  border: 1px solid #367dff;
}
.border-line {
  position: relative;
  display: inline-block;
  height: 100%;
  background: none;
  box-sizing: border-box;
  box-shadow: inset 0 0 0 0px transparent;
}

.border-line::after,
.border-line::before {
  position: absolute;
  content: '';
  display: block;
  width: 0;
  height: 0;
  box-sizing: border-box;
  border: 1px solid transparent;
}

.border-line::after {
  top: 0;
  left: 0;
  -webkit-transition: border-color 0s ease-in 0.8s, width 0.2s ease-in 0.6s, height 0.2s ease-in 0.4s;
  transition: border-color 0s ease-in 0.8s, width 0.2s ease-in 0.6s, height 0.2s ease-in 0.4s;
}

.border-line::before {
  bottom: 0;
  right: 0;
  -webkit-transition: border-color 0s ease-in 0.4s, width 0.2s ease-in 0.2s, height 0.2s ease-in;
  transition: border-color 0s ease-in 0.4s, width 0.2s ease-in 0.2s, height 0.2s ease-in;
}

.border-line:hover::after,
.border-line:hover::before {
  width: 100%;
  height: 100%;
}

.border-line:hover::after {
  border-top-color: #367dff;
  border-right-color: #367dff;
  -webkit-transition: width 0.2s ease-out, height 0.2s ease-out 0.2s;
  transition: width 0.2s ease-out, height 0.2s ease-out 0.2s;
}

.border-line:hover::before {
  border-bottom-color: #367dff;
  border-left-color: #367dff;
  -webkit-transition: border-color 0s ease-out 0.4s, width 0.2s ease-out 0.4s, height 0.2s ease-out 0.6s;
  transition: border-color 0s ease-out 0.4s, width 0.2s ease-out 0.4s, height 0.2s ease-out 0.6s;
}
```

demo3:
``` html
<div class="effect-box">
  <div class="border-line2">
    <p> XXX </p>
    <p> demo </p>
    <p>Read More</p>
  </div>
</div>

.effect-box {
  position: relative;
  display: inline-block;
  overflow: hidden;
  width: 250px;
  height: 158px;
  background: #367dff;
  cursor: pointer
}

.effect-box img {
  display: block;
  width: 100%;
  height: 100%;
  opacity: .7;
  -webkit-transition: opacity .35s;
  transition: opacity .35s
}

.effect-box:hover img {
  opacity: .4
}

.effect-box .border-line2 {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding: 2em;
  font-size: 1.25em;
  color: #fff;
  text-transform: uppercase;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden
}

.effect-box .border-line2::after,
.effect-box .border-line2::before {
  position: absolute;
  top: 30px;
  right: 30px;
  bottom: 30px;
  left: 30px;
  content: '';
  opacity: 0;
  pointer-events: none;
  -webkit-transition: opacity .35s, -webkit-transform .35s;
  transition: opacity .35s, transform .35s
}

.effect-box .border-line2::before {
  border-top: 1px solid #fff;
  border-bottom: 1px solid #fff;
  -webkit-transform: scale(0, 1);
  transform: scale(0, 1)
}

.effect-box .border-line2::after {
  border-right: 1px solid #fff;
  border-left: 1px solid #fff;
  -webkit-transform: scale(1, 0);
  transform: scale(1, 0)
}


.effect-box:hover .border-line2::after,
.effect-box:hover .border-line2::before {
  opacity: 1;
  -webkit-transform: scale(1);
  transform: scale(1)
}

.effect-box .border-line2 p {
  padding: 4px 10px;
  margin: 0;
  font-size: 16px;
  line-height: 1.0;
  text-align: center;
  color: #fff;
  letter-spacing: 1px;
  opacity: 0;
  -webkit-transition: opacity .35s, -webkit-transform .35s;
  transition: opacity .35s, transform .35s;
  -webkit-transform: translate3d(0, 20px, 0);
  transform: translate3d(0, 20px, 0)
}

.effect-box:hover .border-line2 p {
  opacity: 1;
  -webkit-transform: translate3d(0, 0, 0);
  transform: translate3d(0, 0, 0)
}
```


参考文献
---

- [MDN —— Using_CSS_transitions](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions) 
- [W3C —— CSS3 transition 属性](http://www.w3school.com.cn/cssref/pr_transition.asp)
- [掘金 —— transform，transition，animation 的混合使用——结业篇](https://juejin.im/post/592536c32f301e006b39059c)