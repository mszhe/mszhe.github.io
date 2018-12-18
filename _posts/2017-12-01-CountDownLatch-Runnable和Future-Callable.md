---
layout:     post
title: CountDownLatch-Runnable和Future-Callable
date: '2017-12-01'
description:
categories:
    - java
tags:
    - 高并发
    - 多线程
    - Runnable
    - Callable
---

## 线程池
```java 
@Autowired
private ThreadPoolTaskExecutor taskExecutor;
```

## CountDownLatch/Runnable
```java 
final int size = list.size();
final List<PaperDetail> details = new Vector(size);
final CountDownLatch latch = new CountDownLatch(size);
list.stream().forEach(paper -> taskExecutor.execute(() -> {
    details.add(getById(paper.getId()));
    latch.countDown();
}));
try {
    latch.await();
} catch (InterruptedException e) {
    Logs.L().error("paper list error: ", e);
    throw new HNException("内部异常");
}
```

## Future/Callable
```java 
List<PaperDetail> details = Lists.newArrayList();
List<Future<PaperDetail>> futures = Lists.newArrayList();
list.forEach(paper -> futures.add(taskExecutor.submit(() -> getById(paper.getId())))); // 匿名有返回值的线程Callable
for (int i = 0; i < futures.size(); i++) {
    try {
        details.add(futures.get(i).get(10, TimeUnit.SECONDS));
    } catch (Exception e) {
        Logs.L().error("paper list error: ", e);
    }
}
```