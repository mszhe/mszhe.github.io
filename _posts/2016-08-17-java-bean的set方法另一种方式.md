---
layout:     post
title: java bean的set方法另一种方式
date: '2016-08-17'
description:
categories:
    - java
tags:
    - bean
    - set方法

---

直接上示例：

``` java
private String name;
private int age;
private List<String> friends;
public Foo setName(String name) {
    this.name = name;
    return this;
}
public Foo setAge(int age) {
    this.age = age;
    return this;
}
public Foo setFriends(List<String> friends) {
    this.friends = friends;
    return this;
}
```

好处：

``` java
Foo foo = new Foo();
foo.setName("Tom")
   .setAge(25)
   .setFriends(Lists.newArrayList("Jack", "Lily"));
```