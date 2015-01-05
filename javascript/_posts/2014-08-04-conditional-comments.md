---
author: "wangwenming"
---

## IE的条件注释(conditional comments)

# 基本语法有2种
downlevel-hidden  <!--[if expression]> HTML <![endif]-->   非IE浏览器当做注释
downlevel-revealed  <![if expression]> HTML <![endif]> 非IE浏览器可见，因为前后当做无效标签忽略了！

# expression语法
[if !IE]
[if lte IE 6]
[if (gt IE 5)&(lt IE 7)]

# Note
"Important  As of Internet Explorer 10, conditional comments are no longer supported by standards mode."
从IE10起（标准模式）不再支持条件注释。

# 应用
```html
<!--[if lt IE 7 ]><html class="ie6"><![endif]-->
<!--[if IE 7 ]><html class="ie7"><![endif]-->
<!--[if IE 8 ]><html class="ie8"><![endif]-->
<!--[if IE 9 ]><html class="ie9"><![endif]-->
<!--[if (gt IE 9)|!(IE)]><!--><html><!--<![endif]-->
```

```html
<!--[if lt IE 7]><html class="ie6"><![endif]-->
<!--[if IE 7]><html class="ie7"><![endif]-->
<!--[if IE 8]><html class="ie8"><![endif]-->
<!--[if IE 9]><html class="ie9"><![endif]-->
<![if gt IE 9]><html><![endif]>
```

# 引用
1. http://msdn.microsoft.com/en-us/library/ms537512(v=vs.85).aspx

