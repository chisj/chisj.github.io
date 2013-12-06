---
layout: post
title: "SenTestingKit not loaded"
date: 2013-12-06 17:28:02 +0800
comments: true
categories: 
---
上次因为新工程的迁移，为了图方便直接把旧的引用包全选拖动到新建的工程里，结果出现了这个错误并闪退。
搜索了一下:

在发布的target的build phase的link binary删掉SenTestingKit
然后在单元测试的build phase加上它。

记得有人说过：越是看起来表现怪异的bug，其原因往往越是简单。

悲剧地验证了。