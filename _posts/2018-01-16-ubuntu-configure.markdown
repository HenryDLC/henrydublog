---
layout: "post"
title: "ubuntu初始配置"
date: "2018-01-16 22:09"
---
### 替换阿里源

```
sudo gedit /etc/apt/sources.list

\# deb cdrom:[Ubuntu 16.04 LTS \_Xenial Xerus\_ - Release amd64 (20160420.1)]/ xenial main restricted

deb-src http://archive.ubuntu.com/ubuntu xenial main restricted \#Added by software-properties

deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted

deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe \#Added by software-properties

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted

deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe \#Added by software-properties

deb http://mirrors.aliyun.com/ubuntu/ xenial universe

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse

deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse \#Added by software-properties

deb http://archive.canonical.com/ubuntu xenial partner

deb-src http://archive.canonical.com/ubuntu xenial partner

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted

deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe \#Added by software-properties

deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe

deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse

sudo apt-get update

sudo apt-get upgrade
```

### 卸载掉不常用App
```
* sudo apt-get remove unity-webapps-common
* sudo apt-get remove thunderbird（邮箱）
* sudo apt-get remove totem （视频播放器）
* sudo apt-get remove rhythmbox（音乐播放器）
* sudo apt-get remove empathy
* sudo apt-get remove brasero
* sudo apt-get remove simple-scan
* sudo apt-get remove gnome-mahjongg
* sudo apt-get remove aisleriot
* sudo apt-get remove gnome-mines
* sudo apt-get remove transmission-common
* sudo apt-get remove gnome-orca
* sudo apt-get remove webbrowser-app
* sudo apt-get remove gnome-sudoku
* sudo apt-get remove libreoffice-common
```
### 安装常用App
```
* sudo apt-get install wget
* sudo apt-get install vim
* sudo apt-get install git
* sudo apt-get install vpnc
* sudo apt-get install axel
* sudo apt-get install openssh-server
* sudo apt-get install cmake qtcreator
* sudo apt-get install exfat-fuse
* sudo apt-get install lnav
```
### 安装java8
```
* sudo add-apt-repository ppa:webupd8team/java
* sudo apt-get update
* sudo apt-get install oracle-java8-installer
* java -version
* sudo rm /usr/share/upstart/sessions/jayatana.conf
```
### 其他:
```
* 搜狗
* wps
* atom
* Chrome
```
### Anaconda

```
* wget https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86\_64.sh
* bash Anaconda3-5.0.1-Linux-x86\_64.sh
*
* echo 'export PATH="~/anaconda2/bin:$PATH"' \>\> ~/.bashrc
* source ~/.bashrc

* \# 创建python虚拟环境
* conda create --name python3 python=3
*
* \# 启用虚拟环境
* source activate python3 \# for Linux & Mac
*
* \# 验证python版本
* python —version
*
* \# 退出虚拟环境
* source deactivate python34 \# for Linux & Mac
*
* \# 删除虚拟环境
* conda remove --name python3 —all
*
* \# 查看安装的所有包
* conda list
*
* \# 安装python包
* conda install pygame
```
### pyqt5:

```
下载安装qt

http://download.qt.io/official\_releases/online\_installers/

wget http://download.qt.io/official\_releases/online\_installers/qt-unified-linux-x64-online.run

运行:

qt-unified-linux-x64-online.run

安装pyqt5:

pip install pyqt5
```
