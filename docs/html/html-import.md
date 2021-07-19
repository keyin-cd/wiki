# html中的import

同学们，要注意啦！！  
- 这个import不是css中的@import  
- 这个import不是css中的@import  
- 这个import不是css中的@import  

## html的模块加载方案

> 在写静态页面的时候动态加载模块一直是个问题，以前只能用 iframe, ajax 等动态加载，这样整体的加载效果很差，最近才发现 HTML 中可以使用 import 导入。


### 使用方法
```
<head>
  <link rel="import" href="/path/to/imports/stuff.html">
</head>
```

link 中 href 中的导入地址必须允许跨域。

浏览器的网络协议栈(network stack)会对访问相同 URL 的请求自动去重。这意味着从同一个 URL 导入的内容只会被获取一次。无论这个地址被导入多少次，最终它将只执行一次。

### 检查是否可用
```
function supportsImports() {
  return 'import' in document.createElement('link');
}

if (supportsImports()) {
  // 支持导入!
} else {
  // 使用其他的库来加载文件。
}
```

### link.import


```
var content = document.querySelector('link[rel="import"]').import;
```

在下面几种情况下，link.import 值为 null ：

- 浏览器不支持 HTML 导入。
- 没有 rel=”import”。
- 没有被加入到 DOM 中。
- 从 DOM 中被移除。
- 资源没有开启 CORS。


### 实例
假设 warnings.html 包含如下内容：

```
<div class="warning">
  <style scoped>
    h3 {
      color: red;
    }
  </style>
  <h3>Warning!</h3>
  <p>This page is under construction</p>
</div>

<div class="outdated">
  <h3>Heads up!</h3>
  <p>This content may be out of date</p>
</div>
```

导入页面中  

```
<head>
  <link rel="import" href="warnings.html">
</head>
<body>
  ...
  <script>
    var link = document.querySelector('link[rel="import"]');
    var content = link.import;

    // 从 warning.html 的文档中获取 DOM。
    var el = content.querySelector('.warning');

    document.body.appendChild(el.cloneNode(true));
  </script>
</body>
```


### 包含模板


HTML Template 元素是 HTML 导入的好搭档。

##### import.html

```
<template>
  <h1>Hello World!</h1>
  <img src="world.png"> <!-- 只有当模板生效后才会去请求图片 -->
  <script>alert("Executed when the template is activated.");</script>
</template>
```
##### index.html

```
<head>
  <link rel="import" href="import.html">
</head>
<body>
  <div id="container"></div>
  <script>
    var link = document.querySelector('link[rel="import"]');

    // 从导入中复制 <template>。
    var template = link.import.querySelector('template');
    var content = template.content.cloneNode(true)

    document.querySelector('#container').appendChild(content);
  </script>
</body>
```