---
title: Grid 布局
date: 2017-12-05 14:14:45
tags: [CSS]
categories: '编程'
---


{% cq %}
我们总是在注意错过太多，却不注意自己拥有多少

**未闻花名**
{% endcq %}

<!-- more -->


简介
---

CSS Grid 布局是 CSS 中最强大的布局系统。与 flexbox 的一维布局系统不同，CSS Grid 布局是一个二维布局系统，也就意味着它可以同时处理列和行。通过将 CSS 规则应用于 **父元素** (成为 Grid Container 网格容器)和其 **子元素**（成为 Grid Items 网格项），你就可以轻松使用 Grid(网格) 布局。



浏览器支持程度
---

截至今日，许多浏览器都提供了对 CSS Grid 的原生支持，而且无需加浏览器前缀：Chrome（包括 Android ），Firefox，Edge，Safari（包括iOS）和 Opera 。但是 IE 10和11需要额外写代码特殊支持。以下浏览器支持数据来自 [Caniuse](https://caniuse.com/#search=grid) ，点击可查看更多的细节。

PC端浏览器：

| Chrome | Opera | FireFox | IE | Edge | Safari |
|:------:|:-----:|:-------:|:--:|:----:|:------:|
|   57   |  44   |    52   | 10*|  16  |  10.1  |

移动端浏览器：

| iOS Safari | Opera Mobile | Opera Mini | Android | Android Chrome | Android Firefox |
|:----------:|:------------:|:----------:|:-------:|:--------------:|:---------------:|
|    10.3    |      NO      |     NO     |    62   |       62       |       57        |



重要概念
---

grid 的一些属性包括命名都与 flex 十分类似，相信熟悉 flex 布局的朋友看一遍 grid 的 API 就能轻易上手，下面的介绍可能不会太详细，毕竟有一些属性的设置还是要符合前端人员的习惯，拐弯抹角的设置往往会给维护带来成倍的麻烦。

首先来看看 html 模板：
``` html
<div class="wrapper">
  <div class="item1">1</div>
  <div class="item2">2</div>
  <div class="item3">3</div>
  <div class="item4">4</div>
  <div class="item5">5</div>
  <div class="item6">6</div>
</div>
```
怎么样，是不是很干净，强迫症患者总算是舒服了。html就写成这样，我们不需要再做更改了，剩下的只需要写 CSS 样式，包括能让 item2 在 item1 前面，突破了html顺序所带来的瓶颈，是不是很酷！结构与样式分离一直是所有前端人推崇的，Grid(网格) 布局真正做到了这点，对于 CSS 来说是一个巨大的进步。

接下来我们就来梳理一下基本概念：

- 网格容器(Grid Container)：如果把上面的 `wrapper` 类样式设置成 `display: grid`，那么它就是一个网格容器。
- 网格项(Grid Item)：网格容器下的所有子元素（注意，只有是网格容器父子关系的子元素，其它的都不算）。
- 网格线(Grid Line)：构成网格结构的分界线。
- 网格轨道(Grid Track)：两条相邻网格线之间的空间。
- 网格单元格(Grid Cell)：两个相邻的行和两个相邻的列网格线之间的空间。按照上面的代码来看就是每一个 item。
- 网格区域(Grid Area)： 可以由任意数量的 网格单元格(Grid Cell) 组成。

我们再来了解一下基本属性，这里只做简要概括，不常用的就不详细说了，也许以后再补充进去吧。  
<strong style="color: red;font-size: 24px;">网格容器(Grid Container)</strong> 属性：
- **display**  
``` css
.wrapper {
    display: grid | inline-grid | subgrid;
}
```
	这里有三个值，分别表示块级网格，内联网格，子网格（父级也是网格）

- **grid-template-columns / grid-template-rows**  
``` css
.wrapper {
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 100px 100px;
}
```
	这样设置就把上面6个item设置成了两行三列的、每项大小是100px\*100px的网格，这里的值也可以是百分比，或者fr（表示等分剩余可用空间），当然还有用网格线名称设置的高级操作（不建议使用）。上面的代码由于都是相同的值，则可以像下面这样写：
``` css
.wrapper {
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(2, 100px);
}
```
	显示效果是一样的。

- **grid-template-areas**  
``` css
.wrapper {
  grid-template-areas: 
    " . | none | ...";
}
```
	这里有三个值，分别表示一个空的网格单元、不定义网格区域、指定的网格区域名称。还是举个例子吧：
``` css
.wrapper {
  font-size: 20px;
  display: grid;
  grid-gap: 5px;    
  grid-template-columns: repeat(4, 100px);
  grid-template-rows: 50px 300px 50px;
  grid-template-areas:
    ". a a ."
    "b c d e"
    ". f f .";
}
.item1 {
  grid-area: a;
  background-color: #ACF4B6;
}
.item2 {
  grid-area: b;
  background-color: #FFE975;
}
.item3 {
  grid-area: c;
  background-color: #5DFFFA;
}
.item4 {
  grid-area: d;
  background-color: #E6B4FD;
}
.item5 {
  grid-area: e;
  background-color: #26DB0A;
}
.item6 {
  grid-area: f;
  background-color: #FCDCDD;
}
```
	复制到编辑器，然后浏览器打开看看效果就明白了，是不是很简单！

- **grid-template**  
用于定义 grid-template-rows ，grid-template-columns ，grid-template-areas 缩写 (shorthand) 属性。看了API，设置起来真的麻烦，我个人不建议这么写，还是分开写比较好，这里就不过多介绍了，有兴趣的自己去看 API 吧。

- **grid-column-gap / grid-row-gap**  
指定网格线(grid lines)的大小。你可以把它想象为设置列/行之间间距的宽度。
``` css
.wrapper {
  grid-column-gap: 10px;
  grid-row-gap: 15px;
}
```
	上述设置把列间距设置为 10px，行间距设置为 15px。

- **grid-gap**  
grid-column-gap 和 grid-row-gap 的缩写语法，建议用此写法。
``` css
.wrapper {
  grid-gap: 15px 10px;
}
```
	第一个值是行间距，第二个值是列间距。也可以只写一个值，表示行间距和列间距相等。

- **justify-items**  
沿着 行轴线(row axis) 对齐 网格项(grid items) 内的内容，该值用于容器内的所有网格项。可以理解为所有网格项的水平对齐方式：
``` css
.wrapper {
	justify-items: start | end | center | stretch;
}
```
	四个值分别表示左对齐，右对齐，中间对齐和填满整个网格宽度（默认）。

- **align-items**  
沿着 列轴线(column axis) 对齐 网格项(grid items) 内的内容，该值适用于容器内的所有网格项。可以理解为所有网格项的垂直对齐方式：
``` css
.wrapper {
	align-items: start | end | center | stretch;
}
```
	四个值分别表示顶部对齐，底部对齐，垂直居中对齐和填满整个网格高度（默认）。

- **justify-content**  
有时，网格合计大小可能小于其 网格容器(grid container) 大小。 如果所有 网格项(grid items) 都使用像 px 这样的非灵活单位设置大小，在这种情况下，可以设置网格容器内的网格的对齐方式。 此属性沿着 行轴线(row axis) 对齐网格。
``` css
.wrapper {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;    
}
```
	特别说明一下后面三个：  
	*space-around*：在每个网格项之间放置一个均匀的空间，左右两端放置一半的空间  
	*space-between*：在每个网格项之间放置一个均匀的空间，左右两端没有空间  
	*space-evenly*：在每个栅格项目之间放置一个均匀的空间，左右两端放置一个均匀的空间  

- **align-content**  
``` css
.wrapper {
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```
	与上面的情况类似，只是方向上的区别：
	*space-around*：在每个网格项之间放置一个均匀的空间，上下两端放置一半的空间  
	*space-between*：在每个网格项之间放置一个均匀的空间，上下两端没有空间  
	*space-evenly*：在每个栅格项目之间放置一个均匀的空间，上下两端放置一个均匀的空间  

- **grid-auto-columns / grid-auto-rows**  
指定任何自动生成的网格轨道(grid tracks)（又名隐式网格轨道）的大小。在你明确定位的行或列（通过  grid-template-rows / grid-template-columns），超出定义的网格范围时，隐式网格轨道被创建了。我看了一下例子，功能确实强大，但是超出网格范围依然有元素，实际情况几乎用不到，影响到外部元素的做法并不是很可取，在此我不展开讲这个，以后如果有实际情况遇到了，我再加进来。

- **grid-auto-flow**  
如果你有一些没有明确放置在网格上的网格项(grid items)，自动放置算法 会自动放置这些网格项。该属性控制自动布局算法如何工作。
``` css
.wrapper {
  grid-auto-flow: row | column | row dense | column dense
}
```
	*row*：告诉自动布局算法依次填充每行，根据需要添加新行
	*column*：告诉自动布局算法依次填入每列，根据需要添加新列
	*dense*：告诉自动布局算法在稍后出现较小的网格项时，尝试填充网格中较早的空缺，注意，这可能导致你的网格项出现乱序，不建议使用。  
	举个例子：
``` css
.wrapper {
  display: grid;
  grid-template-columns: repeat(6, 60px);
  grid-template-rows: repeat(2, 30px);
  grid-auto-flow: row;
}
.item1 {
  grid-column: 1;
  grid-row: 1 / 3;
}
.item6 {
  grid-column: 6;
  grid-row: 1 / 3;
}
```
	因为我们把 grid-auto-flow 设成了 row ，所以网格中未设置的item2、item3、item4和item5会在第一行。如果把 grid-auto-flow 设成了 column，则它们或按顺序垂直填充进前两列。

- **grid**  
这个属性是老大哥，可以设置所有以下属性的简写： grid-template-rows, grid-template-columns,  grid-template-areas, grid-auto-rows, grid-auto-columns, 和 grid-auto-flow 。它还将grid-column-gap 和 grid-column-gap设置为初始值，即使它们不可以通过grid属性显式地设置。恩，不建议使用。



<strong style="color: red;font-size: 24px;">网格项(Grid Items) </strong>属性
- **grid-column-start / grid-column-end / grid-row-start / grid-row-end**  
这里推荐简写形式，请看下面

- **grid-column / grid-row**  
分别为 grid-column-start + grid-column-end 和 grid-row-start + grid-row-end 的缩写形式。
``` css
.item {
  grid-column: <start-line> / <end-line> | <start-line> / span <value>;
  grid-row: <start-line> / <end-line> | <start-line> / span <value>;
}
```
	这里设置方式有两种，两种都可以，前者用的比较多一些。  
	**注意**，后面一个参数也可以省略：不加 `span` 表示占据一个网格轨道；加 `span` 表示跨越几个网格轨道，可以看例子中的 `wrapper2`

- **grid-area**  
为网格项提供一个名称，以便可以 被使用网格容器 grid-template-areas 属性创建的模板进行引用。 另外，这个属性可以用作grid-row-start + grid-column-start + grid-row-end +  grid-column-end 的缩写。
``` css
.item {
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}
```

- **justify-self**  
沿着 行轴线(row axis) 对齐 网格项 内的内容，此值适用于单个网格项内的内容。可以理解为所有网格项的水平对齐方式，设置方式可参考 justify-items 。

- **align-self**  
沿着 列轴线(row axis) 对齐 网格项 内的内容，此值适用于单个网格项内的内容。可以理解为所有网格项的垂直对齐方式，设置方式可参考 align-items 。




例子
---

如果上面的概念都很清楚了，下面的例子应该不难看懂，部分已经在上面出现过。
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Grid</title>
</head>
<style>
  .container {
    font-size: 20px;
    display: grid;
    grid-gap: 5px;    
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 50px 350px 50px;
    grid-template-areas:
      ". h h h h h h h h h h ."
      "m m c c c c c c c c c c"
      ". f f f f f f f f f f .";
  }
  .container > div {
    border-radius: 10px;
    text-align: center;
    color: #fff;
  }
  .header {
    grid-area: h;
    line-height: 50px;
  }
  .menu {
    grid-area: m;
    line-height: 350px;
  }
  .content {
    grid-area: c;
    line-height: 350px;
  }
  .footer {
    grid-area: f;
    line-height: 50px;
  }

  .wrapper {
    padding-top: 50px;
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    justify-content: center;
  }
  .wrapper .item1 {
    grid-column: 1 / 3;
  }
  .wrapper .item3 {
    grid-row: 2 / 4;
  }
  .wrapper .item4 {
    grid-column: 2 / 4;
  }

  .wrapper2 {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 40px 100px 40px;
  }
  .wrapper2 .item1, .wrapper2 .item4 {
    grid-column: span 12;
  }
  .wrapper2 .item2 {
    grid-column: span 4;
  }
  .wrapper2 .item3 {
    grid-column: span 8;
  }
</style>
<body>
  <div class="container">
    <div class="header" style="background-color: #ACF4B6;">HEADER</div>
    <div class="menu" style="background-color: #FFE975;">MENU</div>
    <div class="content" style="background-color: #5DFFFA;">CONTENT</div>
    <div class="footer" style="background-color: #E6B4FD;">FOOTER</div>
  </div>

  <div class="wrapper">
    <div class="item1" style="background-color: #ACF4B6;">1</div>
    <div class="item2" style="background-color: #FFE975;">2</div>
    <div class="item3" style="background-color: #5DFFFA;">3</div>
    <div class="item4" style="background-color: #E6B4FD;">4</div>
    <div class="item5" style="background-color: #26DB0A;">5</div>
    <div class="item6" style="background-color: #FCDCDD;">6</div>
  </div>

  <div class="wrapper2">
    <div class="item1" style="background-color: #ACF4B6;">1</div>
    <div class="item2" style="background-color: #FFE975;">2</div>
    <div class="item3" style="background-color: #5DFFFA;">3</div>
    <div class="item4" style="background-color: #E6B4FD;">4</div>
  </div>
</body>
</html>
```
