# 居中布局

> 居中布局在面试中很常见，有很多种方法可以实现，随便说几种就可以啦


所谓居中布局就是让一个盒子在它的父级空间内，横向纵向都居中

先写一端公用代码

```
<body>
    <style>
        *{margin:0;padding:0;}
        .content{
            width: 200px;
            height: 200px;
            background: red;
        }
    </style>
    <div class="content"></div>
</body>
```

## planA
```
.content{
    position: absolute;
    top: 50%;
    left: 50%;
    margin: -100px 0 0 -100px;
}
```

## planB
如果不知道宽高就用这种
```
.content{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```

## planC
flex布局
```
html{height: 100%;}
body{
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}
```

## planD
```
.content{
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
}
```

## planE
```
display: table-cell;
text-align: center;
vertical-align: middle;
```
这种table-cell的方法，需要父级有固定宽高，切记

