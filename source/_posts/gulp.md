---
title: 一些常用的gulp插件整理
date: 2017-11-09 11:09:52
tags: [gulp]
categories: '编程'
---

{% cq %}
员工离职，只有两个真实原因：钱没给到位；心受委屈了

**马云**
{% endcq %}

<!-- more -->



gulp 和 grunt 对比
---

我觉得 [<span style="color: red;">这篇对比介绍</span>](http://slides.com/contra/gulp#/) 写的很好，大家可以看看。其中作者提到了为什么 `grunt` 不好的原因：
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


