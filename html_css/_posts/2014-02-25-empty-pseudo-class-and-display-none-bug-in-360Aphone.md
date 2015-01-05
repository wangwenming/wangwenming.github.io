---
---

# 这里可以直接写 HTML 吧

<style>
div:empty {display: none; color: red;}
</style>

在360浏览器(我测试了5.1.5)下，包含`:empty`的选择符，如果值包含`display: none`（只对none有效），则不管元素是否为空，`display`都会生效

CSS为： `div:empty {display: none; color: red;}`

360浏览器下，分割线下面（有一个div）啥也看不到哦！

<div style="border-top: 1px solid #ddd; padding-top: 10px;">如果你看到我了，说明你不是用360浏览器</div>

<script>
console.log('Congs!');
</script>

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>:empty and display:none bug in 360APhone Browser</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
<style>
div:empty {display: none; color: red;}
</style>
</head>
<body>
<p>
在360浏览器(我测试了 5.1.5)下，包含 :empty 的选择符，如果值包含 display:none （只对none有效），则不管元素是否为空 ， display 都会生效
</p>
<p>
CSS为： div:empty {display: none; color: red;}
</p>
<p>360浏览器下，分割线下面（有一个div）啥也看不到哦！</p>
<hr>
<div>如果你看到我了，说明你不是用360浏览器</div>
</body>
</html>
```
