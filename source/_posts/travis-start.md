---
title: Travis 持续集成入门
date: 2018-02-22 11:13:05
tags: [Travis]
categories: '编程'
---



{% cq %}
你指尖跃动的电光，是我此生不灭的信仰

**某科学的超电磁炮**
{% endcq %}


<!-- more -->



今天是过年回来第一天上班，大家都忙着收红包呢，抽空写一篇 Travis 入门吧。

最初是因为看到很多人的项目中都有一个绿色的 `build:passing` 徽章，觉得好高大上，于是就去研究了一下。如何才能拿到这个徽章呢？下面进入正题。

首先，进入 Travis 官方网站，公共仓库点[这里](https://travis-ci.org/)，私有仓库点[这里](https://travis-ci.com/)。按照官网的意思两种仓库的设置方式是一样的，我没有私有仓库，这一点有待证实，只能拿公共仓库继续讲了。首次访问需要授权给 Travis 访问你的 Github 代码库。

然后就可以看到你存放在 Github 上所有的项目目录了，选择想要持续集成的项目，点击将项目前面的 `X` 改成 `√`，再点击项目名称前的设置按钮进入设置界面。

在设置界面中，作为入门级教程，下面的什么环境变量、定时任务我们都可以不用管，只要看最上面 `General` 里面的配置即可。这里默认打开了 `Build pushed branches` 和 `Build pushed pull requests` 这两项，我们还需要将 `Build Only if.travis.yml is present` 这一项也打开。

接下来，我们在项目的根目录下添加 `.travis.yml` 文件，目的是告知 Travis 在我们push之后需要做什么工作。这里的配置，不同岗位的区别比较大，好在 Github 现成的例子到处都是，大家随便搜几个项目看一下，技术栈与自己基本一致的，把他的 `.travis.yml` 贴过来，小改一下就能用了。本人只是一个小小的前端工程师，贴一个
gulp的例子吧：
``` yml
sudo:          false
language:      node_js
node_js:
  - "node"
install:       npm install
script:
  - gulp
cache:
  directories:
    - node_modules

```

更多的配置可参考[官方文档](https://docs.travis-ci.com/)，选择并展开 `Programming Languages` 项，在选择自己项目的语言类型，里面有详细的配置介绍，这里就不做过多说明了。

最后我们只需要将本次项目的改动提交，在push之后，Travis 就会开始工作了，大家可以去到 `https://travis-ci.org/GithubName/ProjectName` 查看 Travis 的工作状态。build成功则会在标题后面显示 `build:passing`，否则则显示 `build:failed`。点击该徽章会生成一个 `Status Image` 界面，将 url 复制出来即可。


那么 Travis 到底有什么用呢？这个问题有待深究，这里暂且不答，只引用廖雪峰老师在[《使用Travis进行持续集成》](https://www.liaoxuefeng.com/article/0014631488240837e3633d3d180476cb684ba7c10fda6f6000)提到的两个问题和解答：
{% note success %}
Q: 是不是用了CI代码质量就有保证了？  
A: 这个问题的答案是否。如果CI能提高代码质量，那软件公司只需要招实习生写代码外加CI就可以了，招那么贵的高级工程师浪费钱干啥？

Q: 是不是用了Travis就实现了CI？  
A: 这个问题的答案还是否。CI是解决问题的手段而不是目的。问题是如何提高代码质量。我见过很多公司的项目，编译一次半小时（不是编译Linux内核那种），测试一次几个小时。不能在短时间内完成编译、测试的代码都有问题，导致CI形同虚设。这里的“短时间”是指5分钟以内。
{% endnote %}
