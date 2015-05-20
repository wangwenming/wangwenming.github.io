## Definition

From: [Understanding JavaScript Promises][Understanding JavaScript Promises]

> A promise represents the eventual result of an asynchronous operation.
> It is a placeholder into which the successful result value or reason for failure will materialize.

## Promise States
1. Pending
2. Fulfilled
3. Rejected will never be fulfilled
一旦到后2个状态，状态就不会转移了。并且后2个状态，其value或reason都不能变化了。
Promise在很多语言中都有，JS使用的是Promises/A+建议标准(proposed standard)。
EcmaScript 6把Promise提升为一等公民(first-class language feature)。

Promises/A+就是使用`then()`方法来注册Fulfilled的value和Rejectd的Reason回调。

## Promises/A+规范描述摘抄
### 1. Terminology
1.1 `promise`包含了then()方法的对象或函数
1.2 `thenable`定义了then()方法的对象或函数(看起来`thenable`和`promise`傻傻分不清)
1.3 `value`可为任意JS值，包括：undefined, a thenable, or a promise
1.4 `exception`是用`throw`语句爬出的值
1.5 `reason`是一个表示为什么Rejected的值

### 2.1 Promise States
2.1.1 When pending, a promise:
    2.1.1.1 may transition to either the fulfilled or rejected state.
2.1.2 When fulfilled, a promise:
    2.1.2.1 must not transition to any other state.
    2.1.2.2 must have a value, which must not change.
2.1.3 When rejected, a promise:
    2.1.3.1 must not transition to any other state.
    2.1.3.2 must have a reason, which must not change.

### 2.2 then()方法
```
promise.then(onFulfilled, onRejected)
```
* onFulfilled 和 onRejected 都是 *after* promise is fulfilled/rejected，且最多只调用一次。
* 只能作为函数调用(i.e. with no this value. That is, in strict mode this will be undefined inside of them; in sloppy mode, it will be the global object.)。
* then may be called multiple times on the same promise. 所有的onFulfilled/onRejected按调用then()的先后顺序执行。
* then must return a promise
```promise2 = promise1.then(onFulfilled, onRejected);```
* onFulfilled/onRejected抛出异常e，promise必须以e为reason rejected


### Chaining
Promise/A+的一个重要的功能，就是链式调用then()，比如第一个then()返回一个普通值：
```
getJSON("/posts.json").then(function(json) {
  // 这里返回的value，会作为下一个then()的onFulfilled的value
  return json.post;
}).then(function(post) {
  // proceed
});
```
真正强大之处是返回一个promise，从而把嵌套的回调写法扁平化了。
这是promise展示其能力的用武之地，解决了大量异步调用时的右缩进("rightward drift")的问题。
```
getJSON("/post/1.json").then(function(post) {
  // save off post
  return getJSON(post.commentURL);
}).then(function(comments) {
  // proceed with access to post and comments
});
```
还有一个很大的优势是try/catch，在一个catch里处理所有阶段的异常。
```
getJSON("/posts.json").then(function(posts) {

}).catch(function(error) {
  // since no rejection handler was passed to the
  // first `.then`, the error propagates.
});
```

多个then()串联时，就是pipe，前一个then的输出，就是后一个then的输入。第一个then的输入是resolve值。


### Handling Errors


### Promise/A
jQuery 1.5 is said to be 'based on the CommonJS Promises/A design'


### jQuery 的 deferred
deferred 对象 promise()

