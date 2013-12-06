---
layout: post
title: "在UIWebView中调用Object-c本地代码"
date: 2013-12-06 17:20:59 +0800
comments: true
categories: 
---

有些时候，HTML5可以做很酷的事情，比如绚丽的动画，或者快速开发跨平台的产品模型。于是我们希望JS代码和Native App多做些交互，比如在UIWebView的JS代码中调用本地的Object-c代码。

实际的需求是我们需要在APP里展示一个动画，动画运行结束后自己消失。
实现的思路是：让Parter做一个HTML5动画，然后用UIWebView调用本地的HTML5文件展示动画，在动画结束之后，用JS代码回调Object-c代码，dismiss掉UIWebView。我们知道UIWebView有一个@optional的Delegate：

    - (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;
    
从文档里可以知道每个UIWebView在load一个frame的时候(说实在的我也不懂load一个frame是什么意思！暂且理解为收到一个某种类型的url request请求吧。)都会调用这个函数，那我们就可以在这个函数里拦截url并决定是否开始load动作。我在这里是想要调用Object-c代码来dismiss掉WebView本身，当然不让load。

以下是实现的代码：

* JS代码

        window.loaction = "action?dismiss";
        
    （我把这个理解成导致UIWebView开始load frame动作的原因，你如果熟悉JS代码的话可以告诉我原理啊。）
	
* Object-c代码

	新建UIWebViewController并显示：

		//加载本地HTML5
        UIWebView *webView = [[UIWebView alloc] initWithFrame:APP_FRAME];
        NSURL *url = [NSURL fileURLWithPath:[[NSBundle mainBundle] pathForResource:@"index" ofType:@"html" inDirectory:@"yourHTMLDirectory"]];
        webView.delegate = self;
        [webView loadRequest:[NSURLRequest requestWithURL:url]];
		
		//Pop WebViewController
		loginAdController = [[UIViewController alloc] initWithNibName:nil bundle:nil];
        loginAdController.view = webView;
        [webView release];
        [homePage presentModalViewController:loginAdController animated:YES];
        [loginAdController release];
     
    UIWebViewDelgate：
    
        - (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    		NSString *url = [[request URL] absoluteString];
    		NSArray *urlArray = [url componentsSeparatedByString:@"/"];
    		NSString *actionString = [urlArray lastObject];
    		if ( actionString && [actionString isEqualToString:@"action?dismiss"]) {
        		[self performSelector:@selector(dismissAdWebView) withObject:nil afterDelay:2.5];
        		return NO;
    		}
    		
    		return YES;
		}
		
	dismiss掉动画：
	
		- (void)dismissAdWebView {
    		[loginAdController dismissModalViewControllerAnimated:YES];
		}
		
至此整个功能完成。

参考文档：

[http://stackoverflow.com/questions/1662473/how-to-call-objective-c-from-javascript](http://stackoverflow.com/questions/1662473/how-to-call-objective-c-from-javascript)