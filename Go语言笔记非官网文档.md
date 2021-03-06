> [B站上的教程的笔记](https://www.bilibili.com/video/BV1qt41147DH?p=174)

[TOC]



# 基础

### 多重赋值方便值的交换

```go
var a int =100
var b int =200
b, a = a, b
fmt.Println(a, b)
```

### 匿名变量 _

匿名变量的特点是一个下画线“_”，“_”本身就是一个特殊的标识符，被称为空白标识符。它可以像其他标识符那样用于变量的声明或赋值（任何类型都可以赋值给它），但任何赋给这个标识符的值都将被抛弃，因此这些值不能在后续的代码中使用，也不可以使用这个标识符作为变量对其它变量进行赋值或运算。使用匿名变量时，只需要在变量声明的地方使用下画线替换即可。

**就是能操作但没用，留不住值。**	

### 内存逃逸

[推荐篇写得好的](https://www.cnblogs.com/iQXQZX/p/14032884.html)

### 值传递与引用传递

- **值类型**：基本数据类型int，float，bool，string，数组和结构体struct

  变量直接贮存值，内存通常在栈中分配

- **引用类型**：指针，slice切片，map，管道chan，interface

  变量储存的是一个地址，地址对应空间才储存真正的数据，内存通常在堆上分配，当没有任何变量引用这个地址时，该地址对应的数据空间就变成一个垃圾，由GC来回收

### 读取当前时间

```go
now := time.Now()
fmt.Println(now.Year(),now.Month(),now.Day(),now.Hour(),now.Minute(),now.Second(),now.Local())
```

年月日时区毫秒啥都有

### 格式化输出日期

```go
	fmt.Println(now.Format("2006-01-02 15:04:05"))
	fmt.Println(now.Format("2006!01?02 15!!04!.05"))
	
	输出：
	2021-01-05 17:31:05
	2021!01?05 17!!31!.05
```

可以看出，中间的分割符号可以自己随便定义，但是数字不许改，不然没法格式化

#### Unix和UnixNano时间戳

是从1970年1月1号开始到现在的秒和纳秒，意义不重要

**主要是用来获取随机数字**

### 错误处理

使用defer➕recover

recover是内置函数，只要有异常会自动捕获，判断recover是不是空，如果不是那就说明有异常

在函数内添加

```go
defer func(){
	err:=recover()
	if err!=nil{
		fmt.Println("err=",err)
	}
}
```

#### 自定义错误

```go
errors.New("你自己定义的错误")
```

### 数组

```go
var intArr [n]int
var 数组名  [大小]数据类型
```

##### 初始化

```go
var numArr = [3]int{1,2,3}
var numArr = [...]int{9,1,12}

//按照下标给值
var numArr = [...]int{1:90,2:13,0:12}
```

##### 遍历

类似Java的for-each循环

```go
for index, value := range arrayInt{
	fmt.Println(index,value)
}
```

不想要index就用_占用，index和value可以用别的词换

##### 数组的赋值

数组默认是值传递，可以用指针提高效率

### 切片

是一个引用类型

底层来说是一个struct结构体

在内存中 有三部分  ：指向的地址，长度，容量

#### 三种使用切片的方式

- 引用一个已创建好的数组

- make一个

  ```go
  var slice []int = make([]int,4,10)
  ```

  三个参数，切片类型，切片大小，容量（可以不写，大于等于长度）

- 用类似数组的方式

  ```go
  var slice []string = []string{"Tome","Jerry","Marry"}
  ```

  **跟数组定义的方式要区分开！数组一定要在方括号里写大小，或者是三个点，下面是数组**

  ```go
  var numArr = [...]int{9,1,12}
  ```

**引用数组和make方式的区别**：

​		引用方式所引用的数组，是事先存在的，程序员可见

​		make方式创建的数组，底层创建维护的，程序员看不见

#### 切片遍历

跟数组遍历一样

#### 切片可以继续切片

#### 切片的append

```go
slice2 = append(slice1,100,200,300)
```

这样slice1本身没有变化，append会返回一个新的slice

```go
silce3 = append(slice1,slice2...)
```

切片后追加切片，最后三个点，代表是切片类型不是别的，不能省略



### Map

定义一个map的语法

```go
var 名字 map[keytype]valuetype
```

key通常是int和string，千万不能是slice，map和function，因为无法用==判断

声明一个key为int，value为string类型的map

```go
var mymap map[int]string
```

声明是不会分配内存的，初始化需要make，分配内存后才能赋值和使用。所以现在这个mymap是不能使用的，需要下面处理

```go
mymap = make(map[int]string,10)
```

10是大小

**Map中key是无序的，且不重复**

Map可以先声明再make也可以直接make出来，或者直接赋值，底层会自动make

```go
cities := make(map[string]string)
heroes := map[string]string{
	"num1":"张柏芝"
	"num2":"John"
	"num3":"Tom"
}
```

#### Map的CRUD操作

直接赋值，要是key不存在就是增加，key存在就是修改

```go
delete(map,"key")
```

key存在就删除，不存在也不会报错，啥也不干

**要是想删除所有的key，要么遍历删除，要么对原来的map进行一次新的make操作，让原来被指向的数组自动被GC回收**

查找直接接受返回值，返回值有两个

```go
val,result := myMap["name"]
```

如果myMap中存在name的key，会返回value到val，同时result是一个布尔类型的，这俩返回值名字都随便起

#### Map的遍历

只能用for-range

```go
for k,v :=range myMap{
  fmt.Println(k,v)
}
```

 自己尝试嵌套遍历

```go
	studentMap := make(map[string]map[string]string)

	studentMap["第一个"] = make(map[string]string, 4)
	studentMap["第一个"]["性别"] = "男"
	studentMap["第一个"]["姓名"] = "张宸"
	studentMap["第一个"]["住址"] = "沈阳"
	studentMap["第一个"]["爱好"] = "篮球"

	studentMap["第二个"] = make(map[string]string, 4)
	studentMap["第二个"]["性别"] = "女"
	studentMap["第二个"]["姓名"] = "王祖贤"
	studentMap["第二个"]["地址"] = "北京"
	studentMap["第二个"]["爱好"] = "无"

	for k, v := range studentMap {

		fmt.Println("==========")
		fmt.Println(k)
		for kk, vv := range v {
			fmt.Println(kk, vv)
		}
		fmt.Println("==========")
	}
```

输出为

```cmake
==========
第一个
性别 男
姓名 张宸
住址 沈阳
爱好 篮球
==========
==========
第二个
性别 女
姓名 王祖贤
地址 北京
爱好 无
==========

```

#### Map切片

```go
sliceMap := []map[string]string
```

声明一个Map类型的切片，然后以后用append来添加

#### Map的排序

把key或者value放到切片里用sort方法排序

#### Map的注意事项

- **Map会自动扩容**
- **Map是引用类型**
- **Map的value更适合用结构体**



## 稍微进阶

### Go语言的面向对象

没有class 只有struct

用struct存东西

#### Struct声明格式

```go
type structname struct{
	field1 type
	field2 type
}
```

如果字段类型是切片或者map，那得先make再用

#### 创建结构体实例的方式

1. ```go
   var struct1 structname
   ```

   然后再赋值字段

2. 直接在创建时候赋值字段

   ```go
   var strct1 structname{"xxx",xxx}
   ```

3. 用new关键字

   ```go
   var pstruct *structname = new(structname)
   ```

   注意，返回的是一个指向结构体的指针，要赋值得这么写

   ```go
   （*pstruct）.field = xxx
     pstruct.field = xxx
   ```

   第二行是简写，也可以，底层会对指针做取值处理

4. 用&

   ```go
   var pstruct *structname = &structname{"xxx", xxx}
   ```

   返回的也是指针，跟3一样赋值

#### 结构体注意事项

1. 内存中 字段是连续的，如果字段是指针类型，指针所在的地址是连续的，但指针指向的地址不一定
2. 结构体在和其他类型相互转换的时候，需要有完全相同的字段名，也就是说除了结构体的名字，其他都一样
3. 结构体进行type重新定义（相当于取别名），会被认为是新的数据类型，但是相互之间可以强转
4. struct的tag



### 方法

方法是坐拥在指定的数据类型上的，即和指定的数据类型绑定

#### 声明

```go
Type A struct{
	Num int
}
func (a A)test(){
  fmt.Println(a.Num)
}
```

表示结构体A有一个名叫test的方法



### Go的封装

把结构体的字段都小写，这样别的包就无法直接访问，然后手动写Set，Get方法，还有手动写一个大写的类似构造函数的东西

### Go的继承

在B中写一个匿名的A结构体，这样B就能用A的字段了

```go
type Goods struct{
  name sring
  price int
}
type Books struct{
  Goods //这句就是匿名结构体
  author string
}
```

**也可以多重继承**

### Go接口

```go
type usb interface{
	Start()
	Stop()
}
```

接口不是显示地被实现，而是他的方法都被实现了，那就说这个接口被实现了

#### 接口注意事项

- 接口本身不能创建实例
- 接口中所有方法都是没有被实现的
- 一个自定义数据类型可以实现多个接口
- 接口中不能有变量
- 接口可以继承多个别的接口，比如接口A继承B，C，那实现接口A也得实现B，C

### 类型断言

类似强转

有时候不能强转，所以加个判断

```go
y, ok :=x.(float64)
if ok {
  //成功 ok是true
}else{
  //失败 ok是false
}
```

也可以写在一行

```go
if y, ok :=x.(float64); ok {
  //成功 ok是true
}else{
  //失败 ok是false
}
```

