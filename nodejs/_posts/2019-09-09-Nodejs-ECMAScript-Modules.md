# Node.js ECMAScript Modules

Node.js现在使用的是 [CommonJS](https://nodejs.org/api/modules.html) 模块格式。

## CommonJS

```require()``` ```exports``` ```module.exports``` 。

```exports.``` 用于把 ```exports``` 当做一个对象。
```module.exports``` 可以重新被赋值为函数或对象。

```module``` 是一个对象，如：

```
Module {
  id: '.',
  path: '/p/a/t/h/<rootDir>',
  exports: {},
  parent: null,
  filename: '/p/a/t/h/<rootDir>/main.cjs',
  loaded: false,
  children: [],
  paths: [
    '/p/a/t/h/<rootDir>/node_modules',
    '/p/a/t/h/node_modules',
    '/p/a/t/node_modules',
    '/p/a/node_modules',
    '/p/node_modules',
    '/node_modules'
  ]
}
```

```
console.log(require.main === module) // true
console.log(module.exports === exports) // true
```

```require``` 是一个函数，包含 ```resolve``` ```main``` ```extensions``` ```cache``` 等属性。
对于入口函数：```require.main === module``` 。

ES模块(ECMAScript modules)是官方标准。模块由 ```import``` 和 ```export``` 语句组成。需要使用 ```--experimental-modules``` 开启。

Node.js在以下情况会当做ES模块加载：

* 文件名后缀 ```.mjs```
* 文件名后缀 ```.js``` 但是最近(***the nearest parent*** package.json)的 ```package.json``` 包含一个根级属性 ```"type": "module"``` 
* ES模块 ```import``` 的文件

require()

module.exports 或者 exports.