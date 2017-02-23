# go的结构体

go的结构体的类似java的类，有公有和私有变量， 但是声明方式不同，
- 首字母大写即为公有，外界可以访问
- 首字母小写即为私有，外界不能访问
```go
type person struct {
    Name string
    Age  int
}
// 使用
a := person{
    Name: "zhangsna",
    Age:  15,
}
```
如果想为person类型的结构体添加方法，也很简单
```
func (p *person)SetName(name string)(err error) {
    p.Name = name
    return
}
```

# go 接口

go 的接口实现是隐性的， 只要实现了接口的方法，就算实现了该接口， 灵活性很高
```go
type USB interface {
	Name() string
	Connect()
}

// PhoneConnecter只要有Name()和Connect()函数即实现了该接口
type PhoneConnecter struct {
	name string
}

func (pc PhoneConnecter) Name() string {
	return pc.name
}

func (pc PhoneConnecter) Connect() {
	fmt.Println(pc.name)
}

func main() {
	var a USB
	a = PhoneConnecter{"PhoneConnecter"}
	a.Connect()
}
```