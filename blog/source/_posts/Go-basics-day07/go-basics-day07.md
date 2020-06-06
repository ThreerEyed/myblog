---
title: Go语言基础学习day07 (接口)
date: 2020-06-03 10:16:19
tags: go
categories: Go语言基础
keywords:
description:
top_img: https://i.loli.net/2020/05/26/SDAJuBVNXrpexls.jpg
comments:
cover: https://i.loli.net/2020/05/26/SDAJuBVNXrpexls.jpg
toc:
toc_number: true
copyright: true
mathjax:
katex:
---



{% note warning%}
这是一个创建于 2020-05-29 14:14:59的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}

<br/>



## 接口

### 接口的定义

接口是一种数据类型，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。

```go
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
```



### 实例：

```go
package main

import "fmt"

// 定义结构体
type person struct {
}

type dog struct{}

type fox struct{}

// 定义方法，人，狗，狐狸都能够叫，都有一个speak 的方法
func (p person) speak() {
	fmt.Println("你好啊")
}

func (d dog) speak() {
	fmt.Println("汪汪汪~")
}

func (f fox) speak() {
	fmt.Println("嘤嘤嘤~")
}

// 定义一个方法，传入一个参数就能实现speak 的方法
func jiao(a person) {
	a.speak()
}

func main() {
	var p1 person
	var d1 dog
	var f1 fox
	jiao(p1)
	jiao(d1)
	jiao(f1)

}

```

以上代码发现一个问题，我们只能使用  `jiao(p1)`  ,因为jiao 这个方法的传入参数的类型只有  person 结构体，所以其他结构体想使用类似方法，只能重新定义其他的方法来实现

```go
var p1 person
var d1 dog
var f1 fox
jiao(p1)
d1.speak()
f1.speak()

// jiao(d1)
// jiao(f1)
```

要使以上结构体都能实现`jiao()`  的方法 ，需要引入新的东西——接口

```go
package main

import "fmt"

// 引出接口的实例，定义接口
type speaker interface {
	speak()
}

// 定义一个结构体
type person struct {
}

type dog struct{}

type fox struct{}

func (p person) speak() {
	fmt.Println("你好啊")
}

func (d dog) speak() {
	fmt.Println("汪汪汪~")
}

func (f fox) speak() {
	fmt.Println("嘤嘤嘤~")
}

func jiao(a speaker) {
	a.speak()
}

func main() {
	var p1 person
	var d1 dog
	var f1 fox
	jiao(p1)

	jiao(d1)
	jiao(f1)

}

```



### 总结

只要实现了我们定义的接口中 的speak()  这个方法的其他类型，我们就说实现了接口（就是这个speaker类型），就都可以使用jiao 这个方法





### 实现接口的条件

一个对象只要全部实现了接口中的方法，那么就实现了这个接口。换句话说，接口就是一个**需要实现的方法列表**。



### 实现了接口的作用

接口类型变量能够存储所有实现了该接口的实例。只要实现了接口，那么这个类型的实例就可以保存在接口中，并通过接口变量来调用类型的相关方法

```go
func main() {
	var x Sayer // 声明一个Sayer类型的变量x
	a := cat{}  // 实例化一个cat
	b := dog{}  // 实例化一个dog
	x = a       // 可以把cat实例直接赋值给x
	x.say()     // 喵喵喵
	x = b       // 可以把dog实例直接赋值给x
	x.say()     // 汪汪汪
}
```

**Tips：** 观察下面的代码，体味此处`_`的妙用

```go
// 摘自gin框架routergroup.go
type IRouter interface{ ... }

type RouterGroup struct { ... }

var _ IRouter = &RouterGroup{}  // 确保RouterGroup实现了接口IRouter
```



### 值接收者和指针接收者实现接口的区别

使用值接收者实现接口和使用指针接收者实现接口有什么区别呢？接下来我们通过一个例子看一下其中的区别。

我们有一个`Mover`接口和一个`dog`结构体。

```go
type Mover interface {
	move()
}

type dog struct {}
```



#### 值接收者实现接口

```go
func (d dog) move() {
	fmt.Println("狗会动")
}
```

此时实现接口的是`dog`类型：

```go
func main() {
	var x Mover
	var wangcai = dog{} // 旺财是dog类型
	x = wangcai         // x可以接收dog类型
	var fugui = &dog{}  // 富贵是*dog类型
	x = fugui           // x可以接收*dog类型
	x.move()
}
```

