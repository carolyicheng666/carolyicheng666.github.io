---
title: canvas-api
date: 2017-12-20 10:08:54
tags: [Canvas, JavaScript]
categories: '编程'
---



{% cq %}
就算有讨厌的事，悲伤的事  
但在这世上还有很多我想保护的东西

**魔法少女小圆**
{% endcq %}


<!-- more -->


简介
---

{% note danger %}
canvas 是 HTML5 新增的元素，可用于通过使用JavaScript中的脚本来绘制图形。例如，它可以用于绘制图形，制作照片，创建动画，甚至可以进行实时视频处理或渲染。
{% endnote %}



基本用法
---

``` html
<canvas id="canvas"></canvas>
```

``` javascript
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

// 在这之后ctx变量调用canvas中的api绘制函数即可绘制图形
```

canvas 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容  
canvas 起初是空白的。为了展示，首先脚本需要找到渲染上下文，然后在它的上面绘制。canvas 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。如上所示，传入 `2d` 表示绘制的是一个 `2d` 的平面图形，当然如果传入 `3d` 就可以绘制 `3d` 图形，奇妙的是canvas的 `2d` 和 `3d` 绘制完全是不同的世界，本教程只介绍 `2d` 的api 



使用moveTo()、lineTo()和stroke()绘制一条直线
---

在基本用法中，canvas 创建画布的同时也创建了坐标轴，默认规定原点在左上角，向右为x轴正方向，向下为y轴正方向  
- moveTo()代表绘制的起点，包含两个参数x和y，分别代表起始点坐标的x值和y值  
- lineTo()表示添加一个新的点，然后创建从该点到画布中最后指定点的线条，**特别提醒**：这个方法并不会创建线条  
- stroke()实际绘制出通过 moveTo() 和 lineTo() 方法定义的路径  

这里可以认为moveTo()和lineTo()都是对绘制状态的设置，最后都要通过stroke()进行实际的绘制，既然是先设置状态再绘制图形，我们自然而然就会想到可以设置线条的宽度、线条的颜色等等，是的，这些在canvas中都是可以设置的先来介绍最常用的这两个，下面可能会遇到其它的，到时候我们再一一细说  
- 设置线条宽度使用的是lineWidth，设置时让它等于一个具体数值，不用带单位，它是按像素计算的  
- 设置线条颜色使用的是strokeStyle，设置时让它等于一个具体的颜色值，这里可以使用渐变色，是一个高级用法，我们放到后面再说  

``` javascript
window.onload = function(){
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  context.moveTo(100, 100);
  context.lineTo(700, 700);
  context.lineWidth = 10;
  context.strokeStyle = "#058";
  context.stroke();
}
```



使用beginPath()重置当前路径
---

上一小节我们绘制了一条直线，有拓展意识的朋友可能会利用其绘制多条直线或折线，但是细心的你一定会发现在我们第二次设置lineWidth或StrokeStyle后，该效果会影响第一次的设置，为什么呢？这么说吧，canvas是基于状态的绘制，无论使用多少次stroke()，最后一次stroke()总会重新绘制之前的stroke()，这时候就需要使用beginPath()，stroke()向上查找直到遇到beginPath()结束，然后根据当前设置进行绘制，这样说比较抽象，举个例子吧，假如说我们想要绘制三条不同颜色的折线：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  context.lineWidth = 10;

  context.beginPath();
  context.moveTo(100, 200);
  context.lineTo(300, 400);
  context.lineTo(100, 600);
  context.strokeStyle = "red";
  context.stroke();

  context.beginPath();
  context.moveTo(300, 200);
  context.lineTo(500, 400);
  context.lineTo(300, 600);
  context.strokeStyle = "green";
  context.stroke();

  context.beginPath();
  context.moveTo(500, 200);
  context.lineTo(700, 400);
  context.lineTo(500, 600);
  context.strokeStyle = "blue";
  context.stroke();
}
```
这里有三个stroke()，分别绘制这三条折线，根据上面对beginPath()的描述，可以很清晰的看到这三条折线的设置方式，这里有两点要说明一下：  
第一，绘制第一条线的beginPath()是可以不需要写的，加上这一行是为了保持格式上的统一；  
第二，我们看到对于lineWidth的设置放在了最外面，这样是比较显式的将线条宽度都设置成10个像素的做法，因为根据第一点，我们也可以放在第一个beginPath()后且在第一个stroke()前，也是一样的效果，但是我们在第二个beginPath()和第二个stroke()之间重置lineWidth，例如说在其中添加一行 `context.lineWidth = 5;` ，根据前面对beginPath()的描述，那么效果就应该是第一条折线的线条宽度是10个像素，而后两条折线的线条宽度是5个像素，大家可以自己动手实践一下。



使用closePath()绘制封闭图形
---

有些朋友可能会用直线的例子绘制一个图形，并且如果结束点和起始点设置成同一个点，就能绘制成一个封闭图形，这里会有一个问题，如果把线条宽度lineWidth的值设大一些，这个问题就会显而易见：最后的这个点会有 lineWidth/2 * lineWidth/2 的小空隙！这个时候我们就需要使用closePath()将图形封闭起来，并且最后设置结束点为起始点的这一行代码可以不需要写，beginPath()和closePath()会将中间设置的代码封闭成为一个图形。  
说到了封闭图形，我们自然会想到为其填充颜色，这里我们就来介绍一下：
- 设置填充的颜色fillStyle，让其等于一个具体的颜色值即可，也可以使用渐变色
- 设置完成之后调用fill()即可填充，类似于stroke()绘制图形一样
这里有点小问题，先stroke()和先fill()的显示效果是不一样的，先stroke()会比先fill()少一半的线条宽度，大家可以自行编码体会一下  

下面依然是举个例子，我们绘制了一个箭头，并填充黄色
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  context.beginPath();
  context.moveTo(100, 350);
  context.lineTo(500, 350);
  context.lineTo(500, 200);
  context.lineTo(700, 400);
  context.lineTo(500, 600);
  context.lineTo(500, 450);
  context.lineTo(100, 450);
  context.closePath();

  context.lineWidth = 10;
  context.strokeStyle = "#058";
  context.fillStyle = "yellow";

  context.fill();
  context.stroke();
}
```



