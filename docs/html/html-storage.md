# 浏览器存储方式

> 介绍浏览器的存储方式，例如，cookie，sessionStorage，localStorage，indexedDB，Web SQL


## cookie

浏览器cookie分为两种
* 会话cookie，不设置过期时间，页面标签关闭立即清除，这种cookie是保存在内存中，而不是保存在硬盘中
* 永久cookie，设置过期时间，cookie数据保存在硬盘上，关闭浏览器不会对cookie造成影响，cookie时效持续到过期时间为止

属性说明:
- Expires或Max-Age 设置过期时间
- Secure 使用https协议
- HttpOnly 不能使用js获取cookie
- Domain 表示指定了哪些主机可以接受Cookie，默认当前主机，不包括子域名

cookie可能会造成CRRF攻击

封装的一些cookie函数
```
/**
 * 存cookie
 * @export
 * @param {*} name
 * @param {*} value
 */
export function setCookie(name, value) {
    const Days = 30000;
    let exp = new Date();
    exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000);
    document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
}
/**
 * 取cookie
 * @export
 * @param {*} name
 * @returns
 */
export function getCookie(name) {
    let arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
    if (arr = document.cookie.match(reg)) {
        return unescape(arr[2]);
    } else { return null; }
}
/**
 * 清除cookie
 * @export
 */
export function clearCookie() {
    var date = new Date();
    date.setTime(date.getTime() - 10000);
    var keys = document.cookie.match(/[^ =;]+(?=\=)/g);
    if (keys) {
        for (var i = keys.length; i--;)
            document.cookie = keys[i] + "=0; expire=" + date.toGMTString() + "; path=/";
    }
}
```


## sessionStorage

以key，value的形式储存在浏览器中，当标签关闭会清除sessionStorage数据

## localStorage

localStorage 中的键值对是以字符串的形式存储，除非清除，否则长期有效

## indexedDB

IndexedDB 是一个事务型的数据库系统. 然而, 它是使用 JavaScript 对象而非列数固定的表格来储存数据的.使用的比较少

## webSql
Web SQL 数据库 API 并不是 HTML5 规范的一部分，但是它是一个独立的规范，引入了一组使用 SQL 操作客户端数据库的 APIs。

如果你是一个 Web 后端程序员，应该很容易理解 SQL 的操作。

```
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (tx) {  
   tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
});
```

## 面试回答
这一类问题比较简单，常问的可能就是session和local的区别，这两个不要记混了，就可以了