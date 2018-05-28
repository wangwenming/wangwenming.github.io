## HTTP(1.1)缓存头

### 简介
一共 *4* 个请求头：
* Cache-Control
* Expires
* ETag
* Last-Modified

前2者为一类，缓存命中的行为是从本地磁盘直接读取，无需网络请求，缓存有效期内服务端修改无效；
后2者为一类，缓存命中的行为是发送请求，如果文件未修改服务器应返回 *304* 响应，修改了则应返回 *200* 响应，有网络请求的，缓存有效期内服务端修改有效。

### 第一类：from cache:

1. Cache-Control: max-age=秒 http1.1，相对时间
2. Expires http1.0，绝对时间

两者都存在时，Cache-Control覆盖Expires，只要没有失效，浏览器只访问自己的缓存。

### 第二类：304

3. ETag (Response: If-None-Match)
4. Last-Modified (Response: If-Modified-Since)

### Last-Modified 是否必须和 Expires 一起使用？
no! 

澄清一下，因为参考文档1说明的不准确。

但是一起使用有好处，就是用户按 F5 刷新时，Last-Modified 就能起到缓存作用了。

### 优先级
整体优先级:
```
Cache-Control  > Expires  > ETag > Last-Modified
```

### 浏览器行为
1. F5忽略 from cache 头
2. CTRL + F5 忽略所有4个缓存头

## References
[浏览器缓存详解:expires,cache-control,last-modified,etag详细说明](http://blog.csdn.net/eroswang/article/details/8302191)
