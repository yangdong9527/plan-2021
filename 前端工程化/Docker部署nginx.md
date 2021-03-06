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
    # history 模式下
    try_files $uri $uri/ /index.html;
  }

  #接口转发
  location ~ /api/ {
    rewrite ^/api/?(.*) /$1 break;
    proxy_pass http://47.103.120.125:7001;
  }
}

# 配置 https
server {
        listen 443 ssl;
        server_name www.xiaoshenxian.top;
        ssl_certificate /etc/nginx/conf.d/6137456_www.xiaoshenxian.top.pem;
        ssl_certificate_key /etc/nginx/conf.d/6137456_www.xiaoshenxian.top.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。
        ssl_prefer_server_ciphers on;
        location / {
                add_header Content-Security-Policy upgrade-insecure-requests;
                root /usr/share/nginx/html;  #站点目录。
                index index.html index.htm;
                try_files $uri $uri/ /index.html;
        }
        location ~ /api/ {
             rewrite ^/api/?(.*) /$1 break;
             proxy_pass http://47.103.120.125:7001;
        }
  			# 需要配置 config 文件中的 publicPath
        location /abc {
						alias /opt/def/;
    				try_files $uri $uri/ /abc/index.html;
    				index index.html index.html;
        }
}

# 重定向
server {
    listen 80;
    server_name abc.top www.abc.top;
    location / {
        rewrite ^(.*)$ https://www.abc.top$1 permanent;
    }
}


```



```shell
docker run -d -p 80:80 -v /www/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /www/nginx/conf.d:/etc/nginx/conf.d -v /www/nginx/html:/usr/share/nginx/html --name nginx nginx
```



其他配置

```nginx
# 502 bad gateway 错误解决配置 start
proxy_buffer_size 64k;
proxy_buffers 32 32k;
proxy_busy_buffers_size 128k;
# 502 bad gateway 错误解决配置 end

server {

    listen 80;
    server_name yabei.520ban.com;

    client_max_body_size     200m; #文件最大大小
    proxy_connect_timeout    600;  #设置超时时间
    proxy_read_timeout       600;
    proxy_send_timeout       600;


    location ^~/doctor/yb/ {
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Real-PORT $remote_port;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_pass http://127.0.0.1:9048/;
            error_page 405 =200 http://$host$request_uri;
    }

    location ^~/platform/yb/ {
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Real-PORT $remote_port;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_pass http://127.0.0.1:9048/;
            error_page 405 =200 http://$host$request_uri;
    }



    location /platform {
            alias /usr/local/yabei/web/platform/;         #静态资源路径
            index  index.html index.htm;
            try_files $uri $uri/ /index.html =404;
    }
 

    location /doctor {
            alias /usr/local/yabei/web/doctor/;         #静态资源路径
            index  index.html index.htm;
            try_files $uri $uri/ /index.html =404;
    }


    location / {
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Real-PORT $remote_port;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_pass http://127.0.0.1:9048;
            error_page 405 =200 http://$host$request_uri;
     }
}
```



### rewrite语法说明

基础语法

```
rewrite <regex> <replacement> [flag]
 关键字   正则		替代内容	 标记
```

+ **正则:** 正则表达式 $1 是正则 ( ) 的 特性

+ **替代内容:**一般配置着匹配到的内容重写路由

+ **flag:**标记

  last表示匹配完这条规则后,继续往下匹配新规则

  break表示匹配完本条规则后,不再往下匹配规则

  redirect302重定向,浏览器地址会显示跳转后的URL地址

  permanent301永久重定向,浏览器会显示跳转后的URL地址

示例

```
location ^~/api {
	rewrite ^/api/(.*)$ /$1 break;
}

location / {
	rewrite ^(.*)$ https://$host$1 permanent;
}
```

