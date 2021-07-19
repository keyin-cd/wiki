# 前端缓存

经典面试问题啊～～～

扯那么多闲话，其实就是把从服务器来的一部分数据短时间储存在客户端，没了～

如果面试官不满意，接着往下看吧

> Web缓存是指一个Web资源（如html页面，图片，js，数据等）存在于Web服务器和客户端（浏览器）之间的副本。缓存会根据进来的请求保存输出内容的副本；当下一个请求来到的时候，如果是相同的URL，缓存会根据缓存机制决定是直接使用副本响应访问请求，还是向源服务器再次发送请求。比较常见的就是浏览器会缓存访问过网站的网页，当再次访问这个URL地址的时候，如果网页没有更新，就不会再次下载网页，而是直接使用本地缓存的网页。只有当网站明确标识资源已经更新，浏览器才会再次下载网页。

## web缓存的作用

- 减少网络带宽消耗（当Web缓存副本被使用时，只会产生极小的网络流量，可以有效的降低运营成本。）

- 降低服务器压力（给网络资源设定有效期之后，用户可以重复使用本地的缓存，减少对源服务器的请求，间接降低服务器的压力。同时，搜索引擎的爬虫机器人也能根据过期机制降低爬取的频率，也能有效降低服务器的压力。）

- 减少网络延迟，加开页面打开速度。

## web缓存的类型

### 数据库数据缓存
Web应用，特别是SNS类型的应用，往往关系比较复杂，数据库表繁多，如果频繁进行数据库查询，很容易导致数据库不堪重荷。为了提供查询的性能，会将查询后的数据放到内存中进行缓存，下次查询时，直接从内存缓存直接返回，提供响应效率。比如常用的缓存方案有memcached等。

### 服务器端缓存
服务器端缓存包含代理服务器缓存和CDN缓存:

- 代理服务器缓存
    代理服务器是浏览器和源服务器之间的中间服务器，浏览器先向这个中间服务器发起Web请求，经过处理后（比如权限验证，缓存匹配等），再将请求转发到源服务器。代理服务器缓存的运作原理跟浏览器的运作原理差不多，只是规模更大。可以把它理解为一个共享缓存，不只为一个用户服务，一般为大量用户提供服务，因此在减少相应时间和带宽使用方面很有效，同一个副本会被重用多次。常见代理服务器缓存解决方案有Squid等，这里不再详述。
- CDN缓存
    CDN（Content delivery networks）缓存，也叫网关缓存、反向代理缓存。CDN缓存一般是由网站管理员自己部署，为了让他们的网站更容易扩展并获得更好的性能。浏览器先向CDN网关发起Web请求，网关服务器后面对应着一台或多台负载均衡源服务器，会根据它们的负载请求，动态将请求转发到合适的源服务器上。虽然这种架构负载均衡源服务器之间的缓存没法共享，但却拥有更好的处扩展性。从浏览器角度来看，整个CDN就是一个源服务器。

### 浏览器端缓存
浏览器缓存（Browser Caching）是浏览器端保存数据用于快速读取或避免重复资源请求的优化机制，有效的缓存使用可以避免重复的网络请求和浏览器快速地读取本地数据，整体上加速网页展示给用户。

### Web应用层缓存
应用层缓存指的是从代码层面上，通过代码逻辑和缓存策略，实现对数据，页面，图片等资源的缓存，可以根据实际情况选择将数据存在文件系统或者内存中，减少数据库查询或者读写瓶颈，提高响应效率。

## web缓存之浏览器端缓存浅析
根据标准，到目前为止，H5 一共有6种缓存机制，有些是之前已有，有些是 H5 才新加入的。
```js
 1. 浏览器缓存机制
 2. Dom Storgage（Web Storage）存储机制
 3. Web SQL Database 存储机制 
 4. Application Cache（AppCache）机制 
 5. Indexed Database （IndexedDB） 
 6. File System API
```

### 浏览器缓存机制
- 非HTTP协议定义的缓存机制
    浏览器缓存机制，其实主要就是HTTP协议定义的缓存机制（如： Expires； Cache-control等）。但是也有非HTTP协议定义的缓存机制，如使用HTML Meta 标签，Web开发者可以在HTML页面的<head>节点中加入<meta>标签，代码如下：
    ```html
    <META HTTP-EQUIV="Pragma" CONTENT="no-cache">
    <META HTTP-EQUIV="Expires" CONTENT="0">
    ```

    上述代码的作用是告诉浏览器当前页面不被缓存，每次访问都需要去服务器拉取。使用上很简单，但只有部分浏览器可以支持，而且所有缓存代理服务器都不支持，因为代理不解析HTML内容本身。下面主要介绍HTTP协议定义的缓存机制。

