## URL中的路径参数(Path Parameter)

### 起因
按约定，合作方在我提供的接口后面追加`?id=xxx`形式的代码。<br>
但实际情况是追加的`;jsessionid=yyy?id=xxx`，经测试，一样可以正常工作。<br>
因此上网查询资料了解更多。

如
```
http://www.mysite.com/admin/UpdateUserServlet;jsessionid=OI24B9ASD7BSSD
```

### RFC
```
reserved    = gen-delims / sub-delims
gen-delims  = ":" / "/" / "?" / "#" / "[" / "]" / "@"
sub-delims  = "!" / "$" / "&" / "'" / "(" / ")"
              / "*" / "+" / "," / ";" / "="
```
从*RFC*定义中可以看到，;和! $ & ' ( ) * + , = 共11个符号，属于*sub-delims*。
> Aside from dot-segments in hierarchical paths, a path segment is considered opaque by the generic syntax. URI producing applications often use the reserved characters allowed in a segment to delimit scheme-specific or dereference-handler-specific subcomponents. For example, the semicolon (";") and equals ("=") reserved characters are often used to delimit parameters and parameter values applicable to that segment. The comma (",") reserved character is often used for similar purposes. For example, one URI producer might use a segment such as "name;v=1.1" to indicate a reference to version 1.1 of "name", whereas another might use a segment such as "name,1.1" to indicate the same. Parameter types may be defined by scheme-specific semantics, but in most cases the syntax of a parameter is specific to the implementation of the URI's dereferencing algorithm.

### JAVA和Servlet中
*HttpServletRequest*类有一个私有属性*pathParameters*，是一个*HashMap*用来存储*Path Parameter*。

对于如下URL，容易导致权限控制的漏洞：
```
http://www.mysite.com/admin/UpdateUserServlet;/user/HomeServlet
```

因为*HttpServletRequest.getRequestURI()*返回的是总是包含*Path Parameter*，但不包含*Query Parameter*。 

****
不同的Servlet版本、容器版本如何解析这种URL都不容，请亲自测试
****

### Reference
* 2016-01-29 [What is the semicolon reserved for in URLs?](http://stackoverflow.com/questions/2163803/what-is-the-semicolon-reserved-for-in-urls)
* 2012-03-23 John Melton [Beware the HTTP Path Parameter in Java
](http://www.jtmelton.com/2011/02/02/beware-the-http-path-parameter/)
