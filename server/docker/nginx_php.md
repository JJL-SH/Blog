# Docker 部署 php+nginx 环境

### 拉取官方镜像

```
docker pull nginx
docker pull php-fpm
```

### 使用 php-fpm 镜像开启 php-fpm 应用容器

```
docker run -d --name myFpm -v /var/www/html:/usr/share/nginx/html php-fpm
```

-d 后台运行容器

-v 指定宿主机与容器的映射关系。/var/www/html 为宿主机的项目目录（自定义的），/usr/share/nginx/html 为 nginx 服务器项目默认的路劲

### 开启 nginx 容器

```
docker run -d --name myNginx -p 8080:80 -v /var/www/html:/usr/share/nginx/html nginx
```

-p 该参数设置端口对应的关系。所有访问宿主机 8080 端口的 URL 会转发到 nginx 容器的 80 端口

### 查看 php-fpm 的 ip 地址，配置给 nginx 用

```
docker inspect myFpm | grep IPAddress
```

### 修改 nginx 配置

```
// 复制 nginx 配置文件
docker cp myNginx:etc/nginx/conf.d/default.conf ./default.conf

// 修改 default.conf
// /scripts 需要修改成网站文件存放位置
location ~ \.php$ {
  fastcgi_pass   你的php-fpmIP 地址:9000;
  fastcgi_index  index.php;
  // fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
  fastcgi_param  SCRIPT_FILENAME  /usr/share/nginx/html$fastcgi_script_name;
  fastcgi_param  SCRIPT_NAME      $fastcgi_script_name;
  include        fastcgi_params;
}

// 把文件放进容器
docker cp ./default.conf mgNginx:etc/nginx/conf.d/default.conf

// 进入容器
docker exec -it myNginx /bin/bash

// 重启 nginx 服务
service nginx reload
```

-i --interactive，交互模式

-t --tty，开启一个伪终端

/bin/bash 必须写，否则会报错。这是开始伪终端时，进入 bash 界面，也就是命令行界面

然后访问 127.0.0.1:8080