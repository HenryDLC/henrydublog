---
layout: post
title: 搭建Jekyll 博客
date: '2017-11-19 15:51'
---

### 1)  安装RVM
```bash
下载安装RVM管理器,
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

& curl -sSL https://get.rvm.io | bash -s stable
or
& curl -L https://raw.githubusercontent.com/wayneeseguin/rvm/master/binscripts/rvm-installer | bash -s stable

启动rvm(or重开终端或重启系统)
& source /etc/profile.d/rvm.sh

更改ruby下载地址
& echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db

rvm 安装路径
& /usr/local/rvm
```

### 2) 安装ruby
```bash
  $ rvm requirements
  $ rvm install 2.3.0
```

### 6) 安装 Bundler
```bash
gem install bundler
```

### 3) 安装rails
```bash
$ gem install rails
```

### 4)  安装RubyGame
 ```bash
 $ gem update
 ```

### 5) 安装jekyll
```bash
$ gem install jekyll
```

### 6) 验证版本
```bash
$ ruby -v

$ gem -v

$ rvm -v
```

### 7) 安装nginx
```bash
 下载 repo 列表
$ wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

 通过 rpm 添加列表
$ rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm

 通过 yum 安装 nginx
$ yum install nginx

 运行Nginx
 设置为开机启动
$ systemctl enable nginx

 配置文件
$ vi /etc/nginx/conf.d/default.conf

 重启Nginx
$ systemctl restart nginx
```

### 8) 安装jekyll 主题vno
```bash
$ git clone https://github.com/onevcat/vno-jekyll.git your_site

$ cd your_site

$ bundler install

$ bundler exec jekyll serve

$ jekyll serve --detach
```
