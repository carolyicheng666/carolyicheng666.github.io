---
title: Hexo+NexT搭建博客之踩坑
date: 2017-11-08 10:24:24
tags: [Hexo, NexT]
categories: '编程'
---

{% cq %}年轻人少看点成功学的书{% endcq %}

<!-- more -->



重要的事情
---

先按照 [<span style="color: red;">Hexo官方文档</span>](https://hexo.io/zh-cn/docs/) 和 [<span style="color: red;">NexT官方文档</span>](http://theme-next.iissnan.com/) 按自己需要一步步搭建
下面才是本文的重点



管理项目
---

其实，`Hexo` 生成的文件里面是有一个 `.gitignore` 的，所以它的本意应该也是想我们把这些文件放到 `GitHub` 上存放的。但是考虑到如果每个 `GitHub Pages` 都需要额外的一个仓库存放这些文件，就显得特别冗余了。这个时候就可以用分支的思路！一个分支用来存放 `Hexo` 生成的网站原始的文件，另一个分支用来存放生成的静态网页。

一、关于搭建的流程
1. [<span style="color: red;">创建仓库</span>](https://github.com/carolyicheng666/carolyicheng666.github.io)
2. 创建两个分支：`master` 与 `hexo`
3. 设置 `hexo` 为默认分支（因为我们只需要手动管理这个分支上的 `Hexo` 网站文件）
4. 拷贝仓库
``` bash
$ git clone git@github.com:carolyicheng666/carolyicheng666.github.io.git
```

5. 在本地 `carolyicheng666.github.io` 文件夹下通过依次执行如下命令，此时当前分支应显示为 `hexo`
``` bash
$ npm install hexo
$ hexo init
$ npm install
$ npm install hexo-deployer-git
```

6. 修改 `_config.yml` 中的 `deploy` 参数，分支应为 `master` 
``` yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:carolyicheng666/carolyicheng666.github.io.git
  branch: master
```
	后面回过头来想想，其实这一点很精髓，无论是改配置还是写博客，都只需要在 `hexo` 分支上完成，然后用 `hexo g -d` 命令发布到 `master` 分支上，页面即可正常显示，本地也不用切换分支

7. 依次执行如下命令，提交网站相关的文件
``` bash
$ git add .
$ git commit -m "..."
$ git push origin hexo
```

8. 执行
``` bash
$ hexo g -d
```
	生成网站并部署到 `GitHub` 上

这样一来，在 `GitHub` 上的仓库就有两个分支，一个 `hexo` 分支用来存放网站的原始文件，一个 `master` 分支用来存放生成的静态网页。

二、关于日常的改动流程
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理。
1. 依次执行
``` bash
$ git add .
$ git commit -m "..."
$ git push origin hexo
```
	指令将改动推送到 `GitHub` （此时当前分支应为 `hexo` ）

2. 然后才执行
``` bash
$ hexo g -d
```
	发布网站到master分支上

虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）。

三、本地资料丢失后的流程
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
1. 使用如下命令，拷贝仓库（默认分支为 `hexo` ）
``` bash
$ git clone git@github.com:carolyicheng666/carolyicheng666.github.io.git
```

2. 在本地新拷贝的 `carolyicheng666.github.io` 文件夹下依次执行下列指令：
``` bash
$ npm install hexo
$ npm install
$ npm install hexo-deployer-git
```



建立关于页
---

在命令行里面输入
``` bash
$ hexo new page "about"
```
然后你会发现 `source` 里面多了个目录 `about` ，里面有个 `index.md` ，其实你也可以手动建立。页面的格式和文章一样，然后在 `themes\next\_config.yml` 将 `about: /about/ || user` 前面的注释取消即可



建立标签云
---

1. 新建一个页面，命名为 `tags` 。命令如下
``` bash
$ hexo new page "tags"
```

2. 编辑刚新建的页面，将页面的类型设置为 tags ，主题将自动为这个页面显示标签云。页面内容如下
``` md
title: Tagcloud
date: 2014-12-22 12:39:04
type: "tags"
---
```
	注意：如果有启用多说 或者 `Disqus` 评论，默认页面也会带有评论。需要关闭的话，请添加字段 `comments` 并将值设置为 `false`。

3. 在菜单中添加链接。编辑 `主题配置文件` ，添加 `tags` 到 `menu` 中，如下
``` yml
menu:
  tags: /tags/ || tags
```



添加网易云音乐
---

打开网易云音乐官网，直接搜索我们想要插入的音乐，然后点击生成外链播放器，然后可以根据你得设置生成相应的 `html` 代码，将获得的html代码插入到你想要插入的位置即可，比如我放在了侧边栏下，找到 `layout\_macro\sidebar.swig` 文件，添加如下代码
``` html
<div id="music163player">
      <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=300 height=86 src="https://music.163.com/outchain/player?type=2&id=33875750&auto=1&height=66"></iframe>;;
</div>
```
注意，`auto=1` 表示自动播放



添加 Fork me on GitHub
---

去 [<span style="color: red;">Ribbons</span>](https://github.com/blog/273-github-ribbons) 挑选自己喜欢的样式，并复制代码，添加到 `themes\next\layout\_layout.swig` 的 `body` 标签之内即可，如果被顶部栏遮住了，在样式里设置 `z-index` 将其拉高即可，记得把里面的 `url` 换成自己的！



站点头像改成圆形
---

在 `themes\next\source\css\_common\components\sidebar\sidebar-author.styl` 中 `.site-author-image` 定义中增加
``` styl
border-radius: 50%;
```



去掉站点链接前面的小圆点
---

如果不太喜欢站点链接前面的小圆点，去掉 `themes\next\source\css\_common\components\sidebar\sidebar-author-links.styl` 中 `a::before` 的定义即可。



hexo-wordcount 实现统计功能
---

``` bash
$ npm install hexo-wordcount --save
```
安装完插件之后在主题的配置文件中开启该功能就可以~
``` yml
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
  totalcount: false
  separated_meta: true
```
然后找到 `themes\next\layout\_macro\post.swig` 文件，看个人喜好吧，修改如下两个部分
``` swig
...
<span title="{{ __('post.wordcount') }}">
    {{ wordcount(post.content) }} 字
</span>
...
<span title="{{ __('post.min2read') }}">
    {{ min2read(post.content) }} 分钟
</span>
...
```



打赏功能和copyright
---

按照官网设置之后，本地测试成功，但是部署之后，生产环境会有问题。
我在 [<span style="color: red;">这篇文章</span>](https://github.com/iissnan/hexo-theme-next/pull/687) 找到了答案，感谢 BalanceUhen 提供的解决办法：
{% note info %}
找到 `themes\next\source\css\_common\components\post\post.styl`，把 `@import "post-reward" `后面的 `if` 给删除掉后主题就会被导入到 `css` 中了。
{% endnote %}

另外，把 `@import "post-copyright"` 后面的 `if` 删除掉，copyright 也能生效了。


