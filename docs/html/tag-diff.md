# 行块标签区别

啧啧~前端入行第一课


## 块元素
块级元素在浏览器显示时，通常会以新行来开始（和结束）

- 总是在新行上开始。
- 高度，宽度，内外边距，都可以进行控制。
- 宽度缺省是他的容器的100%，除非设定一个宽度。
- 它可以容纳行内元素和其他块元素。

p、div、ul、ol、li、dl、dt、dd、h1~h6、form、table、td、thread、tr


## 行元素

行元素也叫内敛元素，英文：inline element，其中文叫法有多种，如：内联元素、内嵌元素、行内元素、直进式元素等。基本上没有统一的翻译。元素在一行内显示。内联元素一般只能容纳文本或者其他内联元素


- 可以和其它元素共处一行，不用另起一行编写。
- 元素的高度，宽度以及顶部和底部的内外边距不能进行设置，(因为设置了不起作用)
- 该行标签的高度，宽度，是它包含的内容(图片，文字)的宽度


a、abbr、b(字体加粗)、br、em、input、select、span、strong、sub、textarea、

## 行<-->块
- 块级标签转化为行内标签：display：inline;
- 行内标签转换为块级标签：display：block；
- 转换为行内块标签：display:inline-block;