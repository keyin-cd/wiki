# link 和 @import 区别

作为引入外部css文件的方法，这两者有哪些区别

link是html的一个标签，而@import是css提供的

页面被加载时，link会同时被加载，而@import引用的css会等到页面加载结束后加载。

link是html标签，因此没有兼容性，而@import只有IE5以上才能识别。

link方式样式的权重高于@import的。

