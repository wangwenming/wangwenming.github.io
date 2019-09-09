## runtimeChunk

### 官方文档阅读笔记

> Setting ```optimization.runtimeChunk``` to ```true``` or ```'multiple'``` adds an additional chunk to each entrypoint containing only the runtime. This setting is an alias for:

> ```
module.exports = {
  //...
  optimization: {
    runtimeChunk: {
      name: entrypoint => `runtime~${entrypoint.name}`
    }
  }
};
```

给每一个 ***entrypoint*** 都增加了一个额外的 ***chunk***，包含 ***runtime*** 。

> The value ```'single'``` instead creates a runtime file to be shared for all generated chunks. This setting is an alias for:

> ```
module.exports = {
  //...
  optimization: {
    runtimeChunk: {
      name: 'runtime'
    }
  }
};
```

设置为 ```'single'``` 则所有块文件共享一个运行时文件。

> Default is ```false```: each entry chunk embeds runtime.

默认每个块文件内置运行时。

### 实战解释

***vue.config.js***

```
module.exports = {
  configureWebpack: {
    optimization: {
      runtimeChunk: true
    }
  }
}
```

build后：

```
  File                                 Size               Gzipped

  dist/js/chunk-vendors.c70ab53f.js    82.83 KiB          29.95 KiB
  dist/js/app.fdd7a5cb.js              3.25 KiB           1.05 KiB
  dist/js/runtime~app.393ee8dc.js      1.49 KiB           0.75 KiB
```

```runtime~app.393ee8dc.js``` 文件只有 ```1.49KB``` (不 uglify 是 6.10KB, 154行) 。定义了 ```__webpack_require__```函数。
在 ```index.html``` 引入代码如下：

```
<script src=/js/runtime~app.393ee8dc.js></script>
<script src=/js/chunk-vendors.c70ab53f.js></script>
<script src=/js/app.fdd7a5cb.js></script>
```

### 什么是 runtime 和 manifest

> 在使用 webpack 构建的典型应用程序或站点中，有三种主要的代码类型：

> 1. 你或你的团队编写的源码。
> 2. 你的源码会依赖的任何第三方的 library 或 "vendor" 代码。
> 3. webpack 的 runtime 和 manifest，管理所有模块的交互。

***rumtime*** 官方定义如下：

> runtime，以及伴随的 manifest 数据，主要是指：在浏览器运行过程中，webpack 用来连接模块化应用程序所需的所有代码。它包含：在模块交互时，连接模块所需的加载和解析逻辑。包括：已经加载到浏览器中的连接模块逻辑，以及尚未加载模块的延迟加载逻辑。


生成了 ```dist/manifest.json``` 文件，内容为：

```
{
  "app.css": "/css/app.cba54c7b.css",
  "app.js": "/js/app.cf85ea4f.js",
  "app.css.map": "/css/app.cba54c7b.css.map",
  "app.js.map": "/js/app.cf85ea4f.js.map",
  "chunk-vendors.js": "/js/chunk-vendors.7ab25cc5.js",
  "chunk-vendors.js.map": "/js/chunk-vendors.7ab25cc5.js.map",
  "runtime~app.js": "/js/runtime~app.d110ae0b.js",
  "runtime~app.js.map": "/js/runtime~app.d110ae0b.js.map",
  "favicon.ico": "/favicon.ico",
  "img/logo.png": "/img/logo.82b9c7a5.png",
  "index.html": "/index.html"
}
```

在 

```
// object to store loaded and loading chunks
// undefined = chunk not loaded, null = chunk preloaded/prefetched
// Promise = chunk loading, 0 = chunk loaded
var installedChunks = {
    "runtime~app": 0
};
```

### 什么是 chunk

> Typically, chunks directly correspond with the output bundles however, there are some configurations that don't yield a one-to-one relationship. <sup>[glossary - c](https://webpack.js.org/glossary/#c)</sup>
> 
> 

### splitChunks.cacheGroups

https://webpack.js.org/plugins/split-chunks-plugin/#splitchunkscachegroups


优先级：

```
maxInitialRequest/maxAsyncRequests < maxSize < minSize.
```

默认配置：

```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async', 
      minSize: 30000,
      maxSize: 0,
      minChunks: 1,
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      automaticNameDelimiter: '~',
      name: true,
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true
        }
      }
    }
  }
}
```

### 实战

node_modules/vue/dist/vue.runtime.esm.js 222K

查看 ```.map``` 文件可以清晰的知道每个 ```chunk``` 包含的 ```module``` 。 

```
dist/js/chunk-vendors~21380ae4.67ddb868.js 34K 63个core-js文件
dist/js/chunk-vendors~d2305125.c7c10ba1.js 200K 只有一个 vue： node_modules/vue/dist/vue.runtime.esm.js
dist/js/chunk-vendors~d939e436.eab7793d.js 16K 6个core-js文件
```
