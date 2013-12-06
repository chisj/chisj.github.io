---
layout: post
title: "Mac OS X 10.8.2 下尝试Go语言"
date: 2013-12-06 17:16:30 +0800
comments: true
categories: 
---

##安装Go
*. 安装Mercurial

        sudo easy_install mercurial
*. 获取Go源码

	$ hg clone -r release https://go.googlecode.com/hg/ go
*. 源码下载完成之后，安装Go

	$ cd go/src
        $ ./all.bash
*. 根据安装信息的最后一段提示，在etc/paths文件里，用管理员指令(sudo)添加Go Path。

##HelloWorld
*. 编写源代码

	package main
  
  	import "fmt"
  
  	func main() {
          fmt.Printf("hello, world\n")
  	}
  	
保存为HelloWorld.go,cd到源码目录

	go build HelloWorld.go
	
如果有错误，检查源代码，很可能是没有错误，并且已经生成了HelloWorld可执行文件。 

	./HelloWorld
	
至此，HelloWorld完成。

##Go之初体验

1. Go语言语法和C语言类似，很适合科班程序员学习新语言或者底层C语言开发人员学应用开发的时候使用。它没有C语言那么自由，比如{}的语句块，前一个{必须放在控制语句后面，不得另起一行，在C语言中，有一种严格的编码规范就是像Go这样，但是这一点貌似不能说明什么，Go只是少了一种无谓的选择。不过，if，for和switch语句控制快少了()，语句结尾不需要";"，甚至，少掉了else语句，干掉了异常处理。

2. 不过，switch语句更智能了，函数支持多个返回值了，并且只要简单的"return reval1, reval2"就可以返回两个值。Go可真是该减肥的地方就减肥了，该丰满的地方就丰满到了恰到好处，我很喜欢的Object-c虽然也近乎完美地扩展了C语言，但其封闭的生态首先比不上Go了.一味简单地往C语言身上加特性，就没有Go的减肥功效啦！

3. 其他更强大的并行特性，网络编程支持等，以后学习了再分享。

##观点

1. [我发现我花了四年时间锤炼自己用 C 语言构建系统的能力，试图找到一个规范，可以更好的编写软件。结果发现只是对 Go 的模仿。——云风《Go 语言初步》](http://blog.codingnow.com/2010/11/go_prime.html)

2. [我为什么喜欢Go语言——AllenDang](http://www.cnblogs.com/AllenDang/archive/2012/03/03/2378534.html)


参考资料：[Go语言文档](https://golang-china.googlecode.com/svn/trunk/Chinese/golang.org/index.html#toc2)	