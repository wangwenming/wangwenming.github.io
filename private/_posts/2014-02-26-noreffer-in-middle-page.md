---
---

## 为什么需要中间页
1. j.www.so.com 是被第三方认可的来源，new1.360.cn, new2.so.com 这些新增的不被第三方认可的来源，此时可以通过 j.www.so.com 把 referrer 修改为 j.www.so.com，达到来源统计的目的。

## 跳转使用中间页的好处
1. 如上，可方便实现来源统计
2. 亦可实现 noreferrer 功能，并且所有逻辑处理由中间页处理，无需每个页面增加一坨JavaScript代码

## 如何实现
```
<iframe src='javascript:window.open("http://tieba.baidu.com/f?kw=%BA%C3", "_top");' style='display:none;'></iframe>
```

## 有何问题
1. GBK编码参数变成乱码（IE6~IE11/Chrome测试都会有此Bug）

## 修复方案
```
(function() {
var url = 'http://tieba.baidu.com/f?kw=%BA%C3';
try {
    var iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src="about:blank";
    document.body.appendChild(iframe);
    iframe.contentWindow.open(url, '_top');
} catch(e) {
    window.location.replace(url);
}
})();
```

## 遗留问题
1. iframe的src为JavaScript伪协议的方式，如果URL含GBK编码，则 Firefox不跳转（测试过 27.0.1 ）

## rel="noreferrer" 的支持率
1. Firefox
