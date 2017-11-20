---
title: compass-to-autoprefixer
date: 2017-11-20 17:51:47
tags: [sass, css, compass, autoprefixer]
categories: '编程'
---

自从学会了SASS之后，就觉得再也离不开SASS了。但是，SASS 和原生 CSS 一样，也要面临着 CSS3 浏览器前缀的问题。众所周知，在 CSS3 发布之后，由于各大浏览器的兼容性问题，我们往往需要对 CSS3 属性加上相应的浏览器前缀，该属性才能才特定的浏览器中生效。
现在主要流行的浏览器内核主要有：
{% note info %}
Trident内核：主要代表为IE浏览器
Gecko内核：主要代表为Firefox
Presto内核：主要代表为Opera
Webkit内核：产要代表为Chrome和Safari
{% endnote %}
而这些不同内核的浏览器，CSS3属性（部分需要添加前缀的属性）对应需要添加不同的前缀：
{% note info %}
Trident内核：前缀为-ms
Gecko内核：前缀为-moz
Presto内核：前缀为-o
Webkit内核：前缀为-webkit
{% endnote %}
这个时候我们就需要来思考一下，如何才能避免加前缀的麻烦呢？这个时候我找到了 [Compass](http://compass-style.org/)，
官网的介绍是这样的：
{% note info %}
Compass is an open-source CSS Authoring Framework.
Compass uses Sass.
{% endnote %}
官网的简介有点迷，其实 Compass 就是用来编译 SASS 的工具，并且里面使用 SASS 的 mixin 为 CSS3 需要带前缀的属性定制了一些 mixin，除 CSS3 之外，Compass还有很多特殊定制，主要分为六大模块：
{% note info %}
reset模块
CSS3模块
layout模块
typography模块
utilities模块
helpers模块
{% endnote %}
具体的内容我就不一一细说了，主要还是这些模块中定制的 mixin 用到的真的不多，有些 mixin 甚至有点鸡肋，实在是找不到哪个地方能够用到。