```javascript
// $.Deferred() 是一个构造函数
// deferred对象包含方法
// then/done/fail/progress/always/promise/state/pipe(deprecated)
// resolve/resolveWith/reject/rejectWith/notify/notifyWith
// then()返回的Promise Object，其方法是Deferred Object的一个子集
// then/done/fail/progress/always/promise/state/pipe(deprecated)
// 目的是阻止用户修改Deferred的state属性(如调用resolve())
// 通常也会显示return deferred.promise 来防止外面调用 resolve() 等方法
var deferred = new $.Deferred();
// 或者这么写也可以，$.Deferred是一个factory function
// var deferred = $.Deferred();
var promise1 = deferred.then(function(result) {
    console.log('deferred.then 1 before resolve:', result);
    return 'new value returned by promise1 done';
});
var promise2 = deferred.then(function(result) {
    console.log('deferred.then 2 before resolve:', result);
    // deferred.done()/then() 默认返回 promise 对象，和无return一个效果
    // return this;
    // 如果是普通值，将传给promise的done()
    // return 'new';
    // 如果是一个新的observable object (Deferred, Promise)
    // 则等这个新对象的值，如返回 promise1
    // return promise1;
    // 或者一个全新的
    // return $.Deferred().resolve('new Deferred returned by promise1');
    // 没有 resolve() 就不执行
    return $.Deferred();
});
// 执行完下面语句，触发输出:
// deferred.then 1 before resolve: any value
// deferred.then 2 before resolve: any value
// 不传参则是 undefined
deferred.resolve('any value');
// 只能执行一次 resolve()，再执行会被忽略，没有任何效果
deferred.resolve('other value');
//执行完下面语句，触发输出:
// deferred.then after resolve: any value
deferred.then(function(result) {
    console.log('deferred.then after resolve:', result);
});
// resolve()执行完，且执行完下面语句，触发输出:
// promise1.then after resolve: undefined
promise1.then(function(result) {
    console.log('promise1.then after resolve:', result);
});
// promise2.then after resolve: any value
promise2.then(function(result) {
    console.log('promise2.then after resolve:', result);
});
promise2.then(function(result) {
    console.log('promise2.then after resolve:', result);
});
```

### Libraries
1. [Q][Q] works in node and the browser, 兼容jQuery
2. [RSVP.js][RSVP.js] 轻量级 `Promises/A+`. `es6 compliant` works in node and the browser (IE6+, all the popular evergreen ones). RSVP即使已经完成了，后续绑定的then还是能执行到。
法语的`répondez, s'il vous plaît`，表示`Please reply`
3. [es6-promise][es6-promise]
4. [when.js][when.js] Promises/A+
5. [WinJS][WinJS] http://try.buildwinjs.com/#promises


### 基于Promise的其他库
1. [task.js][task.js] 使用JS的yield操作符，是的写sequential, blocking I/O代码更简单漂亮。
2. 另外 Chai 和 Mocha 都有对应的 [mocha-as-promised][mocha-as-promised] 和 [chai-as-promised][chai-as-promised]

### 其他语言的Promise
1. [Android Deferred Object](https://github.com/CodeAndMagic/android-deferred-object)
2. Java [JDeferred](http://jdeferred.org/)
3. PHP [sabre/event 2.0](http://sabre.io/event/) 不过PHP其实不需要用Promise，也并不方便支持正真的Promise，除非用libevent。
4. PHP [React/Promise](https://github.com/reactphp/promise) A lightweight implementation of CommonJS Promises/A for PHP.

## Reference
1. [Understanding JavaScript Promises][Understanding JavaScript Promises]
2. [Promises/A+][Promises/A+]
3. [Promises/A][Promises/A]
4. [jQuery 1.5 deferred object][jQuery 1.5 deferred object]
5. [Q][Q]
6. [RSVP.js][RSVP.js]
7. [es6-promise][es6-promise] A polyfill for ES6-style Promises. [RSVP.js][RSVP.js]的子集
8. [task.js][task.js]
9. [when.js][when.js]
10. [WinJS][WinJS]
11. [Understanding JQuery.Deferred and Promise][Understanding JQuery.Deferred and Promise]


[Understanding JavaScript Promises]: https://spring.io/understanding/javascript-promises
[Promises/A+]: https://promisesaplus.com/
[Promises/A]: http://wiki.commonjs.org/wiki/Promises/A
[jQuery 1.5 deferred object]: http://api.jquery.com/category/deferred-object/
[Q]: https://github.com/kriskowal/q
[RSVP.js]: https://github.com/tildeio/rsvp.js
[es6-promise]: https://github.com/jakearchibald/es6-promise
[JavaScript Promises HTML5Rocks article]: http://www.html5rocks.com/en/tutorials/es6/promises/
[task.js]: http://taskjs.org/
[when.js]: https://github.com/cujojs/when
[mocha-as-promised]: https://github.com/domenic/mocha-as-promised
[chai-as-promised]: https://github.com/domenic/chai-as-promised
[WinJS]: https://github.com/winjs/winjs
[Understanding JQuery.Deferred and Promise]: http://joseoncode.com/2011/09/26/a-walkthrough-jquery-deferred-and-promise/
