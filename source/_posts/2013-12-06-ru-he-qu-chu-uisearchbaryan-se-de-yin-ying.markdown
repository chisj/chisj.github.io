---
layout: post
title: "如何去除UISearchBar颜色的阴影"
date: 2013-12-06 17:32:46 +0800
comments: true
categories: 
---

其实是UISearchBar有一个不对外开放的“背景View”：UISearchBarBackground。如果你想让UISearchBar变得“扁平”，或者定制一些特别颜色，设置tintColor或者backgroundColor都是没有理想效果的——依然会得到一个带阴影的颜色。

那么，就让那讨厌的“背景View”透明吧：
	
	@interface UISearchBar (CustomBackground)

	@end
	
	
	@implementation UISearchBar( CustomBackground )
	
	- (void)drawRect:(CGRect)rect {
    	for( UIView *subview in self.subviews ) {
        	if( [subview isKindOfClass:NSClassFromString(@"UISearchBarBackground")]) {
            [subview setAlpha:0.0F];
            break;
        	}
    	}
	}
	
	@end

Update:在实际开发中还是用CustomBackgroundSearchBar : UISearchBar 好一点，毕竟存在有点UISearchBar需要定制而另外的又不需要的情况，用Category比较不好控制。