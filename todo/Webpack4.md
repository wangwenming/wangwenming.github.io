# Webpack 4.12.0

## Webpack是啥
是一个静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

## 什么是 Webpack 模块
注意：最后一种也是模块

* ES2015 import 语句
* CommonJS require() 语句
* AMD define 和 require 语句
* css/sass/less 文件中的 @import 语句
* 样式```(url(...))```或 HTML 文件```(<img src=...>)```中的图片链接(image url)

## 配置文件
从 webpack v4.0.0 开始，可以不用引入一个配置文件。然而，webpack 仍然还是高度可配置的，其配置文件名是 ```webpack.config.js``` 。

### 入口(entry)
可以是一个或者多个。最简单的情况 ```entry``` 是一个字符串，则表示单入口。

### 出口
output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。

```
module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

### loader
loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。

在更高层面，在 webpack 的配置中 loader 有两个目标：

loader 甚至允许你直接在 JavaScript 模块中 import CSS文件！

### 插件(plugins)
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。


## NPM npm-run-script
 https://docs.npmjs.com/cli/run-script 

默认有：test, start, restart, and stop

-- 后面所有参数都会传给执行脚本
```
npm run test -- --grep="pattern"

```

npm run env 是一个内置命令，查看各种环境变量。

> In addition to the shell's pre-existing PATH, npm run adds node_modules/.bin to the PATH provided to scripts. 

package.json 中的 script 
  "scripts": {
    "dev": "webpack --env.NODE_ENV=common --progress --watch",
  },

执行 npm run dev ，会在 
node_modules/.bin/webpack -> ../webpack/bin/webpack.js
找到 webpack 命令。
然后 webpack 又自动找 webpack.config.js 。
