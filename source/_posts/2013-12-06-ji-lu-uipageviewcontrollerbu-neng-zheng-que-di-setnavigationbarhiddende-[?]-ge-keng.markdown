---
layout: post
title: "记录UIPageViewController不能正确地setNavigationBarHidden的一个坑"
date: 2013-12-06 17:25:15 +0800
comments: true
categories: 
---

今天碰到一个奇怪bug：UIPageViewController的viewControllers里面做
```
[self.navigationController setNavigationBarHidden:YES animated:animated];
```
操作，在iOS5里面，可以成功隐藏UIPageViewController的navBar，但是在iOS6里面，却只能在view已经显示完成手动调用才可以隐藏，而写在UIPageViewController的viewcontrollers里面的如下代码

```
- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    [self.navigationController setNavigationBarHidden:YES animated:animated];
}
```

却不起作用！！！

**一般来说，低版本不可用高版本可用，第一步分析是不是使用了高版本才有的api；而高版本不可用低版本却可用，第一步分析是否函数已经过期不再被调用（就像viewDidunload那样)和函数的被调用顺序是否已经改变**

经过打印log分析得出结论：

**在iOS5里，UIPageViewController会先调用自身的viewWillAppear，然后再调用它的viewControllers的viewWillAppear**

**在iOS6里，UIPageViewController会先调用它的viewControllers的viewWillAppear，然后再调用自身的viewWillAppear**

so，为了在iOS5和iOS6里都能正确地在UIPageViewController一显示就隐藏navbar，只要把setNavigationBarHidden写到UIPageViewController的viewWillAppear。听起像白痴的废话，都是苹果造成的！

莫名其妙的一个坑，UIPageViewController Class Reference什么都没提起！！！
