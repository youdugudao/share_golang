# go协程通讯

### 无缓冲channel
Channel是连接并行协程(goroutine)的通道。你可以向一个通道写入数据然后从另外一个通道读取数据。
```go
package main

import "fmt"

func main() {
    // 使用`make(chan 数据类型)`来创建一个Channel, Channel的类型就是它们所传递的数据的类型
    messages := make(chan string)
    // 使用`channel <-`语法来向一个Channel写入数据, 这里我们从一个新的协程向messages通道写入数据ping
    go func() { 
        messages <- "ping" 
    }()
    // 使用`<-channel`语法来从Channel读取数据
    msg := <-messages
    fmt.Println(msg)
}
```
运行结果：

```
> go run test1.go
ping
```

当我们运行程序的时候，数据ping成功地从一个协程传递到了另外一个协程。
默认情况下，协程之间的通信是同步的，也就是说数据的发送端和接收端必须配对使用。
Channel的这种特点使得我们可以不用在程序结尾添加额外的代码也能够获取协程发送端发来的信息。因为程序执行到msg:=<-messages的时候被阻塞了，直到获得发送端发来的信息才继续执行。
当然，

### 有缓冲channel

```go
package main

import "fmt"

func main() {
    messages := make(chan string, 2)
    go func() { 
        messages <- "ping1"
        messages <- "ping2"
    }()

    msg1 := <-messages
    msg2 := <-messages
    fmt.Println(msg1,msg2)
}
```

运行结果：

```
> go run test1.go
ping1 ping2
```