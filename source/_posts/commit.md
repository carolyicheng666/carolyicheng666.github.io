---
title: commit 规范
date: 2018-02-23 17:03:50
tags: []
categories: '随笔'
---



{% cq %}
当你凝视深渊时，深渊也在凝视你

**尼采《善恶的彼岸》**
{% endcq %}


<!-- more -->


最近看到一篇文章：[《Git Commit Message Conventions》](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#)，里面提到了 commit message 要规范化，它可以在出现问题时，快速定位问题，找到出现错误的位置，从而解决问题。我也因此开始慢慢改变以前的坏习惯。

但是人总是健忘的，我决定写下这篇文章，以此来提醒自己。

原文的规范是 Angular 的 commit 规范，包含三个部分：header、body、footer
``` bash
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```
其中，body 和 footer 可以省略，实际情况大多数 commit message 也都只有一行 header，header 中的 scope 范围也可以省略，但是 `type` 和 `subject` 是绝对不能省略的，它们是 commit message 的核心。感觉废话好多...还是直接说这两部分怎么写吧。原文把 `type` 分为 7 类，所有的 commit 只能从这 7 类中选择  
{% note danger %}
- feat (feature)
- fix (bug fix)
- docs (documentation)
- style (formatting, missing semi colons, …)
- refactor
- test (when adding missing tests)
- chore (maintain)
{% endnote %}

翻译过来就是：
{% note info %}
- feat：新功能
- fix：修复 bug
- docs：文档
- style： 格式化代码
- refactor：重构
- test：测试
- chore：其它维护
{% endnote %}

`subject` 就是写一些简要的描述信息，但是也不能乱写，原文提到了三点要求
{% note danger %}
- use imperative, present tense: “change” not “changed” nor “changes”
- don't capitalize first letter
- no dot (.) at the end
{% endnote %}

意思也很简单：
{% note info %}
- 使用祈使句
- 首字母不要大写
- 末尾不要加(.)
{% endnote %}

最后，要改变一个长时间养成的坏习惯真的很不容易...
