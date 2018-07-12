# Mac开发环境搭建手册

### shell

查看已有shells
```
cat /etc/shells
```

查看当前shell
```
echo $SHELL
```

切换到zsh
```
chsh -s /bin/zsh
```

### [oh-my-zsh](ohmyz.sh)

主题 _# ~/.oh-my-zsh/themes/_
`af-magic` `simple`

插件 _# ~/.oh-my-zsh/plugins/_
`git` `osx` `symfony2` `brew`

配置文件
```
~/.zshrc
```
应用配置
```
source ~/.zshrc
```
默认使用vim打开html格式的文件
```
vim ~/.zshrc
alias -s html=vim
```
重写PATH目录
```
vim ~/.zshrc
export PATH="/usr/local/bin:/usr/local/sbin:/usr/bin:/bin:/usr/sbin:/sbin:~/.composer/vendor/bin"
```

### [Homebrew](brew.sh)

### PHP
添加PHP仓库
```
brew tap homebrew/dupes
brew tap homebrew/versions
brew tap homebrew/homebrew-php
```

安装PHP7.1
```
brew install php71
```

启动关闭PHP
```
sudo php71-fpm start
sudo php71-fpm stop
```

### 安装nginx
```
brew install nginx
```

建立项目目录、虚拟主机配置目录及nginx日志存放目录
```
sudo mkdir -p /var/www
sudo chown root:staff /var/www
sudo chmod g+w /var/www
sudo mkdir -p /var/log/nginx
mkdir /usr/local/etc/nginx/servers
```

修改nginx配置文件 `/usr/local/etc/nginx/nginx.conf`
```
user  nobody;
worker_processes  1;

error_log  /var/log/nginx/error.log debug;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    error_log  /var/log/nginx/error.log;

    sendfile        on;
    keepalive_timeout  65;
    client_max_body_size   1024m;

    server {
        listen       80;
        server_name  localhost;
        root         /var/www;
        charset      utf-8;

        #access_log  logs/host.access.log  main;

        location / {
            autoindex on;
            index  index.html index.htm index.php;
        }

        location ~ \.php$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_split_path_info ^(.+\.php)(/.*)$;
            include        fastcgi_params;
            fastcgi_param  SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        }

    }

    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}
```

启动、重启及关闭nginx
```
sudo nginx
sudo nginx -s reload
sudo nginx -s stop
```

### 安装mysql
```
brew install mysql
```

启动关闭mysql
```
mysql.server start
mysql.server stop
```

### 安装node
```
brew install node
```

### 安装yarn
```
npm install -g yarn
```

配置 npm、yarn 源为国内源
```
npm config set registry https://registry.npm.taobao.org
yarn config set registry https://registry.npm.taobao.org
```

配置ssh一键登录
脚本
```
#!/usr/bin/expect
spawn ssh -p[PORT] [IP]
expect "*password:"
send "[PASSWORD]\r"
expect "*#"
interact
```
放到bin目录下，应用bash。
