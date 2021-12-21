[Linux 查看 nginx 安装目录和配置文件路径](https://www.cnblogs.com/ryanzheng/p/13124128.html)

```
vim /etc/nginx/nginx.conf

Nginx日志的默认路径
/var/log/nginx/
重启nginx
service nginx restart
检查文件是否有问题
nginx -t
配置文件生效
nginx -s reload
```

```
server {
    listen 80;
    # gzip config
    gzip on;
    gzip_min_length 1k;
    gzip_comp_level 9;
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    gzip_vary on;
    gzip_disable "MSIE [1-6]\.";

    root /root/epidemic/dist;

    location / {
        # 用于配合 browserHistory 使用
        try_files $uri $uri/ /index.html;

        # 如果有资源，建议使用 https + http2，配合按需加载可以获得更好的体验 
        # rewrite ^/(.*)$ https://preview.pro.antdv.com/$1 permanent;

    }
    location /api {
        # rewrite  api 
        rewrite ^/api/(.*)$ /$1 break;
        proxy_pass http://123.56.170.116:8080;
        proxy_set_header   X-Forwarded-Proto $scheme;
        proxy_set_header   Host              $http_host;
        proxy_set_header   X-Real-IP         $remote_addr;
    }
}
```

