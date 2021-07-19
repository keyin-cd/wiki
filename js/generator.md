# Generator函数

``Generator``函数是``协程``在 ES6 的实现，最大特点就是可以交出函数的执行权（即暂停执行）。

## 协程


协程，又称微线程，纤程。英文名Coroutine。

协程的概念很早就提出来了，但直到最近几年才在某些语言（如Lua）中得到广泛应用。

子程序，或者称为函数，在所有语言中都是层级调用，比如A调用B，B在执行过程中又调用了C，C执行完毕返回，B执行完毕返回，最后是A执行完毕。

所以子程序调用是通过栈实现的，一个线程就是执行一个子程序。

子程序调用总是一个入口，一次返回，调用顺序是明确的。而协程的调用和子程序不同。

协程看上去也是子程序，但执行过程中，在子程序内部可中断，然后转而执行别的子程序，在适当的时候再返回来接着执行。

注意，在一个子程序中中断，去执行其他子程序，不是函数调用，有点类似CPU的中断


## Generator 函数

```js
function* gen(x){
  var y = yield x + 2;
  return y;
}
```
上面代码就是一个 Generator 函数。它不同于普通函数，是可以暂停执行的，所以函数名之前要加星号，以示区别。

整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方，都用 yield 语句注明。Generator 函数的执行方法如下。

```js
var g = gen(1);
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
```
上面代码中，调用 Generator 函数，会返回一个内部指针（即遍历器）。这是 Generator 函数不同于普通函数的另一个地方，即执行它不会返回结果，返回的是指针对象。调用指针 g 的 next 方法，会移动内部指针（即执行异步任务的第一段），指向第一个遇到的 yield 语句，上例是执行到 x + 2 为止。



换言之，next 方法的作用是分阶段执行 Generator 函数。每次调用 next 方法，会返回一个对象，表示当前阶段的信息（ value 属性和 done 属性）。value 属性是 yield 语句后面表达式的值，表示当前阶段的值；done 属性是一个布尔值，表示 Generator 函数是否执行完毕，即是否还有下一个阶段。

## 面试

上一道常见面试题吧

```js
function* a (v){
    console.log(1);
}
const gen = a(1);
```
输出：无

```js
function* a (v){
    console.log(1);
}
const gen = a(1);
console.log(gen.next());
```
输出：
1  
{value: undefined,done:true}  

```js
function* a (v){
    console.log(1);
    const b = yield v + 1;
}
const gen = a(1);
console.log(gen.next());
```
输出：
1  
{value: 2,done: false}

这里需要注意一下，yield执行的是``v+1``，在执行后直接中断了，赋值运算符还没有执行，所以``done``字段依然是``false``



```js
function* a (v){
    console.log(1);
    const b = yield v + 1;
    console.log(3);
    console.log(4);
}
const gen = a(1);
console.log(gen.next());
console.log(gen.next());
```
输出：
1  
{value:2,done:false}  
3  
4  
{value: undefined,done:true}

```js
function* a (v){
    console.log(1);
    const b = yield v + 1;
    console.log(3);
    console.log(4);
    yield console.log(5);
}
const gen = a(1);
console.log(gen.next());
console.log(gen.next());
```
输出：
1  
{value:2,done:false}  
3  
4 
5 
{value: undefined,done:true}
