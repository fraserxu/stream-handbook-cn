# Stream手册 - 简介

[译者注:]译自[@substack](https://github.com/substack) - [stream-handbook](https://github.com/substack/stream-handbook)

本文主要介绍如何使用[streams](http://nodejs.org/docs/latest/api/stream.html)来编写[node.js](http://nodejs.org/)应用.

[![cc-by-3.0](http://i.creativecommons.org/l/by/3.0/80x15.png)](http://creativecommons.org/licenses/by/3.0/)

```
"在处理程序之间的连接问题时,我们希望能够像浇水一样,在需要的时候控制流量大小,从而更好的管理数据.这就是我们所谓的IO."
```

[Doug McIlroy. October 11, 1964](http://cm.bell-labs.com/who/dmr/mdmpipe.html)

![doug mcilroy](http://substack.net/images/mcilroy.png)

***

在[unix最早的时期](http://www.youtube.com/watch?v=tc4ROCJYbm0),Streams就已经出现,用于在构建大型系统时连接各个[功能独立的模块](http://www.faqs.org/docs/artu/ch01s06.html).在unix当中,可以通过使用`|`来使用stream.在node中,[stream module](http://nodejs.org/docs/latest/api/stream.html)已经被作为内置模块被核心的各个库所使用,也可以被[用户空间](http://en.wikipedia.org/wiki/Kernel_space#KERNEL)所使用.与unix类似,node的stream模块主要以`.pipe()`作为操作符,node中还同时提供了解压机制用于控制慢速流程.

Stream是unix编程中的一个重要组成部分,但是同样也有很多其他的抽象的思想值得考虑.

![brain kernighan](http://substack.net/images/kernighan.png)

***

# 为什么要使用streams

node中的I/O都是异步的,因此与硬盘和网络的之间的通讯都需要传入回调函数.在需要从硬盘读取一个文件是你可能会这样写:

```js
var http = require('http');
var fs = require('fs');

var server = http.createServer(function (req, res) {
  fs.readFile(__dirname + '/data.txt', function (err, data) {
    res.end(data);
  });
});
server.listen(8000);
```

这段代码虽然可以正常工作.但是在每次发送请求并返回结果到客户端时,都会把整个`data.txt`文件读取到内存当中.如果`data.txt`非常大,你的程序可能会开始吃掉很大的内存,对于那些连接速度很慢的用户来说更为明显.

这样做的用户体验也会很差,因为用户需要一直等到整个文件被加载到内存当中后才能开始接收数据.

幸运的是`(req, res)`两个参数都是streams,这也意味着我们可以用另外一种更好的方式来写这段代码:

``` js
var http = require('http');
var fs = require('fs');

var server = http.createServer(function (req, res) {
  var stream = fs.createReadStream(__dirname + '/data.txt');
  stream.pipe(res);
});
server.listen(8000);
```

这里`.pipe()`负责监听`fs.createReadStream()`方法的`'data'`和`'end'`事件.这样代码看起来会更加整洁,而且`data.txt`文件的数据会在从硬盘读取到之后立即发送给客户端.

使用`.pipe()`还有一些其他好处.例如在处理backpressure时,针对慢速连接用户,可以控制buffer从而避免将过度的buffer写入用户的内存.

如果你想对数据进行压缩,也有对应的streaming模块来实现.

``` js
var http = require('http');
var fs = require('fs');
var oppressor = require('oppressor');

var server = http.createServer(function (req, res) {
  var stream = fs.createReadStream(__dirname + '/data.txt');
  stream.pipe(oppressor(req)).pipe(res);
});
server.listen(8000);
```

现在读取的文件已经可以被支持gzip的浏览器所使用![oppressor](https://github.com/substack/oppressor)可以自动帮我们解决转码的问题.

在你学习并且理解了stream的api之后,你就可以像使用乐高积木一样随心所欲的使用各个stream模块,从而可以避免强迫自己去记那些不是使用stream模块的各种奇怪的api.

stream的存在,让使用node编程更加的简单,优雅,并且有利于代码的组件化.