- HTTP协议定义的缓存机制
    通过 HTTP 协议头里的 Cache-Control（或 Expires）和 Last-Modified（或 Etag）等字段来控制文件缓存的机制。这应该是 WEB 中最早的缓存机制了，是在 HTTP 协议中实现的，有点不同于 Dom Storage、AppCache 等缓存机制，但本质上是一样的。可以理解为，一个是协议层实现的，一个是应用层实现的。

- HTTP1.0 时代缓存字段详解
    - Pragma：设置页面是否缓存，为Pragma则缓存，no-cache则不缓存。

    - Expires：有了Pragma来禁用缓存，自然也需要有个东西来启用缓存和定义缓存时间，对http1.0而言，Expires就是做这件事的首部字段。 Expires的值对应一个GMT（格林尼治时间），比如Mon, 22 Jul 2002 11:12:01 GMT来告诉浏览器资源缓存过期时间，如果还没过该时间点则不发请求。
    如果Pragma头部和Expires头部同时存在，则起作用的会是Pragma，需要注意的是，响应报文中Expires所定义的缓存时间是相对服务器上的时间而言的，其定义的是资源“失效时刻”，如果客户端上的时间跟服务器上的时间不一致（特别是用户修改了自己电脑的系统时间），那缓存时间可能就没啥意义了

- HTTP1.1 时代缓存字段详解

    - Cache-Control： 针对上述的“Expires时间是相对服务器而言，无法保证和客户端时间统一”的问题，http1.1新增了 Cache-Control 来定义缓存过期时间。注意：若报文中同时出现了 Expires 和 Cache-Control，则以 Cache-Control 为准。

        (1) 最常见的，比如服务器回包：Cache-Control:max-age=600 表示文件在本地应该缓存，且有效时长是600秒（从发出请求算起）。在接下来600秒内，如果有请求这个资源，浏览器不会发出 HTTP 请求，而是直接使用本地缓存的文件。
        (2) Cache-Control: no-cache；这个很容易让人产生误解，使人误以为是响应不被缓存。实际上她是会被缓存的，只不过每次在向客户端（浏览器）提供响应数据时，缓存都要向服务器评估缓存响应的有效性。
        (3) Cache-Control: no-store；这个才是响应不被缓存的意思。

    - Last-Modified/If-Modified-Since：
        (1) Last-Modified：标示这个响应资源的最后修改时间。web服务器在响应请求时，告诉浏览器资源的最后修改时间。
        (2) If-Modified-Since：当资源过期时（使用Cache-Control标识的max-age），发现资源具有Last-Modified声明，则再次向web服务器请求时带上头 If-Modified-Since，表示请求时间。web服务器收到请求后发现有头If-Modified-Since 则与被请求资源的最后修改时间进行比对。若最后修改时间较新，说明资源又被改动过，则响应整片资源内容（写在响应消息包体内），HTTP 200；若最后修改时间较旧，说明资源无新修改，则响应HTTP 304 (无需包体，节省浏览)，告知浏览器继续使用所保存的cache。

    - Etag/If-None-Match：Etag/If-None-Match也要配合Cache-Control使用。

        (1) Etag：web服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器觉得）。Apache中，ETag的值，默认是对文件的索引节（INode），大小（Size）和最后修改时间（MTime）进行Hash后得到的。

        (2) If-None-Match：当资源过期时（使用Cache-Control标识的max-age），发现资源具有Etage声明，则再次向web服务器请求时带上头If-None-Match （Etag的值）。web服务器收到请求后发现有头If-None-Match 则与被请求资源的相应校验串进行比对，决定返回200或304。

    - 既生Last-Modified何生Etag？

        你可能会觉得使用Last-Modified已经足以让浏览器知道本地的缓存副本是否足够新，为什么还需要Etag（实体标识）呢？HTTP1.1中Etag的出现主要是为了解决几个Last-Modified比较难解决的问题：
        (1) Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的修改时间。
        (2) 如果某些文件会被定期生成，当有时内容并没有任何变化，但Last-Modified却改变了，导致文件没法使用缓存。
        (3) 有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形。
        Etag是服务器自动生成或者由开发者生成的对应资源在服务器端的唯一标识符，能够更加准确的控制缓存。Last-Modified与ETag是可以一起使用的，服务器会优先验证ETag，一致的情况下，才会继续比对Last-Modified，最后才决定是否返回304。



























### Dom Storage 存储机制

DOM Storage 是指 HTML5 的本地存储 API sessionStorage 和 localStorage。在介绍HTML5本地存储之前，先来看一看前面几个存储方式的概念。

