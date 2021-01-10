并发：轮流执行

并行：同时进行



# goroutine 协程

在通常来说分进程和线程，但在Go中，分主线程和协程，这个主线程可以理解成线程或者进程，协程比线程还轻量

## Go协程的特点

- 有独立的栈空间
- 共享程序堆空间
- 调度由用户控制
- 协程是轻量级的线程

主线程要是执行完成了，即使协程没执行完也会退出

- 主线程是一个物理线程，直接坐拥在cpu上。是重量级的，非常消耗cpu资源
- 协程是从主线程开启的，是轻量级的线程，是逻辑态，对资源消耗很小
- Golang的协程机制是重要的特点，可以轻松开启上万个协程。

## Channel

### 为什么要用channel

全局变量加锁的话，需要让主线程等待，等待时间长了浪费时间，等待短了可能goroutine还没完主线程就结束了

### channel简介

- 本质是一个数据结构-队列
- 数据先进先出
- 线程安全
- channel具有类型，string类型的channel就只能存放string数据

### channel的声明

```go
var 名字 chan 要存的数据类型 
var intChan chan int
intChan = make(chan int, 3)
```

用之前也得make

向管道里放数据

```go
intChan<- 10
num:= 20
intChan<-num
```

管道不能自动扩容，加多了报错

```go
var outnum int
outnum, ok = <-intChan
```

从管道取数据出来，后面ok是读取成功与否

### channel的关闭

用内置函数claose关闭后，不能再写入，还能读取

关闭后也不能遍历

### channel遍历

只能用for-range遍历，需要channel关闭状态

不能用普通for循环的原因：每次取出，管道长度会变

### channel只读/只写

```go
var onlyRead <-chan int
var onlyWrite chan<- int
```



#  反射

~~没看，以后补~~

 