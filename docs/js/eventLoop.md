# 事件循环

这个面试基本上一定会问到的，网上有很多文章我就不再详细解释了

直接上面试吧


Q: 说说你对js事件循环的理解

A:

首先是宏任务和微任务

像整体代码script，setTimeout，setInterval、setImmediate这些都属于宏任务
Promise.then属于微任务，还有node中的process.nextTick(callback)也属于微任务


js的事件循环需要分为浏览器环境和node环境

浏览器环境中执行一个宏任务就去执行微任务，当没有可执行的微任务才会去执行下一个宏任务

node环境下当这一轮循环中的所有宏任务执行完毕才会去执行微任务


宏任务和微任务的执行机制是一样的，自上而下，碰到宏任务就执行宏任务，碰到微任务加到本轮循环的末尾

--- 
接下来可能会让你看一段代码的输出顺序，不要着急，按照规则一轮一轮的循环往下走，如果面试官不说在那种环境则默认是浏览器环境，千万不要问，因为问了你大概率会得到一个这样的回答

那你把两种环境的输出都说一下吧～～


哈哈哈哈

