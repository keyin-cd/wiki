# symbol
symbol 是一种基本数据类型 （primitive data type）。Symbol()函数会返回symbol类型的值，该类型具有静态属性和静态方法。它的静态属性会暴露几个内建的成员对象；它的静态方法会暴露全局的symbol注册，且类似于内建对象类，但作为构造函数来说它并不完整，因为它不支持语法："new Symbol()"。

每个从Symbol()返回的symbol值都是唯一的。一个symbol值能作为对象属性的标识符；这是该数据类型仅有的目的。更进一步的解析见—— glossary entry for Symbol。

## 面试常见问题

- symbol属于什么变量类型  
属于基本类型，不属于引用类型

- 怎么判断变量属于symbol类型  
```
const s1 = Symbol('s1');
console.log(typeof s1); // symbol
```
- symbol类型的相互判断
```
const s1 = Symbol('s1');
const s2 = Symbol('s1');
console.log(s1 == s2); // false
console.log(s1 === s2); // false
console.log(s1 == s1); // true
console.log(s1 === s1); // true
```
- 如何创建类的私有成员变量，私有函数
```
const sex = Symbol('sex');
class Person {
    constructor(sexValue){
        this[sex] = sexValue;
    }
    getSex(){
        return this[sex];
    }
}
// 这样Person的实例就拥有了一个不可以从外部修改的成员变量sex
```
- 对象中查找 Symbol 属性
```
Object.getOwnPropertySymbols()
```
- 全局共享的 Symbol  
上面使用Symbol() 函数的语法，不会在你的整个代码库中创建一个可用的全局的symbol类型。 要创建跨文件可用的symbol，甚至跨域（每个都有它自己的全局作用域） , 使用 Symbol.for() 方法和  Symbol.keyFor() 方法从全局的symbol注册表设置和取得symbol。

## symbol的坑  
其实坑还是不少的，这里列举两个比较常见的  
1.JSON.stringify()转化object到string时会忽略object的symbol属性
```
JSON.stringify({Symbol('a): '1'}) => '{}';
```
2.symbol是一种类型，不要把它理解成一个随机字符串，它的机制并不是根据传入的字符串来生成一个随机字符串，所以也不要对symbol做一些类型转换之类的操作，没有意义

