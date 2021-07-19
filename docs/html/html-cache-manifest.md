# html5 离线缓存

> html的离线存储和html本地存储(cookie、localstorage)一定要分开


## Application Cache


> HTML5引入了应用程序缓存技术，意味着web应用可进行缓存，并在没有网络的情况下使用，通过创建cache manifest文件，可以轻松的创建离线应用

## 离线缓存优势

* 离线浏览
* 提升页面载入速度
* 降低服务器压力

## app-cache的应用
- 把需要离线存储在本地的文件列在一个cache.manifest配置文件中，并在HTML中设置manifest属性

```
<!DOCTYPE HTML>
<html manifest = "cache.manifest">
...
</html>
```

```
CACHE MANIFEST
#v0.11

CACHE:
js/app.js
css/style.css

NETWORK:
resourse/logo.png //可以使用通配符”*”，默认所有非CACHE文件

FALLBACK:
/ /offline.html  //第一个 URI 是资源，第二个是替补。
```
1.CACHE:
表示需要离线存储的资源列表，由于包含manifest文件的页面将被自动离线存储，所以不需要把页面自身也列出来。

2.NETWORK:
表示在它下面列出来的资源只有在在线的情况下才能访问，他们不会被离线存储，所以在离线情况下无法使用这些资源。

3.FALLBACK:
表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如上面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问offline.html。

## 浏览器解析manifest

浏览器发现html头部有manifest属性，它会请求manifest文件

- 第一次访问：下载所有资源，对manifest指定的离线资源进行缓存
- 再访问时manifest如果没有更新：直接加载离线缓存过的资源
- 再访问时manifest如果已更新：下载新的manifest文件，重新缓存所有离线资源
- 离线时访问：直接加载离线缓存过的资源

## 注意事项

- 如果只是更新了资源而没有更新manifest文件的话，浏览器并不会重新下载资源，还是使用原来离线存储的资源。
- 对于manifest文件最好不要设置缓存。否则如果本地缓存的manifest文件还没过期，即使服务器上的manifest文件已更新也没有用。
- 浏览器在下载manifest文件中的资源的时候，它会一次性下载所有资源，如果某个资源由于某种原因下载失败，那么这次的所有更新就算是失败的，浏览器还是会使用原来的资源。
- 在更新了资源之后，新的资源需要到下次再打开app才会生效，如果需要资源马上就能生效，那要使用window.applicationCache.swapCache()。
用户清空了缓存，会重新下载所有资源