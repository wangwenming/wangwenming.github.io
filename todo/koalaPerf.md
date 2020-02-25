### Resource Timing
```entryType: "resource"```的资源共12个时间戳，从前往后顺序排列如图片：

![Alt Image Text](resource-timing-overview-1.png "Optional Title")

3个可选值为```redirectStart ```, ```redirectEnd```, ```secureConnectionStart``` 。对于没有***302***和非***https***其值为0。

必须注意的是，虽然图中显示有一段时间 ```App cache``` ，但在 Chrome 中实测发现，即使是从磁盘加载(```200 OK (from disk cache)```)，```domainLookupStart = startTime```也成立(实际一样满足```connectEnd = startTime```)，***performace***记录的值像是一个极快的网络请求，和真实网络请求无法区别开来。

例如请求一个 5038 字节的资源 ***http://javascript.ruanyifeng.com/css/main.css***：

```
startTime: 0
...
connectEnd: 0
requestStart: 8.3 + 5.5 = 
responseStart: 13.8 + 2.6 = 
responseEnd: 16.4
```

对比从网络加载：


```
startTime: 0
...
connectEnd: 0
requestStart: 42.1 + 20.2 =
responseStart: 62.3 + 2.9 =
responseEnd: 65.2
```
无法放心的区分二者。

### ***PerformanceNavigationTiming***属性：
![Alt Image Text](navigation-timing-attributes.png "Optional Title")

对比资源加载，在后面新增4项 ***dom*** 事件和 2项 ***load*** 事件。


```
responseEnd: 0
domInteractive: 10390.5 + 0.04 = 
domContentLoadedEventStart: 10390.5 + 28.8 = 
domContentLoadedEventEnd: 10419.3 + 6343.8 = 
domComplete: 16763.1 + 0.03 = 
loadEventStart: 16763.1 + 39.8 = 
loadEventEnd: 16802.9
```

### 提炼关注指标(metrics)

* domainLookup = domainLookupEnd - domainLookupStart
* connect = connectEnd - connectStart
* waiting(ttfb: Time To First Byte) = responseStart - requestStart
* contentDownload = responseEnd - responseStart

* domInteractive = domInteractive - responseEnd # 下载和执行JS/CSS的时间
* domComplete = domComplete - domContentLoadedEventEnd # 下载图片的时间

> * ***domInteractive***：表示浏览器完成对所有 HTML 的解析并且 DOM 构建完成的时间点。
> * ***domContentLoaded***：表示 DOM 准备就绪并且没有样式表阻止 JavaScript 执行的时间点，这意味着现在我们可以构建渲染树了。
> * ***domComplete***：顾名思义，所有处理完成，并且网页上的所有资源（图像等）都已下载完毕，也就是说，加载转环已停止旋转。

评估关键渲染路径：

> HTML 规范中规定了每个事件的具体条件：应在何时触发、应满足什么条件等等。对我们而言，我们将重点放在与关键渲染路径有关的几个关键里程碑上：

> * ***domInteractive*** 表示 DOM 准备就绪的时间点。
> * ***domContentLoaded*** 一般表示 [DOM 和 CSSOM 均准备就绪](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/)的时间点。
	* 如果没有阻塞解析器的 JavaScript，则 DOMContentLoaded 将在 domInteractive 后立即触发。
> * ***domComplete*** 表示网页及其所有子资源都准备就绪的时间点。

	