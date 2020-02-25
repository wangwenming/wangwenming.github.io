"devDependencies": {
    "@vue/cli-plugin-babel": "^4.0.0",
    "babel-eslint": "^10.0.3",
}

├─┬ @vue/cli-plugin-babel@4.0.5
│ ├─┬ @babel/core@7.7.0
  │ │ ├── @babel/code-frame@7.5.5 用于生成错误信息，打印出错误点源代码帧以及指出出错位置    
  │ │ ├─┬ @babel/generator@7.7.0 根据AST生成代码
  │ │ │ ├── @babel/types@7.7.1 用于检验、构建和改变AST树的节点
  │ │ ├─┬ @babel/helpers@7.7.0 一系列预制的babel-template函数，用于提供给一些plugins使用
  │ │ │ ├── @babel/template@7.7.0 辅助函数，用于从字符串形式的代码来构建AST树节点


* babylon: ES6代码输入解析成 AST
* babel-traverse: 对 AST 树进行遍历转译，得到新的 AST
* babel-generator: 通过新的 AST 生成 ES5 代码

Babel 只是转译新标准引入的语法，比如 ES6 的箭头函数转译成 ES5 的函数；而新标准引入的新的原生对象，部分原生对象新增的原型方法，新增的API等（如Proxy、Set等），这些babel是不会转译的。需要用户自行引入 polyfill 来解决。


## presets
如果要自行配置转译过程中使用的各类插件，那太痛苦了，所以babel官方帮我们做了一些预设的插件集，称之为preset，这样我们只需要使用对应的preset就可以了。以JS标准为例，babel提供了如下的一些preset：

* es2015
* es2016
* es2017
* env

es20xx的preset只转译该年份批准的标准，而env则代指最新的标准，包括了latest和es20xx各年份
另外，还有 stage-0到stage-4的标准成形之前的各个阶段，这些都是实验版的preset，建议不要使用。

## polyfill
polyfill是一个针对 ES2015+ 环境的 shim ，实现上来说 babel-polyfill 包只是简单的把 core-js 和 regenerator runtime 包装了下，这两个包才是真正的实现代码所在。
使用babel-polyfill会把ES2015+环境整体引入到你的代码环境中，让你的代码可以直接使用新标准所引入的新原生对象，新API等，一般来说单独的应用和页面都可以这样使用。




babel-core
babel-loader
babel-plugin-transform-runtime
babel-preset-env
babel-preset-es2015
babel-preset-stage-2




开源库 zloirock/core-js 提供了es5、es6的polyfills，包括promises、symbols、collections、iterators、typed arrays、ECMAScript 7+ proposals、setImmediate 等等。
如果使用了 babel-runtime、babel-plugin-transform-runtime 或者 babel-polyfill，你就可以间接的引入了 core-js 标准库。

core-js@3.3.6


babel polyfill 有 3种：
* babel-runtime
* babel-plugin-transform-runtime 依赖babel-runtime
* babel-polyfill

## babel-runtime 和 babel-plugin-transform-runtime 的区别
babel-runtime和 babel-plugin-transform-runtime的区别是，相当一前者是手动挡而后者是自动挡，每当要转译一个api时都要手动加上require('babel-runtime')，

而babel-plugin-transform-runtime会由工具自动添加，主要的功能是为api提供沙箱的垫片方案，不会污染全局的api，因此适合用在第三方的开发产品中。

runtime转换器插件主要做了三件事：

* 当你使用generators/async方法、函数时自动调用babel-runtime/regenerator
* 当你使用ES6 的Map或者内置的东西时自动调用babel-runtime/core-js
* 移除内联babel helpers并替换使用babel-runtime/helpers来替换


babel-polyfill
babel-polyfill则是通过改写全局prototype的方式实现，比较适合单独运行的项目。

## 按 Webpack 官网说明：

> npm install -D babel-loader @babel/core @babel/preset-env webpack

在 webpack 配置对象中，需要将 babel-loader 添加到 module 列表中，就像下面这样：
```
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env'],
          // 
          plugins: ['@babel/plugin-transform-runtime']
        }
      }
    }
  ]
}
```

你可以引入 Babel runtime 作为一个独立模块，来避免重复引入。

> npm install -D @babel/plugin-transform-runtime
> npm install @babel/runtime # 把 @babel/runtime 安装为一个依赖






