# 标准模式和兼容模式的区别

标准模式和兼容模式一般来讲是针对于ie浏览器所说的两种模式，另外比如360安全浏览器，也拥有极速模式和兼容模式。

> 要想写出跨浏览器的CSS，必须知道浏览器解析CSS的两种模式：标准模式(strict mode)和怪异模式(quirks mode)。



所谓的标准模式是指，浏览器按W3C标准解析执行代码；怪异模式则是使用浏览器自己的方式解析执行代码，因为不同浏览器解析执行的方式不一样，所以我们称之为怪异模式。浏览器解析时到底使用标准模式还是怪异模式，与你网页中的DTD声明直接相关，DTD声明定义了标准文档的类型（标准模式解析）文档类型，会使浏览器使用相应的方式加载网页并显示，忽略DTD声明,将使网页进入怪异模式(quirks mode)。



```
<html>
    <head>
        <title>重庆PHP</title>
    </head>
    <body>
        <h3>重庆PHP，最专业的PHP社区</h3>
    </body>
</html>
```

如果你的网页代码不含有任何声明，那么浏览器就会采用怪异模式解析，便是如果你的网页代码含有DTD声明，浏览器就会按你所声明的标准解析。



```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
    <head>
        <title>重庆PHP</title>
    </head>
    <body>
        <h3>重庆PHP，最专业的PHP社区</h3>
    </body>
</html>
```

上面的代码，浏览器会按HTML 4.01的标准进行解析。



关于这个知识我只了解到这里了，面试中并没有遇到过这类问题。