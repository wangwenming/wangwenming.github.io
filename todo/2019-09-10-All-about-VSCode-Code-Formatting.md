# All about VSCode Code Formatting

## 右键菜单 ```Format Document ⌥⇧F``` ， 到底是调用的那个工具的功能呢？

### 命令行配置和使用 ```ESLint``` 工具

官网地址：[https://eslint.org/docs/user-guide/configuring](https://eslint.org/docs/user-guide/configuring)

配置文档支持如下文件（同时存在按如下顺序）：

1. ```js``` => ```.eslintrc.js```
2. ```YAML``` => ```.eslintrc.yaml``` 或 ```.eslintrc.yml```
3. ```json``` => ```.eslintrc.json```
4. Deprecated ```.eslintrc``` JSON 或者 YAML
5. ```package.json``` => ```eslintConfig```

对于 ```Vue``` ，需在 ```package.json``` 添加开发依赖：```eslint``` 和 ```eslint-plugin-vue``` ，然后在命令行运行：

```./node_modules/.bin/eslint src/views/login/index.vue```

### ```ESLint``` 的 ```Formatters```

```ESLint``` 内部了几个 ```Formatters``` ，同时支持第三方 ```Formatters``` 。

### eslint-loader - ESLint Build tools for Webpack


### eslint-plugin-vue - Official ESLint plugin for Vue.js

### ESLint 配置只 extends

eslint:recommended

## VSCode 扩展

### 扩展1: ESLint

这个插件优先寻找项目中的 ESLint library ，如果没有则找全局的。***不会自动安装***。

例如我的 ```User Settings``` 全局配置文件 (```⌘,```可打开可视化编辑界面，可点击 ```Edit in settings.json``` 打开 ```~/Library/Application Support/Code/User/settings.json``` 全局配置文件)，部分相关代码配置如下：

```
{
    "eslint.autoFixOnSave": true,
    "files.autoSave": "off",
    "eslint.validate": [
        "javascript",
        "html",
        "vue",
        // 在html和vue中自动保存成eslint风格的代码
        {
            "language": "html",
            "autoFix": true
        },
        {
            "language": "vue",
            "autoFix": true
        }
    ],
    "eslint.alwaysShowStatus": true
}
```
### x

如果 ```.eslintrc.js``` 配置 ```extends: ['eslint:recommended']``` ，那么是不支持 ```.vue``` 文件的，如果命令行运行会当做 ```.js``` 文件。

这时候即使设置了(这些是 VSCode 插件的配置项，非 ESLint ，和 ```.eslintrc.js``` 没关系的)

```
"eslint.autoFixOnSave": true
```

也无效。

运行VSCode命令 ESLint: Fix all auto-fixable Problems 有无效。
对 .js 文件管用！

应该是没装插件，根本不认识配置
```
    "eslint.validate": [
        {
            "language": "vue",
            "autoFix": true
        }
    ],
```

但是命令行指定后，会找打 ```<script>``` 并按 ```js``` 来。

想支持 请安装 ```Vetur``` ，然后出错了就会标红，保存能自动修复啦！！！

### 扩展2: Vetur - Vue tooling for VS Code

这个是 VSCode 官方团队工程师 Pine Wu 出品的。和 ***ESLint*** 插件集成了。
按官方文档，安装完 ***ESLint*** 后，给 ***eslint.validate*** 添加 ***vue*** 支持：

```
{
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "vue",
      "autoFix": true
    },
  ]
}
```
配置成功后，***ESLint*** 可以格式化 ```<template>``` 和 ```<script>``` 了。
```Vetur``` 内置了 ```eslint-plugin-vue``` 来Lint ```<template>``` ，规则基于 ```eslint-plugin-vue``` 的 [essential rule set](https://vuejs.github.io/eslint-plugin-vue/rules/#priority-a-essential-error-prevention)。

以上 Lint 是不可以配置的，如果要配置，请安装 ```扩展1: ESLint``` ，并关闭 ```Vetur``` 对 ```<template>``` 的 Lint 功能：```vetur.validation.template: false``` 。还要在项目中安装 ```eslint``` 和 ```eslint-plugin-vue``` 。

* Syntax-highlighting
* Snippet
* Emmet
* Linting / Error Checking
* Formatting
* IntelliSense
* Debugging
* Framework Support for Element UI, Onsen UI and Quasar Framework

### eslint-plugin-vue

提供了 A~B 4个级别：

```
Priority A: Essential (Error Prevention) => plugin:vue/essential
Priority B: Strongly Recommended (Improving Readability) => plugin:vue/strongly-recommended
```

如果不安装 


### eslint-config-standard 839,503 weekly downloads


```
This module is for advanced users. You probably want to use standard instead :)
```

可以 ```extends: "standard"```

### @vue/eslint-config-standard 60,834 weekly downloads

供 vue-cli 内部使用，依赖 ```eslint-plugin-standard``` ，要求 ```eslint-plugin-vue``` 也被 extends 。


别人说：

启用 standard 代码编写风格，并安装 standard
npm install eslint-plugin-standard --save-dev 


```
Usage: vue-cli-service lint [options] [...files]

Options:

  --format [formatter] specify formatter (default: codeframe)
  --no-fix             do not fix errors
  --max-errors         specify number of errors to make build failed (default: 0)
  --max-warnings       specify number of warnings to make build failed (default: Infinity)

```

## Format Document 代码格式化

VSCode 为 HTML 和 js 文件内置了代码格式化工具 ```vscode.html-language-features``` 和 ```vscode.typescript-language-features``` ，但是未内置 ```.vue``` 文件的代码格式化工具。

```Vetur``` 提供了 ```.vue``` 文件的代码格式化工具，安装此扩展后 ```.vue``` 文件的右键菜单才会出现 ```Format Document``` 菜单项。

当有多个代码格式化工具时，点击 ```Format Document``` 菜单项时，VSCode 会让选择默认的工具，提示类似如下的信息：
> There are multiple formatters for HTML-files. Select a default formatter to continue.

并弹出工具清单供选择：

```
Prettier - Code formatter esbenp.prettier-vscode
HTML Language Features vscode.html-language-features
```
有多个代码格式化工具时，右键还会多一个菜单项 ```Format Document With...``` 方便选择 ```formatter``` 以及提供设置默认 ```formatter``` 的入口。

设置默认代码格式化工具对应的 ```settings.json``` 如下：

```
{
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    }
}
```

### VSCode 代码格式化工具之 Prettier Formatter(esbenp.prettier-vscode)
Prettier 支持如下语言：

> JavaScript · TypeScript · Flow · JSX · JSON
> 
> CSS · SCSS · Less
> 
> HTML · Vue · Angular

> GraphQL · Markdown · YAML

Prettier 可以和 VSCode ESLint 集成：

* 较好的方式就是让 Prettier 格式化代码，关闭
* To continue to use Prettier and your linter we recommend you use the ESLint or TSLint extensions directly.

### VSCode 代码格式化工具之 Beautify(hookyqr.beautify)

### Vue官方 VSCode 扩展 Vetur - Vue tooling for VS Code

Vetur(0.22.2版本) 的代码格式化调用的是 Prettier ，每种语言对应的配置为：

```
// 设置为 none 关闭对应语言的 formatter
{
  "vetur.format.defaultFormatter.html": "prettyhtml", // prettyhtml js-beautify-html prettier
  "vetur.format.defaultFormatter.css": "prettier", // 唯一
  "vetur.format.defaultFormatter.postcss": "prettier", // 唯一
  "vetur.format.defaultFormatter.scss": "prettier", // 唯一
  "vetur.format.defaultFormatter.less": "prettier", // 唯一
  "vetur.format.defaultFormatter.stylus": "stylus-supremacy", // 唯一
  "vetur.format.defaultFormatter.js": "prettier", // prettier prettier-eslint vscode-typescript
  "vetur.format.defaultFormatter.ts": "prettier" // prettier prettier-eslint vscode-typescript
}
```

其中 ```prettier-eslint``` 是结合了 ```prettier``` 和 ```eslint --fix``` 。从 ```.prettierrc``` 和 ```.eslintrc``` 读取配置。

> Vetur 只支持完整文件的格式化，不支持选中代码的格式化。

> Vetur bundles all the above formatters. When Vetur observes a local install of the formatter, it'll prefer to use the local version.

配置文件优先级：

```.prettierrc``` > VSCode settings 

### Linting
毫无疑问使用 ESLint ，不要使用 Vetur 的。

### Vue CLI 的 ESLint
| vue add eslint --config | .eslintrc.js 的 extends 名 | Vue CLI 内部使用的npm包名 | 实际提供功能的npm包名 | rules官网 |
| ----------------------- | ------------------------- | ----------------------- | ------------------ | -------- |
| airbnb | @vue/airbnb | @vue/eslint-config-airbnb | eslint-config-airbnb-base | [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript) |
| standard | @vue/standard | @vue/eslint-config-standard | eslint-config-standard | [JavaScript Standard Style](https://standardjs.com/) |
| prettier | @vue/prettier | @vue/eslint-config-prettier | eslint-config-prettier | [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) |

***备注1*** 按约定，所有 ESLint 三方配置(即除 ```eslint:recommended``` 外的)，npm包名都以 ```eslint-config``` 开头。

按 https://eslint.vuejs.org/user-guide/#usage 配置了 ```plugin:vue/recommended ``` ，应该注释掉```eslint:recommended``` ，估计是覆盖关系。

.vue 不使用 Vetur Prettier 单独使用 ESLint 和 Vue CLI 提供的默认配置即可。其他文件如 HTML CSS 使用 Prettier 。

### extends 多个配置

***myconfig/index.js*** 

```
module.exports = {
  rules: {
    'no-console': 1,
    quotes: ['error', 'double']
  }
};
```

***myconfig/index2.js*** 

```
module.exports = {
  rules: {
    'no-console': 1,
    quotes: ['error', 'single']
  }
};
```

***.eslintrc.js***

```
  extends: [
    "./myconfig/index2.js",
    "./myconfig/index.js",
  ],
```

数组后出现的覆盖前面出现的。

@vue/standard 要求单引号、末尾无分号 @vue/prettier 要去双引号、末尾有分号
plugin:vue/essential 无定义。

### stylelint

示例配置：
参考： https://stylelint.docschina.org/user-guide/example-config/ 

VSCode 插件不能自动fix: https://github.com/stylelint/stylelint/issues/4170
命令行能搞定的都显示 ```No quick fixes available```

## References

* [Style Guide — Vue.js][Style Guide — Vue.js]
* [风格指南 — Vue.js][风格指南 — Vue.js]
* [eslint-plugin-vue - Official ESLint plugin for Vue.js][eslint-plugin-vue - Official ESLint plugin for Vue.js]
* [VSCode环境下配置ESLint 对Vue单文件的检测][VSCode环境下配置ESLint 对Vue单文件的检测]
* [5分钟学会用 ESLint+Prettier 统一前端代码风格][5分钟学会用 ESLint+Prettier 统一前端代码风格]

[Style Guide — Vue.js]: https://vuejs.org/v2/style-guide/ "Style Guide — Vue.js"
[风格指南 — Vue.js]: https://cn.vuejs.org/v2/style-guide/ "风格指南 — Vue.js"
[eslint-plugin-vue - Official ESLint plugin for Vue.js]: https://eslint.vuejs.org "eslint-plugin-vue - Official ESLint plugin for Vue.js"
[VSCode环境下配置ESLint 对Vue单文件的检测]: https://juejin.im/post/5a15402a6fb9a045167cd624 "VSCode环境下配置ESLint 对Vue单文件的检测"
[5分钟学会用 ESLint+Prettier 统一前端代码风格]: https://juejin.im/post/5c662b47f265da2dcf626eac "5分钟学会用 ESLint+Prettier 统一前端代码风格"