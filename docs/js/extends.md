# js继承

> 面向对象三大基石，封装、继承、多态

继承，允许创建分等级层次的类。

继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为


## 应用到代码中
作为程序员或多或少的都会接触到继承，比如``Animal``作为父类，他的子类``Dog``、``Cat``，继承自``Animal``，那么他们天然机会拥有父类的属性和函数。

那么前端中的继承也是一样，举个例子：  
前端页面中很多时候需要做一些操作埋点，用来统计数据，那么可以这样实现

```html
<div id="div1">11</div>
<p id="p1">22</p>
```
html是这样的，我们的需求是：  
点击div1的时候打印``div1 is clicked``  
点击p1的时候打印``p1 is clicked``  
这两者的点击都会触发不同种类的上报，可以这样来设计


```js
class BuriedPoint {
    constructor(el){
        this.el = el;
    }
    buried (){
        console.log(`@@@ buriedPoint el = ${this.el}`);
        // 在这里处理埋点上报的逻辑
    }
}
```

这是一个专门用来处理埋点逻辑的类，它具有一个el属性用来储存标签，和一个用来上报的函数

```js
class ElBase {
    constructor (el){
        this.el = document.querySelector(el);
        this.buriedPoint = new BuriedPoint(this.el);
    }
    bindClick(fn){
        this.el.addEventListener('click',function (event){
            fn(event);
            this.buriedPoint.buried();
        }.bind(this))
    }
}
```
这是各个元素的基类，可以把它当作是一个调用入口

```js
class ElDiv extends ElBase {
    constructor(el){
        super(el);
    }
    bindEvent(type,fn){
        if(type == 'click'){
            this.bindClick(fn);
        }else if(type == 'mouseover'){
            // ...
        }
    }
}

class ElP extends ElBase {
    constructor(el){
        super(el);
    }
    bindEvent(type,fn){
        if(type == 'click'){
            this.bindClick(fn)
        }else if(type == 'doubleClick'){
            // ...
        }
    }
}
```
这两个分别是处理div和p的事件逻辑的类，这两个类都继承自``ElBase``类，在它们实例化时``ElBase``也会实例化，调用构造函数把``BuriedPoint``也实例化，这样一个点击事件的链条就已经形成了，这就是所谓的分层继承


```js
const div1 = new ElDiv('#div1');
div1.bindEvent('click',function (){
    console.log('div1 is clicked')
})
const p1 = new ElP('#p1');
p1.bindEvent('click',function (){
    console.log('p1 is clicked')
})
```

这样实例化两个类，就已经实现了需求   
这是一种编程思维，你可能会觉得有些繁琐，没有必要，但是想象一下，如果这是一个大型项目，有几百个div和p需要处理上千个上报逻辑，那么这样的设计模式还是有必要的。




