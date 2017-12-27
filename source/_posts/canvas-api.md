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
- **moveTo()**代表绘制的起点，包含两个参数x和y，分别代表起始点坐标的x值和y值  
- **lineTo()**表示添加一个新的点，然后创建从该点到画布中最后指定点的线条，**特别提醒**：这个方法并不会创建线条  
- **stroke()**实际绘制出通过 moveTo() 和 lineTo() 方法定义的路径  

这里可以认为moveTo()和lineTo()都是对绘制状态的设置，最后都要通过stroke()进行实际的绘制，既然是先设置状态再绘制图形，我们自然而然就会想到可以设置线条的宽度、线条的颜色等等，是的，这些在canvas中都是可以设置的先来介绍最常用的这两个，下面可能会遇到其它的，到时候我们再一一细说  
- 设置线条宽度使用的是**lineWidth**，设置时让它等于一个具体数值，不用带单位，它是按像素计算的  
- 设置线条颜色使用的是**strokeStyle**，设置时让它等于一个具体的颜色值，这里可以使用渐变色，是一个高级用法，我们放到后面再说  

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
- 设置填充的颜色**fillStyle**，让其等于一个具体的颜色值即可，也可以使用渐变色
- 设置完成之后调用**fill()**即可填充，类似于stroke()绘制图形一样  
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

- **lineCap**  
设置线条末端线帽的样式，有三个值，默认是butt，表示向线条的每个末端添加平直的边缘；另外还可以设置成round，表示向线条的每个末端添加圆形线帽；设置成square，表示向线条的每个末端添加正方形线帽
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

- **lineJoin**  
当两条线交汇时设置所创建边角的类型，同样有三个值，默认是miter，表示创建尖角；还可以设置成bevel，表示创建斜角；设置成round，表示创建圆角  
- **miterLimit**  
设置最大斜接长度，斜接长度指的是在两条线交汇处内角和外角之间的距离，只有当 lineJoin 设置为 `miter` 时，miterLimit 才有效，默认值是10。这部分内容不做过多解释，有兴趣的朋友可以查阅 [W3C文档](http://www.w3school.com.cn/tags/canvas_miterlimit.asp) 



图形变换
---

- **translate(x, y)**  
平移变换，x和y分别代表水平和垂直的偏移量  
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

- **rotate(angle)**  
旋转变换，angle表示旋转的角度，单位是弧度，角度转弧度的公式是：angle = degrees \* Math.PI / 180  
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

- **scale(x, y)**  
缩放变换，x和y分别表示水平方向和垂直方向的缩放倍数  
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

- **transform(a, b, c, d, e, f)** 添加一个变换矩阵，可以对图形进行平移、倾斜和缩放  
$$
 \left[
 \begin{matrix}
   a & c & e \\\\  
   b & d & f \\\\  
   0 & 0 & 1
  \end{matrix}
  \right] \tag{1}
$$
*a:水平缩放  b:水平倾斜  c:垂直倾斜  d:垂直缩放  e:水平平移  f:垂直平移*  
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

- **setTransform(a, b, c, d, e, f)**  
把当前的变换矩阵重置为单位矩阵，然后以相同的参数运行 transform() ，不过我觉得每次图形的绘制都在save()和restore()中，重置矩阵的操作估计在这之中需要绘制多个图形的时候才可能用到吧  



fillStyle 属性设置
---

之前我们在介绍 fillStyle 的时候说过可以使用渐变色，现在我们来详细说明渐变色的用法（感觉就是将 Photoshop 中“渐变”代码化了）  
- **线性渐变**  
设置分为两步：第一步，创建渐变的变量，设置其为何种渐变
``` javascript
linearGrad = context.createLinearGradient(xstart, ystart, xend, yend);
```
  createLinearGradient()中包含四个变量，代表线段两端的点的坐标(xstart,ystart)和(xend,yend)，表示在这个线段上做线性渐变  
  第二步，在这条线段上设置关键帧，并为其指定颜色
``` javascript
linearGrad.addColorStop(0.0, "white");
linearGrad.addColorStop(1.0, "black");
```
  addColorStop()表示添加关键帧，起始点为0，终止点为1，也就是上面设置的那两个点的坐标，这里用浮点数是为了让大家看得更方便，比如说我们要在线段中点位置添加一个关键帧，并设置灰色，那就可以这么写： 
``` javascript
linearGrad.addColorStop(0.5, "gray");
```
  上面的描述可能比较生硬，大家可以打开Photoshop，点击渐变工具，快捷键是G，不管学过没学过吧，都可以在里面实际操作一下，再回过头来看看代码，理解起来就比较容易了  
  来看个实例
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");

  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  var linearGrad=context.createLinearGradient(0, 0, 800, 800);
  linearGrad.addColorStop(0.0, "white");
  linearGrad.addColorStop(0.25, "yellow");
  linearGrad.addColorStop(0.5, "green");
  linearGrad.addColorStop(0.75, "blue");
  linearGrad.addColorStop(1.0, "black");
  context.fillStyle=linearGrad;
  context.fillRect(0, 0, 800, 800);
}
```

- **径向渐变**  
有了线性渐变的基础，径向渐变就很好理解了  
第一步，依然是创建渐变的变量，设置其为何种渐变
``` javascript
radialGrad = context.createRadialGradient(x0, y0, r0, x1, y1, r1);
```
  createRadialGradient()中包含六个变量，分别是小圆圆心坐标(x0,y0)和小圆半径r0，大圆圆心坐标(x1,y1)和大圆半径r1，表示在这两个圆之间做径向渐变      
  第二步和线性渐变一致，同样是设置关键帧
``` javascript
radialGrad.addColorStop(0.0, "white");
radialGrad.addColorStop(1.0, "black");
```
  来看个例子
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");

  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  var radialGrad=context.createRadialGradient(400, 400, 0, 400, 400, 500);
  radialGrad.addColorStop(0.0, "white");
  radialGrad.addColorStop(0.25, "yellow");
  radialGrad.addColorStop(0.5, "green");
  radialGrad.addColorStop(0.75, "blue");
  radialGrad.addColorStop(1.0, "black");
  context.fillStyle=radialGrad;
  context.fillRect(0, 0, 800, 800);
}
```

