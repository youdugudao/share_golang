## 有什么用途

* 服务器编程，以前你如果使用C或者C++做的那些事情，用Go来做很合适，例如处理日志、数据打包、虚拟机处理、文件系统等。
* 网络编程，这一块目前应用最广，包括Web应用、API应用、下载应用
    > nsq：bitly开源的消息队列系统，性能非常高，拘束目前他们每天处理数十亿条的消息
* 内存数据库，前一段时间google开发的groupcache，couchbase的部分组建
    > nsq：bitly开源的消息队列系统，性能非常高，目前他们每天处理数十亿条的消息
* 云平台，目前国外很多云平台在采用Go开发，CloudFoundy的部分组建，前VMare的技术总监自己出来搞的apcera云平台。
    > docker

## 有什么优势

1. 部署简单
Go 编译生成的是一个静态可执行文件，除了 glibc 外没有其他外部依赖。这大大减少了服务器的维护成本。

2. 学习曲线
它包含了类C语法、GC内置和工程工具。这一点非常重要，因为Go语言容易学习，所以一个普通的大学生花一个星期就能写出来可以上手的、高性能的应用。在国内大家都追求快，这也是为什么国内Go流行的原因之一。

3. 开发效率
它包含了类C语法、GC内置和工程工具。这一点非常重要，因为Go语言容易学习，所以一个普通的大学生花一个星期就能写出来可以上手的、高性能的应用。在国内大家都追求快，这也是为什么国内Go流行的原因之一。

4. 出身名门、血统纯正
google对这门语言还是非常看重的，目前go已经出到了1.8版本。

5. 自由高效：组合的思想、无侵入式的接口
Go语言可以说是开发效率和运行效率二者的完美融合，天生的并发编程支持。Go语言支持当前所有的编程范式，包括过程式编程、面向对象编程以及函数式编程。程序员们可以各取所需、自由组合、想怎么玩就怎么玩。

6. 强大的标准库
这包括`互联网应用`、系统编程和`网络编程`。Go里面的标准库基本上已经是非常稳定了，特别是我这里提到的三个，`网络层`、系统层的库非常实用。

7. 简单的并发
单个 Go 应用也能有效的利用多个 CPU 核，go的并发很有意思，他是自己设计的`goroutine`, 我们就叫他为go的协程，协程之间的交互，使用channel完成。
传统多线程大多使用的共享内存（虽然我也不大懂），而go的协程使用通讯。
