# 项目部署

- 自建部署：部署(docker镜像、二进制安装包) + 反向代理服务(nginx、ngrok、cpolar、natapp)
- 平台部署：Railway、netlify、firbase hosting

## 1.自建部署
### nginx部署

- /etc/nginx/sites-available/default 配置root index
- nginx -t
- nginx -s reload

前端与后端公用一台nginx，配置如下
```
 #前端的dist项目
        root /var/www/html;
        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
        //前端访问的接口转发到服务端（http://127.0.0.1:9911）
        location ^~/api/ {
                proxy_pass http://127.0.0.1:9911;
                proxy_set_header Host $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
```

