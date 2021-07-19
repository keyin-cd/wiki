# 介绍浏览器四大内核

>浏览器最重要或者说核心的部分是“Rendering Engine”，可大概译为“渲染引擎”，不过我们一般习惯将之称为“浏览器内核”。负责对网页语法的解释（如标准通用标记语言下的一个应用HTML、JavaScript）并渲染（显示）网页。 所以，通常所谓的浏览器内核也就是浏览器所采用的渲染引擎，渲染引擎决定了浏览器如何显示网页的内容以及页面的格式信息。不同的浏览器内核对网页编写语法的解释也有不同，因此同一网页在不同的内核的浏览器里的渲染（显示）效果也可能不同，这也是网页编写者需要在不同内核的浏览器中测试网页显示效果的原因。

## Trident
    IE浏览器内核，IE浏览器是目前国内用户量最多的浏览器
    IE诞生于1994年
## Gecko
    Firefox浏览器内核
## Blink
    Opera浏览器内核，Blink为Webkit内核的分支，13年以后chrome不再使用webkit内核，改用Blink
## Webkit
    safari浏览器内核，safari作为macOs的默认浏览器


## 浏览器
1、IE浏览器内核：Trident内核，也是俗称的IE内核  
2、Chrome浏览器内核：统称为Chromium内核或Chrome内核，以前是Webkit内核，现在是Blink内核  
3、Firefox浏览器内核：Gecko内核，俗称Firefox内核  
4、Safari浏览器内核：Webkit内核  
5、Opera浏览器内核：最初是自己的Presto内核，后来是Webkit，现在是Blink内核  
6、360浏览器、猎豹浏览器内核：IE+Chrome双内核  
7、搜狗、遨游、QQ浏览器内核：Trident（兼容模式）+Webkit（高速模式）   
8、百度浏览器、世界之窗内核：IE内核  
9、2345浏览器内核：以前是IE内核，现在也是IE+Chrome双内核 