除了可以填充渐变色以外，我们还可以填充图像
- **填充图像**  
设置方式是先创建一个图像源对象，可以是如下几种：image, video, canvas, CanvasRenderingContext2D, ImageBitmap, ImageData, Blob，然后再用类似渐变的方式绑定到canvas中，我们这里只讨论image和canvas这两个，首先来看第一个：
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");

  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  var backgroundImage=new Image();
  backgroundImage.src="images/repeat.gif";
  backgroundImage.onload=function(){
    var pattern=context.createPattern(backgroundImage, "repeat");
    context.fillStyle=pattern;
    context.fillRect(0, 0, 800, 800);
  }
}
```
  createPattern()包含两个参数，第一个是image对象，第二个是指定如何重复图像，和 CSS 中的设置一样  
  我们再来看第二个canvas的例子
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  var backCanvas=createBackgroundCanvas();
  var pattern=context.createPattern(backCanvas, "repeat");
  context.fillStyle=pattern;
  context.fillRect(0, 0, 800, 800);
}

function createBackgroundCanvas(){
  var backCanvas=document.createElement("canvas");
  backCanvas.width=100;
  backCanvas.height=100;
  var backCanvasContext=backCanvas.getContext("2d");
  drawStar(backCanvasContext, 50, 50, 50, 0);
  return backCanvas;
}

function drawStar(cxt, x, y, R, rot){
  cxt.save();

  cxt.translate(x, y);
  cxt.rotate(rot/180*Math.PI);
  cxt.scale(R, R);

  starPath(cxt);

  cxt.fillStyle="#fb3";
  cxt.fill();

  cxt.restore();
}

function starPath(cxt){
  cxt.beginPath();
  for(var i=0;i<5;i++){
    cxt.lineTo(Math.cos((18+72*i)/180*Math.PI),-Math.sin((18+72*i)/180*Math.PI));
    cxt.lineTo(Math.cos((54+72*i)/180*Math.PI)*0.5,-Math.sin((54+72*i)/180*Math.PI)*0.5);
  }
  cxt.closePath();
}
```
  上面的例子稍微有点复杂，首先创建了绘制五角星的方法drawStar()，然后用createBackgroundCanvas()其绘制到画布上，最后用createPattern()方法设置如何绘制  



