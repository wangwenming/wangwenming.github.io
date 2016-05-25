### 可以基于 IP 地址，或者 HTTP 基础认证 进行访问控制，由 ngx_http_access_module 提供。
> Access can be allowed or denied based on the IP address of a client or by using HTTP basic authentication.

### IP 地址
Context:    http, server, location

语法示例：
```
location / {
    deny  192.168.1.1;
    allow 192.168.1.0/24;
    allow 10.1.1.0/16;
    allow 2001:0db8::/32;
    deny  all;
}
```

例如：
```
allow 211.103.129.202;
deny all;
```

> The rules are checked in sequence until the first match is found.


## References
1. [RESTRICTING ACCESS TO PROXIED HTTP RESOURCES][RESTRICTING ACCESS TO PROXIED HTTP RESOURCES]
2. [Nginx Block And Deny IP Address OR Network Subnets][Nginx Block And Deny IP Address OR Network Subnets]
3. [Module ngx_http_access_module][Module ngx_http_access_module]

[RESTRICTING ACCESS TO PROXIED HTTP RESOURCES]: https://www.nginx.com/resources/admin-guide/restricting-access/ "RESTRICTING ACCESS TO PROXIED HTTP RESOURCES"
[Nginx Block And Deny IP Address OR Network Subnets]: http://www.cyberciti.biz/faq/linux-unix-nginx-access-control-howto/ "Nginx Block And Deny IP Address OR Network Subnets"
[Module ngx_http_access_module]: http://nginx.org/en/docs/http/ngx_http_access_module.html "Module ngx_http_access_module"
