### 新浏览器不支持自定义文案
> Starting with Firefox 4, Chrome 51, Opera 38 and Safari 9.1, a generic string not under the control of the webpage will be shown instead of the returned string. For example, Firefox displays the string "This page is asking you to confirm that you want to leave - data you have entered may not be saved." 

例如，中文环境下 Chrome 提示为：
```
标题：要离开此网站吗？
内容：系统可能不会保存您所做的更改。
```
![对话框截图](/img/onbeforeunload.png)

### 不能调用 alert confirm prompt

> Since 25 May 2011, the HTML5 specification states that calls to window.alert(), window.confirm(), and window.prompt() methods may be ignored during this event. See the HTML5 specification for more details.

### references
[WindowEventHandlers.onbeforeunload](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeunload)
