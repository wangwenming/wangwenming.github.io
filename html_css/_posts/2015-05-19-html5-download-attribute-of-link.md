### 定义
HTML5支持给a标签添加download属性，可以指定属性值作为文件名。

### 浏览器支持
Chrome 14
Firefox 20
IE x
Safari x

iOS Safari x
Android Browser 4.4
Chrome for Android 42
UC Browser for Android x

### 跨域
1. Firefox不允许跨域，指定CORS对download也是无效的[MDN Element/a#attr-download]。
2. Chrome在跨域情况下，如果指定CORS Header，是可以的。不指定的话则忽略。


## Reference
1. [Downloading resources in HTML5: a download][Downloading resources in HTML5: a download]
2. [Can I use][Can I use]
3. [MDN Element/a#attr-download][MDN Element/a#attr-download]

[Downloading resources in HTML5: a download]: http://updates.html5rocks.com/2011/08/Downloading-resources-in-HTML5-a-download
[Can I use]: http://caniuse.com/#feat=download
[stackoverflow]: http://stackoverflow.com/questions/28318017/html5-download-attribute-not-working-when-downloading-from-another-server-even
[MDN Element/a#attr-download]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-download
