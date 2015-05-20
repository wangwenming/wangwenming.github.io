## Iterable protocol
for .. of 结构。
```
var arr = [1, 2, 3];
for (val of arr) {console.log(val);}
```
NodeJS 0.11 开始支持 --harmony_generators https://github.com/joyent/node/wiki/ES6-(a.k.a.-Harmony)-Features-Implemented-in-V8-and-Available-in-Node

## Iterator protocol

##
`function* (expression)`声明定义*generator function*。
其返回值在是一个函数，构造函数是`GeneratorFunction`函数。

## yield operator
yield是一个单元操作符，只能用在*generator function*中，使得函数暂停，并返回表达式的值。
可看成是generator版本的return。
返回的是一个 `IteratorResult` 对象，包含 value 和 done 两个值。

暂停到(外部)调用generator的next()，才会继续执行。从而实现逐步执行。

```
function fibonacci(n) {
    if (n === 0) {
        return 0;
    }

    if (n === 1) {
        reurn 1;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

```
function* fibonacci(n) {
    var [prev, curr] = [0, 1];
    while (i <= n) {
        i++;
        [prev, curr] = [curr, prev + curr];
        yield curr;
    }
}

var fibonacci10 = fibonacci(10);
var value;
while (value = fibonacci10.next().value) {
    console.log(value);
}
```

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield
