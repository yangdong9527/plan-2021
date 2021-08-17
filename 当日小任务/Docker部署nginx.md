### 准备工作

`nginx.conf`文件

```nginx
# 创建 /nginx/conf/nginx.conf
# 默认配置
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

`xiaoshenxian.conf`文件

```nginx
# 在 /nginx/conf.d/xiaoshenxian.conf
server {
  listen 80;
  #监听地址
  server_name 47.103.120.125;

  #静态资源
  location / {
    #根目录
    root /usr/share/nginx/html;
    # 设置默认页
    index index.html;
  }

  #接口转发
  location ~ /api/ {
    proxy_pass http://47.103.120.125:7001;
  }
}
```



