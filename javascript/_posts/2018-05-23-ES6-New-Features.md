# Top 10 ES6 Features Every Busy JavaScript Developer Must Know

## 1. Default Parameters 默认参数
这个最简单了，比如臭名昭著的NPM包 [pad-left](https://npm.taobao.org/package/pad-left)，在 PHP 里面有类似的：
```php
string str_pad ( string $input , int $pad_length [, string $pad_string = " " [, int $pad_type = STR_PAD_RIGHT ]] )
```
其中后2个都有默认参数。

JS的 pad-left 如果加上默认参数，可读性和可用性是不是高很多：
```js
function leftPad (str, len, ch = ' ')
```

## 2. Template Literals 模板文本

```js
var goddess = ['Dilraba', 'Gulnazar']
var gf = {name: 'Dilraba', birthday: '1992-06-03', astrology: '双子'}
console.log(`goddess are ${goddess[0]} and ${goddess[1]}.`, `gf's birthday is ${gf.birthday}`)
```

来自 *CoffeeScript* ，和 *PHP* 的也很像吧，只是必须包含在 ```${}``` ， *PHP* 可以省略 ```{}``` ，当然还是 *ES2015* 这种“茴字只有一种写法”比较好。

## 3. Multi-line Strings
```
var fourAgreements = `You have the right to be you.
    You can only be you when you do your best.`
```

***✎点评***：不需要在行末尾加 ```\``` 了，但是注意第二行前面的4个空格，真的就是字符串里面的4个空格字符！多行字符串特性，我在别的语言里也很少用，因为不能缩进,影响代码美观和可读性，建议用模板文件。

## 4. Destructuring Assignment 解构赋值
```js
var goddess = ['Dilraba', 'Gulnazar']
var gf = {name: 'Dilraba', birthday: '1992-06-03', astrology: '双子'}
// 对象
var {name, birthday} = gf
// 数组
var [goddess1, goddess2] = goddess
```

比 *PHP* 的 [list](http://php.net/list) 优雅多了。

## 5. Enhanced Object Literals 增强的对象文本
```js
var name = 'Dilraba'
var birthday = '1992-06-03'
var gf = {
    name,
    birthday,
    astrology: '金牛',
    toString() {
        return JSON.stringify(this.valueOf())
    }
}
```

## 6. Arrow Functions 箭头函数
参考 *CoffeeScript* 。语法：
```js
// var self = this
$('body').click((event) => {
  // this 还是闭包外面的 
  console.log(this === self) // true
  // 有了箭头函数，就可以避免大量的 self = this 了
  this.sendData()
})
```

```js
var ids = ['5632953c4e345e145fdf2df8', '563295464e345e145fdf2df9']
var messages = ids.map((value, index, list) => `ID of ${index} element is ${value} `) // implicit return
```

## 7. Promises
已经有很多语法稍有差别的 ```q```, ```bluebird```, ```deferred.js```, ```vow```, ```avow```, ```jquery deferred```。还可以用 ```async```, ```generators```, ```callbacks``` 来代替 ```Promise``` 。
```js
var wait1000 =  new Promise((resolve, reject)=> {
  setTimeout(resolve, 1000)
}).then(()=> {
  console.log('Yay!')
})
```

## 8. Block-Scoped Constructs Let and Const 块作用域构造Let 和 Const
```js
// 块作用域，{} 包含
let amount = 1
// immutable, also block-scoped
const amount = 1
```

***✎点评***：*let* 让事情变复杂了，在一个 *function* 作用域内，若出现名字一样，实际指向内存中不同变量，难道不是一个很反智的行为吗？ *let* 根本解决不了这个问题，避免这种情况才是上策。

## 9. Classes
```js
class Goddess {
    constructor(name) {
        this.name = name
    }
    
    getName() {
        return this.name
    }
}

class Gf extends Goddess {
    constructor(name, birthday, astrology) {
        super(name)
        this.birthday = birthday
        this.astrology = astrology
    }
    
    /*
    set name(name) {
        this.name = name
    }

    get name() {
        return `Miss ${this.name}`
    }
    */

    getIntro() {
        return `${this.name}: Birthday/${this.birthday}; Astrology/${this.astrology}`
    } 
}

var goddess = [new Goddess('Dilraba'), new Goddess('Gulnazar')]
var gf = new Gf(goddess[0].name, '1992-06-03', '双子')
console.log(gf.getIntro())
```

## 10. Modules 模块
大家以前使用 ```AMD```, *RequireJS*, *CommonJS* (*Node.js*支持) 等等。*ES2015* 引入了 *import* 和 *export* 操作符。

> There is exactly one module per file and one file per module.

```index.html```
```html
<script src="module1.js" type="module"></script>
<script src="main.js" type="module"></script>
```

```module1.js```:
```
export var port = 3000
export function getUrl() {
  return `http://localhost:${port}`
}
```

```mian.js```: 
```js
import {port, getUrl} from './module1.js'

console.log(port, getUrl())
```

## References
1. 2015-11-10 [Top 10 ES6 Features Every Busy JavaScript Developer Must Know](https://webapplog.com/es6/)
