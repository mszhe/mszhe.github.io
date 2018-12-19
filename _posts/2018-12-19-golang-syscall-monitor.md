---
layout:     post
title:      Golang syscall monitor
subtitle:   
date:       2018-12-19
author:     mszhe
catalog:    true
categories:
    - 开发语言
tags:
    - golang
    - syscall
    - dtruss
    - strace
    - 用户空间
    - 系统空间
---

 - 进程不能直接访问硬件设备，当进程需要访问硬件设备(比如读取磁盘文件，接收网络数据等等)时，
 必须由用户态模式切换至内核态模式，通过系统调用访问硬件设备。
 - strace可以跟踪到一个进程产生的系统调用,包括参数，返回值，执行消耗的时间: linux平台
 - mac: dtruss(dtruss -p [PID])
 
 
 - 用户空间：就是用户进程所在的内存区域
 - 系统空间：就是操作系统占据的内存区域