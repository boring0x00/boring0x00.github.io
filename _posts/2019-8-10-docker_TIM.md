---
layout: post
title: "Tim和WEChat在Linux的一种解决方式"
subtitle: '通过docker 安装 Tim'
author: "boring"
header-img: "img/claudio-testa-306745-unsplash.jpg"
tags:
  - Tim
  - Docker
---

#### 1：安装docker
   由于debian发行版不支持docker-ee，所以我们安装docker-ce。 
   1. parrot之前是有源的，目前版本没有，所以首先添加额外源。 
   
```bash
sudo echo -e "# docker-ce \n deb [arch=amd64] https://download.docker.com/linux/debian stretch stable" >> /etc/apt/sources.list
```
 
   2. 之后就是 导入官方的秘钥， 再更新源，直接安装便可。 
   
```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -

sudo apt update

sudo apt install docker-ce
```
   3. 如果后面docker下载镜像速度太慢，可以配置永久加速
   
```bash
sudo pluma /etc/docker/daemon.json

# 然后把下面这段复制进去

{
 "registry-mirrors": ["https://registry.docker-cn.com"]
}

```
   4.  重启docker服务.
   
```bash
sudo systemctl restart docker.service
```
   5. 通过运行hello world验证是否安装成功 #可以通过配置用户组，使得使用docker不用加sudo，这里没有详细讲了
   
```bash
sudo docker run hello-world
```

#### 2. 创建tim容器
1. 第一步 安装镜像

```bash
sudo docker pull bestwu/qq
```
2. 第二步 创建一个yml文件。

```bash
pluma docker-tim.yml


# 然后把下面的复制进去
version: '2'
services:
 qq:
   image: bestwu/qq:office    # 后面这个 office 改成 latest ， 登录的就是QQ，否则是Tim
   container_name: qq
   devices:
     - /dev/snd #声音
   volumes:
     - /tmp/.X11-unix:/tmp/.X11-unix
     - $HOME/TencentFiles:/TencentFiles
   environment:
     - DISPLAY=unix$DISPLAY
     - XMODIFIERS=@im=fcitx #中文输入
     - QT_IM_MODULE=fcitx
     - GTK_IM_MODULE=fcitx
     - AUDIO_GID=29 # 可选 (29 parrotsec) 主机audio gid 解决声音设备访问权限问题
     - GID=$GID # 可选 默认1000 主机当前用户 gid 解决挂载目录访问权限问题
     - UID=$UID # 可选 默认1000 主机当前用户 uid 解决挂载目录访问权限问题
```
3. 第三步 安装docker-compose

```bash
sudo apt install docker-compose
```
4. 第四步 执行文件

```bash
sudo docker-compose -f docker-tim.yml up

# 之后每次启动只需要 sudo docker start qq
```
   
    
