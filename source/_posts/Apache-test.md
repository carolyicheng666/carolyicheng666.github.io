---
title: Apache 压力测试
date: 2017-12-07 15:20:15
tags: [Nodejs, Apache]
categories: '编程'
---



{% cq %}
“如果下雨了，你愿意留下吗？”  
“即使不下雨，我也在这里啊。”

**言叶之庭**
{% endcq %}

<!-- more -->

简介
---

在项目上线之前，我们通常需要对其进行各种各样的测试，测试的分类有很多种：按开发阶段分为单元测试、集成测试、系统测试和验收测试；按是否运行分为静态测试和动态测试；按是否关系内部结构分为黑盒测试、白盒测试和灰盒测试；等等等等，不过这些对本文都不重要，今天我主要是来介绍一下，如何用 Apache 的 ab 工具对项目进行压力测试，顺带一提，压力测试属于黑盒测试中性能测试的其中一种。



下载和安装
---

首先要使用 Apache ，必须要到 [<span style="color: red;">官网</span>](http://www.apache.org/dyn/closer.cgi) 去下载安装才行，可是无奈官网链接也是各种打不开，好不容易进去了又找不到下载的地方，这里给个 [<span style="color: red;">传送门</span>](http://httpd.apache.org/download.cgi) ，顺着指引一步步点击，下载完成后解压到想放的文件夹下，不用安装，直接进入文件夹下的 bin 目录下，执行命令
``` bash
$ httpd -k install
```
把 Apache 安装成 Windows 后台服务，中间如果遇到了什么问题，请自行百度



参数释义
---

假设上面的步骤都正常，那么就可以使用 ab 命令进行压力测试了  
使用方式是在 Apache 的 bin 目录下执行：
``` bash
$ ./ab -n1000 -c10 url
```
url换成想测试的项目的url即可，Linux用户不用加 `./` ，直接使用 ab 即可，这里有两个参数：-n1000表示总共有1000次请求，默认值是1；-c10表示请求的并发数是10，默认值同样是1  
下面列出比较常用的参数及释义：  
<table>
  <tr>
    <th width="20%" style="text-align:center;">参数</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">-h</td>
    <td>显示帮助信息</td>
  </tr>
  <tr>
    <td style="text-align:center;">-V</td>
    <td>查看版本号</td>
  </tr>
  <tr>
    <td style="text-align:center;">-c</td>
    <td>并发用户数</td>
  </tr>
  <tr>
    <td style="text-align:center;">-n</td>
    <td>总请求数</td>
  </tr>
  <tr>
    <td style="text-align:center;">-C</td>
    <td>请求cookie，用法 "cookie_name=value", 可重复</td>
  </tr>
  <tr>
    <td style="text-align:center;">-d</td>
    <td>不显示"percentage served within XX [ms] table"</td>
  </tr>
  <tr>
    <td style="text-align:center;">-H</td>
    <td>增加额外的请求头</td>
  </tr>
  <tr>
    <td style="text-align:center;">-i</td>
    <td>用HEAD请求代替GET</td>
  </tr>
  <tr>
    <td style="text-align:center;">-k</td>
    <td>启用Http keepalive功能</td>
  </tr>
  <tr>
    <td style="text-align:center;">-v</td>
    <td>显示详细信息  "-v4"  4或更大值会显示头信息，"-v3" 3或更大值可以显示响应代码(404，200等)，2或更大值可以显示警告和其他信息 </td>
  </tr>
  <tr>
    <td style="text-align:center;">-w</td>
    <td>以html格式输出结果</td>
  </tr>
  <tr>
    <td style="text-align:center;">-t</td>
    <td>超时时间，所有请求的最大执行时间，默认没有限制</td>
  </tr>
</table>

更多的参数及释义请查阅官网的 API ，[<span style="color: red;">传送门</span>](http://httpd.apache.org/docs/2.4/programs/ab.html)，注意自己的 Apache 版本



执行压力测试
---

现在我们用 Nodejs 简单跑一个服务
``` javascript
var http = require('http');

http.createServer(function(req, res){
	res.writeHead(200, {'content-Type': 'text-Plain'});
	res.write('Hello Nodejs');
	res.end();
})
.listen(2017);
```

然后执行命令对其进行压力测试，注意 url 后要多加一个`/`
``` bash
$ ./ab -n1000 -c10 http://localhost:2017
```

我们来看一下输出的内容，每个字段的意思我写在注释中方便对照查看
``` bash
This is ApacheBench, Version 2.3 <$Revision: 1796539 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests

/* 服务器名称 */
Server Software:
/* url主机名称 */
Server Hostname:        localhost
/* 端口号 */
Server Port:            2017

/* url中绝对路径 */
Document Path:          /
/* http相应数据的正文长度 */
Document Length:        12 bytes

/* 并发用户数，即我们用-c设置的参数 */
Concurrency Level:      10
/* 所有请求处理完花费的时间 */
Time taken for tests:   0.177 seconds
/* 总请求数，即我们用-n设置的参数 */
Complete requests:      1000
/* 失败的请求数 */
Failed requests:        0
/* http响应头和正文的总和 */
Total transferred:      113000 bytes
/* 相应正文的总和 */
HTML transferred:       12000 bytes
/* 每秒处理请求数，即吞吐量 */
Requests per second:    5649.53 [#/sec] (mean)
/* 用户平均等待时间 */
Time per request:       1.770 [ms] (mean)
/* 服务器平均处理时间 */
Time per request:       0.177 [ms] (mean, across all concurrent requests)
/* 每秒从服务器获取的数据长度 */
Transfer rate:          623.43 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.5      0      16
Processing:     0    2   3.6      0      16
Waiting:        0    2   3.6      0      16
Total:          0    2   3.6      0      16

/* 每个请求处理时间的分布 */
Percentage of the requests served within a certain time (ms)
  50%      0
  66%      1
  75%      2
  80%      2
  90%      5
  95%     16
  98%     16
  99%     16
 100%     16 (longest request)
Finished 1000 requests

```

上面的分布说明：其中 80% 的用户响应时间小于 2 毫秒， 90% 的用户响应时间小于 5 毫秒， 最大的响应时间小于 16 毫秒  



特别提醒
---

通过上面的学习我们就可以愉快的玩耍了，记得千万不要随便拿别人的网站来测试哦~

