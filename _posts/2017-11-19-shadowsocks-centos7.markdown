---
layout: "post"
title: "CentOS 7 配置 shadowsocks 及打开谷歌TCP BBR 算法优化"
date: "2017-11-19 16:34"
---
前言:
---

一直在用的SS服务商给我发了封邮件…..内容是它被查水表了 需要关闭所有SS服务…..

没办法 我只能在自己的blog服务器中 搭建自己的SS服务器 并且开启BBR算法 优化服务器的上网速度

### SS服务器你需要:

硬件层:
```
一台非中国大陆地区的Linux系统服务器(本文使用centOS7)
```
软件层:
```
Python2         python第二版 一般Linux系统自带

pip             python包管理器

shadowsocks     ss服务器端

Linux内核       内核版本>=4.9
```
### 服务器的选择:


你需要购买一台非大陆地区的服务器

因为后续要开启BBR算法优化,所以需要你的服务器使用虚拟化技术搭建(VPS服务器)

国内阿里云/百度云/腾讯云都有非大陆地区的VPS服务器租赁,但是有流量带宽限制且价格昂贵…..好处就是可以租香港/台湾/新加坡这些物理距离较近的服务器地址(P.S物理距离越近 理论上ping值越小)

海外主流的VPS提供商:

>Linode

>digitalocean

>Vultr

对于购买VPS 需要三点注意事项:

1)物理距离,一般国内体验比较好的是日本VPS

2)ping值,ping值和丢包率 直接影响连接速度 掉线率 和 带宽速度

3)IP环绕 现阶段很多vps服务器 从中国请求访问 不直接连接服务器地址 而是先绕到美国 再返回到服务器地址…..反之亦然…..

### 购买建议:

我正在用的VPS是Linode公司的日本机房

Linode 是老牌的VPS提供商 线路稳定 机房众多 请求从中国大陆直接连接日本服务器 不会参与IP环绕 但是第一次购买需要信用卡付款 再之后可以选择PayPal支付

具体速度测试及更多购买建议请参考:<http://www.jianshu.com/p/da8f4d0d983c>

### 开始安装:摘自
<http://morning.work/page/2015-12/install-shadowsocks-on-centos-7.html>

安装pip包管理器:
```
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"

python get-pip.py
```
### 安装shadowsocks:
```
pip install --upgrade pip

pip install shadowsocks
```
### 配置shadowsocks:
```
vim /etc/shadowsocks.json
```
增加以下内容:

Shadowsocks接受这样的[JSON](http://www.json.org/)格式配置：

```
{
    "server":"ip地址",
    "server_port":8388,
    "local_port":1080,
    "password":"密码",
    "timeout":600,
    "method":"aes-256-cfb"
}
```

加密方式:可选`aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table`

配置自启动:
------
```
vim /etc/systemd/system/shadowsocks.service
```
增加以下内容:
```
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0

ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json
[Install] WantedBy=multi-user.target
```

更新配置,启动Shadowsocks
------------------
```
systemctl enable shadowsocks

systemctl start shadowsocks
```
检查SS服务是否启动:
-----------
```
systemctl status shadowsocks -l
```
显示类似以下内容表示成功:

>shadowsocks.service - Shadowsocks
>
 >Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: disabled)
>
 >Active: active (running) since 二 2017-09-26 16:19:17 UTC; 10h ago
>
 >Main PID: 3632 (ssserver)
>
 >CGroup: /system.slice/shadowsocks.service
>
 >└─3632 /usr/bin/python /usr/bin/ssserver -c /etc/shadowsocks.json

开启BBR:
------

摘自:<https://www.vultr.com/docs/how-to-deploy-google-bbr-on-centos-7>

为了启用BBR算法，您需要修改`sysctl`配置如下：

```python
echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

现在，您可以使用以下命令确认启用了BBR：

```python
sudo sysctl net.ipv4.tcp_available_congestion_control
```

输出应该类似于：

>net.ipv4.tcp\_available\_congestion\_control = bbr cubic reno

接下来，验证：

```python
sudo sysctl -n net.ipv4.tcp_congestion_control
```

输出应为：

>bbr