- HTTP Cookie 
    Cookie是为了解决HTTP无状态的特性而出现的，也可以叫用户识别机制。常用的用户识别机制包括：
    - 承载用户信息的HTTP首部

    - 客户端IP地址追踪技术，通过用户的IP地址对其进行识别

    - 用户登录，用认证机制来识别用户

    - 胖URL，一种在URL中嵌入识别信息的技术

    - cookie，一种强大且高效的持久身份识别技术

    对于购物网站而言，cookie是非常重要的，为了实现购物车功能，把已选物品加入cookie，可以实现不同页面之间数据的同步，同时在提交订单的时候又会把这些cookie传到后台，大大方便了前后端开发。

-  userData
    userData是微软在上世纪90年代的浏览器大战时推出的本地存储方案，借助DHTML的behaviour属性来存储本地数据， 允许每个页面最多存储64K数据，每个站点最多640K数据，userData的缺点显而易见，它不是Web标准的一部分，除非你的程序只需要支持IE， 否则它基本没什么用处。

- Flash cookie
    Flash cookie的名字有些误导，它实际上和HTTP cookie并不是一回事，或许它的名字应该叫做"Flash本地存储”，Flash cookie默认允许每个站点存储不超过100K的数据，如果超出了，Flash会自动向用户请求更大的存储空间，借助Flash的 ExternalInterface接口，你可以很轻松地通过Javascript操作Flash的本地存储。Flash的问题很简单，就是因为它是 Flash。

- Gears
    Gears是Google在07年发布的一个开源浏览器插件，旨在改进各大浏览器的兼容性，Gears内置了一个基于SQLite的嵌入式 SQL数据库，并提供了统一API对数据库进行访问，在取得用户授权之后，每个站点可以在SQL数据库中存储不限大小的数据，Gears的问题就是 Google自己都已经不用它了。
- HTML5 的本地存储 API sessionStorage 和 localStorage
    Dom Storage 是通过存储字符串的 Key/Value 对来提供的，并提供 5MB （不同浏览器可能不同，分 HOST)的存储空间（Cookies 才 4KB)。另外 Dom Storage 存储的数据在本地，不像 Cookies，每次请求一次页面，Cookies 都会发送给服务器。
    DOM Storage 分为 sessionStorage 和 localStorage。localStorage 对象和 sessionStorage 对象使用方法基本相同，它们的区别在于作用的范围不同。sessionStorage 用来存储与页面相关的数据，它在页面关闭后无法使用。而 localStorage 则持久存在，在页面关闭后也可以使用。







### Web SQL存储机制
H5 也提供基于 SQL 的数据库存储机制，用于存储适合数据库的结构化数据。根据官方的标准文档，Web SQL Database 存储机制不再推荐使用，将来也不再维护，而是推荐使用 AppCache 和 IndexedDB。

### Application Cache 机制

Application Cache（简称 AppCache)似乎是为支持 Web App 离线使用而开发的缓存机制。它的缓存机制类似于浏览器的缓存（Cache-Control 和 Last-Modified）机制，都是以文件为单位进行缓存，且文件有一定更新机制。但 AppCache 是对浏览器缓存机制的补充，不是替代。
AppCache 的原理有两个关键点：manifest 属性和 manifest 文件。

这部分内容在``html/html5 离线储存``已经说过了


### Indexed Database

IndexedDB 也是一种数据库的存储机制，但不同于已经不再支持的 Web SQL Database。IndexedDB 不是传统的关系数据库，indexedDB中没有表的概念，类似于 Dom Storage 的 key-value 的存储方式，但功能更强大，且存储空间更大。它的特点是：

- 以key-value 的方式存取对象，可以是任何类型值或对象，包括二进制。

- 可以对对象任何属性生成索引，方便查询。

- 较大的存储空间，默认推荐250MB(分 HOST)，比 Dom Storage 的5MB要大的多。

- 通过数据库的事务（tranction）机制进行数据操作，保证数据一致性。

- 异步的 API 调用，避免造成等待而影响体验。


### File System API
File System API 是 H5 新加入的存储机制。它为 Web App 提供了一个虚拟的文件系统，就像 Native App 访问本地文件系统一样。由于安全性的考虑，这个虚拟文件系统有一定的限制。Web App 在虚拟的文件系统中，可以进行文件（夹）的创建、读、写、删除、遍历等操作。

File System API 也是一种可选的缓存机制，和前面的 SQLDatabase、IndexedDB 和 AppCache 等一样。File System API 有自己的一些特定的优势：

- 可以满足大块的二进制数据（ large binary blobs）存储需求。

- 可以通过预加载资源文件来提高性能。

- 可以直接编辑文件。

---

