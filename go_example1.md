# 一个简单的go程序

## go的工作目录
>go的环境变量， mac或者Ubuntu的安装都很简单。但是go有一个有意思的地方。
go规定了go的应用都放到go的工作目录中。

go的环境变量查看很简单，`go env` 即可
```
GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/chimps/web_back/go"
GORACE=""
GOROOT="/usr/local/Cellar/go/1.7.4_1/libexec"
GOTOOLDIR="/usr/local/Cellar/go/1.7.4_1/libexec/pkg/tool/darwin_amd64"
CC="clang"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/w0/c49mvxnd0q1gpsb6zwb0kp3c0000gn/T/go-build965260476=/tmp/go-build -gno-record-gcc-switches -fno-common"
CXX="clang++"
CGO_ENABLED="1"
```
gopath 即为工作目录，运行`ls $GOPATH`便会发现有三个目录
- bin 存放可执行文件，都是系统编译好的可执行文件
- pkg 
- src 即为存放我们的代码的地方

## 我们正式写一个小的go程序
文件名: `test1.go`
```go
package main

import (
	"time"
	"fmt"
)

func main() {
	var a int
	a = 90
	t := time.Duration(a) * time.Second
	fmt.Println(t)
}
```
运行 `go run test1.go`