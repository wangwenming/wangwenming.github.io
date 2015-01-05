---
---

## 通用的 MockHttpServer 代码
```
var http = require('http');
var fs = require('fs');

fs.readFile('data.txt', function(err, data) {
    if(err) throw err;

    http.createServer(function (req, res) {
        res.writeHead(200, {'Content-Type': 'text/plain'});
        setTimeout(function() {
            res.end(data);
        }, 300);
    }).listen(9501, '127.0.0.1');
    console.log('Server running at http://127.0.0.1:9501/');
});
```
