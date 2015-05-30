--- 
layout: post 
title: "UINavigationController返回手势失效问题" 
comments: true 
categories: Octopress 
---

#UINavigationController返回手势失效问题


从iOS7开始，系统为UINavigationController提供了一个interactivePopGestureRecognizer用于右滑返回(pop),但是，如果自定了back button或者隐藏了navigationBar，该手势就失效了。

![image](http://7xjbdg.com1.z0.glb.clouddn.com/抓狂.png) 

这是为什么呢？
###原因

我们知道，interactivePopGestureRecognizer从手势触发到行为发生，要经过下面的阶段：

![image](http://7xjbdg.com1.z0.glb.clouddn.com/手势过程.png) 

interactivePopGestureRecognizer还存在，但没有起作用，可能是delegate里被阻断了没调用target/action，或者是调用了target/action没运行动画。

经过尝试（参考别人博客），发现自定义返回按钮或者隐藏navigationBar导致的该手势未起作用是因为在delegate阶段被阻断了。


如果我们知道action的名字，则可以添加一个自定义的滑动手势，直接调用该系统action。但API文档并没有提供。

结果就是要么自己实现滑动返回的动画action，要么自己重写interactivePopGestureRecognizer的delegate以让手势继续下去，触发系统的动画action。


###实现方法

那就把delegate自己实现一下吧。

新建一个类`BaseNavigationController`,实现delegate：

```
- (void)viewDidLoad {
    [super viewDidLoad];
    self.interactivePopGestureRecognizer.delegate =  self;
}
```

让手势生效

```
- (BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer {
    if (self.viewControllers.count <= 1 ) {
        return NO;
    }

    return YES;
}
```
在需要滑动返回的地方的`UINavigationController`换成`BaseNavigationController`。

完成！

![image](http://7xjbdg.com1.z0.glb.clouddn.com/赞.png) 

然后考虑到在push动画发生的时候，禁止滑动手势，在`BaseNavigationController`添加

```
- (void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated {
    [super pushViewController:viewController animated:animated];
    self.interactivePopGestureRecognizer.enabled = NO;
}
```
在使用navigationController的viewcontroller里添加

```
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    self.navigationController.interactivePopGestureRecognizer.enabled = YES;
}
```

###结果

当然不需要把上面的代码都抄一遍，因为这么通用的功能由一位韩国开发者做成组件，放在了github [https://github.com/devxoul/SwipeBack](https://github.com/devxoul/SwipeBack)。你只需要

```
platform :ios, '7.0'
pod 'SwipeBack', '~> 1.0'
```
该工程用了category+JRSwizzle交换了上面涉及到的UINavigationController的那些方法，还额外考虑了只自定义back button而不隐藏navigationBar的情况。无需一行代码，让系统的右滑返回动画重新回来！