绘制曲线
---

- **arc(x, y, r, sAngle, eAngle, counterclockwise)**  
用于绘制圆和部分圆，原点中心坐标为(x,y)，半径为r，起始角是sAngle，结束角是eAngle，单位是弧度，counterclockwise是布尔值，默认是false，表示顺时针绘制，反之为true时，表示逆时针绘制，具体请参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arc)和[W3C](http://www.w3school.com.cn/tags/canvas_arc.asp)，这里提供一个便于理解的实例，例子中1和2，3和4，5和6对照着看
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=1024;
  canvas.height=768;

  var context=canvas.getContext("2d");

  context.lineWidth=5;
  context.strokeStyle="#005588";
  for (var i=0; i<10; i++){
    context.beginPath();
    context.arc(50+i*100, 60, 40, 0, 2*Math.PI*(i+1)/10);
    context.stroke();
  }

  for (var i=0; i<10; i++){
    context.beginPath();
    context.arc(50+i*100, 180, 40, 0, 2*Math.PI*(i+1)/10);
    context.closePath();
    context.stroke();
  }

  for (var i=0; i<10; i++){
    context.beginPath();
    context.arc(50+i*100, 300, 40, 0, 2*Math.PI*(i+1)/10, true);
    context.stroke();
  }

  for (var i=0; i<10; i++){
    context.beginPath();
    context.arc(50+i*100, 420, 40, 0, 2*Math.PI*(i+1)/10, true);
    context.closePath();
    context.stroke();
  }

  context.fillStyle="#005588";
  for (var i=0; i<10; i++){
    context.beginPath();
    context.arc(50+i*100, 540, 40, 0, 2*Math.PI*(i+1)/10);
    context.fill();
  }

  context.fillStyle="#005588";
  for (var i=0; i<10; i++){
    context.beginPath();
    context.arc(50+i*100, 660, 40, 0, 2*Math.PI*(i+1)/10, true);
    context.closePath();
    context.fill();
  }
}
```

- **arcTo(x1, y1, x2, y2, r)**  
创建介于两个切线之间的弧或曲线。这里面有几个概念：开始点(x0,y0)，控制点(x1,y1)，结束点(x2,y2)。一般是先用 moveTo() 方法设置开始点，然后使用 arcTo() 方法创建同时相切于开始点到控制点和控制点到结束点的两条切线，且半径为r的圆弧，这里我们可以想象的到，如果半径比某条切线长度要大时，切点就不会在切线的所在的线段上，而是在切线的延长线上，这种情况也是可以的。举个例子，大家可以自行更改数值看看效果：
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  arcToTest(context, 150, 100, 650, 100, 650, 600, 500);
}

function arcToTest(cxt, x0, y0, x1, y1, x2, y2, R){
  cxt.beginPath();
  cxt.moveTo(x0, y0);
  cxt.arcTo(x1, y1, x2, y2, R);

  cxt.lineWidth=6;
  cxt.strokeStyle="red";
  cxt.stroke();

  cxt.beginPath();
  cxt.moveTo(x0, y0);
  cxt.lineTo(x1, y1);
  cxt.lineTo(x2, y2);

  cxt.lineWidth=2;
  cxt.strokeStyle="gray";
  cxt.stroke();
}
```

- **quadraticCurveTo(x1, y1, x2, y2)**  
绘制二次贝塞尔曲线，学过图形学的朋友看到这个一定不会陌生，用法与arcTo()非常类似，同样先需要使用 moveTo() 方法设置开始点，然后再使用quadraticCurveTo()设置控制点(x1, y1)和结束点(x2, y2)，与arcTo()不同的是没有半径r，并且开始点就是曲线的开始点，结束点就是曲线的结束点，这样这条曲线就是唯一确定了，我们来使用它绘制一个月亮
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  fillMoon(context, 400, 400, 300, 0);
}