从上面的代码中我们可以发现，使用值接收者实现接口之后，不管是dog结构体还是结构体指针*dog类型的变量都可以赋值给该接口变量。因为Go语言中有对指针类型变量求值的语法糖，dog指针`fugui`内部会自动求值`*fugui`。



#### 指针接收者实现接口

同样的代码我们再来测试一下使用指针接收者有什么区别：

```go
func (d *dog) move() {
	fmt.Println("狗会动")
}
func main() {
	var x Mover
	var wangcai = dog{} // 旺财是dog类型
	x = wangcai         // x不可以接收dog类型
	var fugui = &dog{}  // 富贵是*dog类型
	x = fugui           // x可以接收*dog类型
}
```

此时实现`Mover`接口的是`*dog`类型，所以不能给`x`传入`dog`类型的wangcai，此时x只能存储`*dog`类型的值。



### 接口与类型的关系

一个类型可以实现多个接口，一个接口可以被多个类型实现



### 接口嵌套

接口与接口只见可以实现嵌套的关系，一个接口可以存在于另一个接口内

```go
// Sayer 接口
type Sayer interface {
	say()
}

// Mover 接口
type Mover interface {
	move()
}

// 接口嵌套
type animal interface {
	Sayer
	Mover
}
```



### 空接口

空接口是指没有定义任何方法的接口，任何的类型都实现了空接口，空接口类型的变量可以存储任意类型的变量。

```go
func main() {
	// 定义一个空接口x
	var x interface{}
	s := "Hello 沙河"
	x = s
	fmt.Printf("type:%T value:%v\n", x, x)
	i := 100
	x = i
	fmt.Printf("type:%T value:%v\n", x, x)
	b := true
	x = b
	fmt.Printf("type:%T value:%v\n", x, x)
}
```



#### 空接口作为函数的参数

空接口可以用来当做函数参数类型，这样我们在给函数传入参数的时候可以不用考虑类型，任何类型都可以传入

```go
func variable_name(a interface{}) {
    ...
}
```



#### 空接口作为map的值

使用空接口实现可以保存任意值的字典。

```go
// 空接口作为map值
	var studentInfo = make(map[string]interface{})
	studentInfo["name"] = "沙河娜扎"
	studentInfo["age"] = 18
	studentInfo["married"] = false
	fmt.Println(studentInfo)
```



### 类型断言

空接口可以存储任意类型的值，那我们如何获取其存储的具体数据呢？

#### 接口值

一个接口的值（简称接口值）是由`一个具体类型`和`具体类型的值`两部分组成的。这两部分分别称为接口的`动态类型`和`动态值`。

我们来看一个具体的例子：

```go
var w io.Writer
w = os.Stdout
w = new(bytes.Buffer)
w = nil
```

请看下图分解：![](https://cdn.jsdelivr.net/gh/ThreerEyed/CDN/img/interface.png)

想要判断空接口中的值这个时候就可以使用类型断言，其语法格式：

```go
x.(T)
```

其中：

- x：表示类型为`interface{}`的变量
- T：表示断言`x`可能是的类型。

该语法返回两个参数，第一个参数是`x`转化为`T`类型后的变量，第二个值是一个布尔值，若为`true`则表示断言成功，为`false`则表示断言失败。

举个例子：

```go
func main() {
	var x interface{}
	x = "Hello 沙河"
	v, ok := x.(string)
	if ok {
		fmt.Println(v)
	} else {
		fmt.Println("类型断言失败")
	}
}
```

上面的示例中如果要断言多次就需要写多个`if`判断，这个时候我们可以使用`switch`语句来实现：

```go
func justifyType(x interface{}) {
	switch v := x.(type) {
	case string:
		fmt.Printf("x is a string，value is %v\n", v)
	case int:
		fmt.Printf("x is a int is %v\n", v)
	case bool:
		fmt.Printf("x is a bool is %v\n", v)
	default:
		fmt.Println("unsupport type！")
	}
}
```

因为空接口可以存储任意类型值的特点，所以空接口在Go语言中的使用十分广泛。

关于接口需要注意的是，只有当有两个或两个以上的具体类型必须以相同的方式进行处理时才需要定义接口。不要为了接口而写接口，那样只会增加不必要的抽象，导致不必要的运行时损耗。



{% note warning %}

文章部分内容来源于[菜鸟教程](https://www.runoob.com/go/go-interfaces.html)  [李文周的博客](https://www.liwenzhou.com/posts/Go/12_interface/)

{% endnote %}