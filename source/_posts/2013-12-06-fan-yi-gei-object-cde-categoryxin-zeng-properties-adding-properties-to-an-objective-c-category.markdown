---
layout: post
title: "[翻译]给Object-c的Category新增Properties(Adding Properties to an Objective-C Category)"
date: 2013-12-06 17:18:41 +0800
comments: true
categories: 
---


如果需要给一个category保存一些信息，很不幸，你不能直接给它新增实例变量，但你可以新增一个“Associated Reference”，文档在

[这里](https://developer.apple.com/library/ios/#DOCUMENTATION/Cocoa/Conceptual/ObjectiveC/Chapters/ocAssociativeReferences.html)

>Associative references,Mac OS X 10.6以后可用，给当前类新增一个模拟的实例变量

## 使用Associated Reference实现Property存储
我们可以用这种技术给编译后的Class设置一个想要的Property。在这个例子里，我想给UIView指定类型，新增一个styleName。
        
        @interface UIView (DHStyleManager)
		@property (nonatomic, copy) NSString* styleName;
		@end

		#import "UIViewDHStyleManager.h"
 
		NSString * const kDHStyleKey = @"kDHStyleKey";
 
 <br>
 
		@implementation UIView (DHStyleManager)
 
		- (void)setStyleName:(NSString *)styleName
		{
			objc_setAssociatedObject(self, kDHStyleKey, styleName, OBJC_ASSOCIATION_COPY);
		}
 
		- (NSString*)styleName
		{
			return objc_getAssociatedObject(self, kDHStyleKey);
		}
 
		@end

当UIView实例release的时候，对应的Associated Reference也会被release。

这种技术使得设置自定义Property给运行时的UIView成为可能，如果你只是想实现一些诸如增加Property之类的简单需求，这无疑是一种很好的继承替代方案。

		#import UIViewDHStyleManager.h"
 
		UIView* v = [[[UIView alloc] init] autorelease];
		v.styleName = @"someStyleName";
		NSLog(@"v = %@",v.styleName); //Logs 'someStyleName'
		
## 关于

你好，我是[David Hamrick](http://www.twitter.com/davidhamrick),我是[Hamrick Software](http://www.hamrick.com/)软件公司拥有人之一。我们开发了一款叫做“VueScan”的扫描仪驱动软件，它支持OS X，Windows，Linux，和iOS。

---------

注：@property为assign的时候，如果property指向的对象已经被释放，用 ![]self styleName]貌似不能判断为空，property已经成了野指针，需谨慎使用以免带来bug。

原文地址：[http://www.davidhamrick.com/2012/02/12/Adding-Properties-to-an-Objective-C-Category.html](http://www.davidhamrick.com/2012/02/12/Adding-Properties-to-an-Objective-C-Category.html)