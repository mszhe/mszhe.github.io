---
layout:     post
title: 修改github的push方式
date: '2017-10-24'
description:
categories:
    - github
tags:
    - push
---

## 现象
github每次push的时候，都要输入用户名密码


## 解决：将push方式由https方式修改为git方式
```bash
git remote -v
git remote rm origin
git remote add origin git@github.com:xxx/xxx.git
git push origin
```