function fillMoon(cxt, x, y, R, rot, fillColor){
  cxt.save();
  cxt.translate(x, y);
  cxt.rotate(rot*Math.PI/180);
  cxt.scale(R, R);
  pathMoon(cxt);
  cxt.fillStyle=fillColor || "#fd5";
  cxt.fill();
  cxt.restore();
}

function pathMoon(cxt){
  cxt.beginPath();
  cxt.arc(0, 0, 1, 0.5*Math.PI, 1.5*Math.PI, true);
  cxt.moveTo(0, -1);
  cxt.quadraticCurveTo(1.2, 0, 0, 1);
  cxt.closePath();
}
```

- **bezierCurveTo(x1, y1, x2, y2, x3, y3)**  
绘制三次贝塞尔曲线，类似二次贝塞尔曲线，只是中间多了一个控制点，这样我们就可以绘制出更复杂的曲线了，综合前面我们绘制的月亮星星的例子，我们来绘制一个星空的场景
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=1200;
  canvas.height=800;

  var context=canvas.getContext("2d");

  //sky
  var radialGrad=context.createRadialGradient(
    canvas.width/2, canvas.height, 0, 
    canvas.width/2, canvas.height, canvas.height);
  radialGrad.addColorStop(0.0, "#035");
  radialGrad.addColorStop(1.0, "black");
  context.fillStyle=radialGrad;

  context.fillRect(0, 0, canvas.width, canvas.height);
  for (var i=0; i<200; i++) {
    var r=Math.random()*5 + 5;
    var x=Math.random()*canvas.width;
    var y=Math.random()*canvas.height*0.6;
    var a=Math.random()*360;
    drawStar(context, x, y, r, r/2.0, a);   
  }
  fillMoon(context, 900, 200, 100, 30); 
  drawland(context);
}

//land
function drawland(cxt){
  cxt.save();
  cxt.beginPath();
  cxt.moveTo(0, 600);
  cxt.bezierCurveTo(540, 400, 660, 800, 1200, 600);
  cxt.lineTo(1200, 800);
  cxt.lineTo(0, 800);
  cxt.closePath();

  var landStyle=cxt.createLinearGradient(0, 800, 0, 0);
  landStyle.addColorStop(0.0, "#030");
  landStyle.addColorStop(1.0, "#580");
  cxt.fillStyle=landStyle;
  cxt.fill();
  cxt.restore();
}

//star
function drawStar(cxt, x, y, R, r, rot){
  cxt.beginPath();
  for(var i=0;i<5;i++){
    cxt.lineTo(Math.cos((18+72*i-rot)/180*Math.PI)*R+x,-Math.sin((18+72*i-rot)/180*Math.PI)*R+y);
    cxt.lineTo(Math.cos((54+72*i-rot)/180*Math.PI)*r+x,-Math.sin((54+72*i-rot)/180*Math.PI)*r+y);
  }
  cxt.closePath();
  cxt.fillStyle="#fb3";
  cxt.strokeStyle="#fd5";
  cxt.lineWidth=3;
  cxt.lineJoin="round";
  cxt.fill();
  cxt.stroke();
}

// moon
function fillMoon(cxt, x, y, R, rot, fillColor){
  cxt.save();
  cxt.translate(x, y);
  cxt.rotate(rot*Math.PI/180);
  cxt.scale(R, R);
  pathMoon(cxt);
  cxt.fillStyle=fillColor || "#fd5";
  cxt.fill();
  cxt.restore();
}

function pathMoon(cxt){
  cxt.beginPath();
  cxt.arc(0, 0, 1, 0.5*Math.PI, 1.5*Math.PI, true);
  cxt.moveTo(0, -1);
  cxt.quadraticCurveTo(1.2, 0, 0, 1);
  cxt.closePath();
}
```



文字渲染
---

