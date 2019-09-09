# Learning Vue CLI

备注：此文档写于***2019-08-20***，Vue CLI版本号为***3.3.0***

## 简介

> Vue CLI 致力于将 Vue 生态中的***工具基础标准化***。它确保了各种构建工具能够基于智能的***默认配置***即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。

* CLI ```@vue/cli``` 提供了终端里的 ```vue``` 命令。
* CLI 服务 ```@vue/cli-service``` 是一个开发环境依赖。它是一个 npm 包，局部安装在每个 ```@vue/cli``` 创建的项目中。

	CLI 服务是构建于 [webpack](http://webpack.js.org/) 和 [webpack-dev-server](https://github.com/webpack/webpack-dev-server) 之上的。它包含了：
	* 加载其它 CLI 插件的核心服务；
	* 一个针对绝大部分应用优化过的内部的 webpack 配置；
	* 项目内部的 ```vue-cli-service``` 命令，提供 ```serve```、```build``` 和 ```inspect``` 命令。
* CLI 插件，例如 Babel/TypeScript 转译、ESLint 集成、单元测试和 end-to-end 测试等。Vue CLI 插件的名字以 ```@vue/cli-plugin-``` (内建插件) 或 ```vue-cli-plugin-``` (社区插件) 开头，非常容易使用。

当你在项目内部运行 ```vue-cli-service``` 命令时，它会自动解析并加载 ```package.json``` 中列出的所有 CLI 插件。

## 实战

```
$ vue create koala-perf-default-analytics-web
```
后目录结构如下，注意***没有Webpack配置***如```vue.config.js```

```
├── node_modules/
├── README.md
├── babel.config.js
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   └── index.html
└── src
    ├── App.vue
    ├── assets
    │   └── logo.png
    ├── components
    │   └── HelloWorld.vue
    └── main.js
```


***package.json***:

```
{
  "name": "koala-perf-default-analytics-web",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^2.6.5",
    "vue": "^2.6.10"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "^3.10.0",
    "@vue/cli-plugin-eslint": "^3.10.0",
    "@vue/cli-service": "^3.10.0",
    "babel-eslint": "^10.0.1",
    "eslint": "^5.16.0",
    "eslint-plugin-vue": "^5.0.0",
    "vue-template-compiler": "^2.6.10"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "rules": {},
    "parserOptions": {
      "parser": "babel-eslint"
    }
  },
  "postcss": {
    "plugins": {
      "autoprefixer": {}
    }
  },
  "browserslist": [
    "> 1%",
    "last 2 versions"
  ]
}
```

***main.js***

```
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```

```Vue.config.productionTip = false```设置为 false 以阻止 vue 在启动时生成生产提示。即控制台打印提示：

> You are running Vue in development mode.
> 
> Make sure to turn on production mode when deploying for production.
> 
> See more tips at [https://vuejs.org/guide/deployment.html](https://vuejs.org/guide/deployment.html)

***App.vue***

```
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

```vue-cli-service serve``` 命令会启动一个开发服务器 (基于 webpack-dev-server) 并附带开箱即用的模块热重载 (Hot-Module-Replacement)。

除了通过命令行参数，你也可以使用 ```vue.config.js``` 里的 [devServer](https://cli.vuejs.org/zh/config/#devserver) 字段配置开发服务器。

```vue-cli-service build``` 会在 ```dist/``` 目录产生一个可用于生产环境的包，带有 JS/CSS/HTML 的压缩，和为更好的缓存而做的自动的 vendor chunk splitting。它的 chunk manifest 会***内联***在 HTML 里。

生成的 ```index.html``` 格式化后如下：

```
<!DOCTYPE html>
<html lang=en>

<head>
    <meta charset=utf-8>
    <meta http-equiv=X-UA-Compatible content="IE=edge">
    <meta name=viewport content="width=device-width,initial-scale=1">
    <link rel=icon href=/favicon.ico>
    <title>koala-perf-default-analytics-web</title>
    <link href=/css/app.cba54c7b.css rel=preload as=style>
    <link href=/js/app.c704f050.js rel=preload as=script>
    <link href=/js/chunk-vendors.c70ab53f.js rel=preload as=script>
    <link href=/css/app.cba54c7b.css rel=stylesheet>
</head>

<body>
    <noscript><strong>We're sorry but koala-perf-default-analytics-web doesn't work properly without JavaScript enabled. Please enable it to continue.</strong></noscript>
    <div id=app></div>
    <script src=/js/chunk-vendors.c70ab53f.js></script>
    <script src=/js/app.c704f050.js></script>
</body>

</html>
```

默认生成如下3个文件：

```
File                                 Size               Gzipped
dist/js/chunk-vendors.c70ab53f.js    82.83 KiB          29.95 KiB
dist/js/app.89680c2c.js              4.60 KiB           1.65 KiB
dist/css/app.e2713bb0.css            0.33 KiB           0.23 KiB
```
修改 ***main.js*** 会导致 ***app.[hash].js*** 的 ***hash*** 变化，但是其余2个文件不变。

修改 ***App.vue*** 会导致 ***app.[hash].js*** 和 ***app.[hash].css*** 变化，但是 ***chunk-vendors*** 文件不变。

```
$ ./node_modules/.bin/vue-cli-service inspect --mode production
```

```webpack.config.js``` 文件重点内容摘要如下：

```json
{
  mode: 'production',
  context: '项目根目录，后续 CONTEXT 指的也是这个目录',
  devtool: 'source-map',
  output: {
    path: 'CONTEXT/dist',
    filename: 'js/[name].[contenthash:8].js',
    publicPath: '/',
    chunkFilename: 'js/[name].[contenthash:8].js'
  },
  resolve: {
    alias: {
      '@': 'CONTEXT/src',
      vue$: 'vue/dist/vue.runtime.esm.js'
    },
    modules: [
      'node_modules',
      'CONTEXT/node_modules',
      'CONTEXT/node_modules/@vue/cli-service/node_modules'
    ]
  },
  resolveLoader: {
    modules: [
      'CONTEXT/node_modules/@vue/cli-plugin-eslint/node_modules',
      'CONTEXT/node_modules/@vue/cli-plugin-babel/node_modules',
      'node_modules',
      'CONTEXT/node_modules',
      'CONTEXT/node_modules/@vue/cli-service/node_modules'
    ]
  },
  module: {
    noParse: /^(vue|vue-router|vuex|vuex-router-sync)$/,
    rules: [
      /* config.module.rule('vue') */
      {
        test: /\.vue$/,
        use: [
          /* config.module.rule('vue').use('cache-loader') */
          {
            loader: 'cache-loader',
            options: {
              cacheDirectory: 'CONTEXT/node_modules/.cache/vue-loader',
              cacheIdentifier: '70ce5665'
            }
          },
          /* config.module.rule('vue').use('vue-loader') */
          {
            loader: 'vue-loader',
            options: {
              compilerOptions: {
                preserveWhitespace: false
              },
              cacheDirectory: 'CONTEXT/node_modules/.cache/vue-loader',
              cacheIdentifier: '70ce5665'
            }
          }
        ]
      },
      /* config.module.rule('images') */
      /* config.module.rule('fonts') */
      /* config.module.rule('css') */
      /* config.module.rule('postcss') */
      /* config.module.rule('scss') */
      /* config.module.rule('sass') */
      /* config.module.rule('less') */
      /* config.module.rule('stylus') */
      /* config.module.rule('js') */
      /* config.module.rule('eslint') */
    ]
  },
  optimization: {
    minimizer: [
      {
        options: {
          test: /\.m?js(\?.*)?$/i,
          chunkFilter: () => true,  📦
          warningsFilter: () => true,  📦
          extractComments: false,  📦
          sourceMap: true, // Default: false ✏️
          cache: true, // Default: false ✏️
          cacheKeys: defaultCacheKeys => defaultCacheKeys,  📦
          parallel: true, // Default: false ✏️
          include: undefined,  📦
          exclude: undefined,  📦
          minify: undefined,  📦
          terserOptions: {
            output: {
              comments: /^\**!|@preserve|@license|@cc_on/i
            },
            compress: {
              arrows: false,
              collapse_vars: false,
              comparisons: false,
              computed_props: false,
              hoist_funs: false,
              hoist_props: false,
              hoist_vars: false,
              inline: false,
              loops: false,
              negate_iife: false,
              properties: false,
              reduce_funcs: false,
              reduce_vars: false,
              switches: false,
              toplevel: false,
              typeofs: false,
              booleans: true,
              if_return: true,
              sequences: true,
              unused: true,
              conditionals: true,
              dead_code: true,
              evaluate: true
            },
            // 混淆
            mangle: {
              safari10: true
            }
          }
        }
      }
    ],
    splitChunks: {
      cacheGroups: {
        vendors: {
          name: 'chunk-vendors',
          test: /[\\\/]node_modules[\\\/]/,
          priority: -10,
          chunks: 'initial'
        },
        common: {
          name: 'chunk-common',
          minChunks: 2,
          priority: -20,
          chunks: 'initial',
          reuseExistingChunk: true
        }
      }
    }
  },
  plugins: [
    /* config.plugin('vue-loader') */
    new VueLoaderPlugin(),
    /* config.plugin('define') */
    new DefinePlugin(
      {
        'process.env': {
          NODE_ENV: '"production"',
          BASE_URL: '"/"'
        }
      }
    ),
    /* config.plugin('case-sensitive-paths') */
    new CaseSensitivePathsPlugin(),
    /* config.plugin('friendly-errors') */
    new FriendlyErrorsWebpackPlugin(
      {
        additionalTransformers: [
          function () { /* omitted long function */ }
        ],
        additionalFormatters: [
          function () { /* omitted long function */ }
        ]
      }
    ),
    /* config.plugin('extract-css') */
    new MiniCssExtractPlugin(
      {
        filename: 'css/[name].[contenthash:8].css',
        chunkFilename: 'css/[name].[contenthash:8].css'
      }
    ),
    /* config.plugin('optimize-css') */
    new OptimizeCssnanoPlugin(
      {
        sourceMap: false,
        cssnanoOptions: {
          preset: [
            'default',
            {
              mergeLonghand: false,
              cssDeclarationSorter: false
            }
          ]
        }
      }
    ),
    /* config.plugin('hash-module-ids') */
    new HashedModuleIdsPlugin(
      {
        hashDigest: 'hex'
      }
    ),
    /* config.plugin('named-chunks') */
    new NamedChunksPlugin(
      function () { /* omitted long function */ }
    ),
    /* config.plugin('html') */
    new HtmlWebpackPlugin(
      {
        templateParameters: function () { /* omitted long function */ },
        minify: {
          removeComments: true,
          collapseWhitespace: true,
          removeAttributeQuotes: true,
          collapseBooleanAttributes: true,
          removeScriptTypeAttributes: true
        },
        template: 'CONTEXT/public/index.html'
      }
    ),
    /* config.plugin('preload') */
    new PreloadPlugin(
      {
        rel: 'preload',
        include: 'initial',
        fileBlacklist: [
          /\.map$/,
          /hot-update\.js$/
        ]
      }
    ),
    /* config.plugin('prefetch') */
    new PreloadPlugin(
      {
        rel: 'prefetch',
        include: 'asyncChunks'
      }
    ),
    /* config.plugin('copy') */
    new CopyWebpackPlugin(
      [
        {
          from: 'CONTEXT/public',
          to: 'CONTEXT/dist',
          toType: 'dir',
          ignore: [
            '.DS_Store'
          ]
        }
      ]
    )
  ],
  entry: {
    app: [
      './src/main.js'
    ]
  }
}
```

> ```cache-loader``` 会默认为 Vue/Babel/TypeScript 编译开启。文件会缓存在 ```node_modules/.cache``` 中——如果你遇到了编译方面的问题，记得先删掉缓存目录之后再试试看。


### 什么是“无需 Eject”
***“Eject”***概念来源于 ```react```，```create-react-app``` 把所有细节都隐藏起来了，项目的 ```package.json``` 就非常干净。但是如果要搞复杂的事情，则需要暴露这些细节，***Eject*** 到项目的 ```package.json``` 文件。有个 ```react-scripts eject``` 工具来做这个事。

### webpack 相关
> 调整 webpack 配置最简单的方式就是在 ```vue.config.js``` 中的 ```configureWebpack``` 选项提供一个对象。该对象将会被 [webpack-merge](https://github.com/survivejs/webpack-merge) 合并入最终的 webpack 配置。

> 如果你需要基于环境有条件地配置行为，或者想要直接修改配置，那就换成一个函数 (该函数会在环境变量被设置之后懒执行)。该方法的第一个参数会收到已经解析好的配置。在函数内，你可以直接修改配置，或者返回一个将会被合并的对象：

> ```
// vue.config.js
module.exports = {
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
    } else {
      // 为开发环境修改配置...
    }
  }
}
```

## References
* [Vue CLI-Vue.js 开发的标准工具][Vue CLI-Vue.js 开发的标准工具]

[Vue CLI-Vue.js 开发的标准工具]: https://cli.vuejs.org/zh/ "Vue CLI-Vue.js 开发的标准工具"