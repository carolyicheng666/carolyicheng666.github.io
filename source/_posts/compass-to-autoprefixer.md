---
title: 从 Compass 到 Autoprefixer
date: 2017-11-20 17:51:47
tags: [Sass, CSS, Compass, Autoprefixer, Gulp]
categories: '编程'
---

{% cq %}
一个人若没有怀揣过大的梦想，也就无法到达向往的地方

**冰上的尤里**
{% endcq %}

<!-- more -->



自从学会了 Sass 之后，就觉得再也离不开 Sass 了。但是，Sass 和原生 CSS 一样，也要面临着 CSS3 浏览器前缀的问题。众所周知，在 CSS3 发布之后，由于各大浏览器的兼容性，我们往往需要对 CSS3 属性加上相应的浏览器前缀，该属性才能才特定的浏览器中生效。

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

这个时候我们就需要来思考一下，如何才能避免加前缀的麻烦呢？

[Compass](http://compass-style.org/)
---
{% note danger %}
Compass is an open-source CSS Authoring Framework.  
Compass uses Sass.
{% endnote %}

因为 Sass 并不能被浏览器识别，要编译成普通的 CSS 才行，Compass 使用 Sass 的 mixin 为 CSS3 需要带前缀的属性定制了一些 mixin，这使得 Compass 在编译 Sass 时能够自动加上浏览器前缀。
当然，除 CSS3 之外，Compass 还有很多特殊定制，主要分为六大模块：
{% note info %}
[reset模块](http://compass-style.org/reference/compass/reset/)  
[CSS3模块](http://compass-style.org/reference/compass/css3/)  
[layout模块](http://compass-style.org/reference/compass/layout/)  
[typography模块](http://compass-style.org/reference/compass/typography/)  
[utilities模块](http://compass-style.org/reference/compass/utilities/)  
[helpers模块](http://compass-style.org/reference/compass/helpers/)
{% endnote %}

具体的内容我就不一一细说了，主要还是这些模块中定制的 mixin 用到的真的不多，有些 mixin 甚至有点鸡肋，实在是找不到哪个地方能够用到。我在 Compass 官网的 [更新日志](http://compass-style.org/CHANGELOG/) 中找到了问题所在，Compass 的最新版本是 Version 1.0.1 ，但这却是2014年8月19号更新的，也就是说截至今日，它已经三年多没有再更新过了。三年以来，对于很多 CSS3 的属性，随着浏览器的更新早已不再需要添加私有前缀了，这时候我们必须要换掉 Compass 了，不再更新的 Compass 已经过时而被淘汰了。

[Autoprefixer](https://www.npmjs.com/package/autoprefixer)
---
{% note danger %}
PostCSS plugin to parse CSS and add vendor prefixes to CSS rules using values from Can I Use. It is recommended by Google and used in Twitter and Taobao.
{% endnote %}

用SASS、LESS、Stylus或者其他类似的工具都是属于 CSS 的预处理器（Preprocessor），而 Autoprefixer 则是一种后处理器（Postprocessor）。它是直接针对 CSS 本身来进行处理，不需要任何额外的语法。因为它是在 CSS 代码产生之后才进行后续处理。  
Autoprefixer 会分析 CSS 代码，并且根据 Can I Use 所提供的资料来决定要加上哪些浏览器前缀，而你要做的事情就是把他加入自己的自动化开发工具中（如Grunt或Gulp），然后就可以直接使用 W3C 的标准来写 CSS，不需要加上任何浏览器的私有前缀。  
这样任务就得到了分解，拿 Gulp 来举例：  
第一步：我们需要将原始的 Sass 文件编译成普通的 CSS 文件（使用 [gulp-sass](https://github.com/dlmanning/gulp-sass)）  
第二步：我们再将编译后的 CSS 文件后处理成带有浏览器私有前缀的 CSS 文件（使用 [gulp-autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer)）

关于PostCSS，gulp-autoprefixer的作者是这么描述的：
{% note info %}
If you use other PostCSS based tools, like cssnano, you may want to run them together using gulp-postcss instead of gulp-autoprefixer. It will be faster, as the CSS is parsed only once for all PostCSS based tools, including Autoprefixer.
{% endnote %}

这段话大致是说，如果你只使用autoprefixer这一个插件，那么就用gulp-autoprefixer；如果还要使用别的插件，那就使用 PostCSS ，将其他插件写在它里面，速度更快，写法像下面这样：
``` javascript
gulp.task('css', function () {
  var postcss    = require('gulp-postcss');
  var sourcemaps = require('gulp-sourcemaps');

  return gulp.src('src/**/*.css')
    .pipe( sourcemaps.init() )
    .pipe( postcss([ require('precss'), require('autoprefixer') ]) )
    .pipe( sourcemaps.write('.') )
    .pipe( gulp.dest('build/') );
});
```

当然，还得 PostCSS 支持才行，比如它对于语法（syntax）这一项的描述：
{% note warning %}
PostCSS can transform styles in any syntax, not just CSS. If there is not yet support for your favorite syntax, you can write a parser and/or stringifier to extend PostCSS.
- postcss-scss allows you to work with SCSS (but does not compile SCSS to CSS).
- postcss-sass allows you to work with Sass (but does not compile Sass to CSS).
- postcss-less allows you to work with Less (but does not compile LESS to CSS).
{% endnote %}

重点是括号里面的话，哈哈，看看就好。  
我个人比较喜欢一个东西只干一件事，不太喜欢集成式的思维逻辑，毕竟分开干活，查 Bug 非常快，pipe的精髓就是如此，上一级的输出作为下一级的输入，谁输出的有问题，一目了然。上面提到的两个步骤，用两个 pipe 就可以搞定，当然更极端一点，可以把它们分为两个单独的 task 分开执行，注意执行顺序即可。

随着技术不断发展，CSS3 属性前缀的问题已不再是一个问题。如今你完全可以忽略各个 CSS3 属性要不要加前缀，要加哪些前缀，而只需要专心去写你的代码。把这些烦人的事情交给 Autoprefixer 去处理。当然，越到后面，或许我们都不需要使用任何前缀。