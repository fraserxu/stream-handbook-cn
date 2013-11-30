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


