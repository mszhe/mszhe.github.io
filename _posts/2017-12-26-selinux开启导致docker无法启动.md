---
layout:     post
title: selinux开启导致docker无法启动
date: '2017-12-26'
description:
categories:
    - docker
    - linux
tags:
    - selinux
    - centos

---


## 现象：docker突然无法启动，错误如下
<img src="/img/2017-12-26-selinux开启导致docker无法启动/docker启动错误.png" alt="" width="600">

## 解决：禁用selinux

- 经查selinux处于启动状态

<img src="/img/2017-12-26-selinux开启导致docker无法启动/selinux状态.png" alt="" width="600">

- 禁用selinux`vim /etc/selinux/config`

<img src="/img/2017-12-26-selinux开启导致docker无法启动/禁用selinux.png" alt="" width="600">

- 重启机器,`reboot`, 再看selinux状态`selinux -v`

<img src="/img/2017-12-26-selinux开启导致docker无法启动/查看selinux状态.png" alt="" width="300">

- 再次启动docker, 正常启动!