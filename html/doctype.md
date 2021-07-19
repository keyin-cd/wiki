# DOCTYPE的作用
>浏览器分为两种模式，一种是标准模式，一种是怪异模式，使用工具开发html的时候自动生成的都是标准模式 这两种模式就是是通过doctype的定义来区分

什么是doctype，doctype是一种标准通用标记语言的文档类型声明，目的是告诉标准通用标记语言解析器要使用什么样的文档类型定义(DTD)来解析文档

doctype 最早是xml的概念，在xml中它的定义是通过一种特定的语法，作为一种元数据，来描述xml文档中允许出现的元素，以及各元素的组成，规则等

doctype在html中的作用是触发浏览器的标准模式，如果html中省略了doctype，浏览器会进入到Quirks模式的怪异状态也称之为兼容模式，在这种模式下有些样式，布局会和标准模式存在差异，html标准，DOM标准只规定了标准模式下的行为，没有对Quirks模式做出规定，因此不同浏览器在Quirks模式下的处理也是不同的，应用Quirks比较困难，所以需要慎用

在html4中 doctype有三种模式

* 严格模式：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"  "http://www.w3.org/TR/html4/strict.dtd">

* 过渡模式：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"  "http://www.w3.org/TR/html4/loose.dtd">

* 框架模式：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"  "http://www.w3.org/TR/html4/frameset.dtd">

因为浏览器有容错能力，实际上运用这三种模式并不十分严格,浏览器都能正确解析出用户想要的结果，所以

在html5中doctype就简化成了<!DOCTYPE html>

但是为了向后兼容性，可扩展性等事情，html5还定义了其他几种组合

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0//EN">  

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0//EN"  "http://www.w3.org/TR/REC-html40/strict.dtd"> 

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN">  

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN"  "http://www.w3.org/TR/html4/strict.dtd">  

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">  

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN"  "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<!doctype HTML system "about:legacy-compat">（ 兼容久远时代的历史遗产而准备的DOCTYPE，about:legacy-compat，注意这段文本是大小写敏感的）

DOCTYPE的组成说明如下：

- 一段文本，即<!DOCTYPE，大小写不敏感
- 1个或多个空格   
- 字符HTML，同样大小写不敏感  
- 1个或多个空格   
- 字符PUBLIC，大小写不敏感   
- 继续1个或多个空格   
- 对引号或单引号（必须前后匹配），引号中放一个Public ID  
- 可选内容：
   * 1个或多个空格 

   * 一对引号或单引号（必须前后匹配），引号中放一个与前面的Public ID对应的System ID   

- 1个或多个空格  

- 结束标记，即>

Public ID和System ID是有严格的对应关系的，如果规定的System ID不能有Public ID，则上面的第8项可选内容也就不能存在 HTML5彻底放弃了HTML4中的过渡型和框架型的DOCTYPE，同时整合了XHTML的DOCTYPE 声明

对于DOCTYPE的作用，在真正的浏览中，仅仅起到触发浏览器的标准模式的作用 虽然根据标准，一个HTML文档中，DOCTYPE前可以有其他的元素，如几个注释，一点空格，但是在当前的状态下，并没有这么理想：  

对于IE6到IE9，如果DOCTYPE前存在注释，会进入Quirks模式   

对于IE6，如果DOCTYPE前存在一个XML声明，会进入Quirks模式   

标准模式与怪异模式   

至于标准模式与怪异模式的表现，

在标准模式中，网页元素的宽度是由padding、border、width三者的宽度相加决定的；

在怪异模式中，width本身就包括了padding和border的宽度    

此外，标准模式下块级元素的经典居中方法：设定width，margin:0px auto; 在怪异模式下无法正常工作