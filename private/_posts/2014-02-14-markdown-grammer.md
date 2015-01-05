---
---

#关于
这篇文章是个人的markdown笔记，方便忘记的时候自己查看，不作为markdown教程使用。

#概述
Markdown 是一种轻量级的标记语言，由John Gruber和Aaron Swartz创建，最初由Gruber应用在Perl语言中。
Markdown 允许 HTML 语法，所以使用者如果需要可以直接用 HTML来表示是可以的。
HTML 是一种发布的格式，Markdown 是一种书写的格式。

Markdown 的目标是实现「易读易写」。
Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客。世界上最流行的博客平台WordPress和大型CMS如joomla、drupal都能很好的支持Markdown。

LaTeX，Docbook。

#语法
##标题
```
# 这是 H1

## 这是 H2

###### 这是 H6
```

##列表
1. 无序列表使用星号、加号或是减号作为列表标记，都一样
2. 有序列表则使用数字接着一个英文句点
3. 列表项目可以包含多个段落，每个项目下的段落都必须缩进 4 个空格或是 1 个制表符
4. 如果要放代码区块的话，该区块就需要缩进两次，也就是 8 个空格或是 2 个制表符

##区块引用 Blockquotes
* 每行前加 >
* 或者偷懒只在段落前加 >
* 还可以嵌套
* 引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等

##代码区块
1. 要在 Markdown 中建立代码区块很简单，只要简单地缩进 4 个空格或是 1 个制表符就可以
2. 在代码区块里面， & 、 < 和 > 会自动转成 HTML 实体

##代码
* 如果要标记一小段行内代码，你可以用反引号把它包起来

##分隔线
一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西

##链接
* This is [an example](http://example.com/ "Title") inline link.
* 参考式的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：`This is [an example] [id] reference-style link`。接着，在文件的任意处，你可以把这个标记的链接内容定义出来：`[id]: http://example.com/  "Optional Title Here"` 。你也可以把 title 属性放到下一行，也可以加一些缩进，若网址太长的话，这样会比较好看：
```
[id]: http://example.com/longish/path/to/resource/here
    "Optional Title Here"
```
* 自动链接：`<http://example.com/>`

##图片
1. 行内式： `![Alt text](/path/to/img.jpg "Optional title")`
2. 参考式： `![Alt text][id]`

##强调
* Markdown 使用星号（*）和底线（_）作为标记强调字词的符号
* 强调也可以直接插在文字中间

#参考
1. [wowubuntu] http://wowubuntu.com/markdown/
    "Markdown 语法说明 (简体中文版)"
2. [mahua] http://mahua.jser.me/
    "MaHua 在线markdown编辑器"
