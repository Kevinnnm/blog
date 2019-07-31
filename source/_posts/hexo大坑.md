---
title: hexo大坑
date: 2019-07-22 17:20:52
tags: hexo
cover: https://i.loli.net/2019/07/22/5d358d83b3a0315599.png
---

## 坑一、Template render error 模板渲染错误
写了一下午博文，高高兴兴地hexo g却发现报错了！   
`INFO  Start processing
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Nunjucks Error:  [Line 2, Column 6] unexpected token: }}
 at formatNunjucksError (/home/kevin/blog/node_modules/hexo/lib/extend/tag.js:102:13)
    at Promise.fromCallback.catch.err (/home/kevin/blog/node_modules/hexo/lib/extend/tag.js:124:34)
    at tryCatcher (/home/kevin/blog/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/home/kevin/blog/node_modules/bluebird/js/release/promise.js:517:31)
    at Promise._settlePromise (/home/kevin/blog/node_modules/bluebird/js/release/promise.js:574:18)
    at Promise._settlePromise0 (/home/kevin/blog/node_modules/bluebird/js/release/promise.js:619:10)
    at Promise._settlePromises (/home/kevin/blog/node_modules/bluebird/js/release/promise.js:695:18)
    at _drainQueueStep (/home/kevin/blog/node_modules/bluebird/js/release/async.js:138:12)
    at _drainQueue (/home/kevin/blog/node_modules/bluebird/js/release/async.js:131:9)
    at Async._drainQueues (/home/kevin/blog/node_modules/bluebird/js/release/async.js:147:5)
    at Immediate.Async.drainQueues [as _onImmediate] (/home/kevin/blog/node_modules/bluebird/js/release/async.js:17:14)
    at processImmediate (internal/timers.js:443:21)`
    
原因是nunjucks模板标签导致MD文件解析报错的问题，我试验了一下，在md文档中出现`双大括号`,`左大括号+#`,`左大括号+%`等都会报错(原谅我这么打，因为我打出来符号的话这篇博客就发不出来了)，下面是一位大神的[解决办法](http://xcoding.tech/2018/08/08/hexo/%E5%A6%82%E4%BD%95%E4%BB%8E%E6%A0%B9%E6%9C%AC%E8%A7%A3%E5%86%B3hexo%E4%B8%8D%E5%85%BC%E5%AE%B9%7B%7B%7D%7D%E6%A0%87%E7%AD%BE%E9%97%AE%E9%A2%98/)，他提供了几种解决办法，讲的很详细，可以参考一下

我觉得有点麻烦就没采用(说多了就是菜～),下面是我的办法:
1. 既然出现上面的内容就会报错，那就尽量避免出现呗...(说的都是废话)
2. 使用`\lbrace`代替`\{`,使用`\rbrace`代替`\}`
推荐大神的解决办法，一劳永逸，以后就不用管了


## 坑二、Markdown的空行
在写表格的时候要把表格体前后各空一行，不然你写的表格是这样式儿的，崩溃啊！
![](https://i.loli.net/2019/07/22/5d358cfee55f989745.png)

有可能是hexo解析的问题，我在使用markdown-preview的时候看的是正常的
