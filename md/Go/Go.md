# Go

## 基础

### 安装

[参考](https://go.dev/doc/install)


### helloworld

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("Hello, World!")
	fmt.Println(rand.Intn(10))
}
```

### 初始化项目

```shell
#初始化
go mod init project-name
#设置国内镜像
go env -w GOPROXY=https://goproxy.cn,direct
#拉取模块
go mod tidy
#运行
go run .
```