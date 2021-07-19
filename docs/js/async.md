# JavaScript异步

你可能知道，Javascript语言的执行环境是"单线程"（single thread）。

所谓"单线程"，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，以此类推。

这种模式的好处是实现起来比较简单，执行环境相对单纯；坏处是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。

为了解决这个问题，Javascript语言将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。

"同步模式"就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；"异步模式"则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。

"异步模式"非常重要。在浏览器端，耗时很长的操作都应该异步执行，避免浏览器失去响应，最好的例子就是Ajax操作。在服务器端，"异步模式"甚至是唯一的模式，因为执行环境是单线程的，如果允许同步执行所有http请求，服务器性能会急剧下降，很快就会失去响应。

"异步模式"编程的4种方法，理解它们可以让你写出结构更合理、性能更出色、维护更方便的Javascript程序。

## 回调函数
这是异步编程最基本的方法。

假定有两个函数fn1和fn2，后者等待前者的执行结果。
```
fn1();
fn2();
```
如果fn1是一个很耗时的任务，可以考虑改写fn1，把fn2写成fn1的回调函数。

```
function fn1 (callback){
    setTimeout(function (){
        callback();
    },1000)
}
```
那么fn2的调用就变成了这样
```
fn1(fn2)
```
采用这种方式，我们把同步操作变成了异步操作，fn1不会堵塞程序运行，相当于先执行程序的主要逻辑，将耗时的操作推迟执行。

回调函数的优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度耦合（Coupling），流程会很混乱，而且每个任务只能指定一个回调函数。

## 事件监听

另一种思路是采用事件驱动模式。任务的执行不取决于代码的顺序，而取决于某个事件是否发生。

还是以fn1和fn2为例。首先，为fn1绑定一个事件（这里采用的jQuery的写法）。

```
fn1.on('done', fn2);
```
上面这行代码的意思是，当fn1发生done事件，就执行fn2。然后，对fn1进行改写：

```
function fn1 (){
    setTimeout(function (){
        // do something
        fn1.trigger('done');
    },1000)
}
```
fn1.trigger('done')表示，执行完成后，立即触发done事件，从而开始执行fn2。

这种方法的优点是比较容易理解，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以"去耦合"（Decoupling），有利于实现模块化。缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。

## 发布/订阅

之前的文章中有实现过发布订阅模式，这里不再赘述，假设我们有个发布订阅中心类。
```
class PublishSubscribeCenter {
    constructor (){
        // ...
    }
    registerPublish(){

    }
    registerSubscribe(){

    }
    handle(publishType){

    }
    // ...
}
```
这里根据代码就可以讲fn1注册为发布者，fn2注册为订阅者
```
publishSubscribeCenter.registerPublish('fn1');
publishSubscribeCenter.registerSubscribe('fn1',fn2);
```
当fn1函数中的耗时操作执行完毕
```
function fn1 (){
    setTimeout(function (){
        // do something
        publishSubscribeCenter.handle('fn1');
    },1000)
}
```
此时fn2就会在fn1执行完毕后被触发，这种方式和第二种方法事件驱动模式有些类似，都是靠消息在各个模块传递，进而调用对应函数。

## Promise
Promises对象是CommonJS工作组提出的一种规范，目的是为异步编程提供统一接口。

简单说，它的思想是，每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数。比如，f1的回调函数f2,可以写成：
```
f1().then(f2);
```
fn1需要进行如下改写
```
function fn1 (){
    return new Promise(function (r,j){
        setTimeout(function (){
            r('@');
        },1000)
    })
}
```
这样写的优点在于，回调函数变成了链式写法，程序的流程可以看得很清楚，而且有一整套的配套方法，可以实现许多强大的功能。
```
f1().then(f2).then(f3);
```
再比如，指定发生错误时的回调函数：
```
f1().then(f2).fail(f3);
```
如果一个任务已经完成，再添加回调函数，该回调函数会立即执行。所以，你不用担心是否错过了某个事件或信号,这种方法的缺点就是编写和理解，都相对比较难。

es7中新增了,async/await特性，也可以利用上。
代码改造成这样fn1不变,调用的时候修改一下就可以
```
(async function (){
    await fn1();
    fn2();
})()
```
这样的代码看起来就好理解多了

generator函数也可以做到和async/await相同的效果，不过generator还有很多作用，不光是用来处理promise

