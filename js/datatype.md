# JavaScript 数据类型

> JavaScript数据类型分为两种，基本数据类型，引用数据类型

## 基本数据类型
js基本数据类型共有六种
- string
- number
- boolean
- null
- undefined
- symbol

基本数据类型保存在栈内存中  
基本类型的比较是直接的值比较


## 引用数据类型

js 引用类型有很多种统称为Object类型，大致分为：
- Object
- Array
- Function
- Date
- RegExp

引用数据类型保存在堆内存中，栈内存中保存的是堆内存的地址  
引用数据类型的比较是比较的内存地址
```
const obj1 = {};
const obj2 = {};
const obj3 = obj1;

console.log(obj1 == obj2); // false
console.log(obj1 === obj2); // false
console.log(obj1 == obj3); // true
console.log(obj1 === obj3); // true
```

## 类型检测

针对如此多的类型检测也有不同的办法

先来看看typeof的效果
```
const a = 'a'
const b = 1;
const c = false;
const d = null;
const e = undefined;
const f = Symbol('a');
const g = {a:1};
const h = function (){};
const i = new Date();
const j = new RegExp();
const k = [];

console.log(typeof a); // string
console.log(typeof b); // number
console.log(typeof c); // boolean
console.log(typeof d); // object
console.log(typeof e); // undefined
console.log(typeof f); // symbol
console.log(typeof g); // object
console.log(typeof h); // function 
console.log(typeof i); // object
console.log(typeof j); // object
console.log(typeof k); // object
```

从上面可以看出大部分的类型是可以明确的通过typeof来检测出来的，但是在引用类型中 object Date RegExp Array 是无法通过typeof 来进行区分的，另外基本类型的null通过typeof检测居然也是object～～


typeof也并不能完全的覆盖所有类型，接下来看看instanceof的表现如何
```
const d = null;
const g = {a:1};
const h = function (){};
const i = new Date();
const j = new RegExp();
const k = [];

console.log(d instanceof Object); // false
console.log(d === null); // true
console.log(g instanceof Object); // true
console.log(h instanceof Function);// true
console.log(i instanceof Date); // true
console.log(j instanceof RegExp); // true
console.log(k instanceof Array); // true
console.log(Array.isArray(k)); // true
```

还有像 Object.prototype.toString.call()也可以比较全面的判断类型
```
console.log(Object.prototype.toString.call('a')); // [object String]
console.log(Object.prototype.toString.call(1)); // [object Number]
console.log(Object.prototype.toString.call(null)); // [object Null]
console.log(Object.prototype.toString.call(true)); // [object Boolean]
console.log(Object.prototype.toString.call(undefined)); // [object Undefined]
console.log(Object.prototype.toString.call(Symbol('a'))); // [object Symbol]
console.log(Object.prototype.toString.call({})); // [object Object]
console.log(Object.prototype.toString.call([])); // [object Array]
console.log(Object.prototype.toString.call(/a/g)); // [object RegExp]
console.log(Object.prototype.toString.call(new Date())); // [object Date]
console.log(Object.prototype.toString.call(function(){})); // [object Function]
```

也可以通过构造器（constructor）来判断
```
[].constructor === Array // true
[].constructor === Object // false
window.constructor === Window // true
```
构造器的判断其实不太稳定，因为它是可以被修改的，不过面试回答一下可能会让面试官有不一样的感觉～