## 前言
参考文档[<sup>1. es5新特性总结</sup>][es5新特性总结]

    ECMAScript 5.1就是我们常说的es5。它在2012年就已经被公开。
    时至今日，除了一些较低版本的浏览器，各大主流浏览器都已经实现支持了es5的绝大部分特性。

按参考文档[<sup>2. Chapter 25. New in ECMAScript 5</sup>][Chapter 25. New in ECMAScript 5]摘录和翻译笔记如下：

## 1. New Features 新特性
### 1.1 Strict mode (严格模式)
在文件的首行，或者函数的开始，这样使用到不良的JS特性时会报错：
```
'use strict';
// a 未定义，使用严格模式可以发现这个问题
console.log(a);
```

### 1.2 Accessors (Getters and Setters)

可以通过方法重载属性的设置和获取。
有两种方式来定义：
方式一：*Defining Accessors via an Object Literal*
```
var obj = {
    get foo() {
        return 'getter';
    },
    set foo(value) {
        console.log('setter: '+value);
    }
};
```
方式二：*Defining Accessors via Property Descriptors*
```
var obj = Object.create(
    Object.prototype, {  // object with property descriptors
        foo: {  // property descriptor
            get: function () {
                return 'getter';
            },
            set: function (value) {
                console.log('setter: '+value);
            }
        }
    }
);
```

Getters/setters 可通过 prototypes 继承。
这个特性是Vue的生存之基础。

## 2. Syntactic Changes 语法变化

### 2.1 Reserved words as property keys (保留字可以不括起来使用了)，例如
```
var obj = { new: 'abc' };
obj.new
```

### 2.2 Legal trailing commas
印象中别的浏览器一直支持此特性，IE6则会报错。

### 2.3 Multiline string literals (多行字符串)
\每行结尾可以定义多行字符串

## 3. New Functionality in the Standard Library

### 3.1 Metaprogramming 元编程

插入下什么是元编程，例如：
```
#!/bin/bash  
# metaprogram  
echo '#!/bin/bash' >program  
for ((I=1; I<=992; I++)) do 
echo "echo $I" >>program  
done  
chmod +x program 
```

当然看完这个，还是没理解JS的这些特性以及元编程，按MDN的说法，是ES6的 `Proxy` 和 `Reflect` 才使得 JS 可以进行元编程，这是后话。

Property Attributes(属性的特性) and Property Descriptors(属性描述符)这项特性应该是参考的JAVA。
属性描述符有3种：
1. 数据属性描述符(Data Descriptor): Value Writable
2. 访问器属性描述符(Accessor Descriptor): Get Set
3. 一般属性描述符(Generic Descriptor) Enumerable Configurable

✎ Getting and setting prototypes (see Getting and Setting the Prototype):
```
Object.create(proto[, propertiesObject]) // Setting prototypes
Object.getPrototypeOf(obj) // Getting prototypes
```

✎ Managing property attributes via property descriptors (see Property Descriptors):
```
Object.defineProperty(obj, prop, descriptor)
Object.defineProperties(obj, props)
Object.create(proto[, propertiesObject])
Object.getOwnPropertyDescriptor(obj, prop)
Object.getOwnPropertyDescriptors(obj)
```

✎ Listing properties:
```
Object.keys(obj)
Object.getOwnPropertyNames(obj)
```

✎ Protecting objects
```js
Object.preventExtensions(obj) // 只能删除属性
Object.isExtensible(obj)
Object.seal(obj) // 封印：*configurable => false* 值可以修改，attributes 无法修改
Object.isSealed(obj)
Object.freeze(obj) // 值和 attributes 都不能修改了
Object.isFrozen(obj)
```

Pitfall: Protection Is Shallow
```
var obj = {
    foo: 1,
    bar: ['a', 'b']
};
Object.freeze(obj);
obj.bar.push('c'); // changes obj.bar
```

✎ New Function method:
```
Function.prototype.bind()
```

应用举例：copy object
```
function copyObject(orig) {
    // 1. copy has same prototype as orig
    var copy = Object.create(Object.getPrototypeOf(orig));

    // 2. copy has all of orig’s properties
    copyOwnPropertiesFrom(copy, orig);

    return copy;
}

function copyOwnPropertiesFrom(target, source) {
    Object.getOwnPropertyNames(source)  // (1)
    .forEach(function(propKey) {  // (2)
        var desc = Object.getOwnPropertyDescriptor(source, propKey); // (3)
        Object.defineProperty(target, propKey, desc);  // (4)
    });
    return target;
};
```

### 3.2 New Methods

Strings:
```
String.prototype.trim()
Access characters via the bracket operator [...]
```

(9) Array methods:
```
Array.isArray()
Array.prototype.every()
Array.prototype.filter()
Array.prototype.forEach()
Array.prototype.indexOf()
Array.prototype.lastIndexOf()
Array.prototype.map()
Array.prototype.reduce()
Array.prototype.some()
```

New Date methods:
```javascript
Date.now()
Date.prototype.toISOString() // 2018-03-20T10:08:31.531Z
```

### 3.3 JSON
```js
JSON.parse()
JSON.stringify()
// Some built-in objects have special toJSON() methods
// 试了一下，Chrome 只支持 Date 的，其他并不支持啊？
Boolean.prototype.toJSON()
Number.prototype.toJSON()
String.prototype.toJSON()
// 输入结果如：2018-03-12T08:33:07.261Z
Date.prototype.toJSON()
```

### Reference
1. [es5新特性总结][es5新特性总结]
2. [Chapter 25. New in ECMAScript 5][Chapter 25. New in ECMAScript 5] 
3. [Object.defineProperty()][Object.defineProperty()]
4. [Meta programming][Meta programming]

[es5新特性总结]: https://www.jianshu.com/p/38b0c4666338 "es5新特性总结"
[Chapter 25. New in ECMAScript 5]: http://speakingjs.com/es5/ch25.html "Chapter 25. New in ECMAScript 5"
[Object.defineProperty()]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty "Object.defineProperty()"
[Meta programming]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Meta_programming "Meta programming"
