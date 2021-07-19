# meta标签详解

## meta标签的定义和用法


``<meta>`` 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。  


``<meta>`` 标签位于文档的头部，不包含任何内容。``<meta>`` 标签的属性定义了与文档相关联的名称/值对。



## 必要属性


content -> 定义与 http-equiv 或 name 属性相关的元信息


## 可选属性

- http-equiv
    content-type、expires、refresh、set-cookie  
    把 content 属性关联到 HTTP 头部。  
    http-equiv 属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前先在要传送给浏览器的 MIME 文档头部包含名称/值对。

    当服务器向浏览器发送文档时，会先发送许多名称/值对。虽然有些服务器会发送许多这种名称/值对，但是所有服务器都至少要发送一个：content-type:text/html。这将告诉浏览器准备接受一个 HTML 文档。

    使用带有 http-equiv 属性的 ``<meta>`` 标签时，服务器将把名称/值对添加到发送给浏览器的内容头部。例如，添加：
    ```
    <meta http-equiv="charset" content="iso-8859-1">
    <meta http-equiv="expires" content="31 Dec 2008">
    ```
    这样发送到浏览器的头部就应该包含：
    ```
    content-type: text/html
    charset:iso-8859-1
    expires:31 Dec 2008
    ```
    当然，只有浏览器可以接受这些附加的头部字段，并能以适当的方式使用它们时，这些字段才有意义。



- name
    author、description、keywords、generator、revised、others  
    把 content 属性关联到一个名称。  
    name 属性提供了名称/值对中的名称。HTML 和 XHTML 标签都没有指定任何预先定义的 ``<meta>`` 名称。通常情况下，您可以自由使用对自己和源文档的读者来说富有意义的名称。

    "keywords" 是一个经常被用到的名称。它为文档定义了一组关键字。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。

    类似这样的 meta 标签可能对于进入搜索引擎的索引有帮助：
    ```
    <meta name="keywords" content="HTML,ASP,PHP,SQL">
    ```
    如果没有提供 name 属性，那么名称/值对中的名称会采用 http-equiv 属性的值。


- scheme 
    定义用于翻译 content 属性值的格式。


## 常用

其中关于meta做常用的可能就是移动端的viewport，这个大家应该都用过的。

```
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```

设置文档编码格式

```
<meta charset="utf-8">
```

再比如可以设置屏幕缩放，以前我就见过一个移动端的网站通过屏幕缩放来实现小于12px的字体，不过我没有实际试验过~~