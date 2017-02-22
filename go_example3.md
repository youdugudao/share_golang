# goroutine协程

## 协程的特点：
>- Goroutine是Go并行设计的核心。goroutine说到底其实就是线程，但是它比线程更小，十几个goroutine可能体现在底层就是五六个线程，Go语言内部帮你实现了这些goroutine之间的内存共享。执行goroutine只需极少的栈内存(大概是4~5KB)，当然会根据相应的数据伸缩。也正因为如此，可同时运行成千上万个并发任务。goroutine比thread更易用、更高效、更轻便。
- Goroutine奉行通过通讯来共享内存，而不是共享内存来通讯

## 并行功能
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go run("zhangsan")
	for i := 0; i < 5; i++ {
		fmt.Println("lisi", i)
	}
    // 如果此时不sleep1秒，程序执行到这main函数就结束了，run函数可能还没有执行完毕
	time.Sleep(time.Second * 1)
}

func run(str string) {
	for i := 0; i < 5; i++ {
		fmt.Println(str, i)
	}
}
```
当然， 此时可以设置go并发时利用的cpu核数。
这便是一个简单的go并发程序， 很简单， 利用`go`关键字即可
go协程之间如果要实现同步机制只需要`channel`便可解决，咱们先放一放。先来看看goroutine的设计