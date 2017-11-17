---
title: Sublime Text 3 + LiveReload实时刷新网页
date: 2017-11-17 16:05:26
tags: [Sublime]
categories: '编程'
---

{% cq %}
不是因为有翅膀所以能飞翔，而是因为想要飞翔，所以才有翅膀

**加速世界**
{% endcq %}

<!-- more -->

&emsp;&emsp;之前的文章《一些常用的gulp插件整理》中最后提到了 `browser-sync` 插件，这是一个浏览器同步测试的插件，我们只要修改了插件监听的文件，网页就会实时刷新。但是如果我们只是写一个小项目，不用搞这么大的工程，不用什么 `Grunt`、`Gulp` 来构建，那么我们就需要来思考下如何实时监听了。

&emsp;&emsp;于是，我找了到这么一款插件 —— Live​Reload。它需要在 Sublime Text 3 和浏览器里一起安装使用。下面介绍这个插件的安装、配置及使用方法。

&emsp;&emsp;首先，在 Sublime Text 3 中使用 `ctrl+shift+p` 打开 `Package Control` ，输入 `Package Control: Install Package` 并回车后，搜索 `LiveReload` 插件，敲下回车安装此插件。
&emsp;&emsp;当然，如果由于种种原因一直打不开插件列表，没办法搜索插件，没关系，我们可以到作者的 [<span style="color: red;">Github项目地址下载</span>](https://github.com/Grafikart/ST3-LiveReload)，安装方式是在 `Sublime` 工具栏 `Preferences > Browser Packages...` 打开 `Packages` 目录，解压下载的插件压缩包到这个目录下，并重命名为 `LiveReload` 。
**注意**，插件作者的介绍中明确指出：
{% note danger %}
A web browser page reloading plugin for the Sublime Text 3 editor.
{% endnote %}
还停留在远古版本的 Sublime Text 2 的老铁们悲剧了...

&emsp;&emsp;其次，在浏览器中扩展程序中搜索 `LiveReload` 插件并安装，也可以直接外部搜索下载后添加到扩展程序中。
安装完之后，选择管理扩展程序，把 `LiveReload` 这个插件的**允许访问网址文件**这一选项勾选上。

&emsp;&emsp;最后，如何使用呢？
&emsp;&emsp;我们在 Sublime Text 3 中使用 `ctrl+shift+p` 打开 `Package Control`，输入 `LiveReload: Enable/disable plugins` 并回车，选择 `Enable - Simple Reload` 这一选项，完成配置后打开我们的 `html` 文件，并点击浏览器地址栏旁边的 `LiveReload` 按钮，这时看到按钮变成了实心的，于是我们就可以愉快的玩耍了。

&emsp;&emsp;总结一下，`LiveReload` 是一款非常棒的插件，可以在浏览器中实时刷新页面，这样就可以做到保存就能立马看到效果。省去配置本地环境和消除 `Grunt` 和 `Gulp`工具的局限性，操作起来也非常简单。
