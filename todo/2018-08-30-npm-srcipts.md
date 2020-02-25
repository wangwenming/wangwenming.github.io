2018-08-30-npm-run-srcipts.md

## 语法
```
npm run-script <command> [--silent] [-- <args>...]
```

其实 ```npm run``` 是 ```npm run-script``` 的别名。

## 用法
### 列举所有可用脚本，类似 help
```
npm run
```

### 4个不需要 ```run``` 的特殊命令

* test
* start
* restart
* stop

可以省略 ```run```

### 传参
> As of npm@2.0.0, you can use custom arguments when executing scripts. The special option -- is used by getopt to delimit the end of the options. npm will pass all the arguments after the -- directly to your script:

例如命令行 ```npm run dev -- --local-crm``` 可以在 Webpack 里使用：
```
let isLocalCrm = process.argv.indexOf('--local-crm') !== -1;
```

### 关于 *PATH* 环境变量
/usr/local/lib/node_modules/npm/node_modules/npm-lifecycle/node-gyp-bin:
 ```npm run``` 会自动添加 ```node_modules/.bin``` 到 *PATH* 环境变量。
这样就可以在 package.json 直接使用 ```webpack``` 和 ```webpack-dev-server``` 命令，因为 ```node_modules/.bin``` 有这些命令：
```
node_modules/.bin/webpack -> ../webpack/bin/webpack.js
node_modules/.bin/webpack-dev-server -> ../webpack-dev-server/bin/webpack-dev-server.js
```

### ```webpack``` 的 ```mode``` 参数
> Providing the mode configuration option tells webpack to use its built-in optimizations accordingly.

> Possible values for mode are: *none*, *development* or *production*(default).

会设置 ```DefinePlugin``` 的 ```process.env.NODE_ENV``` 为 ```mode``` 的值。

不需要手动设置 ```DefinePlugin```
```
// webpack.development.config.js
module.exports = {
+ mode: 'development'
- devtool: 'eval',
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.NamedChunksPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}
```

### 关于 ```webpack``` 引用外部项目链接的问题
因为我们的项目发布到不同机器，所以加了一个 *SERVER* 变量。
因为 *NODE_ENV* 已经被 *mode* 占用了，除了 *NODE_ENV* 这个变量，别的用啥都行，

### 理解 ```DefinePlugin``` 的 *compile* time
在 *Webpack* 官网：
> The ```DefinePlugin``` allows you to create global constants which can be configured at *compile* time. 

其中 *compile* 加粗了。意思是在 *webpack.config.js* 文件是无法使用这个变量，但是在 *src* 源码里是可以使用的，相当于做了一次常量替换。

https://webpack.js.org/concepts/mode/

https://webpack.js.org/plugins/define-plugin/

https://tomasalabes.me/blog/web-development/2017/01/03/Useful-Webpack-Define-Plugin-Usages.html


