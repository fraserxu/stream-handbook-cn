# 简介

[译者注:]本文翻译自[@substack](https://github.com/substack) - [stream-handbook](https://github.com/substack/stream-handbook)

本文主要介绍如何使用[streams](http://nodejs.org/docs/latest/api/stream.html)来编写[node.js](http://nodejs.org/)应用.

[![cc-by-3.0](http://i.creativecommons.org/l/by/3.0/80x15.png)](http://creativecommons.org/licenses/by/3.0/)

```
"在处理程序之间的连接问题时,我们希望能够像浇水一样,在需要的时候控制流量大小,从而更好的管理数据.这就是我们所谓的IO."
```

[Doug McIlroy. October 11, 1964](http://cm.bell-labs.com/who/dmr/mdmpipe.html)

![doug mcilroy](http://substack.net/images/mcilroy.png)

***


