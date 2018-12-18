---
layout:     post
title: golang_panic_recover
date: '2017-09-28'
description:
categories:
    - golang
tags:
    - err
    - panic
    - recover

---

```go
package main

import (
	"errors"
	"log"
	"fmt"
)

func main() {
	defer func() {
		if err := recover(); err != nil {
			switch x := err.(type) {
			case string:
				err = errors.New(x)
			case error:
				err = x
			default:
				err = errors.New("Unknow panic ")
			}
			log.Fatal(err)
		}
	}()
	panic(1)
	fmt.Println(12) // not executed
}
```

