# go协程的应用 - net

### golang的标准包
golang的标准包的代码设计的很不错，golang对于标准包的审核还是挺好的。
对于开发 webapi golang的标准包就足以。基本不需要什么框架
推荐github用户codegangsta的gitbook项目：[Building Web Apps with Go](https://www.gitbook.com/book/codegangsta/building-web-apps-with-go/details)

### 一个简单的 webapi demo
```go
package main

import (
	"net/http"
)

func main() {
	http.HandleFunc("/markdown", GenerateMarkdown)
	http.HandleFunc("/", func(rw http.ResponseWriter, r *http.Request) {
		rw.Write([]byte("hello world"))
	})
	http.ListenAndServe(":8080", nil)
}

func GenerateMarkdown(rw http.ResponseWriter, r *http.Request) {
	rw.Write([]byte("markdown"))
}
```

### 我们来看一下运行的原理

首先， http包HandleFunc在源码中是这样写的，第二个参数只要是实现了`func(ResponseWriter, *Request))`的函数即可
```go
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}
```

然后我们来看一下http包的`ListenAndServe`源码
```go
func ListenAndServe(addr string, handler Handler) error {
    // 初始化server
	server := &Server{Addr: addr, Handler: handler}
    // 调用ListenAndServe方法
	return server.ListenAndServe()
}
```

实际上此处重点是调用了server的`ListenAndServe`方法
我们顺藤摸瓜，看一下ListenAndServe源码：
```go
func (srv *Server) ListenAndServe() error {
	addr := srv.Addr
	if addr == "" {
		addr = ":http"
	}
	ln, err := net.Listen("tcp", addr)
	if err != nil {
		return err
	}
	return srv.Serve(tcpKeepAliveListener{ln.(*net.TCPListener)})
}
```

这儿基本就明晓了，关键是Server的Serve方法
```go
func (srv *Server) Serve(l net.Listener) error {
	defer l.Close()
	if fn := testHookServerServe; fn != nil {
		fn(srv, l)
	}
	var tempDelay time.Duration // how long to sleep on accept failure

	if err := srv.setupHTTP2_Serve(); err != nil {
		return err
	}

	baseCtx := context.Background()
	ctx := context.WithValue(baseCtx, ServerContextKey, srv)
	ctx = context.WithValue(ctx, LocalAddrContextKey, l.Addr())
	for {
		rw, e := l.Accept()
		if e != nil {
			if ne, ok := e.(net.Error); ok && ne.Temporary() {
				if tempDelay == 0 {
					tempDelay = 5 * time.Millisecond
				} else {
					tempDelay *= 2
				}
				if max := 1 * time.Second; tempDelay > max {
					tempDelay = max
				}
				srv.logf("http: Accept error: %v; retrying in %v", e, tempDelay)
				time.Sleep(tempDelay)
				continue
			}
			return e
		}
		tempDelay = 0
		c := srv.newConn(rw)
		c.setState(c.rwc, StateNew) // before Serve can return
		go c.serve(ctx)
	}
}
```
>大概意思：判断是否有请求，如果有，则运行到 go c.serve(ctx)，从而产生一个新的goroutine
没有的话则 time.Sleep(tempDelay)，sleep的时间为`tempDelay = 5 * time.Millisecond`或者`tempDelay *= 2`
```go
package time
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)
```