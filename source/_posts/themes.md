---
title: 关于themes/next无法上传github备份
date: 2017-11-08 16:49:47
tags: [Hexo, themes]
---

<blockquote class="blockquote-center">业精于勤，荒于嬉；行成于思，毁于随
<strong>韩愈</strong>
</blockquote>

<!-- more -->

&emsp;&emsp;作为一个写代码的人来说，保存和备份就像是吃饭和喝水一下重要，所以随手保存和存有备份已经成为我的习惯了。使用 `Hexo` 在 `github` 搭建的博客，仓库里只有生成的静态网页文件，是没有 `Hexo` 的源文件的，如果现在这个电脑出现了什么问题，需要换一台电脑，那就麻烦了，所以我就研究了一下怎么备份。

&emsp;&emsp;关于如何备份我已经在前一篇文章《Hexo+NexT搭建博客之踩坑》中 “管理项目” 这一章节详细说明了，但是细心的朋友会发现，即使按照步骤一步步做下来，依然会发现自己引入的 `themes` （我这里就是 `next`）这个文件夹无法备份。当然，我在 `next` 的 `Issues` 找到了[该问题及其解决办法](https://github.com/iissnan/hexo-theme-next/issues/932)。关于大神们提到的 `submodule` 和 `subtree` 这两种方式，算是比较官方的做法了，我自己也试过了 `subtree` ，国内速度太慢实在是受不了，于是乎我就开始想如何简化这些操作，下面请开始我的表演：

&emsp;&emsp;首先，进入 `themes/next` 文件夹，把里面 `.git` 文件夹删除

&emsp;&emsp;然后，回到上一级目录 `themes` ，执行

``` bash
$ git rm -r --cached .
$ git add .
$ git commit -m "..."
$ git push -u origin hexo
```

&emsp;&emsp;这样就 `push` 上去了，以后就像原来一样操作就行了。

&emsp;&emsp;最后，我说一下这样做的缺点：这种做法完全是本地化的行为，也就是说，如果 `NexT` 主题的原作者对其项目做了更新，我们是没办法第一时间更新我们自己的项目的，相当于我们只 `fork` 了原作者的项目，拿到本地来自己个性化配置，然后 `push` 到我们自己的项目中使用。而 `submodule` 和 `subtree` 的做法是将原项目当做公用库来使用，避免了我这种做法的缺点，但是，至于个性化的配置，由于我们是直接更改原项目的配置文件，即官方文档所说的更改 `主题配置文件` ，所以我还是觉得 `submodule` 和 `subtree` 这两种做法稍显鸡肋了。