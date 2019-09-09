# npm semver and the Caret Ranges

## semver定义
对于 ```MAJOR.MINOR.PATCH``` 格式，按如下规则升级版本号<sup>[semver.org][semver.org]</sup>：

1. 进行了不兼容的 API 升级时，升级 ```MAJOR``` 版本
2. 添加了向后兼容的功能时，升级 ```MINOR``` 版本
3. 进行了向后兼容的Bug修复时，升级 ```PATCH``` 版本

## Caret(/'kærət/脱字符) Ranges

> Allows changes that do not modify the left-most non-zero digit in the [major, minor, patch] tuple. In other words, this allows patch and minor updates for versions 1.0.0 and above, patch updates for versions 0.X >=0.1.0, and no updates for versions 0.0.X.<sup>[npm-semver - The semantic versioner for npm][npm-semver - The semantic versioner for npm]
</sup>

[major, minor, patch] 最左的非0锁定

* >=1.0.0 可以升级到 ```1.*.*```
* 0.X 可以升级到 ```0.X.*```
* 0.0.X 不可以升级到 0.0.(X+1)，只能升级 ```prerelease``` 部分

```--save```安装的默认是这个版本。

## Tilde(/tɪldə/波浪符) Ranges

> Allows patch-level changes if a minor version is specified on the comparator. Allows minor-level changes if not.<sup>[npm-semver - The semantic versioner for npm][npm-semver - The semantic versioner for npm]


1. 指定了 minor 锁 minor，只能(向上)改 patch
2. 只指定了 major 锁 major，只能(向上)改 minor & patch

如：

```~1.2.3``` := ```>=1.2.3 <1.3.0```

```~1.2``` := ```~1.2.0``` := ```>=1.2.0 <1.3.0```

```~1``` := ```>=1.0.0 <2.0.0```

```"lodash": "~4.17.10",```
    
## References

* [npm-semver - The semantic versioner for npm][npm-semver - The semantic versioner for npm]
* [semver.org][semver.org]

[npm-semver - The semantic versioner for npm]: https://docs.npmjs.com/misc/semver.html "npm-semver - The semantic versioner for npm"
[semver.org]: http://semver.org/ "semver.org"
