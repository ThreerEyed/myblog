---
title: Go语言基础学习day06（结构体）
date: 2020-05-30 11:54:49
tags: go
categories: Go语言基础
keywords: 
description:
top_img: //i.loli.net/2020/05/27/HS3ukmqGgh7x2K4.png
comments:
cover: //i.loli.net/2020/05/27/HS3ukmqGgh7x2K4.png
toc:
toc_number: true
copyright: true
mathjax:
katex:
---



{% note warning%}
这是一个创建于 2020-05-27 22:31:21 的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}



<br/>

## 结构体

在go 语言中，数组可以用来存储同一类型的数据，结构体可以用来存储不同类型的数据。

结构体是由一系列具有相同类型或不同类型的数据构成的数据集合。



### 定义结构体

结构体定义需要使用 type 和 struct 语句。struct 语句定义一个新的数据类型，结构体中有一个或多个成员。type 语句设定了结构体的名称。结构体的格式如下：

```go
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}
```

一旦定义了结构体类型，它就能用于变量的声明，语法格式如下：

```go
variable_name := structure_variable_type {value1, value2...valuen}
或
variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}
```

```go
package main

import "fmt"

type people struct {
	name  string
	age   int
	sex   string
	height int
}

func main() {

	people1 := people{name: "张三", age: 18, sex: "男", height: 173}

	// 创建一个新的结构体
	fmt.Println(people1)

	// 也可以使用 key => value 格式
	fmt.Println(people{name: "赵四", age: 20, sex: "男", height: 165})

	// 忽略的字段为 0 或 空
	fmt.Println(people{name: "joe", age: 18})
}

```

运行结果为：

> {张三 18 男 173}
> {赵四 20 男 165}
> {joe 18  0}

<br/>

### 访问结构体成员

使用  `.` 来访问结构体成员，用法为：struct_name.成员名

```go
people1 := people{name: "张三", age: 18, sex: "男", height: 173}

fmt.Println(people1.name)
```

运行结果为：

> 张三

<br/>



### 结构体作为函数参数

结构体可以像其他数据类型一样当做参数传入函数，可以在函数内部通过`.`   的形式来传入

```go
package main

import "fmt"

type peoples struct {
	name   string
	age    int
	sex    string
	height int
}

func main() {

	var people1 peoples

	people1.name = "张三"
	people1.age = 14
	people1.sex = "男"
	people1.height = 165

	printPeople(people1)

}

func printPeople(person peoples) {
	fmt.Println(person.name)
	fmt.Println(person.age)
	fmt.Println(person.sex)
	fmt.Println(person.height)
}

```

以上代码输出结果为：

> 张三
> 14
> 男
> 165



<br/>

### 结构体指针

结构体类似其他类型的输出一样，定义指向结构体的指针，结构体是值传递类型，有时我们修改结构体内的成员值，需要通过指针来修改，通过struct类型的指针找到struct 类型的指针变量，再通过指针变量找到地址，然后修改值



## 构造函数

创建一个新的对象，类似于python中的`__init__`, 创建对象并初始化赋值，总与new运算符一起使用在创建对象的语句中

```go
package main

import "fmt"

type peoples struct {
	name   string
	age    int
	sex    string
	height int
}

// 构造函数
func newPeoples(name string, age int, sex string, height int) peoples {
	return peoples{
		name:   name,
		age:    age,
		sex:    sex,
		height: height,
	}
}

func main(){
    p1 := newPeoples("张三", 27, "男", 187)
    fmt.Println(p1)
}
```



## 结构体匿名字段

结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为匿名字段。

```go
//Person 结构体Person类型
type Person struct {
	string
	int
}

func main() {
	p1 := Person{
		"小王子",
		18,
	}
	fmt.Printf("%#v\n", p1)        //main.Person{string:"北京", int:18}
	fmt.Println(p1.string, p1.int) //北京 18
}
```

匿名字段默认采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只能有一个。

匿名字段的使用场景较少，因为字段不能重复。





## 结构体嵌套

一个结构体中可以嵌套包含另一个结构体或结构体指针。

```go
//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address Address
}

func main() {
	user1 := User{
		Name:   "小王子",
		Gender: "男",
		Address: Address{
			Province: "山东",
			City:     "威海",
		},
	}
	fmt.Printf("user1=%#v\n", user1)//user1=main.User{Name:"小王子", Gender:"男", Address:main.Address{Province:"山东", City:"威海"}}
}
```



## 结构体继承

继承仍然是结构体的嵌套。

Go语言中使用结构体也可以实现其他编程语言中面向对象的继承。

```go
//Animal 动物
type Animal struct {
	name string
}

func (a *Animal) move() {
	fmt.Printf("%s会动！\n", a.name)
}

//Dog 狗
type Dog struct {
	Feet    int8
	*Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
	fmt.Printf("%s会汪汪汪~\n", d.name)
}

func main() {
	d1 := &Dog{
		Feet: 4,
		Animal: &Animal{ //注意嵌套的是结构体指针
			name: "乐乐",
		},
	}
	d1.wang() //乐乐会汪汪汪~
	d1.move() //乐乐会动！
}
```



## 结构体字段的可见性

结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）。





## 结构体与Json（avaScript Object Notation, JS 对象简谱 ）

JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。易于人阅读和编写。同时也易于机器解析和生成。JSON键值对是用来保存JS对象的一种方式，键/值对组合中的键名写在前面并用双引号`""`包裹，使用冒号`:`分隔，然后紧接着值；多个键值之间使用英文`,`分隔。

```go
//Student 学生
type Student struct {
	ID     int
	Gender string
	Name   string
}

//Class 班级
type Class struct {
	Title    string
	Students []*Student
}

func main() {
	c := &Class{
		Title:    "101",
		Students: make([]*Student, 0, 200),
	}
	for i := 0; i < 10; i++ {
		stu := &Student{
			Name:   fmt.Sprintf("stu%02d", i),
			Gender: "男",
			ID:     i,
		}
		c.Students = append(c.Students, stu)
	}
	//JSON序列化：结构体-->JSON格式的字符串
	data, err := json.Marshal(c)
	if err != nil {
		fmt.Println("json marshal failed")
		return
	}
	fmt.Printf("json:%s\n", data)
	//JSON反序列化：JSON格式的字符串-->结构体
	str := `{"Title":"101","Students":[{"ID":0,"Gender":"男","Name":"stu00"},{"ID":1,"Gender":"男","Name":"stu01"},{"ID":2,"Gender":"男","Name":"stu02"},{"ID":3,"Gender":"男","Name":"stu03"},{"ID":4,"Gender":"男","Name":"stu04"},{"ID":5,"Gender":"男","Name":"stu05"},{"ID":6,"Gender":"男","Name":"stu06"},{"ID":7,"Gender":"男","Name":"stu07"},{"ID":8,"Gender":"男","Name":"stu08"},{"ID":9,"Gender":"男","Name":"stu09"}]}`
	c1 := &Class{}
	err = json.Unmarshal([]byte(str), c1)
	if err != nil {
		fmt.Println("json unmarshal failed!")
		return
	}
	fmt.Printf("%#v\n", c1)
}
```





{% note warning%}
本节部分内容来源于[李文周的博客](https://www.liwenzhou.com/posts/Go/10_struct/)
{% endnote %}