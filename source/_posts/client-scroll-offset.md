---
title: width、clientWidth、scrollWidth 和 offsetWidth
date: 2017-11-23 14:37:37
tags: [CSS]
categories: '编程'
---

{% cq %}
或许前路永夜，即便如此我也要前进，因为星光即使微弱也会为我照亮前路

**四月是你的谎言**
{% endcq %}

<!-- more -->

话不多说，扯那些个概念没啥用，直接上例子，对着盒模型看公式，一看就懂：
``` html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
</head>
<style>
textarea {
  width: 200px;
  height: 200px;
  line-height: 300px;
  resize: none;
  margin: 30px;
  padding: 20px;
  border: 10px solid red;
  white-space: nowrap;
}
</style>

<body>
  <textarea id='t'>123123213211232132132123132123132111111</textarea>
</body>
<script>
  var obj = document.getElementById('t');
  console.log('width: ' + getComputedStyle(obj).width);
  console.log('clientWidth: ' + obj.clientWidth + 'px');
  console.log('scrollWidth: ' + obj.scrollWidth + 'px');
  console.log('offsetWidth: ' + obj.offsetWidth + 'px');
</script>
</html>
```

我们假定预设元素的宽度为 `width0`，在上面代码中就是 `200px`，那么上面的四个值就可以用如下公式表示：
{% note info %}
width = width0 - 滚动条宽度

clientWidth = width + padding

scrollWidth = clientWidth + 溢出部分宽度

offsetWidth = width0 + padding + border
{% endnote %}

换个通俗点的解释吧：
{% note info %}
width = content内容的宽度

clientWidth = 白色可见区域宽度

scrollWidth = 白色区域总宽度（可见 + 不可见）

offsetWidth = 白色可见区域宽度 + 滚动条宽度 + 红色框框宽度
{% endnote %}