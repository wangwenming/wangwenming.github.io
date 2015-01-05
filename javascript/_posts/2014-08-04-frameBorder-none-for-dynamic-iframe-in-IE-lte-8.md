---
---

##
```js
var iframe = document.createElement('iframe');
// IE6/7/8 插入到DOM节点前设置有效
// iframe.setAttribute('frameBorder', 0);
document.body.appendChild(iframe);
// IE6/7/8 插入到DOM节点后设置无效
iframe.setAttribute('frameBorder', 0);
```
textilize2


## Ref:
http://stackoverflow.com/questions/1625835/ie-8-iframe-border

