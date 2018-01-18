---
title: Webpack学习记录
date: 2018-01-16 10:54:46
tags: [Webpack]
categories: '编程'
---



{% cq %}
爱的时候不辜负人  
玩的时候不辜负风景  
睡觉时不辜负床  
一个人时不辜负自己  

**卢思浩**
{% endcq %}

<!-- more -->


前段时间在掘金看过一篇文章，叫做[《webpack 为什么这么难用？》](https://juejin.im/post/5a2ff0b2f265da433562bce4)，深有感触吧，之前我有学过一段时间，但是由于官方文档确实太过于杂乱了，而且怎么说呢，按文档介绍的流程学习下来，太片段化了，没有形成一个知识体系，于是乎过一段时间不用就忘记了。看过前面的那篇文章之后，我又去翻了翻 Webpack 的官方文档，似乎比以前好理解了一些，可能是以前水平不够理解不了吧，哈哈。这篇文章简单介绍下，以我目前的开发习惯所配套使用到的 Webpack 知识点，以后学习的过程中有什么新的发现我会在这篇文章中更新。使用到的项目：[psd-to-html](https://github.com/carolyicheng666/psd-to-html)  



准备工作
---

- 安装  
  下面是官网原话：
{% note danger %}
不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。
{% endnote %}
  所以请在项目的主目录下使用命令进行本地安装
``` bash
$ npm install --save-dev webpack
```

- 创建webpack.config.js  
  以后关于项目的复杂配置全都写在此文件中，避免每次编译等操作都在终端键入命令的麻烦

- 添加npm脚本  
  在 `package.json` 中 scripts 字段内添加
``` json
"start": "webpack"
```
  这样我们就可以使用 `npm start` 来执行 `webpack.config.js` 了

接下来我们来介绍如何配置 webpack.config.js 。官网给出的[可配置项](https://doc.webpack-china.org/configuration)特别多，我在前面说过了，我只挑符合我开发习惯的讲，所以不会太全面和深入  



输入输出
---

任何一种打包工具，大家最关心的可能都是如何输入输出，比如基于任务流的 gulp 采用管道的方式，开始的 `gulp.src()` 作为输入，一级一级向后，每一个管道的输入都是上一个管道的输出，最终由 `gulp.dest()` 输出。

当然，拿 gulp 和 webpack 比较是不恰当的。gulp 是一种工具，能够优化前端工作流程，而 webpack 本身是一种 js 模块化的方案，通过配置模块loader和插件等可以将css，html等也进行模块化，两者功能上有重合之处，但各自侧重点不同。

回到正题，还是来说一下webpack是如何输入输出的：    
**输入**  
`entry:` 后写的是输入的文件路径，可以接受字符串、字符串数组和对象。如果是字符串或字符串数组，那么默认输出文件会被命名为main；如果是对象，那么默认输出文件就会被命名为其对应的key，当然在输出的时候都可以更改这些默认名。  
**输出**  
`output:` 后写的是输出的配置，配置项特别多，大概有二三十个，挑几个比较重要的说下：  
*path*表示所有输出文件的目标路径，特别说明必须是绝对路径，所以请使用 `__dirname` 这个参数，这里有两种写法，一种是官方使用的 `path.resolve(__dirname, "dist")` ，这种方式需要在文件头部引入path，即 `const path = require('path');` ；另一种是使用字符串拼接的办法，即 `__dirname + "/dist/"`，两种方式效果是一样的  
*filename*表示输出文件的文件名，可以动态设置，如 `[name].js` 或 `[chunkhash].js` 等等  
*publicPath*表示输出解析文件的目录，在 path 不确定的情况下这个配置会很有用  
*chunkFilename*表示非入口文件的名称  
......



模块配置
---

模块的配置写在 module 字段的 rules 中，它是一个对象数组，这里确实没什么好说的，无非就是下载 loader，使用 loader 的过程，使用方式去 github 上搜索对应 loader 即可

我把我的项目[psd-to-html](https://github.com/carolyicheng666/psd-to-html)中使用的 module 拿出来介绍一下
``` javascript
module: {
  rules: [
    {
      test: /\.scss$/,
      use: [
        'style-loader',
        'css-loader',
        'postcss-loader',
        'sass-loader'
      ]
    },
    {
      test: /\.(png|svg|jpg|gif)$/,
      use: [
        'file-loader?name=images/[hash].[ext]'
      ]
    },
    {
      test: /\.html$/,
      loader: 'html-withimg-loader'
    }
  ]
}
```
需要新建一个 postcss.config.js 配置文件，用于将插件集成至 postcss 中
``` javascript
module.exports = {
  plugins: [
    require('autoprefixer')({ browsers: ['last 2 versions'], cascade: false})
  ]
}
```
需要什么 loader 就去下载，反正就是一顿 `npm i XXX --save-dev`，然后看看使用方式，复制粘贴再改改就好了



插件
---

插件的配置写在 plugins 字段中，它是一个数组，还是拿前面那个项目举例：
``` javascript
plugins: [
  new webpack.BannerPlugin('版权所有，翻版必究'),
  new webpack.optimize.CommonsChunkPlugin({
    names: ['vendor', 'manifest']
  }),
  new CleanWebpackPlugin(['webpack-build'], { verbose: true }),
  new webpack.optimize.UglifyJsPlugin(),
  new HtmlWebpackPlugin({
    template: __dirname + "/dist/index.tmpl.html"
  })
]
```
`webpack.XXX` 为 webpack 内置插件，不需要额外下载，只需要在文件头部引入webpack即可，即 `const webpack = require('webpack');` ，其余都需要额外下载和引入，比如上面这个例子下载完还需要
``` javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require("clean-webpack-plugin");
```
这些插件的作用分别是：  
- **BannerPlugin** 表示在输出文件头部打上注释 `版权所有，翻版必究`；  
- **CommonsChunkPlugin** 表示将公共模块拆分出来单独打包，这样在调用时只需要加载一次，提高运行速度，*注意*，name传入的是名称，如果是第三方库可以在入口entry处写上路径；  
- **CleanWebpackPlugin** 表示编译时清空 `webpack-build` 文件夹；
- **UglifyJsPlugin** 表示使编译后的文件压缩；
- **HtmlWebpackPlugin** 表示使用已有 `html` 模板生成 html 文件  

Webpack还有很多值得探索和学习的东西，大家加油~