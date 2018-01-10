---
title: Element
date: 2018-01-10 17:46:21
tags: [Element, Vue]
categories: '编程'
---


{% cq %}
无主之物啊  
汇聚于梦之杖，成为吾之力量吧  
Secure!

**魔卡少女樱 CLEAR CARD篇**
{% endcq %}

<!-- more -->



最近学习了**Element**，它是一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库。官方文档**[<span style="color: red;">地址</span>](http://element-cn.eleme.io/#/zh-CN)**

这个库有**4大特性**，分别是：  

**一致性 Consistency**

- 与现实生活一致：与现实生活的流程、逻辑保持一致，遵循用户习惯的语言和概念；
- 在界面中一致：所有的元素和结构需保持一致，比如：设计样式、图标和文本、元素的位置等。

**反馈 Feedback**

- 控制反馈：通过界面样式和交互动效让用户可以清晰的感知自己的操作；
- 页面反馈：操作后，通过页面元素的变化清晰地展现当前状态。

**效率 Efficiency**

- 简化流程：设计简洁直观的操作流程；
- 清晰明确：语言表达清晰且表意明确，让用户快速理解进而作出决策；
帮助用户识别：界面简单直白，让用户快速识别而非回忆，减少用户记忆负担。

**可控 Controllability**

- 用户决策：根据场景可给予用户操作建议或安全提示，但不能代替用户进行决策；
- 结果可控：用户可以自由的进行操作，包括撤销、回退和终止当前操作等。

最新的官方文档在 2018-01-08 更新到了 2.0.11 ，之前有过 Vue 的基础，我就按照 Vue 的路线一路学下来了，另外也可以按照 React 和 Angular 的路线学习

想要上手它除了前面说的 Vue 或 React 或 Angular 基础以外，还需要会使用 Webpack，官方提供了初学者的[模板](https://github.com/ElementUI/element-starter)，可以先 clone 下来研究一下，安装好相关的依赖之后，就可以进行开发了，开发在 `src` 目录下，编译后会生成 `dist` 目录，可放入生产环境中

``` bash
//开发模式
npm run dev

//编译
npm run build
```

我把官方文档的例子整合到了我的**[<span style="color: red;">代码仓库</span>](https://github.com/carolyicheng666/element-study)**中，修改 `src` 目录下的 `main.js` 文件中 `import App from 'XXX'`这一行，可以看到与官方文档相对应的章节内容

目前官方文档仍然有一些bug，而且部分实例和代码呈现的效果不一致，能处理的我都尽量处理了，不能处理的我拿红色的 Bug 字眼标明了，期待官方的更新和修复

