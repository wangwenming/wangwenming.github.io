### `__proto__` 属性

>Warning: While `Object.prototype.__proto__` *is supported today in most browsers*, its existence and exact behavior has only been standardized in the ECMAScript 2015 specification as a legacy feature to ensure compatibility for web browsers. For better support, it is recommended that only `Object.getPrototypeOf()` be used instead.


`__proto__` 属性是一个对象或者 `null` ，V8、Spidermonkey、Rhino等都支持，属于 `widely supported non-standard ` 。

代码示例：
```js
function Person() {}
var p1 = new Person();
var p2 = new Person();

p1.__proto__ // 一个新创建的对象
p1.__proto__ === p2.__proto__ // true

// true
p2.__proto__.__proto__ === new Object().__proto__ === Object.prototype

// true
new Object().constructor === Object

var p3 = Object.create(null, {});
p3.__proto__ // null
var p4 = Object.create({name: 1}, {});
p4.__proto__ // {name: 1}
```


Object.prototype.__proto__


https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto
https://stackoverflow.com/questions/3830800/object-defineproperty-in-es5

