---
layout:     post
title: 将文件编码成base64字符串
date: '2017-09-28'
description:
categories:
    - golang
tags:
    - 编码
    - base64

---

```go
package main

import (
	"fmt"
	"io/ioutil"
	"encoding/base64"
)

func main() {
	bytes, err := ioutil.ReadFile("/file/path/to/encode")
	if err != nil {
		panic(err)
	}
	str := string(base64.StdEncoding.EncodeToString(bytes))
	fmt.Println(str)
}
```

