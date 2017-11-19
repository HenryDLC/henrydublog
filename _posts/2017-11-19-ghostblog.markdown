---
layout: "post"
title: "搭建Ghost 博客"
date: "2017-11-19 16:36"
---
升级所有包，不改变软件设置和系统设置，系统版本升级，内核不改变
```
yum -y upgrade
```

### 更新 yum 的 repo list
```
rpm -Uvh http://download-i2.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

验证成功
```
rpm -q epel-release
```
重建缓存
```
yum makecache
```

### 安装wget（-bash: wget: command not found）
```
yum -y install wget
```
### 安装 node 和 npm
```
wget http://nodejs.org/dist/v0.12.18/node-v0.12.18-linux-x64.tar.gz
```

解压node
```bash
tar zxvf node-v0.12.18-linux-x64.tar.gz
```
设置全局
```bash
cd node-v0.12.18-linux-x64/bin/
ln -s /root/node-v0.12.18-linux-x64/bin/node /usr/local/bin/node
ln -s /root/node-v0.12.18-linux-x64/bin/npm /usr/local/bin/npm
```
验证安装
```
node -v
npm -v
```

### 安装Nginx
下载 repo 列表
```
wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

### 通过 rpm 添加列表
```
rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
### 通过 yum 安装 nginx
```
yum install nginx
```
### 运行Nginx 设置为开机启动
```
systemctl enable nginx
```
### 配置文件
```
vi /etc/nginx/conf.d/default.conf
```

```
server {  
    listen       80;
    #你的域名
    server_name  undefinedblog.com;

    #后续Wordpress原有数据导入章节会详细说明
    location /wp-content/ {
        root /var/www/content/images;
    }

    #下面几个location都是让nginx直接serve静态文件
    location ~ ^/(img/|css/|lib/|vendor/|fonts/|robots.txt|humans.txt) {
      root /var/www/core/client/assets;
      access_log off;
      expires max;
    }

    location ~ ^/(shared/|built/) {
      root /var/www/core;
      access_log off;
      expires max;
    }

    location ~ ^/(favicon.ico) {
      root /var/www/core/shared;
      access_log off;
      expires max;
    }

    location ~ ^/(content/images/) {
      root /var/www;
      access_log off;
      expires max;
    }

    #后续Wordpress原有数据导入会详细讲解
    rewrite ^/(\d+)/(\d+)/(.*)$ http://undefinedblog.com/$3 permanent;

    #核心block，将请求proxy到Ghost实例上
    #其中端口可以在Ghost的config.js中修改，但要保持一致
    location / {
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        Host      $http_host;
        proxy_pass              http://127.0.0.1:2368;
    }
}
```

重启Nginx
```
systemctl restart nginx
```

安装MariaDB
```
sudo yum install mariadb-server -y  //安装
sudo systemctl enable mariadb	//开机启动
sudo systemctl start mariadb	//启动mariadb
```

### 配置MariaDB
```
sudo mysql_secure_installation  //进入mariadb
输入安装 MySQL 时指定的 root 密码，一般直接按回车
是否改变 root 密码，输入 y
输入新的 root 密码
是否删除匿名用户，输入 y
是否禁止 root 远程登录，输入 y
是否删除默认的 test 数据库，输入 y
是否马上应用最新的设置，输入 y
```
为了避免数据库存放的中文是乱码，我们还需要设置Mysql的编码
```
vi /etc/my.cnf
```
写入以下内容
```
[client]
default-character-set=utf8  
[mysql]
default-character-set=utf8  
[mysqld]
character-set-server=utf8  
collation-server=utf8_general_ci
```

### 创建数据库
登录MySQL
```bash
mysql -u root -p 你的密码
```
创建名为ghost的用户并新建名为ghost的数据库，同时给ghost用户授予ghost数据库的所有权限
```
CREATE DATABASE ghost;  
GRANT ALL PRIVILEGES ON ghost.* To 'ghost'@'127.0.0.1' IDENTIFIED BY '707116148';

exit
```

```
systemctl restart mariadb
```

安装Ghost（Ghost v0.7.4 full (zh)）
```
mkdir /var/www && cd /var/www
wget http://dl.ghostchina.com/Ghost-0.7.4-zh-full.zip
yum -y install unzip	//安装unzip（-bash: unzip: command not found）
unzip Ghost*
```

配置Ghost
```
cp config.example.js config.js
vi config.js
```
```
production: {  
    url: 'http://undefinedblog.com', //替换为你自己的域名。
    mail: {},
    database: {
        client: 'mysql',
        connection: {
            host     : '127.0.0.1',
            user     : 'ghost', //上面配置过
            password : 'ghost对应的密码', //上面配置过
            database : 'ghost', //我们前面为 Ghost 创建的数据库名称
            charset  : 'utf8'
        }
    },
    server: {
        host: '127.0.0.1',
        port: '2368'//若修改该端口记得在nginx中做相应改变
    }
},
```
```
npm install --production
```
测试
```
npm start
```
结束测试
```
Ctrl + C
```

运行
```
nohup npm start &
```

https://undefinedblog.com/centos-installing-ghost/
https://segmentfault.com/a/1190000006245224
http://blog.csdn.net/xiaofeiaiai/article/details/55048812
http://zzimoo.com/first/

### 搭建 FTP服务器
```
yum check-update
yum -y install vsftpd
```
修改参数：
```
vi /etc/vsftpd/vsftpd.conf
```
```
anonymous_enable=NO
chroot_local_user=YES
allow_writeable_chroot=YES
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
```
重启FTP
```
systemctl restart vsftpd.service
systemctl enable vsftpd.service
```
创建用户和FTP文件夹以及设置权限
```
useradd -d /home/www -m henrydu -s /sbin/nologin
cd /home/
chmod -R 777 www/
```
设置ftp密码
```
passwd henrydu
```
删除mysql数据库:
```
sudo rm /usr/local/mysql

sudo rm -rf /usr/local/mysql\*

sudo rm -rf /Library/StartupItems/MySQLCOM

sudo rm -rf /Library/PreferencePanes/My\*

rm -rf ~/Library/PreferencePanes/My\*

sudo rm -rf /Library/Receipts/mysql\*

sudo rm -rf /Library/Receipts/MySQL\*

sudo rm -rf /var/db/receipts/com.mysql.

调用vim /etc/hostconfig，删除 MYSQLCOM=-YES-这一行。
```
