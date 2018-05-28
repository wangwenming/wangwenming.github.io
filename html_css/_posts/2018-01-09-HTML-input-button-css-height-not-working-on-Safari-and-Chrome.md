## 现象
下面代码不管用(高度未生效)
```
<input type="submit" value="Send" style="height:50px;" />
```

## 解决方案

经分析，浏览器默认样式里有：
```
background-color: buttonface;
```

因此，设置背景色是可以解决问题，如：
```
background: #eee;
```

不过，按参考文档，设置如下CSS更合理：
```
-webkit-appearance: none;
```

## References
[HTML input button css-height not working on Safari and Chrome](https://stackoverflow.com/questions/12450776/html-input-button-css-height-not-working-on-safari-and-chrome)
[how to stop background-color: buttonface; on submit button?](https://stackoverflow.com/questions/29279123/how-to-stop-background-color-buttonface-on-submit-button)

