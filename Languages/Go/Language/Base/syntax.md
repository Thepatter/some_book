### Go 基础语法

#### 基础

##### 变量和声明

默认情况下，Go 会为变量分配默认值：`Integers` 默认为 `0`，`booleans` 默认为 `false`，`strings` 默认值是 `""`。

```go
// 声明与赋值
var power int = 9000
// 变量类型推断
power := 9000
// 多个变量同时赋值
name, power := "GoKu", 9000
```

在相同作用域下，相同的变量不能被声明两次，Go 不允许程序拥有未使用的变量，像 import 不能导入未使用的包一样。

初始化一个变量，`var NAME TYPE`，声明及赋值 `NAME := VALUE`，给之前已声明变量赋值 `NAME = VALUE`

字符串和字节数组紧密相关，可以轻松转换，字符串是不可变的，转换实际上创建了数据的副本。

```go
stra := "the spice must flow"
byts := []byte(stra)
strb := string(byts)
```

##### 运行 go 代码

* 运行

  ```shell
  go run main.go
  ```

* 编译

  ```shell
  go build main.go
  ```

在开发中，你既可以使用 `go run` 也可以使用 `go build`，但正式部署代码时，应该通过 `go build` 生成二进制文件并执行它。

在 go 中程序入口必须是 `main` 函数，并且在 `main` 包内。

##### 导入

go 有很多内建函数，可以在没有引用的情况下直接使用。`import` 关键字被用于声明文件中代码要使用的包

```go
import (
  "fmt",
  "os"
)
// 或者
import "fmt"
import "os"
```

##### 函数声明

```go
// 没有返回值
func log(message string) {}
// 一个返回值
func add(a int, b int) int {}
// 两个返回值
func power(name string) (int, bool) {}
```

调用函数

```go
value, exists := power("goku")
if exists == false {
    
}
// 将其他返回值赋给空白符_
_, exists := power("goku")
if exists == false {
    
}
```

函数是一种类型

```go
type Add func(a int, b int) int
// 使用
func process(adder Add) int {
    return adder(1, 2)
}
```

#### 结构体

##### 声明和初始化

不是面向对象语言，没有对象和继承，没有多态和重载。go 具有结构体，可以将一些方法和结构体关联。字段可以是任何类型，包含其他结构体，array，maps，interfaces，functions 等。

*定义结构体*

```go
type Saiyan struct {
    Name string
    Power int
}
```

*声明和初始化*

```go
goku := Saiyan {
	Name: "Goku",
	Power: 900
}
goku1 := Saiyan{}
goku1.Power = 9000
gok2 := Saiyan {"Goke", 9000}
```

可以使用 `&` 来获取指针，并使用指针修改原始内存地址值，使用 `*`  来声明指针参数。结构体没有构造器，可以创建一个返回所期望类型的实例函数（类似于工厂）

```go
// 返回结构体指针
func NewSaiyan(name string, power int) *Saiyan {
	return &Saiyan {
		Name: name,
		Power: power,
	}
}
// 返回结构体
func NewSaiyan(name string, power int) Saiyan {
	return Saiyan {
		Name: name,
		Power: power,
	}
}
```

内置 `new` 函数，使用它来分配类型所需要的内存。`new(x)` 与 `&x{}` 相同。

```go
goku := new(Saiyan)
// 等价
goku := &Saiyan{}
```

##### 结构体上函数

*方法关联在结构体上*

```go
type Saiyan struct {
    Name string
    Power int
}
func (s *Saiyan) Super() {
    s.Power += 10000
}
```

`*Saiyan` 类型是 `Super` 方法的接受者，调用 Super 方法

```go
goku := &Saiyan{"Goku", 9001}
goku.Super()
fmt.Println(goku.Power)
```

##### 组合

```go
type Person struct {
	Name string
}
func (p *Person) Introduce() {
	fmt.Printf("Hi, I'm %s\n", p,Name)
}
type Saiyan struct {
	*Person
	Power int
}
// 使用组合
goku := &Saiyun {
    Person: &Person{"Goku"},
    Power: 9001,
}
goku.Introduct()
```

#### 映射数组切片

##### 数组

go 中，数组长度是固定的，在声明一个数组时需要指定它的长度。索引从 0 开始。数组越界访问会报错。

```go
scores := [4]int{9001, 9002, 9999, 10000}
// 等价于
var scores [4]int
scores[0] = 900
// 遍历
for index, value := range scores {
    
}
```

##### 切片

在 go 语言中，很少直接使用数组。取而代之的是使用切片。切片是轻量的包含并表示数组的一部分结构。

```go
// 创建切片
scores := []int{1, 4, 2, 5, 3}
// 创建长度是 10，容量是 10 的切片
scores := make([]int, 10)
// 长度是 0， 容量是 10
scores := make([]int, 0, 10)
```

长度是切片的长度，容量是底层数组的长度，在使用 `make` 创建切片时，可以分别指定切片的长度和容量。使用 `append` 扩充切片，会自动扩展底层数组。

