---
title: CSS 选择器
date: 2017-12-06 10:01:11
tags: [CSS]
categories: '编程'
---


{% cq %}
我要用什么样的速度，才能与你相遇

**秒速五厘米**
{% endcq %}

<!-- more -->



简介
---

CSS 选择器一般分成三大类：基本选择器、属性选择器和伪类选择器。



基本选择器
---

- 通配符选择器 ( \* )  
通配符选择器是用来选择所有元素，也可以选择某个元素下的所有元素
``` css
* { ... }
.class * { ... }
```

- 元素选择器 ( element )  
元素选择器用来选择文档的元素，如html,body,div,span等等
``` css
html { ... }
div { ... }
```

- 类选择器 ( .className )  
给html中的元素定义类名，就能用类选择器选中该元素，并为其指定样式了，使用方式是在类名前面加 “.”
``` html
<div class="class1"></div>

.class1 { ... }
```

- id选择器 ( #id )  
给html中的元素定义id名，就能用id选择器选中该元素，并为其指定样式了，使用方式是在类名前面加 “#”  
**注意**，id与类的区别是：id是唯一的，而class可以不唯一  
``` html
<div id="id1"></div>

#id1 { ... }
```

- 后代选择器 ( element1 element2 )
选择某元素的所有后代元素，举个例子：
``` css
div span { ... }
```
	选中的就是div下所有的span元素

- 子元素选择器 ( element1 > element2 )  
选择某元素的所有子元素，不敢包括其子元素内的子元素（即只存在父子关系的元素），举个例子：
``` css
div>span { ... }
```
	选中的是div下的span元素，不包括span下的span元素

- 相邻兄弟元素选择器 ( element1 + element2 )  
选择紧跟在某元素后的兄弟元素，它们具有一个相同的父元素，举个例子：
``` html
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>

li+li { ... }
```
	这里选中的就是第二和第三个li元素

- 通用兄弟元素选择器 ( element1 ~ element2 )  
选择跟在某元素后的所有兄弟元素，它们具有一个相同的父元素，可参考相邻兄弟元素选择器

- 群组选择器 ( element1, element2, ..., elementN )  
群组选择器可以把相同样式的元素写在一起，用 “,” 隔开



属性选择器
---

- element[attribute]  
选中拥有某个属性的所有元素，举个例子：
``` css
a[href] { ... }
```
	选中的就是有href属性的a元素，也可以多属性选择，像下面这样：
``` css
a[href][title]
```
	选中的就是同时有href和title属性的a元素

- element[attribute = "value"]  
选中某个属性值为value的所有元素，注意要**完全匹配**才行，举个例子：
``` css
a[href="#"] { ... }
```
	选中的就是href值为#的a元素，其它值都不能被选中

- element[attribute ~= "value"]  
选中某个属性值包含value的所有元素，类似于模糊匹配，注意与上一个的区别

- element[attribute ^= "value"]  
选中某个属性值是以value开头的所有元素，注意可以**不等于**value，与上面两个有本质区别

- element[attribute $= "value"]  
选中某个属性值是以value结尾的所有元素，知道正则匹配的一定对^和$不陌生

- element[attribute \*= "value"]  
选中某个属性值是包含子串value的所有元素

- element[attribute |= "value"]  
选中某个属性值是等于value或者以value-开头的所有元素，**注意是value-开头**



伪类选择器
---

伪类选择器的使用方式都是在元素后面加“:”，再跟上伪类的名称，下面逐一介绍  

- 动态伪类  
:link —— 选择未访问链接  
:visited —— 选择已访问链接  
:hover —— 鼠标位于元素上时  
:focus —— 获取焦点（即选中）时  
:active —— 激活链接（键盘space，鼠标点击）

这里特别说明一下，一定要按上面的顺序设置，不然可能会带来不必要的麻烦，关于最后两个选择器，有困扰的朋友可以看看 [what-is-the-difference-between-focus-and-active](https://stackoverflow.com/questions/1677990/what-is-the-difference-between-focus-and-active)，也就是说:active是在:focus后触发，实际上的代码是:focus:active，所以整个动作是：点击时是先:focus，同时发生:active，而松开后变回:focus。

- 状态伪类  
这些类主要针对的是html里的form元素，例如input，option等等  
:enabled —— 选择器匹配每个已启用的元素  
:disabled —— 选择器匹配每个被禁用的元素  
:checked —— 选择器匹配每个已被选中的 input 元素（只用于单选按钮和复选框），目前只有 Opera 支持 :checked 选择器。

- nth选择器  
:first-child —— 选择某个元素的第一个子元素  
:last-child —— 选择某个元素的最后一个子元素  
:nth-child() —— 选择某个元素的一个或多个特定的子元素  
:nth-last-child() —— 选择某个元素的一个或多个特定的子元素，从这个元素的最后一个子元素开始算  
:nth-of-type() —— 选择指定的元素  
:nth-last-of-type() —— 选择指定的元素，从元素的最后一个开始计算  
:first-of-type —— 选择一个上级元素下的第一个同类子元素  
:last-of-type —— 选择一个上级元素的最后一个同类子元素  
:only-child —— 选择的元素是它的父元素的唯一一个了元素  
:only-of-type —— 选择一个元素是它的上级元素的唯一一个相同类型的子元素  
:empty —— 选择的元素里面没有任何内容  

我个人建议使用后半部分 `*-of-type` 这种，会比前半部分的 `*-child` 好很多，初学者可能总是会碰到写了样式，却不生效的情况，W3C 上的例子规避了一些我们经常遇到的问题和习惯性的错误，建议看看张鑫旭老师的[CSS3选择器:nth-child和:nth-of-type之间的差异](http://www.zhangxinxu.com/wordpress/2011/06/css3%E9%80%89%E6%8B%A9%E5%99%A8nth-child%E5%92%8Cnth-of-type%E4%B9%8B%E9%97%B4%E7%9A%84%E5%B7%AE%E5%BC%82/)，这篇文章也许能解答大多数人的疑惑  
顺便介绍一个github上大佬写的库——[family.scss](https://github.com/LukyVj/family.scss/)，很容易上手

- 否定选择器  
:not(selector) —— 选择器匹配非指定元素/选择器的每个元素

- 伪元素  
:first-letter —— 选择器用于选取指定选择器的首字母  
:first-line —— 选择器用于选取指定选择器的首行  
:before —— 选择器在被选元素的内容前面插入内容，需要使用 content 属性来指定要插入的内容  
:after —— 选择器在被选元素的内容后面插入内容，需要使用 content 属性来指定要插入的内容  
:lang —— 选择器用于选取带有以指定值开头的 lang 属性的元素  
::selection —— 选择器匹配被用户选取的部分，只能向 ::selection 选择器应用少量 CSS 属性：color、background、cursor 以及 outline。  
:target —— 选择器可用于选取当前活动的目标元素  



参考文献
---

- [W3C——CSS 选择器参考手册](http://www.w3school.com.cn/cssref/css_selectors.asp)
- [w3cplus——CSS3选择器 伪类选择器](https://www.w3cplus.com/css3/pseudo-class-selector)
- [stackoverflow——what-is-the-difference-between-focus-and-active](https://stackoverflow.com/questions/1677990/what-is-the-difference-between-focus-and-active)
- [张鑫旭——CSS3选择器:nth-child和:nth-of-type之间的差异](http://www.zhangxinxu.com/wordpress/2011/06/css3%E9%80%89%E6%8B%A9%E5%99%A8nth-child%E5%92%8Cnth-of-type%E4%B9%8B%E9%97%B4%E7%9A%84%E5%B7%AE%E5%BC%82/)
- [family.scss](https://github.com/LukyVj/family.scss/)