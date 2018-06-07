# FastClick原理简析

## 300ms延迟的原因

为了支持 `double tap` 缩放或滚动，因此设计成间隔 ***300ms*** 来检查是否有第二次 tap

## 现在是否还需要

简而言之：***不需要***。

官网描述：
> Note: As of late 2015 most mobile browsers - notably Chrome and Safari - no longer have a 300ms touch delay, so fastclick offers no benefit on newer browsers, and risks introducing bugs into your application. Consider carefully whether you really need to use it.

其原因如下：

1. 桌面浏览器完全不用
2. 现在移动页面几乎都设置了 `width=device-width` ，在 *Chrome 32+ on Android* 不再支持双击放大（用户可以用 *pinch zoom* 放大）
3. 移动页面如果页面设置了 `user-scalable=no` ，在 *Chrome on Android (all versions)* 不再支持双击放大（用户【不】可以用 *pinch zoom* 放大）
4. 对于情况2、3，浏览器已取消 300ms 延迟，而移动页面都做到了2，浏览器只对 PC  z站点继续有 300ms 延迟

## 用法

通常绑定在 `body` 元素：

```
if ('addEventListener' in document) {
  document.addEventListener('DOMContentLoaded', function() {
    FastClick.attach(document.body);
  }, false);
}
```

## 简要实现原理描述

1. touchstart 事件: 对于单点触摸事件，记录下时间、touchStartX、touchStartY
2. touchmove 事件: X或Y方向任意一个移动达到一个值（默认10px）则放弃触发 click
3. touchend 事件:
  如果时间超过一个值（默认 700ms），则放弃触发 click
  通过 `document.createEvent('MouseEvents')` 合成（Synthesise）click事件并通过 `targetElement.dispatchEvent(clickEvent);` 触发

***注意：***
完整流程比这要复杂的多，前端兼容性大家都懂的，这只是主要流程的简单描述

## References
[ftlabs/fastclick](https://github.com/ftlabs/fastclick)