使用rect()绘制矩形
---

按照上面的方法，如果我们要绘制一个矩形，那么就需要绘制其中三条边即可，这里我们也可以使用rect()进行绘制，这个方法接收4个参数，分别是矩形左上角x坐标、矩形左上角y坐标、矩形的宽度和矩形的高度，单位按像素计算，我们只需传入具体数值即可，举个例子：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  r(context, 100, 100, 400, 400, 10, "#058", "red");
  r(context, 300, 300, 400, 400, 10, "#058", "rgba(0, 255, 0, 0.5)");
}

function r(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle) {
  ctx.beginPath();
  ctx.rect(x, y, width, height);
  ctx.closePath();

  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fill();
  ctx.stroke();
}
```
绘制矩形我们也可以使用fillRect()和strokeRect()一起绘制，参数与rect()一致，**注意**，这两个方法与rect()本质是不同的，rect()是设置，最终还是需要fill()和stroke()进行绘制；而fillRect()和strokeRect()是绘制，也就是说使用它们就不需要额外使用fill()和stroke()了。我们将上面的例子改进一下：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  r(context, 100, 100, 400, 400, 10, "#058", "red");
  r(context, 300, 300, 400, 400, 10, "#058", "rgba(0, 255, 0, 0.5)");
  r1(context, 400, 50, 350, 350, 10, "#058", "rgba(0, 0, 255, 0.3)")
}

function r(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle) {
  ctx.beginPath();
  ctx.rect(x, y, width, height);
  ctx.closePath();

  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fill();
  ctx.stroke();
}
function r1(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle) {
  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fillRect(x, y, width, height);
  ctx.strokeRect(x, y, width, height);
}
```



线条的其它样式
---

lineCap 设置线条末端线帽的样式，有三个值，默认是butt，表示向线条的每个末端添加平直的边缘；另外还可以设置成round，表示向线条的每个末端添加圆形线帽；设置成square，表示向线条的每个末端添加正方形线帽
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.height=800;
  canvas.width=800;

  var context=canvas.getContext("2d");
  
  context.lineWidth=50;
  context.strokeStyle="#058";

  context.beginPath();
  context.moveTo(100, 200);
  context.lineTo(700, 200);
  context.lineCap="butt";
  context.stroke();

  context.beginPath();
  context.moveTo(100, 400);
  context.lineTo(700, 400);
  context.lineCap="round";
  context.stroke();

  context.beginPath();
  context.moveTo(100, 600);
  context.lineTo(700, 600);
  context.lineCap="square";
  context.stroke();

  context.lineWidth=1;
  context.strokeStyle="#27a";
  context.moveTo(100, 100);
  context.lineTo(100, 700);
  context.moveTo(700, 100);
  context.lineTo(700, 700);
  context.stroke();

}
```

lineJoin 当两条线交汇时设置所创建边角的类型，同样有三个值，默认是miter，表示创建尖角；还可以设置成bevel，表示创建斜角；设置成round，表示创建圆角  
miterLimit 设置最大斜接长度，斜接长度指的是在两条线交汇处内角和外角之间的距离，只有当 lineJoin 设置为 `miter` 时，miterLimit 才有效，默认值是10。这部分内容不做过多解释，有兴趣的朋友可以查阅 [W3C文档](http://www.w3school.com.cn/tags/canvas_miterlimit.asp) 


<strong style="color: red;">未完待续...</strong>
