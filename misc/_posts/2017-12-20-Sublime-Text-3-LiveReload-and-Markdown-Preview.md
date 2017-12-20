# Sublime Text 3 LiveReload and Markdown Preview
## LiveReload

### 每次开启 Sublime ，都需要启动 LiveReload
*Command + Shift + P* 调出 *Command Palette* ， 
输入 *LiveReload* ，选择 *LiveReload: Enable/disable plug-ins* ，再选择 *Enable - Simple Reload*

### 普通HTML中使用
*Command + Shift + P* 调出 *Command Palette* ， 
输入 *LiveReload* ，选择 *Insert livereload.js script* 
会生成类似下面的代码：
```html
<script>document.write('<script src="http://' + (location.host || 'localhost').split(':')[0] + ':35729/livereload.js?snipver=1"></' + 'script>')</script>
```

### 配合Markdown Preview使用
点击菜单 ```Sublime Text -> Preferences -> Package Settings -> Markdown Preview -> Settings - User``` ，内容如下：
```json
{
  "enable_autoreload": true
}
```

## Markdown Preview
### Markdown Preview快捷键设置
点击菜单 ```Sublime Text -> Preferences -> Key Bindings``` ，在右侧的 user 快捷键增加以下内容
```json
{ "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"} }
```

以后按 *alt+m* 快捷键进行预览即可。

注意：每次退出Sublime Text后，都需要重新启动 LiveReload

### MarkdownEditing
补充一个 MarkdownEditing 插件，可以在 Sublime 里做 Markdown 的语法高亮。

## Reference
* [Live​Reload](https://packagecontrol.io/packages/LiveReload)
* [Markdown Preview](https://packagecontrol.io/packages/Markdown%20Preview)
* [MarkdownEditing](https://packagecontrol.io/packages/MarkdownEditing)
