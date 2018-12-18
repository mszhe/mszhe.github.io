---
layout:     post
title: java缓存策略
date: '2016-12-22'
description: java的Byte,Short,Integer,Long,Float,Double
categories:
    - java
tags:
    - java缓存
    - autoboxing
    - 自动装箱
    - IntegerCache  
    - ByteCache     
    - ShortCache    
    - LongCache     
    - CharacterCache

---

## 引入版本、好处、范围

- 引入版本：java5引入
- 好处：节省内存、提高性能
- 范围：仅在自动装箱（autoboxing）的时候有用，使用构造器创建的 Integer 对象不能被缓存
- 自动装箱（autoboxing）: `Integer a = 10; ` 相当于 `Integer b = Integer.valueOf(10);`


## 示例

```java
package com.mszhe;

public class Demo {
    public static void main(String[] args) {
        Integer a = 1;
        Integer b = 1;
        Integer c = 512;
        Integer d = 512;
        System.out.println(a == b); // true
        System.out.println(c == d); // false
    }
} 
```

## 源码

```java 
/**
 * Cache to support the object identity semantics of autoboxing for values between
 * -128 and 127 (inclusive) as required by JLS.
 *
 * The cache is initialized on first usage.  The size of the cache
 * may be controlled by the {@code -XX:AutoBoxCacheMax=<size>} option.
 * During VM initialization, java.lang.Integer.IntegerCache.high property
 * may be set and saved in the private system properties in the
 * sun.misc.VM class.
 */

private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static final Integer cache[];

    static {
        // high value may be configured by property
        int h = 127;
        String integerCacheHighPropValue =
            sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        if (integerCacheHighPropValue != null) {
            try {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
            } catch( NumberFormatException nfe) {
                // If the property cannot be parsed into an int, ignore it.
            }
        }
        high = h;

        cache = new Integer[(high - low) + 1];
        int j = low;
        for(int k = 0; k < cache.length; k++)
            cache[k] = new Integer(j++);

        // range [-128, 127] must be interned (JLS7 5.1.7)
        assert IntegerCache.high >= 127;
    }

    private IntegerCache() {}
}
```

## 其它

- IntegerCache  [-128, 127] 可修改:
    - `-XX:AutoBoxCacheMax=NEWVALUE` 
    - `-Djava.lang.Integer.IntegerCache.high=NEWVALUE`
- ByteCache     [-128, 127] 不可修改
- ShortCache    [-128, 127] 不可修改
- LongCache     [-128, 127] 不可修改
- CharacterCache[0, 127]    不可修改
