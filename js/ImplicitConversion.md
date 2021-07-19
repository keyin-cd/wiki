# JavaScript隐式转换 Implicit conversion

因为js是一种若类型语言，当两个类型不同的变量相互比较运算时，会有一套内置的转换法则来供参考



## 减、乘、除
因为 JS 并没有类型声明，所以任意两个变量或字面量，都可以做加减乘除。

> 我们在对各种非Number类型运用数学运算符(- * /)时，会先将非Number类型转换为Number类型。

```js 
1 - true // 0， 首先把 true 转换为数字 1， 然后执行 1 - 1
1 - null // 1,  首先把 null 转换为数字 0， 然后执行 1 - 0
1 * undefined //  NaN, undefined 转换为数字是 NaN
2 * ['5'] //  10， ['5']首先会变成 '5', 然后再变成数字 5
```

## 加法的特殊性
为什么加法要区别对待？因为JS里 +还可以用来拼接字符串。谨记以下3条：

- 当一侧为String类型，被识别为字符串拼接，并会优先将另一侧转换为字符串类型。
- 当一侧为Number类型，另一侧为原始类型，则将原始类型转换为Number类型。
- 当一侧为Number类型，另一侧为引用类型，将引用类型和Number类型转换成字符串后拼接。

> 以上 3 点，优先级从高到低，即 3+'abc' 会应用规则 1，而 3+true会应用规则2

```js
123 + '123' // 123123   （规则1）
123 + null  // 123    （规则2）
123 + true // 124    （规则2）
123 + {}  // 123[object Object]    （规则3）
```

## 逻辑语句中的类型转换

当我们使用 if while for 语句时，我们期望表达式是一个Boolean，所以一定伴随着隐式类型转换。而这里面又分为两种情况:

- 单个变量
    如果只有单个变量，会先将变量转换为Boolean值。

    我们可以参考类型转换表的转换表来判断各种类型转变为Boolean后的值。

    不过这里有个小技巧：

    只有 null undefined '' NaN 0 false 这几个是 false，其他的情况都是 true，比如 {} , []。

- 使用 == 比较中的5条规则
    虽然我们可以严格使用 ``===``，不过了解==的习性还是很有必要的。
    根据 == 两侧的数据类型，我们总结出 5 条规则：

    - 规则 1：NaN和其他任何类型比较永远返回false（包括和他自己）
        ```js 
        NaN == NaN // false
        ```
    - 规则 2：Boolean 和其他任何类型比较，Boolean 首先被转换为 Number 类型。
        ```js
        true == 1  // true 
        true == '2'  // false, 先把 true 变成 1，而不是把 '2' 变成 true
        true == ['1']  // true, 先把 true 变成 1， ['1']拆箱成 '1', 再参考规则3
        true == ['2']  // false, 同上
        undefined == false // false ，首先 false 变成 0，然后参考规则4
        null == false // false，同上
        ```

    - 规则 3：String和Number比较，先将String转换为Number类型。 
        ```js
        123 == '123' // true, '123' 会先变成 123
        '' == 0 // true, '' 会首先变成 0
        ```
    - 规则 4：null == undefined比较结果是true，除此之外，null、undefined和其他任何结果的比较值都为false。

        ```js
        null == undefined // true
        null == '' // false
        null == 0 // false
        null == false // false
        undefined == '' // false
        undefined == 0 // false
        undefined == false // false
        ```
    - 规则 5：原始类型和引用类型做比较时，引用类型会依照ToPrimitive规则转换为原始类型。
    
        ToPrimitive规则，是引用类型向原始类型转变的规则，它遵循先valueOf后toString的模式期望得到一个原始类型。

        如果还是没法得到一个原始类型，就会抛出 TypeError

        ```js
        '[object Object]' == {} 
        // true, 对象和字符串比较，对象通过 toString 得到一个基本类型值
        '1,2,3' == [1, 2, 3] 
        // true, 同上  [1, 2, 3]通过 toString 得到一个基本类型值
        ```

## 面试常见
``` js
[] == ![]

第一步，![] 会变成 false
第二步，应用 规则2 ，题目变成： [] == 0
第三步，应用 规则5 ，[]的valueOf是0，题目变成： 0 == 0
所以， 答案是 true ！//
```


```js
[undefined] == false
第一步，应用 规则5 ，[undefined]通过toString变成 '',题目变成  '' == false
第二步，应用 规则2 ，题目变成  '' == 0
第三步，应用 规则3 ，题目变成  0 == 0
所以， 答案是 true ！
// 但是 if([undefined]) 又是个true！
```
## 类型转换表

|类型|值|to Boolean|to Number| to String|
|:---:|:---:|:---:|:---:|:---:|
|Boolean|true|true|1|"true"|
|Boolean|false|false|0|"false"|
|Number|123|true|123|"123"|
|Number|Infinity|true|Infinity|"Infinity"|
|Number|0|false|0|"0"|
|Number|NaN|false|NaN|"NaN"|
|String|''|false|0|""|
|String|'123'|true|123|"123"|
|String|'123abc'|true|NaN|"123abc"|
|String|'abc'|true|NaN|"abc"|
|Null|null|false|0|"null"|
|Undefined|undefined|false|NaN|"undefined"|
|Function|function(){}|true|NaN|"function(){}"|
|Object|{}|true|NaN|"[object Object]"|
|Array|[]|true|0|""|
|Array|['abc']|true|NaN|"abc"|
|Array|['123']|true|123|"123"|
|Array|['123','abc']|true|NaN|"123,abc"|