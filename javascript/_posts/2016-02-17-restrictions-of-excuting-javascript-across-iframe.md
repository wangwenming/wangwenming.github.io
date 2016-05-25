## iframe 互相执行 JavaScript 的限制

按照同域、同一级域、跨域 3 种情况来分析。

### 同域
* 有权限互相操作，包括 iframe 和被 iframe 页面的 DOM ，以及所有全局函数、变量。

### 同一级域，例如 a.example.com 和 example.com
通过在a.example.com页面加入JS代码
```document.domain = 'example.com';```
后有权限通过JavaScript互相操作。

### 跨域
不能直接互相操作。

### 补充1：本地文件
对于本地文件，其默认域名为可理解为空字符串，则不可以和任何URL的通信。并且两个同目录下的本地页面也不可以互相操作。

### 补充2：onload
iframe.onload 可以跨域。

```
 如果是
     var iframe = document.createElement('iframe');
     iframe.src = 'http://localhost/iframe.html';
     document.body.appendChild(iframe);
则下面的绑定方式生效
     iframe.contentWindow.onload = function() {
          console.log('[iframe.contentWindow.onload]');
     };

     iframe.contentWindow.addEventListener('load', function() { //不能写成onload
          console.log("[iframe.contentWindow.addEventListener('load', ...)]");
     }, false );
但是这种绑定不生效
     iframe.contentDocument.addEventListener('DOMContentLoaded', function() { // bug
          console.log("[iframe.contentDocument.addEventListener('DOMContentLoaded', ...)]");
     }, false );

     iframe.contentWindow.document.addEventListener('DOMContentLoaded', function() { // bug
          console.log("[iframe.contentWindow.document.addEventListener('DOMContentLoaded', ...)]");
     }, false );

如果是
     var iframe = document.getElementById('iframe');
     iframe.src = 'http://localhost/iframe.html';
则只有iframe.onload生效。
```
