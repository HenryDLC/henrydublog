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