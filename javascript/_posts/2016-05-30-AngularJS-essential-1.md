## 最好的学习资料和最坏的学习资料
1. [最好的学习资料](https:/docs.angularjs.org/) [AngularJS-Learning](https://github.com/jmcunningham/AngularJS-Learning)
2. [最坏的学习资料](https://www.baidu.com/) [不太好的学习资料](http://docs.angularjs.cn/) 号称“angularjs中文社区”，但是在翻译方面做的并不好，发起者叫破狼，在其[博客](http://greengerong.com/)写的关于AngularJS的文章倒是不错，并合著了一本书[《AngularJS深度剖析与最佳实践》](https://book.douban.com/subject/26708133/)。另一本推荐数据是[《精通AngularJS》/《Mastering Web Application Development with AngularJS》](https://book.douban.com/subject/26022847/)。

> 备注：angularjs.org本身没有被墙，但是其引用到的 `https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js` 被墙了。
> 
> 参考 [原 ajax.googleapis.com等公共库加载被“墙”的解决方法！](http://blog.csdn.net/xufei512/article/details/50323243) 访问官网。

## 文档分类
1. Tutorial (PhoneCat) 入门教程 [官网英文版](https://docs.angularjs.org/tutorial/) [xdsnet翻译中文版](https://xdsnet.gitbooks.io/angular-phonecat-book-zhcn/content/chapter0/chapter0.html)
2. Developer Guide 进阶教程 [官网英文版](https://docs.angularjs.org/guide) [中文版/ng1.2.18](http://docs.ngnice.com/guide) [中文版(更新于 2014年8月9日)](https://github.com/jingyanjiaoliu/angular-guide-zh)
3. API Reference API文档，目前尚无中文版

> 备注: 最好的方式是使用 [API文档浏览器和代码片段管理器(API Documentation Browser and Code Snippet Manager)Dash](https://kapeli.com/dash) 来阅读文档。当然，再一次，官方API无中文版。

## There is no silver bullet
### 库和框架的区别
1. 库(library): jQuery
2. 框架(frameworks): AngularJS

### 适用场景
abstraction 程度越高， flexibility越低。不是所有 app 适合用 AngularJS 。<br>
适用于 CRUD 。
游戏和编辑器大量操作DOM，非 CRUD app，则更适合直接使用 jQuery 。

### The Zen of Angular
* `declarative code` > `imperative code`
* 更适合构建UI（building UIs）和关联组件（wiring software components together），`imperative code`更适合表达业务逻辑

> Manipulating HTML DOM programmatically: Manipulating HTML DOM is a cornerstone of AJAX applications, but it's cumbersome and error-prone. 
> 
> By declaratively describing how the UI should change as your application state changes, you are freed from low-level DOM manipulation tasks.
> 
> Most applications written with Angular never have to programmatically manipulate the DOM, although you can if you want to.

## Things I Wish I Were Told About Angular.js

* 学习曲线 (Learning Curve)，入门简单，因为入门教程很好；深入难，因为文档太衰；相对的，Backbone.js 需要先学习很多知识，才能做一个应用；当你学完之后，发现 Backbone.js 除了一些常用的构建工具和最佳实践外，其实啥也没做。
* 奇怪的 view 不刷新的问题，需要用到 $scope.$apply(); 来解决
* 切记不要在 Controller 里操作 DOM ，即使有时候需要用到 jQuery 插件，请先封装成 Directive 。
* Angular Router 不好使，我们都用 angular-ui-router


## 参考文档
1. [Things I Wish I Were Told About Angular.js](http://ruoyusun.com/2013/05/25/things-i-wish-i-were-told-about-angular-js.html)
2. [Dash：程序员的的好帮手](http://blog.csdn.net/meegomeego/article/details/8798665)
3. [A Better Way to Learn AngularJS](https://thinkster.io/a-better-way-to-learn-angularjs) 

