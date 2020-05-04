# Nginx 操作



**Nginx 80 转 443**

```javascript
server {
    listen 80;
    server_name yanyan.site;
    rewrite ^(.*)$ https://${server_name}$1 permanent;
  }
```

**Nginx 监听 443 及证书配置**

```javascript
server {
        listen 443 ssl;
            charset utf-8;
        server_name  yanyan.site;

                # 配置证书路径
        ssl_certificate /etc/nginx/cert/www/2645838_yanyan.site.pem;
        ssl_certificate_key /etc/nginx/cert/www/2645838_yanyan.site.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;

                location ~ /^[H,h][T,t][T,t][P,p][S,s]/ {

                proxy_pass http://127.0.0.1:90;
                proxy_set_header Host           $http_addr;
                proxy_set_header X-Real-IP    $remote_addr;
                proxy_set_header X-Forwarded-For     $proxy_add_x_forwarded_for;
                # 以上三行，目的是将代理服务器收到的用户的信息传到真实服务器上
                }

                # 这里配置使用HTTP访问的请求
        location / {

                proxy_pass http://127.0.0.1:90;
                        proxy_set_header Host      $http_addr;
                        proxy_set_header X-Real-IP    $remote_addr;
                        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
         }

        # error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

**Nginx 操作命令**

1. 启动 `systemctl start nginx`
2. 停止 `systemctl stop nginx`

