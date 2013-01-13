# Gentoo 安装 PHP、Nginx 和 MySQL
- tags: gentoo, linux

---

## 添加以下 USE 到 /etc/portage/package.use

    dev-lang/php fpm ming xml curl mysql cgi ctype gd hash nginx

需要的其它组件也可以在这里添加。

## 安装 PHP、Nginx 和 MySQL

```bash
emerge nginx mysql php
```

## 添加开启启动

```bash
rc-update add nginx default
rc-update add php-fpm default
rc-update add mysql default
```

## 初始化配置 MySQL

```bash
# 初始化数据库
/usr/bin/mysql_install_db
# 启动 MySQL
/etc/init.d/mysql start
# 设置 MySQL 的 root 密码
mysqladmin -u root password '密码'
```

记得安装完一定要先运行 `/usr/bin/mysql_install_db` 来初始化数据库，否则 MySQL 无法启动

## 配置 Nginx

编辑 `/etc/nginx/nginx.conf`，在 http 项结束的花括号之前添加：

    include /etc/nginx/conf.d/*.conf;

这样以后添加新的网站配置的时候，不必编辑 nginx.conf ，只需要把单个网站的配置如 localhost.conf 放在 `/etc/nginx/conf.d` 文件夹并重启 nginx 即可。

```bash
# 不要忘记创建这个文件夹
mkdir /etc/nginx/conf.d
```

### 网站配置示例

比如我们给 localhost 添加 php 支持，在 `/etc/nginx/conf.d` 文件夹里面新建一个文件如 localhost.conf，内容如下：

```nginx
server {
    listen 80;
    server_name localhost;
    location / {
        root /var/www/localhost/htdocs;
        index index.php index.html index.htm;
    }
    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /var/www/localhost/htdocs$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```

保存，顺手在 `/var/www/localhost/htdocs` 文件夹新建文件 phpinfo.php，内容如下：

```php
<?php
phpinfo();
?>
```

现在可以用 `nginx -t` 测试下我们的配置文件是否出错。第一次运行，可能会提示无法创建 `/var/tmp/nginx/client` 文件夹，没关系，我们去创建下就是了：

```bash
mkdir /var/tmp/nginx
mkdir /var/tmp/nginx/client
```

现在我们启动 nginx 和 php-fpm：

```bash
/etc/init.d/nginx start
/etc/init.d/php-fpm start
```

不出意外的话，你可以在 http://localhost/phpinfo.php 页面看到熟悉的 PHPInfo 页面啦。