---
layout:     post
title:      mysql docker 安装
subtitle:   
date:       2019-04-12
author:     mszhe
catalog:    true
categories:
    - docker
    - mysql
tags:
    - docker
    - mysql
---

```bash
sudo docker run -p 3306:3306 --name wp_mysql -v /root/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=199529 -d mysql:8.0

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '199529';

FLUSH PRIVILEGES;
```