--- 
layout: post
title: MqttPython库学习 
date: 2023-05-12
Author: Shengbin 
tags: [python]
comments: true
toc: true
---

mqtt协议一个在应用层的协议通常用于消息队列服务,通过TCP传输数据。
paho-mqtt是一个mqtt的python库。

## 服务端环境配置

服务器版本:Debian GNU/Linux 12

客户端版本:Arch Linux

```shell
apt install mosquitto
```
修改配置文件 /etc/mosquitto/mosquitto.conf 设置监听IP端口为所有IP均可访问与设置允许匿名

```shell 
# Place your local configuration in /etc/mosquitto/conf.d/
#
# A full description of the configuration file is at
# /usr/share/doc/mosquitto/examples/mosquitto.conf.example
 
listener 1883 0.0.0.0 
use_username_as_clientid false 
allow_anonymous true
pid_file /run/mosquitto/mosquitto.pid
 
persistence true
persistence_location /var/lib/mosquitto/
 
log_dest file /var/log/mosquitto/mosquitto.log
 
include_dir /etc/mosquitto/conf.d
```
重启服务,使配置文件生效。

```shell
systemctl restart mosquitto.service
```
验证，在客户端下载mqtt客户端

```
yay -S mosquito
```

```shell
mosquitto_sub -h 192.168.122.52 -t
 "#" -u wildcard -v
mosquitto_pub -h 192.168.122.52 -t "te12dqwdqw1w2st" -m "asdas"
```
## Python开发环境搭建

创建虚拟环境;
```
mkdir environment
python -m venv  
```
加载虚拟环境(具体命令见你的shell)

```
source  environment/bin/activate.fish
```
安装paho-mqtt包

```
pip install paho-mqtt
```