- **font**  
设置文字的属性，与 CSS 设置方式一致。具体可设置的值请看 [W3C文档](http://www.w3school.com.cn/tags/canvas_font.asp)，比如这样：
``` javascript
context.font="italic small-caps bold 12px arial";
```
  那么如何填入文字呢？请看下面两个方法

- **fillText(text, x, y, maxWidth)**  
表示在画布上绘制填色的文本，默认是黑色，text是文本内容，x和y分别是开始绘制文本的（相对于画布）x坐标位置和y坐标位置，maxWidth可以设置文本最大的宽度，这个参数是可选的

- **strokeText(text, x, y, maxWidth)**  
表示在画布上绘制文本（没有填色），可以认为是绘制边框，默认同样是黑色，参数及含义与上面的 fillText() 一致，下面来看个综合实例
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");

  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  context.font="40px bold Arial";

  context.fillStyle="#058";
  context.fillText("欢迎大家学习《canvas绘图接口详解》！", 40, 100);

  context.lineWidth=1;
  context.strokeStyle="#058";
  context.strokeText("欢迎大家学习《canvas绘图接口详解》！", 40, 200);

  context.fillText("欢迎大家学习《canvas绘图接口详解》！", 40, 300, 200);
  context.strokeText("欢迎大家学习《canvas绘图接口详解》！", 40, 400, 400);

  var linearGrad=context.createLinearGradient(0, 0, 800, 0);
  linearGrad.addColorStop(0.0, "red");
  linearGrad.addColorStop(0.25, "orange");
  linearGrad.addColorStop(0.5, "yellow");
  linearGrad.addColorStop(0.75, "green");
  linearGrad.addColorStop(1.0, "purple");
  context.fillStyle=linearGrad;
  context.fillText("欢迎大家学习《canvas绘图接口详解》！", 40, 500);

  var backgroundImage=new Image();
  backgroundImage.src="images/repeat.gif";
  backgroundImage.onload=function(){
    var pattern=context.createPattern(backgroundImage, "repeat");
    context.fillStyle=pattern;
    context.font="100px bold Arial";
    context.fillText("Canvas!", 40, 650);
    context.strokeText("Canvas!", 40, 650);
  }
}
```

- **textAlign**  
设置文本的对齐方式，有五个取值：默认是start，表示文本在指定的位置开始；end，表示文本在指定的位置结束；center，表示居中对齐；left，表示左对齐；right，表示右对齐。来看个综合实例：
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");

  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  context.fillStyle="#058";
  context.font="40px bold san-serif";

  context.textAlign="left";
  context.fillText("textAlign = left", 400, 100);

  context.textAlign="center";
  context.fillText("textAlign = center", 400, 200);

  context.textAlign="right";
  context.fillText("textAlign = right", 400, 300);

  context.textAlign="start";
  context.fillText("textAlign = start", 400, 400);

  context.textAlign="end";
  context.fillText("textAlign = end", 400, 500);

  context.strokeStyle="#888";
  context.moveTo(400, 0);
  context.lineTo(400, 800);
  context.stroke();
}
```

- **textBaseline**  
设置文本的基线，有六个取值：top，基线在顶端；middle，基线在正中；bottom，基线在低端；默认值alphabetic，普通字母的基线；hanging，印度语的基线；ideographic，方块字如中文日文等的基线。综合实例：
``` javascript
window.onload = function() {
  var canvas = document.getElementById("canvas");

  canvas.width = 800;
  canvas.height = 800;

  var context = canvas.getContext("2d");

  context.fillStyle = "#058";
  context.font = "40px bold san-serif";

  context.textBaseline = "top";
  context.fillText("textBaseline = top", 40, 100);
  drawBaseline(context, 100);

  context.textBaseline = "middle";
  context.fillText("textBaseline = middle", 40, 200);
  drawBaseline(context, 200);

  context.textBaseline = "bottom";
  context.fillText("textBaseline = bottom", 40, 300);
  drawBaseline(context, 300);

  context.textBaseline = "alphabetic";
  context.fillText("中文日本語कितने बज रहे हैंalphabetic", 40, 500);
  drawBaseline(context, 500);

  context.textBaseline = "middle";
  context.fillText("中文日本語कितने बज रहे हैंhanging", 40, 600);
  drawBaseline(context, 600);

  context.textBaseline = "bottom";
  context.fillText("中文日本語कितने बज रहे हैंideographic", 40, 700);
  drawBaseline(context, 700);

  function drawBaseline(cxt, h) {
    var width = cxt.canvas.width;

    cxt.save();
    cxt.strokeStyle = "#888";
    cxt.lineWidth = 2;
    cxt.moveTo(0, h);
    cxt.lineTo(width, h);
    cxt.stroke();
    cxt.restore();
  }
}
```

