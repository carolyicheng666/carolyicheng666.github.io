---
title: rollup 入门实例教程
date: 2018-03-13 11:33:59
tags: [rollup]
categories: '编程'
---


{% cq %}
向来缘浅 奈何情深  
既然琴瑟起 何以笙箫默

**顾漫《何以笙箫默》**
{% endcq %}

<!-- more -->


{% note danger %}
Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，例如 library 或应用程序。Rollup 对代码模块使用新的标准化格式，这些标准都包含在 JavaScript 的 ES6 版本中，而不是以前的特殊解决方案，如 CommonJS 和 AMD。ES6 模块可以使你自由、无缝地使用你最喜爱的 library 中那些最有用独立函数，而你的项目不必携带其他未使用的代码。ES6 模块最终还是要由浏览器原生实现，但当前 Rollup 可以使你提前体验。
{% endnote %}

>源码仓库：[https://github.com/carolyicheng666/rollup-study](https://github.com/carolyicheng666/rollup-study)，欢迎Star

下面先进行一些准备工作，先把源码拷贝一份到自己的本机上：
``` bash
$ git clone https://github.com/carolyicheng666/rollup-study.git
```

然后下载所依赖的库文件：
``` bash
$ npm i
```

我源码的 rollup 版本是 **0.56.5**，如果版本有更新，可能存在写法上的差异，建议到[rollup官网](https://rollupjs.org/guide/en)查询。



demo01： 输出包的格式
---

一般分为五种：
{% note success %}
**amd** – 异步模块定义，用于像RequireJS这样的模块加载器  
**cjs** – CommonJS，适用于 Node 和 Browserify/Webpack  
**es** – 将软件包保存为ES模块文件  
**iife** – 一个自动执行的功能，适合作为`<script>`标签。（如果要为应用程序创建一个捆绑包，您可能想要使用它，因为它会使文件大小变小。）  
**umd**
 – 通用模块定义，以amd，cjs 和 iife 为一体  
{% endnote %}

如果没有设置，会报下列错误：
``` bash
[!] Error: You must specify options.format, which can be one of 'amd', 'cjs', 'es', 'iife' or 'umd'
```

命令行执行是 `rollup -i 入口文件 -f 输出包格式 -o 输出文件`，如果是 **iife** 和 **umd** 格式，还需要加 `-n 输出的包名`。更多的配置请点击[这里](https://rollupjs.org/guide/en#core-functionality)

我们简单配置一下 `package.json`：
``` json
"scripts": {
  "build:amd": "rollup index.js -f amd -o ./dist/dist.amd.js",
  "build:cjs": "rollup index.js -f cjs -o ./dist/dist.cjs.js",
  "build:es": "rollup index.js -f es -o ./dist/dist.es.js",
  "build:iife": "rollup index.js -f iife -n result -o ./dist/dist.iife.js",
  "build:umd": "rollup index.js -f umd -n result -o ./dist/dist.umd.js",
  "build:all": "npm run build:amd && npm run build:cjs && npm run build:es && npm run build:iife && npm run build:umd"
}
```

执行命令即可：
``` bash
$ npm run build:all
```



demo02： 使用配置文件 rollup.config.js
---

rollup.config.js 完整的配置是
``` javascript
export default {
  // 核心选项
  input,     // 必须
  external,
  plugins,

  // 额外选项
  onwarn,

  // danger zone
  acorn,
  context,
  moduleContext,
  legacy

  output: {  // 必须 (如果要输出多个，可以是一个数组)
    // 核心选项
    file,    // 必须
    format,  // 必须
    name,
    globals,

    // 额外选项
    paths,
    banner,
    footer,
    intro,
    outro,
    sourcemap,
    sourcemapFile,
    interop,

    // 高危选项
    exports,
    amd,
    indent
    strict
  },
};
```

本例中只设置了必须的选项，其它一些常用的选项会在后面的例子中看到。

如果使用了配置文件，那么命令行运行的命令就是 `rollup -c`，我们在 `package.json` 中设置一下：
 ``` json
"scripts": {
  "build": "rollup -c"
}
```

这时候就可以运行命令
``` bash
$ npm run build
```



demo03： 实时监听
---

每一次改动都要运行一次命令实在是太麻烦了，所以，几乎每一种工具都会有实时监听的设置，rollup也不例外，在命令后加上 `-w` 即可
``` json
"scripts": {
  "dev": "rollup -c -w"
}
```

运行命令实现实时监听
``` bash
$ npm run dev
```



demo04： 集成 npm packages
---

需要用到两个插件：`rollup-plugin-node-resolve` 和 `rollup-plugin-commonjs`

`rollup-plugin-node-resolve` 插件可以告诉 Rollup 如何查找外部模块  
一些库导出成你可以正常导入的ES6模块 - **the-answer** 就是一个这样的模块。 但是目前，npm中的大多数包都是以CommonJS模块的形式出现的。 在它们更改之前，我们需要将CommonJS模块转换为 ES2015 供 Rollup 处理。`rollup-plugin-commonjs` 插件就是用来将 CommonJS 转换成 ES2015 模块的。

`rollup.config.js` 配置：
``` javascript
import commonjs from 'rollup-plugin-commonjs';
import resolve from 'rollup-plugin-node-resolve';

export default {
  input: 'index.js',
  output: {
    format: 'cjs',
    file: './dist/dist.js'
  },
  plugins: [ resolve(), commonjs() ]
}
```



demo05：Peer dependencies
---

假如我们引入了 `the-answer` 和 `lodash` 这两个模块， Rollup 默认会把他们打包在一起，那么这样打包出来的文件就会很大，因此我们希望 `lodash` 变成外部引用，这是可以做到的，设置 `external` 选项即可，这个选项接收一个数组，代表所有需要外部引入的模块，**此数组不能处理通配符**。

本例中可以这样设置：
``` javascript
external: ['lodash']
```



demo06： Babel
---

使用 Babel 和 Rollup 的最简单方法是使用 [rollup-plugin-babel](https://github.com/rollup/rollup-plugin-babel)

和其它工具一样，先要创建 `.babelrc` 文件：
``` json
{
  "presets": [
    ["env", {
      "modules": false
    }]
  ],
  "plugins": [
    "external-helpers"
  ]
}
``` 

这里我们需要 `babel-preset-env` 和 `babel-plugin-external-helpers` 这两个插件，为什么不用 ES2015 而用 env，可以看看[这篇文章](http://babeljs.io/env/)。而 `external-helpers` 插件，它允许 Rollup 在包的顶部只引用一次 “helpers”，而不是每个使用它们的模块中都引用一遍，大家可以对比一下引入这个插件和不引入打包后生成的文件的区别。

``` javascript
import resolve from 'rollup-plugin-node-resolve';
import babel from 'rollup-plugin-babel';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    resolve(),
    babel({
      exclude: 'node_modules/**' // 只编译我们的源代码
    })
  ]
};
```



demo07： CDN 引入
---

比如我们想使用 CDN 方式引入 `jquery`，那么在 `output` 的 `path` 选项添加即可 
``` javascript
output: {
  ...
  paths: {
    jquery: 'https://cdn.bootcss.com/jquery/3.2.1/jquery.js'
  },
  ...
}
```



demo08： 集成 Gulp
---

Rollup 返回 gulp 能明白的 promises，所以集成是很容易的。

`gulpfile.js`:
``` javascript
const gulp = require('gulp');
const rollup = require('rollup');
const resolve = require('rollup-plugin-node-resolve');
const babel = require('rollup-plugin-babel');

gulp.task('build', () => {
  return rollup.rollup({
    input: './index.js',
    plugins: [
      resolve(),
      babel({
        exclude: 'node_modules/**'
      })
    ]
  }).then(bundle => {
    return bundle.write({
      file: './dist/dist.js',
      format: 'umd',
      name: 'dist',
      sourcemap: true
    });
  });
});
```

也可以用 `async/await` 语法：
``` javascript
const gulp = require('gulp');
const rollup = require('rollup');
const resolve = require('rollup-plugin-node-resolve');
const babel = require('rollup-plugin-babel');

gulp.task('build', async function () {
  const bundle = await rollup.rollup({
    input: './index.js',
    plugins: [
      resolve(),
      babel({
        exclude: 'node_modules/**'
      })
    ]
  });

  await bundle.write({
    file: './dist/dist.js',
    format: 'umd',
    name: 'dist',
    sourcemap: true
  });
});
```



demo09： 添加 banner/footer
---

可以在输出文件的头部添加 banner 和尾部添加 footer 注释信息，例如：
``` javascript
import pkg from './package.json';
export default {
  ...
  banner: '/* dist version ' + pkg.version + ' */',
  footer: '/* follow me on Github! @' + pkg.author + ' */'
}
```



demo10： 使用插件
---

rollup的插件有很多，[这里](https://github.com/rollup/rollup/wiki/Plugins)是插件列表，稍微看看配置就能上手。  
比如我们使用[rollup-plugin-json](https://github.com/rollup/rollup-plugin-json)插件解析 json 文件：  
`main.js`:
``` javascript
import answer from 'the-answer';
import pkg from './package.json';

export default function () {
  console.log('the answer is ' + answer + ', the version is ' + pkg.version);
}
```

`rollup.config.js`:
``` javascript
import json from 'rollup-plugin-json';
export default {
  ...
  plugins: [ json() ]
}
```

再比如我们使用[rollup-plugin-uglify](https://github.com/TrySound/rollup-plugin-uglify)压缩输出文件：  
`rollup.config.js`:
``` javascript
import uglify from 'rollup-plugin-uglify';
export default {
  ...
  plugins: [ uglify() ]
}
```



demo11： 引入 sass 文件
---

rollup 不仅能打包 js，还能打包 css 或者 css 的预处理文件 sass，less，stylus等。就拿 sass 举例，我们需要引入 [rollup-plugin-scss](https://github.com/thgh/rollup-plugin-scss) 插件  
`rollup.config.js`:
``` javascript
import scss from 'rollup-plugin-scss';

export default {
  ...
  plugins: [
    scss({
      output: './dist/test.css'
    })
  ]
}
```

`index.js`:
``` javascript
import './main.scss';
...
```

想打包其它文件也可以去上面的插件列表中查找，相信大家会有更多的收获



demo12： 开发环境和生产环境
---

与其它打包工具一样，rollup 也能设置开发环境和生产环境（这一点其实和rollup没啥关系，还是nodejs的功劳）  
简单配置一下（只有压缩和不压缩的区别，哈哈）：  
`package.json`:
``` json
...
"scripts": {
  "dev": "rollup -c --environment NODE_ENV:development",
  "build": "rollup -c --environment NODE_ENV:production"
}
...
```

`rollup.config.js`:
``` javascript
import resolve from 'rollup-plugin-node-resolve';
import json from 'rollup-plugin-json';
import uglify from 'rollup-plugin-uglify';

const base = [resolve(), json()];
const dev = [];
const prod = [uglify()];

let isProd = process.env.NODE_ENV === 'production';
let plugins = [...base].concat(isProd ? prod : dev);

export default {
  input: 'index.js',
  output: {
    file: './dist/dist.js',
    format: 'cjs',
    name: 'dist',
    sourcemap: isProd
  },
  plugins: plugins
}
```

开发环境: `npm run dev`  
生产环境: `npm run build`



Rollup VS Webpack
---

用过这两个工具的朋友一定会有这种疑问，这两个工具非常类似，有感觉有一点点不同，那么到底什么时候用什么比较好呢？我找到了一篇文章，大家可以参考参考，[传送门](http://www.css88.com/archives/7703)



参考文献
---

- [rollup官方文档](https://rollupjs.org/guide/en)
- [rollup中文文档](https://rollupjs.cn/)
- [JS打包工具rollup—完全入门指南](https://blog.kainstar.cn/2017/08/12/JS%E6%89%93%E5%8C%85%E5%B7%A5%E5%85%B7rollup%E2%80%94%E5%AE%8C%E5%85%A8%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97/)
- [Webpack 和 Rollup ：一样但又不同](http://www.css88.com/archives/7703)