---
title: canvas-api
date: 2017-12-20 10:08:54
tags: [Canvas, JavaScript]
categories: '编程'
mathjax: true
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



图形变换
---

- translate(x, y) 平移变换，x和y分别代表水平和垂直的偏移量  
我们拿之前矩形的例子做个试验：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  r(context, 100, 100, 400, 400, 10, "#058", "red", 100, 100);
  r(context, 100, 100, 400, 400, 10, "#058", "green", 0, 0);
}

function r(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle, translateX, translateY) {
  // ctx.save();
  ctx.translate(translateX, translateY);

  ctx.beginPath();
  ctx.rect(x, y, width, height);
  ctx.closePath();

  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fill();
  ctx.stroke();
  // ctx.restore();
}
```
  上面的代码绘制了两个矩形，一个是红色的，一个是绿色的，可是运行出来却只有一个绿色的矩形，准确的说是红色的矩形和绿色的矩形重合了，但是我们可以看到，红色的矩形和绿色的矩形设置的偏移量并不同，这是怎么回事？  
这是因为在canvas中，图形变换是叠加的，我们将红色的矩形向x轴和y轴各平移100个像素，而绿色的矩形没有做平移变换，这就使得绿色的矩形在红色矩形上方绘制，从而覆盖住了红色的矩形，大家可以改变绿色矩形的偏移量再看看效果。换一个角度说，translate()实际上是对原点的平移变换，改变了坐标系  
canvas针对这种情况提供了它自己的解决办法，大家可以将代码中 `ctx.save();` 和 `ctx.restore();` 前面的注释去掉，save()用于保存当前canvas设置的所有状态，我们在save()之后执行各种操作，再调用一次restore()，这个方法用于返回到之前save()保存的状态，这两个方法是成对出现的，以后我们在做一些复杂的设置和图形绘制时，大家一定会体会到这两个方法的巨大作用  

- rotate(angle) 旋转变换，angle表示旋转的角度，单位是弧度，角度转弧度的公式是：angle = degrees \* Math.PI / 180  
有了上面平移的例子，我们应该能很好的理解旋转变换了，还是拿矩形的例子试验一下：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  r(context, 200, 200, 400, 400, 10, "#058", "red", 10*Math.PI/180);
  r(context, 200, 200, 400, 400, 10, "#058", "green", -10*Math.PI/180);
}

function r(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle, angle) {
  ctx.save();
  ctx.rotate(angle);

  ctx.beginPath();
  ctx.rect(x, y, width, height);
  ctx.closePath();

  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fill();
  ctx.stroke();
  ctx.restore();
}
```

- scale(x, y) 缩放变换，x和y分别表示水平方向和垂直方向的缩放倍数  
依然是矩形的例子：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  r(context, 100, 100, 400, 400, 10, "#058", "red", 1.1, 1.1);
  r(context, 200, 200, 400, 400, 10, "#058", "green", 1.3, 1.3);
}

function r(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle, scaleX, scaleY) {
  ctx.save();
  ctx.scale(scaleX, scaleY);

  ctx.beginPath();
  ctx.rect(x, y, width, height);
  ctx.closePath();

  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fill();
  ctx.stroke();
  ctx.restore();
}
```
这里我们可以看到这个图形都进行了放大，细心的朋友会发现连 lineWidth 也一并进行了放大，很多人也许认为这是个canvas的bug或者是canvas对于图形缩放处理的不够好的地方，其实我倒觉得不是这样的，因为在translate()中我已经说过，translate()实际上是对原点的平移变换，scale()也可以看做是对该图形当前的坐标系进行了缩放变换而已  

- transform(a, b, c, d, e, f) 添加一个变换矩阵，可以对图形进行平移、倾斜和缩放  
$$
 \left[
 \begin{matrix}
   a & c & e \\\\  
   b & d & f \\\\  
   0 & 0 & 1
  \end{matrix}
  \right] \tag{3}
$$
a:水平缩放  
b:水平倾斜  
c:垂直倾斜  
d:垂直缩放  
e:水平平移  
f:垂直平移  
画布中每个对象都有一个当前的变换矩阵，默认是单位矩阵，也就是transform(1, 0, 0, 1, 0, 0)，意思是缩放为1，倾斜和平移都为0，即不变换  
大家根据下面的实例更改数值，就很好理解了，还可以加深记忆
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  r(context, 100, 100, 400, 400, 10, "#058", "red", 1.1, 0.2, 0.3, 1.2, 20, 30);
}

function r(ctx, x, y, width, height, lineWidth, strokeStyle, fillStyle, a, b, c, d, e, f) {
  ctx.save();
  ctx.transform(a, b, c, d, e, f);

  ctx.beginPath();
  ctx.rect(x, y, width, height);
  ctx.closePath();

  ctx.lineWidth = lineWidth;
  ctx.strokeStyle = strokeStyle;
  ctx.fillStyle = fillStyle;

  ctx.fill();
  ctx.stroke();
  ctx.restore();
}
```

- setTransform(a, b, c, d, e, f) 把当前的变换矩阵重置为单位矩阵，然后以相同的参数运行 transform() ，不过我觉得每次图形的绘制都在save()和restore()中，重置矩阵的操作估计在这之中需要绘制多个图形的时候才可能用到吧  





<strong style="color: red;">未完待续...</strong>
