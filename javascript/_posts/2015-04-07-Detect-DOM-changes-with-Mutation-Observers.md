## 一句话
W3C DOM4 标准引入了[MutationObserver] [MutationObserver]接口替代DOM Level 2(2000-11-13定稿)引入的低效的[Mutation Events] [MutationEvents](基本未被熟知，也未被普遍使用过)。

## Mutation Events 简介:
DOM的每个修改(包括属性和文本)都会得到通知。这些事件都无法 cancelable 。

可用如下代码检测是否支持：

```javascript
document.implementation.hasFeature('MutationEvents', '2.0');
```

包含如下列表的7个事件：

* DOMSubtreeModified
* DOMNodeInserted
* DOMNodeRemoved
* DOMNodeRemovedFromDocument
* DOMNodeInsertedIntoDocument
* DOMAttrModified
* DOMCharacterDataModified

_Removed事件在DOM操作完成之前触发，其他事件在之后触发。_

## 示例代码
```javascript
document.addEventListener("DOMNodeInserted", function(e) {
console.log(+new Date, e.target);
}, false);
```

在<http://www.haosou.com/>输入框测试，可以看到Suggest构造的script标签(jsonp请求)和li标签(显示Suggest Item)

## MutationObserver简介
observer.observe(target, options)
options 包含如下7个多选选项：

* childList `bool`
* attributes `bool`
* characterData `bool`
* subtree `bool`
* attributeOldValue `bool`
* characterDataOldValue `bool`
* attributeFilter `a list of attribute local names`

## MutationObserver[浏览器支持] [CanIUse]
* Chrome     >= 31
* IE         >= 11
* Firefox    >= 35
* iOS Safari >= 7.1
* AB >= 4.4
* AC >= 41

## 示例代码，参考[html5rocks] [html5rocks]
```javascript
var insertedNodes = [];
var observer = new MutationObserver(function(mutations) {
 mutations.forEach(function(mutation) {
   for (var i = 0; i < mutation.addedNodes.length; i++)
     insertedNodes.push(mutation.addedNodes[i]);
 })
});
observer.observe(document, { childList: true });
console.log(insertedNodes);
```

[MutationObserver]: http://www.w3.org/TR/dom/#mutationobserver
[MutationEvents]: http://www.w3.org/TR/DOM-Level-2-Events/events.html#Events-eventgroupings-mutationevents
[CanIUse]: http://caniuse.com/#search=MutationObserver
[html5rocks]: http://updates.html5rocks.com/2012/02/Detect-DOM-changes-with-Mutation-Observers
