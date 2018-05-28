### 如何让一个浮动元素居中
```left: %``` 的含义是 
> Sets the left edge position in % of containing element. Negative values are allowed

也就是父元素的宽度的百分比。

> 父元素和子元素同时左浮动，然后父元素相对左移动50%，再然后子元素相对右移动50%，或者子元素相对左移动-50%也就可以了。

代码如下：

```html
<!DOCTYPE html>
<html>
<head>
    <title>Demo</title>
    <meta charset="utf-8" />
    <style type="text/css">
    .p {
        float: left;
        position: relative;
        left: 50%;
        border: 1px solid #fee;
    }
    .p1 {
        float: left;
        margin-left: 50%;
        border: 1px solid #fee;
    }

    .c {
        position: relative;
        float: left;
        right: 50%;
        border: 1px solid #00e;
    }
    </style>
</head>
<body>
    <div class="p">
        <h1 class="c">Test Float Element Center</h1>
    </div>
    <div style="clear: both"></div>
    <div class="p1">
        <h1 class="c">Test Float Element Center</h1>
    </div>
</body>
</html>
```

##References
[前端攻城狮学习笔记七：常见前端面试题之HTML/CSS部分（二）](http://www.cnblogs.com/jscode/archive/2012/07/10/2583856.html)

