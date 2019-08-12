# 如何升级所有的npm嵌套包版本

## 问题描述
***babel-core@6.26.3*** （最新版）依赖 ***lodash@4.17.11*** （最新版4.17.15），但是项目不直接依赖 ***lodash*** ，如何升级 ***babel-core*** 嵌套依赖（nested dependecy）为 ***lodash@4.17.15*** ？

## 答案
### 添加为直接依赖（虽然项目不直接使用）

执行 ```npm install lodash@latest``` 添加直接依赖。

```package.json``` 的 diff 如下：

```diff
  "dependencies": {
+    "lodash": "^4.17.15",
  }
```

```package-lock.json``` 的 diff 如下：

```
   "dependencies": {
     "lodash": {
-      "version": "4.17.11",
-      "resolved": "http://npm.hzfapi.com/lodash/-/lodash-4.17.11.tgz",
-      "integrity": "sha1-s56mIp72B+zYniyN8SU2iRysm40="
+      "version": "4.17.15",
+      "resolved": "http://npm.hzfapi.com/lodash/-/lodash-4.17.15.tgz",
+      "integrity": "sha1-tEf2ZwoEVbv+7dETku/zMOoJdUg="
     }
   }
```

```npm ls lodash``` 可以看出所有 ***lodash*** 包依赖基本都更新到这个版本，除了 ***ejs-loader@3.10.1***外，因为其 Major 版本不同。

```
├─┬ babel-core@6.26.3
│ └─┬ babel-generator@6.26.1
│   └── lodash@4.17.15  deduped
├─┬ ejs-loader@0.3.1
│ └── lodash@3.10.1
└─┬ html-webpack-plugin@3.2.0
  └── lodash@4.17.15  deduped
```

在项目的 ***packge.json*** 有：

```
  "devDependencies": {
    "ejs-loader": "^0.3.1",
  }
```

### 补充：***ejs-loader*** 直接依赖的升级方式

***ejs-loader*** 可以直接升级，原理一致：

```npm update ejs-loader```

升级结果：

```package.json``` 的 diff 如下：

```
-    "ejs-loader": "^0.3.1",
+    "ejs-loader": "^0.3.3",
```

```package-lock.json``` 的 diff 如下：

```
   "dependencies": {
     "ejs-loader": {
-      "version": "0.3.1",
-      "resolved": "http://registry.npm.taobao.org/ejs-loader/download/ejs-loader-0.3.1.tgz",
-      "integrity": "sha1-KAyOAwvTJCjCmCb2u/a20MFPfKQ=",
+      "version": "0.3.3",
+      "resolved": "http://npm.hzfapi.com/ejs-loader/-/ejs-loader-0.3.3.tgz",
+      "integrity": "sha1-AhqhlriFjwW28JVXbEr+YQEszC4=",
       "dev": true,
       "requires": {
         "loader-utils": "^0.2.7",
-        "lodash": "^3.6.0"
+        "lodash": "^4.17.11"
       },
     }
   }
```

升级后的依赖：

```
├─┬ ejs-loader@0.3.3
│ └── lodash@4.17.15  deduped
```

### 重要备注

***备注1：*** 非直接依赖，使用 ```npm update lodash``` 是不会做任何操作的。

***备注2：*** 如果其中某个直接依赖包已更新 ***lodash*** 版本，如 ***eslint@5.16.0*** ，那么把 ***eslint*** 升级到 ***5.16.0*** 可实现但个包升级 ***lodash*** ，其余包不受影响。

## References
* [npm-package-locks](https://docs.npmjs.com/files/package-locks.html)
* [npm-update](https://docs.npmjs.com/cli/update.html)
* [How do I override nested NPM dependency versions?](https://stackoverflow.com/questions/15806152/how-do-i-override-nested-npm-dependency-versions)
