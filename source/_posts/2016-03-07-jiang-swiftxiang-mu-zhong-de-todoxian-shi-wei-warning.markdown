---
layout: post
title: "将Swift项目中的TODO显示为Warning"
date: 2016-03-07 22:25:38 +0800
comments: true
categories: 
---

我的代码很少注释[^1]。但有一个例外：使用// TODO: and // FIXME:以高亮我不久后将要重新查看的代码段。这样的好处是在编辑框的条专栏弹窗里，点击一下粗体字就能跳转到该行代码。

![icon1](http://ww2.sinaimg.cn/large/73a4517bgw1f1on1p5h0qj20m808o751.jpg)

这也有个问题，就是容易忘记。我曾经把它们记在我的todo管理里，[Tings](http://culturedcode.com/things/) . 这又变成了重复工作。如何更好地标记这些错误呢？

[杰弗里Sambells写了一篇](http://jeffreysambells.com/2013/01/31/generate-xcode-warnings-from-todo-comments)如何在Objective-C工程中将这些注释变成Xcode Warning的文章。稍加改动，这是一个将Swift项目的TODO:和FIXME：显示为Warning的Script(应用于工程的Build Phases设置)：
	
	TAGS="TODO:|FIXME:"
	echo "searching ${SRCROOT} for ${TAGS}"
	find "${SRCROOT}" \( -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-	number --only-matching "($TAGS).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/"
	
运行一下，TODO:和FIXME:变成了醒目的警告。

![icon2](http://ww3.sinaimg.cn/large/73a4517bgw1f1onk1chmwj20m8025mxl.jpg)

比起记录在todo list的东西，这些黄色的警告总让我有莫大的冲动去清理他们，你觉得呢？


[^1]: If your code needs commenting, it isn’t clear enough. Refactor until it is. If it doesn’t make sense because of semantics, rethink your naming conventions.


[原文地址https://bendodson.com/weblog/2014/10/02/showing-todo-as-warning-in-swift-xcode-project/](https://bendodson.com/weblog/2014/10/02/showing-todo-as-warning-in-swift-xcode-project/)

