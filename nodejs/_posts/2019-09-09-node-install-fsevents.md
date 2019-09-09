# fsevents

> Native access to MacOS FSEvents in Node.js
> The ***FSEvents*** API in MacOS allows applications to register for notifications of changes to a given directory tree. It is a very fast and lightweight alternative to ***kqueue***.


```
node-pre-gyp ERR! Tried to download(404): https://fsevents-binaries.s3-us-west-2.amazonaws.com/v1.1.2/fse-v1.1.2-node-v64-darwin-x64.tar.gz
node-pre-gyp ERR! Pre-built binaries not found for fsevents@1.1.2 and node@10.16.0 (node-v64 ABI) (falling back to source compile with node-gyp)
node-pre-gyp ERR! Tried to download(undefined): https://fsevents-binaries.s3-us-west-2.amazonaws.com/v1.1.2/fse-v1.1.2-node-v64-darwin-x64.tar.gz
node-pre-gyp ERR! Pre-built binaries not found for fsevents@1.1.2 and node@10.16.0 (node-v64 ABI) (falling back to source compile with node-gyp)
```


```
> fsevents@1.2.9 install /Users/wenmingwang/Downloads/tmp/vue-test-utils-getting-started/node_modules/sane/node_modules/fsevents
> node install

node-pre-gyp WARN Using request for node-pre-gyp https download
[fsevents] Success: "/Users/wenmingwang/Downloads/tmp/vue-test-utils-getting-started/node_modules/sane/node_modules/fsevents/lib/binding/Release/node-v64-darwin-x64/fse.node" is installed via remote
npm WARN vue-test-utils-getting-started@1.0.0 No repository field.

added 69 packages from 36 contributors in 6.57s
```


```
$ npm ls fsevents
vue-test-utils-getting-started@1.0.0 /Users/wenmingwang/Downloads/tmp/vue-test-utils-getting-started
├── fsevents@2.0.7
└─┬ jest@21.2.1
  └─┬ jest-cli@21.2.1
    └─┬ jest-haste-map@21.2.0
      └─┬ sane@2.2.0
        └── fsevents@1.2.9
```

```fsevents@1.2.9``` 没有针对 ```Node v10.16.0``` 的 ```Pre-built binaries```

## References

* [fsevents 404 when installing #236](https://github.com/fsevents/fsevents/issues/236)