```go
scores := make([]int, 0, 10)
// 扩展切片
scores = append(scores, 5)
```

`[x:]` 是从 X 到结尾的简写，`[:x]` 是从开始到 X 的简写，Go 不支持负数索引。

##### 映射

类似 `hashtable`，可以获取，设置和删除其中的值，映射和切片一样，使用 `make` 方法来创建。

```go
func main() {
    lookup := make(map[string]int)
    lookup["goku"] = 9001
    power, exists := lookup["vegeta"]
    # 等价于
    lookup := map[string]int{
        "goku": 9001,
        "gohan": 2044
    }
    fmt.Println(power, exists)
}
// 获取映射的键的数量
total := len(lookup)
// 删除键对应的值
delete(lookup, "goku")
```

映射是动态变化的，如果事先知道映射会有多少键值，定义一个初始大小将会帮助改善性能，迭代映射是没有顺序的，每次迭代查找会随机返回键值对

```go
// 指定初始大小
lookup := make(map[string]int, 100)
// 迭代映射
for key, value := range lookup {
    
}
```

#### 代码组织和接口

##### 包

在 Go 中，包名遵循 Go 项目的目录结构。命名一个包时，可以通过 `package` 关键字，提供一个值，而不是完整的层次结构。当导入一个包时，需要指定完整路径。导入包不能形成循环导入。Go 用了一个简单的规则定义什么类型和函数包外可见：**如果类型或函数名称以一个大写字母开始，它就具有了包外可见性。如果以一个小写字母开始，它就不可以，对结构体一样，如果一个字段名以一个小写字母开始，只有包内的代码可以访问它们**。可以使用 `models.func` 访问包内函数。

可以使用 `get` 子命令来获取第三方库。`go get` 支持各种协议，`go get` 获取远端的文件并把它们存储在工作区中。如果在一个项目内使用 `go get`，它将浏览所有文件，查找 `imports` 的第三方库然后下载它们。使用 `go get -u` 更新所有包，或 `go get -u FULL_PACKAGE_NAME` 更新一个具体的包。

##### 接口

接口定义了合约但并没有实现的类型

```go
type Logger interface {
	Log(message string)
}
// 在结构中使用
type Server struct {
    logger Logger
}
// 函数参数
func process(logger Logger) {
    logger.log("hello!")
}
```

内部有一个没有任何方法的空接口：`interface{}`。空接口时隐式实现的，因此每种类型都满足空接口契约。为了将一个接口变量转化为一个显式的类型，可以用 `.(TYPE)`

```go
func add(a interface{}, b interface{}) interface{} {
    return a.(int) + b.(int)
}
```

##### 风格

在一个项目内的时候，可以运用格式化规则到这个项目及其子项目

```go
go fmt ./..
```

#### 并发

##### go 协程

使用 `go` 关键字然后使用想要执行的函数，也可以之间使用匿名函数

```go
go func() {
    fmt.Println("processing")
}
func process() {
    fmt.Println("Processing")
}
go process()
```

协程易于创建且开销很小，多个协程将会在同一个线程上运行。主进程在退出前协程才会有机会运行（主进程在退出前不会等待全部协程执行完毕）

##### 同步

常规操作是使用互斥量（`sync.Mutex`），互斥量序列化会锁住锁下的代码访问。通道在共享不相关数据的情况下，让并发编程变得更健壮。通道是协程之间用于传递数据的共享管道。一个协程可以通过一个通道向另外一个协程传递数据。在任意时间点，只有一个协程可以访问数据

一个通道，和其他任何变量一样，都有一个类型。这个类型是在通道中传递的数据的类型

```go
// 创建一个通道用于传递一个整数
c := make(chan int)
// 使用
func worker(c chan int) {}
```

通道只支持两个操作：接收和发送。接收和发送操作是阻塞的。当我们从一个通道接收的时候，`goroutine` 将会直到数据可用才会继续执行。

```go
// 向通道发送数据
CHANNEL <- DATA
// 从通道接收数据
VAR := <-CHANNEL
```

如果没有 worker 可用，想去临时存储数据在某些队列中。通道内建这种缓冲容量，使用 `make` 创建通道时，可以设置通道的长度

```go
c := make(chan int, 100)
```

使用 `select` ，可以提供当通道不能发送数据的时候处理代码。

```go
for {
    select {
        case c <- rand.Int():
        // 可选代码
        default:
        // 可以留空静默删除数据
    }
    time.Sleep(time.Millisecond * 50)
}
```

`select` 的主要目的是管理多个通道，`select` 将阻塞直到第一个通道可用。如果没有通道可用，如果提供了`default` ，将会执行。如果多个通道都可用，随机挑选一个，如果没有 default，select 将会阻塞

```go
for {
    select {
        case c <- rand.Int():
        case <-time.After(time.Millisecond * 100):
        fmt.Println("timed out")
    }
    time.Sleep(time.Millisecond * 50)
}
```

`time.After` 返回了一个通道，这个通道可以在指定时间之后被写入