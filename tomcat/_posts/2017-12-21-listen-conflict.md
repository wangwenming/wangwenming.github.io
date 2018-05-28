17:54 Nginx和SpringBoot EmbedTomcat同时监听了8080端口，Nginx优先
sudo lsof -iTCP -sTCP:LISTEN -n -P
sudo vim /usr/local/etc/nginx/nginx.conf
/usr/local/bin/nginx -s reload