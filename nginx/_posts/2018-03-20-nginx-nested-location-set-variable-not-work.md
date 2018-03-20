### 介绍
nginx 包含 simple directive (;结尾的单条指令) 和 block directives ({}包起来的) 两种，如果 block directive 可以包含其他指令，则称为 `context` ，如 `events` `http` `server` `location`

其包含顺序是:
```
main
    events
    http
        server
            location
```

`if` 和 `set` 指令是 [ngx_http_rewrite_module](http://wiki.nginx.org/HttpRewriteModule) 提供的。

### Nginx嵌套location示例配置
```
location / {
    # 外层 location 的 set 指令对内层无效，情况①✘
    if ($cookie_uid = '') {
        set $uid "in if block of outer location";
    }
    # 外层 location 的 set 指令对内层无效，情况②✘
    set $uid "in outer location";

    # 外层 location 的 add_header 指令对内层无效，情况①✔
    # 当然前提是内层 location 不再定义其他任意 add_header ，否则这里定义的会被覆盖
    # 因为 add_header 是 Array Directive ，具体请阅读参考文档3和4
    add_header Set-Cookie "uid=$uid;Domain=xyk.localhost.com;Path=/;Max-Age=31104000";
    
    # 外层 location 的 add_header 指令对内层无效，情况②✘
    if ($cookie_uid = '') {
        add_header Set-Cookie "uid=$uid;Domain=xyk.localhost.com;Path=/;Max-Age=31104000";
    }

    location /hx/ {
        # 内层 location 可随便使用，都可以支持
        if ($cookie_uid = '') {
            add_header Set-Cookie "uid=$uid;Domain=xyk.localhost.com;Path=/;Max-Age=31104000";
        }
        rewrite ...
    }
}
```

所以：
1. 外层 `location` 的 `if` 下的指令都内层都无效✘
2. 外层 `location` 的 `add_header` 指令都内层都有效✔，`set` 指令对内层无效✘
3. `server` 下可以安全使用 `if` ，并且在下面使用 `set ，`server` 下不能直接使用 `add_header` 指令

虽然 Nginx 官网文档没有写，但是有如下规则<sup>[5][Is this how variable (set $var) inheritance works?]</sup>：
1. - anything set in server { } block is inherited:
2. - anything set in location { } block is not inherited:

>Nested location blocks inherit variables only by name, which means
the inherited variables are only created and marked as uninitialized.

也就是说 `location` 里的 `set` ，其值不会被 **继承**，仅仅能起到不报变量未定义的错而已。这样 `nested location` 其实用处不大了吧？

### References
1. [If Is Evil](https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/)
2. [ngx_http_core_module: location](https://nginx.org/en/docs/http/ngx_http_core_module.html#location)
3. [UNDERSTANDING THE NGINX CONFIGURATION INHERITANCE MODEL][UNDERSTANDING THE NGINX CONFIGURATION INHERITANCE MODEL]
4. [add_header directives in location overwriting add_header directives in server](https://serverfault.com/questions/400197/add-header-directives-in-location-overwriting-add-header-directives-in-server)
5. [Is this how variable (set $var) inheritance works?][Is this how variable (set $var) inheritance works?]

[UNDERSTANDING THE NGINX CONFIGURATION INHERITANCE MODEL]: https://blog.martinfjordvald.com/2012/08/understanding-the-nginx-configuration-inheritance-model/ "UNDERSTANDING THE NGINX CONFIGURATION INHERITANCE MODEL"

[Is this how variable (set $var) inheritance works?]: http://mailman.nginx.org/pipermail/nginx/2012-February/031699.html "Is this how variable (set $var) inheritance works?"
