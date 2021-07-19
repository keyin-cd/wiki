# 改变this的指向

这个真挺简单的，很好记

具体区别就不再说了  

说一些很常见的考点吧

```js
function log(){
    console.log(this.a);
}
const a = {a:'a'}
const b = {a:'b'}
log.bind(a).bind(b)();
```
输出：a  
bind输出的函数是不能再次改变this指向的

```js
const log = () => console.log(this.a);
log.call({a:1})
log.apply({a:2})
log.bind({a:3})();
```
输出：
undefined  
undefined  
undefined  

箭头函数的this指向是不能被改变的，因为它在声明的时候就已经被确定了


另外，好好理解下面对三个函数的实现，这个经常会遇到的

## call
```js
Function.prototype.myCall = function (context){
    // 获取指向的上下文
    context = context ? context : window;
    // 给上下文增加函数方法
    context.fn = this;
    let res;
    // 收集参数
    if(arguments.length > 0){
        let args = [];
        for(let i = 1; i < arguments.length;i++){
            args.push(arguments[i]);
        }
        res = context.fn(...args);
    }else{
        res = context.fn();
    }
    delete context.fn;
    return res;
}
```

## bind
```js
Function.prototype.myBind = function (){
    if(typeof this !== 'function') throw new TypeError('@');
    var fn_self = this,
        fn_args = Array.prototype.slice.call(arguments,1);
    var fn_empty = function (){}
    var fn = function (){
        return fn_self.apply(this instanceof fn ? this : arguments[0],fn_args.concat(Array.prototype.slice.call(arguments)))
    }
    if(this.prototype){
        fn_empty.prototype = this.prototype;
    }
    fn.prototype = new fn_empty();
    return fn;
}
```


## apply

```js
// apply 接收一个数组作为参数集
Function.prototype.myApply = function (context,args){

    context = context || window;
    context.fn = this;

    let str = ''
    if(args){
        str = 'context.fn(' + args +')';
    }else {
        str = 'context.fn()'
    }
    let res = eval(str);
    delete context.fn;
    return res;

}
```