- **measureText(text)**  
这个方法能返回一个对象，截止目前只能用它来获取文本的宽度，使用方式是 context.measureText(text).width，希望将来能够获取更多关于这个文本的信息  



高级属性
---

- **阴影**  
shadowColor：设置阴影的颜色  
shadowOffsetX：设置图形与阴影的水平距离  
shadowOffsetY：设置图形与阴影的垂直距离  
shadowBlur：设置阴影的模糊级数  
同样，这些属性对于使用过 Photoshop 中模糊滤镜的朋友都非常简单易懂，我们举个例子，给之前绘制的那个五角星加个阴影
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  context.shadowColor="#058";
  context.shadowOffsetX=10;
  context.shadowOffsetY=10;
  context.shadowBlur=5;

  drawStar(context, 150, 300, 400, 400, 30);
}

//五角星
function drawStar(cxt, R, r, x, y, rot){
  cxt.beginPath();
  for(var i=0;i<5;i++){
    cxt.lineTo(Math.cos((18+72*i-rot)/180*Math.PI)*R+x,-Math.sin((18+72*i-rot)/180*Math.PI)*R+y);
    cxt.lineTo(Math.cos((54+72*i-rot)/180*Math.PI)*r+x,-Math.sin((54+72*i-rot)/180*Math.PI)*r+y);
  }
  cxt.closePath();
  cxt.stroke();
  cxt.fillStyle="#fd5";
  cxt.fill();
}
```

- **合成**  
globalAlpha：设置当前绘图的透明度，与 CSS 一致，0是完全透明，1是完全不透明。大家可以更改下面例子的数值看下效果
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=1200;
  canvas.height=800;

  var context=canvas.getContext("2d");

  context.globalAlpha=0.7;
  for (var i=0; i<100; i++) {
    var r = Math.floor(Math.random()*255);
    var g = Math.floor(Math.random()*255);
    var b = Math.floor(Math.random()*255);

    context.fillStyle="rgb("+ r +","+ g +","+ b +")";

    context.beginPath();
    context.arc(Math.random()*canvas.width, Math.random()*canvas.height, Math.random()*100, 0, 2*Math.PI);
    context.fill();
  }
}
```
  globalCompositeOperation：设置如何将新的图像绘制到旧的图像中去，总共有11个取值，大家可以查看 [W3C文档](http://www.w3school.com.cn/tags/canvas_globalcompositeoperation.asp)，我这里提供一个交互式的例子，把这11个取值都放进去了，大家可以点击看看，对比他们之间的不同
``` html
// css
#buttons {
  width: 1200px;
  margin: 10px auto;
  clear: both;
}
#buttons a {
  font-size: 16px;
  display: block;
  float: left;
  margin-right: 14px;
}

// html
<canvas id="canvas" style="border: 1px solid #aaa; display: block; margin: 50px auto;"></canvas>
<div id="buttons">
  <a href="#">source-over</a>
  <a href="#">source-atop</a>
  <a href="#">source-in</a>
  <a href="#">source-out</a>
  <a href="#">destination-over</a>
  <a href="#">destination-atop</a>
  <a href="#">destination-in</a>
  <a href="#">destination-out</a>
  <a href="#">lighter</a>
  <a href="#">copy</a>
  <a href="#">xor</a>
</div>

//js
window.onload=function(){
  draw("source-over");

  var buttons=document.getElementById("buttons").getElementsByTagName("a");
  for (var i=0; i<buttons.length; i++) {
    buttons[i].onclick=function() {
      draw(this.text);
      return false;
    }
  }
}
function draw(compositeStyle) {
  var canvas=document.getElementById("canvas");
  canvas.width=1200;
  canvas.height=800;

  var context=canvas.getContext("2d");
  context.clearRect(0, 0, canvas.width, canvas.height);

  context.font="bold 40px Arial";
  context.textAlign="center";
  context.textBaseline="middle";
  context.fillStyle="#058";
  context.fillText("globalCompositeOperation=" + compositeStyle, canvas.width/2, 60);

  context.fillStyle="blue";
  context.fillRect(300, 150, 500, 500);

  context.globalCompositeOperation=compositeStyle;
  context.fillStyle="red";
  context.beginPath();
  context.moveTo(700, 250);
  context.lineTo(1000, 750);
  context.lineTo(400, 750);
  context.closePath();
  context.fill();
}
```

- **clip()**  
从原始画布中剪切任意形状和尺寸。一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切的区域内（不能访问画布上的其他区域）。您也可以在使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 restore() 方法）。我们来做一个类似管中窥豹的效果：
``` javascript
window.onload=function(){
  var canvas=document.getElementById("canvas");
  canvas.width=800;
  canvas.height=800;

  var context=canvas.getContext("2d");

  context.beginPath();
  context.fillStyle="black";
  context.fillRect(0, 0, canvas.width, canvas.height);

  context.beginPath();
  context.arc(400, 400, 150, 0, 2*Math.PI);
  context.fillStyle="#fff";
  context.fill();
  context.clip();

  context.font="bold 150px Arial";
  context.textAlign="center";
  context.textBaseline="middle";
  context.fillStyle="#058";
  context.fillText("CANVAS", canvas.width/2, canvas.height/2);
}
```

- **非零环绕原则**  
我们可以利用非零环绕原则制作一些图形中部的镂空效果，这里虽然可以使用覆盖白色图形的方式实现，但是这样实现不了内部的阴影效果。另外，非零环绕原则在拓扑学应用的非常广，不懂的小伙伴们可以自行百度，系统的学习一下。我们现在来制作一个带有阴影的圆环效果：
``` javascript
window.onload = function(){
  var canvas = document.getElementById("canvas");
  canvas.width = 800;
  canvas.height = 600;
  var context = canvas.getContext("2d");
  context.fillStyle = "#FFF";
  context.fillRect(0, 0, 800, 600);

  context.shadowColor = "#545454";
  context.shadowOffsetX = 5;
  context.shadowOffsetY = 5;
  context.shadowBlur = 2;

  context.arc(400, 300, 150, 0, Math.PI * 2 ,false);
  context.arc(400, 300, 230, 0, Math.PI * 2 ,true);
  context.fillStyle = "#00AAAA";
  context.fill();
}
```

- **isPointInPath(x, y)**  
如果指定的点(x,y)位于当前的路径中，则返回true；否则返回false。我们可以利用这个特性制作一些交互的效果，下面的例子当用户点击画布中的圆形小球时，该小球会变成红色的：
``` javascript
var balls = [];
var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");

window.onload=function(){
  canvas.width=800;
  canvas.height=800;

  for (var i=0; i<10; i++) {
    balls.push({x: Math.random()*canvas.width, y: Math.random()*canvas.height, r: Math.random()*50+30});
  }

  draw();
  canvas.addEventListener("mouseup", detect);
}

function draw() {
  for (var i=0; i<balls.length; i++) {
    context.beginPath();
    context.arc(balls[i].x, balls[i].y, balls[i].r, 0, 2*Math.PI);
    context.fillStyle="#058";
    context.fill();
  }
}

function detect(e) {
  var x = e.clientX - canvas.getBoundingClientRect().left;
  var y = e.clientY - canvas.getBoundingClientRect().top;

  for (var i=0; i<balls.length; i++) {
    context.beginPath();
    context.arc(balls[i].x, balls[i].y, balls[i].r, 0, 2*Math.PI);
    if (context.isPointInPath(x, y)) {
      context.fillStyle="#f00";
      context.fill();
    }
  }
}
```

- **clearRect(x, y, width, height)**  
用于清空一个矩形的内部元素，参数与rect()一致。这个方法之前我们在 globalCompositeOperation 这一小节用到过，我们在每一次绘制之前都清空一下画布中的元素。


[<strong style="color: red;">代码仓库</strong>](https://github.com/carolyicheng666/canvas-demo)  
<strong style="color: red;">未完待续...</strong>

