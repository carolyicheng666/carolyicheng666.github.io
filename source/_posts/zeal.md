---
title: Zeal + Sublime 快速查阅API文档
date: 2017-11-28 15:28:09
tags: [Zeal, Sublime]
categories: '软件'
---


{% cq %}
远的不是距离，而是次元啊

**路人女主的养成方法**
{% endcq %}

<!-- more -->



简介
---

{% note danger %}
Zeal is an offline documentation browser for software developers.
{% endnote %}
一个简单的离线 `API` 文档浏览器



[下载地址](https://zealdocs.org/)
---

面向的用户是 `Windows/Linux/macOS` ，`Windows` 难民可以使用，开不开心 ^O^!!
macOS用户建议使用 `Dash` ，[<span style="color: red;">下载地址</span>](https://kapeli.com/dash)



[下载API](https://zealdocs.org/usage.html)
---

{% note danger %}
After installing Zeal go to `Tools -> Docsets` to browse and download docsets.
{% endnote %}
挑选自己平时用到的 `API` 下载即可



Sublime安装Zeal
---

- `Ctrl + Shift + P`，输入 `zeal` 并选择 `Zeal` 插件安装。

- 安装完成后，在 `Preference -> Package Settings -> Zeal -> Settings User` 中加入如下字段：
"zeal_command": "Zeal安装路径/zeal.exe"
如："zeal_command": "C:/Program Files/Zeal/zeal.exe"
**注意**，从目录复制过来的路径要把反斜杠改成斜杠

- 使用方式：`F1` 和 `Shift + F1`
{% note danger %}
F1 - Open Zeal documentation for current/selected word.
Shift + F1 - Open Zeal search bar. 
{% endnote %}

- 在Zeal内搜索方式是：
{% note danger %}
`string` will search all docsets for string
`python:string` will search only docsets related to Python for string
{% endnote %}

- 如果你使用的是别的编辑器，例如 Atom、Vim等等，[<span style="color: red;">这里</span>](https://zealdocs.org/usage.html) 也可以找到安装配置的方法



写在后面
---

- 下载的 API 文档存放在 `C:\Users\XXX\AppData\Local\Zeal\Zeal\docsets` 目录下
