---
layout:     post
title:      ArrayList新增元素报UnsupportedOperationException错
subtitle:   
date:       2018-12-28
author:     mszhe
catalog:    true
categories:
    - Java
tags:
    - ArrayList
    - UnsupportedOperationException
    - Exception
    - 异常
---

- 代码
```java
public static void main(String[] args) {
    List<String> list = Arrays.asList(new String[]{});
    list.add("1");
    list.add("2");
}
```

- 报错
```java
Exception in thread "main" java.lang.UnsupportedOperationException
	at java.util.AbstractList.add(AbstractList.java:148)
	at java.util.AbstractList.add(AbstractList.java:108)
	at com.yeting.tarot.service.UserService.main(UserService.java:193)
```

- 原因

    Arrays.asList内部ArrayList是内部单独继承实现的，没有实现add方法，跟java.util.ArrayList不是一回事，如图:
    
    <img src="/img/20181228/01.png" alt="" width="600">