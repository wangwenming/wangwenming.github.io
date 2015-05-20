## 语法
```
.example {
    transition: [transition-property] [transition-duration] [transition-timing-function] [transition-delay];
}
```

还可以用 , 分隔多个，如:
```
div {
  transition: background 0.2s ease,
              padding 0.8s linear;
}
```

如果没有transition-delay参数，则顺序无所谓；
如果　有transition-delay参数，则第一个时间参数，总是被解析为duration，以后的则是delay。

不是每个属性都可以transition，参考: http://www.w3.org/TR/css3-transitions/#animatable-properties

## 理解 transition
以下代码，在hover on的时候有动画，hover off的时候则没有。
```
.box {
  width: 150px;
  height: 150px;
  background: red;
  margin-top: 20px;
  margin-left: auto;
  margin-right: auto;
}

.box:hover {
  background-color: green;
  cursor: pointer;
  -webkit-transition: background-color 2s ease-out;
  -moz-transition: background-color 2s ease-out;
  -o-transition: background-color 2s ease-out;
  transition: background-color 2s ease-out;
}
```
如果希望hover off的时候有相反的动画，则把transition加到.box上。
试试：
http://codepen.io/impressivewebs/pen/zohgt

## 无需前缀的浏览器支持
| IE | Firefox | Chrome | Safari | Opera | iOS Safari | Android Browser | Chrome for Android |
|----|---------|--------|--------|-------|------------|-----------------|--------------------|
| 10 | 16      | 26     | 6.1    | 12.1  | 7.0        | 4.4             | 42                 |

## timing function
### predefined keyword value
* ease
* linear
* ease-in
* ease-out    cubic-bezier(0, 0, 0.58, 1)
* ease-in-out
* step-start  steps(1, start)
* step-end

### stepping function
steps(, [start|end]) default to end


### cubic Bézier curve
ease-out

## 参考
https://css-tricks.com/almanac/properties/t/transition-timing-function/

