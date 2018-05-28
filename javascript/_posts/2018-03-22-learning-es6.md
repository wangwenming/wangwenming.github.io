### Constants
```js
const PI = 3.1415926
// ✖报错: Uncaught TypeError: Assignment to constant variable.
PI = 0

const ME = {age: 28}
// ✖报错: Uncaught TypeError: Assignment to constant variable.
ME = {age: 16};
```

`const`所在的代码块为块级作用域，所以其变量只在块级作用域内使用或其中的闭包使用：

```js
if(true) {
    const PI = 3.1415926
}
// ✖报错: Uncaught ReferenceError: PI is not defined
console.log(PI);
```

用ES5模拟：
```
//  only in ES5 through the help of object properties
//  and only in global context and not in a block scope
Object.defineProperty(typeof global === "object" ? global : window, "PI", {
    value:        3.141593,
    enumerable:   true,
    writable:     false,
    configurable: false
})
PI > 3.0;
```