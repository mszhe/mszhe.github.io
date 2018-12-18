---
layout:     post
title: spring service中获取session
date: 2016-08-10
categories:
    - java
tags:
    - spring
    - session
---

##### Spring 在service层中如何获取session？
```java
ServletRequestAttributes sAtts = (ServletRequestAttributes)RequestContextHolder.currentRequestAttributes();
HttpServletRequest req = sAtts.getRequest();
HttpSession session = req.getSession();
```
