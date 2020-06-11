---
title: 基础练习
date: 2020-06-10 11:55:53
tags: go
categories: Go语言基础
keywords:
description: 使用go语言完成的一些简单练习
top_img: https://i.loli.net/2020/06/10/qGcp7Cghz5M8eZD.jpg
comments: true
cover: https://i.loli.net/2020/06/10/qGcp7Cghz5M8eZD.jpg
toc: true
toc_number: true
copyright: true
mathjax:
katex:
---



## 关于go语言的基础性的练习



### 1. 分金币

```go
package main

import (
	"fmt"
	"strings"
)

/*
你有50枚金币，需要分配给以下几个人：Matthew,Sarah,Augustus,Heidi,Emilie,Peter,Giana,Adriano,Aaron,Elizabeth。
分配规则如下：
a. 名字中每包含1个'e'或'E'分1枚金币
b. 名字中每包含1个'i'或'I'分2枚金币
c. 名字中每包含1个'o'或'O'分3枚金币
d: 名字中每包含1个'u'或'U'分4枚金币
写一个程序，计算每个用户分到多少金币，以及最后剩余多少金币？
程序结构如下，请实现 ‘dispatchCoin’ 函数
*/
var (
	coins = 50
	users = []string{
		"Matthew", "Sarah", "Augustus", "Heidi", "Emilie", "Peter", "Giana", "Adriano", "Aaron", "Elizabeth",
	}
	distribution = make(map[string]int, len(users))
)

// 方法一、（代码繁杂，不够简练）
func dispatchCoin1(allCoins int, peoples []string) int {
	var getCoin int
	var left int
	for _, name := range peoples {
		getCoin = 0
		// 名字中每包含1个'e'或'E'分1枚金币
		if strings.Contains(name, "e") || strings.Contains(name, "E") {
			getCoin += strings.Count(name, "e")
			getCoin += strings.Count(name, "E")
		}
		// 名字中每包含1个'i'或'I'分2枚金币
		if strings.Contains(name, "i") || strings.Contains(name, "I") {
			getCoin += strings.Count(name, "i") * 2
			getCoin += strings.Count(name, "I") * 2
		}
		// 名字中每包含1个'o'或'O'分3枚金币
		if strings.Contains(name, "o") || strings.Contains(name, "O") {
			getCoin += strings.Count(name, "o") * 3
			getCoin += strings.Count(name, "O") * 3
		}
		// 名字中每包含1个'u'或'U'分4枚金币
		if strings.Contains(name, "u") || strings.Contains(name, "U") {
			getCoin += strings.Count(name, "u") * 4
			getCoin += strings.Count(name, "U") * 4
		}
		fmt.Printf("%s 得到的金币数量是 %v\n", name, getCoin)

		distribution[name] = getCoin
		left = allCoins - getCoin
		allCoins = left
	}
	fmt.Println(distribution)
	return left

}

// 方法二、（使用switch 和case 更加简洁高效）
func dispatchCoin2(allCoins int, peoples []string) int {
	leftCoins := allCoins
	for _, name := range users {
		for _, i := range name {
			switch i {
			case 'e', 'E':
				distribution[name]++
			case 'i', 'I':
				distribution[name] += 2
			case 'o', 'O':
				distribution[name] += 3
			case 'u', 'U':
				distribution[name] += 4
			}
		}
		leftCoins -= distribution[name]
	}
	fmt.Println(distribution)
	return leftCoins
}

func main() {
	left := dispatchCoin1(coins, users)
	fmt.Printf("还剩下%v 个金币\n", left)
	// dispatchCoin(coins, users)
}

```

方法一是我自己写的，方法二是教程上的，自己写的过于繁杂，过多的if 语句会让代码显得臃肿，



以上代码练习了以下知识点：

1. 文档注释的规范性
2. 全局变量的声明以及集合map的创建
3. 函数的创建，参数传入，返回类型
4. if else 的练习以及switch的练习