---
title: animation
date: 2017-12-28 15:27:02
tags: [CSS]
categories: '编程'
---



{% cq %}
只要那一抹笑容尚存，我便心无旁骛  

**声之形**
{% endcq %}

<!-- more -->



简介
---

{% note danger %}
CSS Animations 是CSS的一个模块，它定义了如何用关键帧来随时间推移对CSS属性的值进行动画处理。关键帧动画的行为可以通过指定它们的持续时间，它们的重复次数以及它们如何重复来控制。
{% endnote %}



animation 浏览器兼容性
---

以下浏览器数据支持来自 [Caniuse](https://caniuse.com/#search=animation) ，对比一下之前的 transition ，支持程度虽然稍显逊色，但依然可以看出还是不错的

PC端浏览器：

| Chrome | Opera | FireFox | IE | Edge | Safari |
|:------:|:-----:|:-------:|:--:|:----:|:------:|
|   43   |   30  |   16    | 10 |  Yes |    9   |

移动端浏览器：

| iOS Safari | Opera Mobile | Opera Mini | Android | Android Chrome | Android Firefox |
|:----------:|:------------:|:----------:|:-------:|:--------------:|:---------------:|
|    9.2     |     12.1     |     No     |   62    |       62       |       57        |




keyframes
---

在开始介绍 Animation 之前我们需要来介绍一下什么是 keyframes 。顾名思义，它是动画的关键帧，回想一下，前面我们在使用 transition 制作一个简单的动画效果时，我们包括了初始属性和最终属性，一个开始执行动作时间和一个延续动作时间以及动作的变换速率，其实这些值都是一个中间值，如果我们要控制的更细一些，比如说我要第一个时间段执行什么动作，第二个时间段执行什么动作，这样的效果我们用 transition 很难实现，此时我们就需要借助 Animation 的力量。在使用 Animation 之前，我们需要为 Animation 定义 keyframes，即让绑定动画的元素在特定的帧表现特定的效果，接下来我们就来介绍一下如何使用 keyframes。  
keyframes 具有其自己的语法规则，他的命名是由 `@keyframes` 开头，后面紧接着是这个动画的名称 `animation-name` 和花括号 `{}` ，在里面定义不同时间段所显示的样式，写法和 CSS 一样。`@keyframes` 内部是由多个百分比构成的，如 `0%`到 `100%` 之间，我们可以在其中按顺序创建任意多个百分比，并且附上不同的 CSS 样式，就可以实现动画效果了。注意，我们也可以使用 `from`和`to`分别取代 `0%`和 `100%` ，并且，`0%` 不能像别的属性取值一样把百分比符号省略，我们在这里必须加上百分符号，如果没有加上，那么这个 keyframes 是无效的，keyframes 的单位只接受百分比值。  
对照上面的介绍，我们拿 W3C 的例子加深理解：
``` css
@keyframes mymove {
  0%   {top:0px;}
  25%  {top:200px;}
  50%  {top:100px;}
  75%  {top:200px;}
  100% {top:0px;}
}
```



基本用法
---

``` css
animation: name duration timing-function delay iteration-count direction fill-mode play-state;
```

- animation-name: keyframename|none;  
定义动画的名称，这个名称要与 Keyframes 中定义的动画名称一致，可以给某一元素绑定多个动画

- animation-duration：time;  
规定完成一次动画所需要的时间，单位是秒或毫秒

- animation-timing-function: ease|linear|ease-in|ease-out|ease-in-out|cubic-bezier(n, n, n, n);  
规定动画的速度曲线，详细介绍请查看之前 transition 文章，这里与其一致

- animation-delay: time;  
规定动画的延迟时间，单位是秒或毫秒

- animation-iteration-count: n|infinite;  
定义动画的播放次数，infinite表示无限次播放

- animation-direction: normal|reverse|alternate|alternate-reverse;  
定义是否应该轮流反向播放动画，默认是normal，表示正常播放；reverse，表示反向播放；alternate表示奇数次正向播放，偶数次反向播放；alternate-reverse与alternate正好相反，表示奇数次反向播放，偶数次正向播放。注意，如果动画只播放一次，则这个属性没有效果

- animation-fill-mode: none|forwards|backwards|both;  
控制元素在动画执行前与动画完成后的样式，这个属性不太好理解，索性用处也不是很大，推荐大家去看大漠老师的这篇文章：[理解animation-fill-mode属性](https://www.w3cplus.com/css3/understanding-css-animation-fill-mode-property.html)

- animation-play-state: running|paused;  
定义一个动画是否运行或者暂停，利用这个属性，可以使用js控制动画暂停和播放

就本人经验来看，前六个属性用的比较多，后面两种一般网页可能都不太能用到，如果是做游戏开发的可能比较容易碰到吧，有兴趣的朋友随便找几个[W3C例子](http://www.w3school.com.cn/cssref/pr_keyframes.asp)，修改其中的参数看看效果



[animate.css3](https://github.com/daneden/animate.css)
---

用 animate 写过动画的朋友对这个库一定不会陌生，这个库在 github 上拥有恐怖的接近 50000 个 star，有一点animate基础的朋友就可以立马上手，如果不是特别刁钻的需求，这个库都能满足要求，点击标题即可跳转，强烈推荐使用。

