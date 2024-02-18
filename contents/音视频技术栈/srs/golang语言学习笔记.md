https://www.topgoer.com/ 

1.首先开启go module

```
go env -w GO111MODULE=on     // Windows  export GO111MODULE=on        // macOS 或 Linux
```

2.配置goproxy:

阿里配置：

```
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/       // Windows  
export GOPROXY=https://mirrors.aliyun.com/goproxy/          // macOS 或 Linux
```

七牛云配置：

```
go env -w GOPROXY=https://goproxy.cn      // Windows  
export GOPROXY=https://goproxy.cn         // macOS 或 Linux
```



注意：

Go 1.13设置了默认的GOSUMDB=[sum.golang.org](https://links.jianshu.com/go?to=http%3A%2F%2Fsum.golang.org)，是用来验证包的有效性。这个网址由于墙的原因可能无法访问，所以可以使用下面命令来关闭：

```
go env -w GOSUMDB=off // Windows  
export GOSUMDB=off // macOS 或 Linux
```

channel是一种特殊的（引用）类型，声明格式如下，通道类型的空值是nil
```less
var ch1 chan int   // 声明一个传递整型的通道
var ch2 chan bool  // 声明一个传递布尔型的通道
var ch3 chan []int // 声明一个传递int切片的通道

fmt.Println(ch1)   // <nil>
```

声明的通道需要使用make函数初始化之后才能使用
```less
make(chan 元素类型, [缓冲区大小])
ch1 := make(chan int)
ch1 := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
```

channel有发送（send）、接收(receive）和关闭（close）三种操作，发送和接收都使用`<-`符号。
```less
// 发送
ch <- 10 // 把10发送到ch中

// 接收
x := <- ch // 从ch中接收值并赋值给变量x
<-ch       // 从ch中接收值，忽略结果

// 关闭
close(ch)
```

关于关闭通道需要注意的事情是，只有在通知接收方goroutine所有的数据都发送完毕的时候才需要关闭通道。通道是可以被垃圾回收机制回收的，它和关闭文件是不一样的，在结束操作之后关闭文件是必须要做的，但关闭通道不是必须的。

关闭后的通道有以下特点：
1.对一个关闭的通道再发送值就会导致panic。
2.对一个关闭的通道进行接收会一直获取值直到通道为空。
3.对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值。
4.关闭一个已经关闭的通道会导致panic。



