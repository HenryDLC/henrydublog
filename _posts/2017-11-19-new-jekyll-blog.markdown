---
layout: post
title: 搭建Jekyll 博客
date: '2017-11-19 15:51'
---

### 1)  安装ruby
```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```


### 6) 验证版本
```bash
$ ruby -v
$ gem -v

```

### 8) 安装jekyll 
```bash
gem install jekyll bundler
```

安装主题vno
```bash
$ git clone https://github.com/onevcat/vno-jekyll.git your_site

$ cd your_site

$ bundler install

$ 测试:bundler exec jekyll serve

$ 后台运行:bundle exec jekyll serve --detach
```


### n) 安装nginx
```bash
> 下载nginx
wget http://nginx.org/download/nginx-1.15.8.tar.gz
wget https://ftp.pcre.org/pub/pcre/pcre-8.36.tar.gz
wget http://www.zlib.net/zlib-1.2.11.tar.gz

> 安装gcc
apt-get install gcc libc6-dev g++
sudo apt-get install build-essential

> 解压nginx以及依赖:
tar -xzvf nginx-1.15.8.tar.gz
tar -xzvf pcre-8.36.tar.gz
tar -xzvf zlib-1.2.11.tar.gz

> 安装pcre
cd pcre-8.36
sudo ./configure 
sudo make
sudo make install

> 安装zlib
cd zlib-1.2.11
sudo ./configure 
sudo make
sudo make install

> 安装nginx
cd nginx-1.15.8
# pcre 和 zlib的源文件路径要检查对应:
./configure --prefix=/usr/local/nginx --with-pcre=/home/ubuntu/nginx/pcre-8.36 --with-zlib=/home/ubuntu/nginx/zlib-1.2.11
sudo make
sudo make install`
sudo vim /usr/local/nginx/nginx.conf
```

> 配置nginx
主要更改location --> root /home/www/_site;
每次运行`bundler exec jekyll serve`都会更新`_site`目录,生成新的静态文件
jekyll是一个生成静态网站的程序
```bash
http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format main '$remote_addr - $remote_user [$time_local] "$request" '
    #                '$status $body_bytes_sent "$http_referer" '
    #                '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   /home/www/_site;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
}
```