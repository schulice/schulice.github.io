---
title: "WSL Configuration"
description: 
date: 2024-08-17T16:57:55+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: fasle
---

# TL;DR

本文介绍wsl的一些实用配置。

# 安装

WSL安装见[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/)

建议阅读文档中的其余内容来学习维护wsl的知识。

建议使用Debian或者Ubuntu，微软的支持比较好。

# 配置

这里列举出配置供参考。

## .wslconf

具体选项内容对照文档高级wsl配置一节。

不建议使用稀疏磁盘（issue中有不少被破坏文件系统的），可以定期使用hyperv的磁盘管理工具释放多余空间（见文档）。

```
[wsl2]
processors=4
memory=8GB
swap=4GB
vmIdleTimeout=30000
guiApplications=true
nestedVirtualization=true # 嵌套虚拟化
networkingMode=mirrored
dnsTunneling=true
firewall=true

[experimental]
autoMemoryReclaim=dropcache # 可以在 gradual 、dropcache 、disabled 之间选择
# sparseVhd=true # maybe cause some error
hostAddressLoopback=true # 外部可访问wsl内部服务
```

## wsl.conf

开启systemd支持，command 处设置kvm组权限，以便发行版使用kvm。

```
[boot]
systemd=true
command=/bin/bash -c 'chown -v root:kvm /dev/kvm && chmod 660 /dev/kvm'

[interop]
appendWindowsPath=false # need add path on profile
```

## /etc/profile

隔离wsl与windows的path后添加部分实用app到PATH中。

```
# windows path
export PATH="$PATH:/mnt/c/Users/schulice/AppData/Local/Microsoft/WindowsApps"
export PATH="$PATH:/mnt/c/Program Files/Docker/Docker/resources/bin"
export PATH="$PATH:/mnt/c/Windows"
export PATH="$PATH:/mnt/c/Program Files/Microsoft VS Code/bin"
```

# windows启动时同时启动wsl

放在 C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup

```
set ws=wscript.CreateObject("wscript.shell")
ws.run "wsl -d Debian", 0
```

Debian处更改为发行版名称，此后

# 维护

## wsl启动失败

关闭后再开启wsl，虚拟机平台等windows功能，有概率恢复正常，具体原因不明。

## wsl空间使用过多

[RTFM](https://learn.microsoft.com/zh-cn/windows/wsl/disk-space)

## 其他的虚拟机

可以使用windows自带的hyperv管理作为hypervisor，有一些网络nat的设置需要具体的指令（gui中没有）。

## CUDA toolkit

阅读cuda官方文档。只需要安装toolkit包，已有驱动支持（与wsl的驱动有冲突）。


## docker

安装docker-desktop，后续使用阅读docker文档。