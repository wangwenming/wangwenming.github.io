### content model
分为 7 类：

* Metadata content:  base link meta noscript script style template title
* Flow content: 大部分可以在 boby 中使用的标签都是这个，例如 a article aside b br button link...
* Sectioning content:  article aside nav section 全在 Flow content 出现过
* Heading content:  h1 h2 h3 h4 h5 h6
* Phrasing content: 也是好多， 
* Embedded content:  audio, canvas, embed, iframe, img, MathML math, object, picture, SVG svg, video 
* Interactive content

其中

* div 的 *content model* 是 *flow content*
* p 的 *content model* 是 *phrasing content*

### P标签只可以包含 inline 标签

> For the P element, it specifies the following, which indicates that P elements are only allowed to contain inline elements.
> ```<!ELEMENT P - O (%inline;)*            -- paragraph -->```

包括 P 标签本身也不行哦！所以 P 标签是不能嵌套的！

### References
[Why &lt;p&gt; tag can't contain &lt;div&gt; tag inside it?](https://stackoverflow.com/questions/8397852/why-p-tag-cant-contain-div-tag-inside-it)
