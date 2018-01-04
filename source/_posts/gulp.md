---
title: 一些常用的Gulp插件整理
date: 2017-11-09 11:09:52
tags: [Gulp]
categories: '编程'
---

{% cq %}
员工离职，只有两个真实原因：钱没给到位；心受委屈了

**马云**
{% endcq %}

<!-- more -->



Gulp 和 Grunt 对比
---

我觉得 [<span style="color: red;">这篇对比介绍</span>](http://slides.com/contra/gulp#/) 写的很好，大家可以看看。其中作者提到了为什么 `Grunt` 不好的原因：
- Plugins do multiple things：Want a banner? Use the javascript minifier
- Plugins do things that don't need to be plugins：Need to run your tests? Use a plugin
- Grunt config format is a mess that tries to do everything：Not idiomatic with "the node way"
- Headache of temp files/folders due to bad flow control

作者明确指出：
{% note danger %}
Your build system should empower not impede.
It should only manipulate files - let other libraries handle the rest.
{% endnote %}
这一点我也表示赞同，不过还是看个人喜好吧，这里不做过多讨论。
下面才是本文的重点，都是目前我用过的插件，点击标题即可跳转至官方文档，持续更新...



[gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css)
---
{% note info %}
gulp plugin to minify CSS, using clean-css  
一款压缩css的插件
{% endnote %}

``` javascript
let gulp = require('gulp');
let cleanCSS = require('gulp-clean-css');
 
gulp.task('minify-css', () => {
  return gulp.src('styles/*.css')
    .pipe(cleanCSS({compatibility: 'ie8'}))
    .pipe(gulp.dest('dist'));
});
```
其中`compatibility: 'ie8'`表示兼容模式为 `ie8+`



[gulp-uglify](https://www.npmjs.com/package/gulp-uglify)
---
{% note info %}
Minify JavaScript with UglifyJS2  
一款压缩js的插件
{% endnote %}

官方文档用到了 `pump`，我这里还是使用原始的做法吧，和压缩 `css` 一样的写法就行
``` javascript
var gulp = require('gulp');
var uglify = require('gulp-uglify');
 
gulp.task('compress', function () {
  return gulp.src('lib/*.js')
    .pipe(uglify())
    .pipe(gulp.dest('dist'));
});
```



[gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin)
---
{% note info %}
gulp plugin to minify HTML.  
一款压缩html的插件
{% endnote %}

``` javascript
var gulp = require('gulp');
var htmlmin = require('gulp-htmlmin');
 
gulp.task('minify', function() {
  return gulp.src('src/*.html')
    .pipe(htmlmin({collapseWhitespace: true}))
    .pipe(gulp.dest('dist'));
});
```
更多配置选项设置请看 [<span style="color: red;">官方文档</span>](https://github.com/kangax/html-minifier)



[gulp-rename](https://github.com/hparra/gulp-rename)
---
{% note info %}
gulp-rename is a gulp plugin to rename files easily.  
一款重命名文件名的插件
{% endnote %}

``` javascript
var rename = require("gulp-rename");

// rename via string
gulp.src("./src/main/text/hello.txt")
  .pipe(rename("main/text/ciao/goodbye.md"))
  .pipe(gulp.dest("./dist")); // ./dist/main/text/ciao/goodbye.md

// rename via function
gulp.src("./src/**/hello.txt")
  .pipe(rename(function (path) {
    path.dirname += "/ciao";
    path.basename += "-goodbye";
    path.extname = ".md";
  }))
  .pipe(gulp.dest("./dist")); // ./dist/main/text/ciao/hello-goodbye.md

// rename via hash
gulp.src("./src/main/text/hello.txt", { base: process.cwd() })
  .pipe(rename({
    dirname: "main/text/ciao",
    basename: "aloha",
    prefix: "bonjour-",
    suffix: "-hola",
    extname: ".md"
  }))
  .pipe(gulp.dest("./dist")); // ./dist/main/text/ciao/bonjour-aloha-hola.md
```



[gulp-uncss](https://www.npmjs.com/package/gulp-uncss)
---
{% note info %}
Remove unused CSS with UnCSS  
一款移除无用css的插件
{% endnote %}

``` javascript
var gulp = require('gulp');
var uncss = require('gulp-uncss');
 
gulp.task('default', function () {
  return gulp.src('site.css')
    .pipe(uncss({
        html: ['index.html', 'posts/**/*.html', 'http://example.com']
    }))
    .pipe(gulp.dest('./out'));
});
```
说明一下，`html` 中的文件指的是引入了前面的 `site.css` 的。
**注意**，虽然 [<span style="color: red;">官方文档的Example</span>](https://www.npmjs.com/package/gulp-uncss) 中使用了 `gulp-cssnano` 插件压缩css，但是相比较于 `gulp-clean-css` 效果较差，故不推荐使用



[gulp-concat](https://www.npmjs.com/package/gulp-concat)
---
{% note info %}
Concatenates files  
一款合并文件的插件
{% endnote %}

``` javascript
var concat = require('gulp-concat');
 
gulp.task('scripts', function() {
  return gulp.src('./lib/*.js')
    .pipe(concat('all.js'))
    .pipe(gulp.dest('./dist/'));
});
```



[gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)
---
{% note info %}
Minify PNG, JPEG, GIF and SVG images with imagemin  
一款压缩图片的插件
{% endnote %}

``` javascript
const gulp = require('gulp');
const imagemin = require('gulp-imagemin');

//If versions >= 3
gulp.task('default', () =>
  gulp.src('src/images/*')
  .pipe(imagemin([
    imagemin.gifsicle({interlaced: true}),
    imagemin.jpegtran({progressive: true}),
    imagemin.optipng({optimizationLevel: 5}),
    imagemin.svgo({
      plugins: [
        {removeViewBox: true},
        {cleanupIDs: false}
      ]
    })
  ]))
  .pipe(gulp.dest('dist/images'))
);

//If versions < 3
gulp.task('default', () =>
  gulp.src('src/images/*')
  .pipe(imagemin({
    interlaced: true,
    progressive: true,
    optimizationLevel: 5,
    svgoPlugins: [{removeViewBox: true}]
  }))
  .pipe(gulp.dest('dist/images'))
);
```
看名字就知道啦：
- gifsicle — Compress GIF images
- jpegtran — Compress JPEG images
- optipng — Compress PNG images
- svgo — Compress SVG images

不过从结果来看，依然比不过强大的 [<span style="color: red;">tinypng</span>](https://tinypng.com/) ，如果对压缩的需求不是特别大，或者觉得手动 `tinypng` 太麻烦的话，就凑合着用吧，hiahia~



[imagemin-pngquant](https://www.npmjs.com/package/imagemin-pngquant)
---
{% note info %}
pngquant imagemin plugin  
配合 `gulp-imagemin` 进一步压缩图片的插件
{% endnote %}

``` javascript
const imagemin = require('imagemin');
const imageminPngquant = require('imagemin-pngquant');
 
imagemin(['images/*.png'], 'build/images', {use: [imageminPngquant()]}).then(() => {
    console.log('Images optimized');
});
```



[gulp-clean](https://www.npmjs.com/package/gulp-clean)
---
{% note info %}
Removes files and folders  
一款移除文件和文件夹的插件
{% endnote %}

``` javascript
var gulp = require('gulp');
var clean = require('gulp-clean');
 
gulp.task('default', function () {
  return gulp.src('app/tmp', {read: false})
    .pipe(clean());
});
```
`{read: false}`只是为了删除的更快，如果删除文件的内容在同一个工作流中有被用到，那么就不要设置这一项。



[gulp-compass](https://www.npmjs.com/package/gulp-compass)
---
{% note info %}
Compile Sass to CSS using Compass  
用 `compass` 把 `sass`编译成 `css` 的插件，即把手动编译的操作集成到 `gulp` 的 `task` 中
{% endnote %}

``` javascript
var compass = require('gulp-compass');
 
gulp.task('compass', function() {
  gulp.src('./src/*.scss')
    .pipe(compass({
      config_file: './config.rb',
      css: 'stylesheets',
      sass: 'sass'
    }))
    .pipe(gulp.dest('app/assets/temp'));
});
```
**注意**，`stylesheets` 和 `sass` 的值指的是文件路径



[gulp-sass](https://www.npmjs.com/package/gulp-sass)
---
{% note info %}
Sass plugin for Gulp.  
为gulp编译sass的插件
{% endnote %}

弃用compass后，我才开始使用这个，功能比较单一，经常要与PostCSS混合使用

``` javascript
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});
```

我们可以修改outputStyle，还是我们熟悉的那四种方式：  
嵌套输出方式——nested  
展开输出方式——expanded  
紧凑输出方式——compact  
压缩输出方式——compressed



[gulp-watch](https://www.npmjs.com/package/gulp-watch)
---
{% note info %}
File watcher that uses super-fast chokidar and emits vinyl objects  
文件监控，热更新
{% endnote %}

``` javascript
var gulp = require('gulp'),
    watch = require('gulp-watch');
 
gulp.task('stream', function () {
  // Endless stream mode 
  return watch('css/**/*.css', { ignoreInitial: false })
    .pipe(gulp.dest('build'));
});
 
gulp.task('callback', function () {
  // Callback mode, useful if any plugin in the pipeline depends on the `end`/`flush` event 
  return watch('css/**/*.css', function () {
    gulp.src('css/**/*.css')
      .pipe(gulp.dest('build'));
  });
});
```
说明：如果 `css/**/*.css` 发生改变，就执行后面的操作



[gulp-html-replace](https://www.npmjs.com/package/gulp-html-replace)
---
{% note info %}
Replace build blocks in HTML. Like useref but done right  
替换 `HTML` 中的代码块，比 `gulp-useref` 做的更好
{% endnote %}

html文件：
``` html
<!-- build:<name> -->
Everything here will be replaced
<!-- endbuild -->
```
gulpfile.js文件：
``` javascript
// Options is a single string 
htmlreplace({<name>: 'js/main.js'})
 
// Options is an array of strings 
htmlreplace({<name>: ['js/monster.js', 'js/hero.js']})
```
这边的要求有些严格，注释里面只能全是 `js` 或 `css` 才能这样写，如果不是，只能套用模板，举个例子：
``` javascript
// Options is an object 
htmlreplace({
  js: {
    src: 'img/avatar.png',
    tpl: '<img src="%s" align="left" />'
  }
})
 
// Multiple tag replacement 
htmlreplace({
  js: {
    src: [['data-main.js', 'require-src.js']],
    tpl: '<script data-main="%s" src="%s"></script>'
  }
})
```
为什么说比 `gulp-useref` 做的更好呢，请看下一小节~~



[gulp-useref](https://www.npmjs.com/package/gulp-useref)
---
{% note info %}
Parse build blocks in HTML files to replace references to non-optimized scripts or stylesheets with useref  
与 `gulp-html-replace`一样，也是替换 `HTML` 中的代码块，但是可以创建新的文件，写在注释中即可，说白了就是 `gulp-html-replace` 和 `gulp-concat` 的集成，如果同时使用 `gulp-if` ，可以把 `gulp-uglify` 和 `gulp-clean-css` 都写在这个 `task` 中，相当于把原先的多个 `task` 合并成一个
{% endnote %}

输入的html文件：
``` html
<html>
<head>
  <!-- build:css css/combined.css -->
  <link href="css/one.css" rel="stylesheet">
  <link href="css/two.css" rel="stylesheet">
  <!-- endbuild -->
</head>
<body>
  <!-- build:js scripts/combined.js -->
  <script type="text/javascript" src="scripts/one.js"></script> 
  <script type="text/javascript" src="scripts/two.js"></script> 
  <!-- endbuild -->
</body>
</html>
```
输出的html文件：
``` html
<html>
<head>
  <link rel="stylesheet" href="css/combined.css"/>
</head>
<body>
  <script src="scripts/combined.js"></script> 
</body>
</html>
```
gulpfile.js文件：
``` javascript
var gulp = require('gulp'),
    useref = require('gulp-useref'),
    gulpif = require('gulp-if'),
    uglify = require('gulp-uglify'),
    minifyCss = require('gulp-clean-css');
 
gulp.task('html', function () {
  return gulp.src('app/*.html')
    .pipe(useref())
    .pipe(gulpif('*.js', uglify()))
    .pipe(gulpif('*.css', minifyCss()))
    .pipe(gulp.dest('dist'));
});
```
要注意输出文件的编码格式
对于**大量集成`task`**的这种做法，我本人持保留态度，毕竟我还是习惯比较灵活的 `task` 配置方式，想执行哪个就执行哪个，一步到位的集成在 `default` 里面就 ok 啦~



[browser-sync](https://www.npmjs.com/package/browser-sync)
---
{% note info %}
Time-saving synchronised browser testing. Keep multiple browsers & devices in sync when building websites.  
浏览器同步测试，可以保证多个浏览器和设备的同步
{% endnote %}

关于在 `gulp` 中的搭建方式建议看 [<span style="color: red;">这里</span>](https://browsersync.io/docs/gulp)
我自己的项目结构一般是这样的，首先，主文件夹下放各种配置文件，比如上面有提到过的 `config.rb` 、 `package.json` 、 `LICENSE` 、 `appcache` 、 `gulpfile.js` 等等，其次，主文件夹下有个 `dist` 文件夹放项目的原文件，执行 `gulp` 输出到同级的 `build` 文件夹， `build` 这个文件夹是可以直接放入生产环境的，所以针对官方文档的样例，我需要改造一下：

``` javascript
var gulp        = require("gulp");
var sass        = require("gulp-ruby-sass");
var browserSync = require("browser-sync").create();

gulp.task('reload', ['default'], () =>
  browserSync.reload()
);

gulp.task('watch', ['default'], () => {
  browserSync.init({
      server: './'
  })
  gulp.watch(['./dist/*', './dist/**/*'], ['reload'])
});
```

这里面server的值前面一定要加上 `./` ，大家可以试一试，效果是不一样的


[gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer)
---
{% note info %}
Prefix CSS with Autoprefixer  
增加浏览器的私有前缀，让你不用再考虑为了写浏览器的兼容前缀而头疼
{% endnote %}

``` javascript
const gulp = require('gulp');
const autoprefixer = require('gulp-autoprefixer');
 
gulp.task('default', () =>
  gulp.src('src/app.css')
    .pipe(autoprefixer({
        browsers: ['last 2 versions'],
        cascade: false
    }))
    .pipe(gulp.dest('dist'))
);
```
autoprefixer 内置有 8 个选项可供配置：
{% note success %}
* `browsers` (array): list of browsers query (like `last 2 versions`), which are supported in your project. We recommend to use `browserslist` config or `browserslist` key in `package.json`, rather than this option to share browsers with other tools. See [Browserslist docs](https://github.com/ai/browserslist#queries) for available queries and default value.
* `env` (string): environment for Browserslist.
* `cascade` (boolean): should Autoprefixer use Visual Cascade, if CSS is uncompressed. Default: `true`
* `add` (boolean): should Autoprefixer add prefixes. Default is `true`.
* `remove` (boolean): should Autoprefixer [remove outdated] prefixes. Default is `true`.
* `supports` (boolean): should Autoprefixer add prefixes for `@supports` parameters. Default is `true`.
* `flexbox` (boolean|string): should Autoprefixer add prefixes for flexbox properties. With `"no-2009"` value Autoprefixer will add prefixes only for final and IE versions of specification. Default is `true`.
* `grid` (boolean): should Autoprefixer add IE prefixes for Grid Layout properties. Default is `false`.
* `stats` (object): custom [usage statistics](https://github.com/ai/browserslist#custom-usage-data) for `> 10% in my stats` browsers query.
{% endnote %}

官方的 Tip 中提到了 PostCSS：
{% note success %}
If you use other PostCSS based tools, like cssnano, you may want to run them together using gulp-postcss instead of gulp-autoprefixer. It will be faster, as the CSS is parsed only once for all PostCSS based tools, including Autoprefixer.
{% endnote %}

这段话大致是说，如果你只使用autoprefixer这一个插件，那么就用gulp-autoprefixer；如果还要使用别的插件，那就使用 PostCSS ，将其他插件写在它里面，速度更快。关于 PostCSS ，请看下一小节。

<span style="color: red;">更新一下，</span>大家在PostCSS中使用这个插件或者别的插件的时候可能会遇到如下问题：
``` bash
...
Error: [object Object] is not a PostCSS plugin
...
```
额，这里PostCSS支持的插件列表里并没有gulp-autoprefixer，而支持的是它的原版autoprefixer，我们只要改一下require就行了，用法是一样的，以后遇到这类问题一定得回头找一下PostCSS支持的[插件列表](https://github.com/postcss/postcss/blob/master/docs/plugins.md)


[gulp-postcss](https://www.npmjs.com/package/gulp-postcss)
---
{% note info %}
PostCSS gulp plugin to pipe CSS through several plugins, but parse CSS only once.  
PostCSS 将许多插件集成在一个 CSS 管道中，但只解析一次 CSS
{% endnote %}

比如说我们想要集成 autoprefixer 和 cssnano 这两个插件，那就要这么写：
``` javascript
var postcss = require('gulp-postcss');
var gulp = require('gulp');
var autoprefixer = require('autoprefixer');
var cssnano = require('cssnano');
 
gulp.task('css', function () {
  var plugins = [
    autoprefixer({browsers: ['last 1 version']}),
    cssnano()
  ];
  return gulp.src('./src/*.css')
    .pipe(postcss(plugins))
    .pipe(gulp.dest('./dest'));
});
```
不过需要它的插件列表里有支持这个插件才行，具体支持情况请看 [<span style="color: red;">这里</span>](https://github.com/postcss/postcss/blob/master/docs/plugins.md)，当然，不要忘了它始终还是一款 CSS 插件，千万不要让它干别的事情，它也干不了呀，哈哈~




