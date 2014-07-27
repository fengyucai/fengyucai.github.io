---
layout: post
title: "iOS学习总结1: Views"
date: 2013-11-02 21:32
comments: true
categories: iOS
---

视图（View) 是UIView类或者UIView子类的实例。一个视图能够将自身绘制到界面的某个矩形区域内。 
我们可以通过在nib编辑器中拖动控件来绘制视图，也可以部分或者全部通过代码来实现。自定义view的
代码可以控制视图出现(appear)，消失(disappear)，移动(move), 改变大小（resize) 甚至显示
一些动画效果(animation)。同时UIView也是UIResponder的子类，这意味着Views可以响应来自用户
的触摸事件，比如轻敲(tap) , 滑动(swipe), 长按(long press)等等。

应用中的Views是通过视图层次结构(view hierarchy)组织起来的。一个视图可以有多个子视图(subviews)，
但只能有一个直接父视图(superview)。当一个视图从界面中移除时，其子视图也会随着移除。一个视图移动,
其子视图也会跟着移动。视图层次结构是响应对象链(responder chain)的基础。

##UIWindow
UIWindow实例位于视图层次结构的最顶部，UIWindow也是UIView的子类。UIWindow实例的
主要功能是为Views提供显示空间以及向其他Views分发事件。一般情况下应用只有唯一一个UIWindow实例，
它在app启动时创建并占据整个屏幕, 然后在程序的整个生命周期(lifetime)都一直存在。
App Delegate 类拥有一个window属性(property)，指向这个UIWindow实例。当应用启动时，
main.m文件中的UIApplicationMain被main函数调用，创建并初始化App Delegate实例。app
delegate实例也将一直存在于应用的整个生命周期里。在app delegate中创建UIWindow的代码示例如下：
{% codeblock lang:objc %}
UIWindow* w = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
{% endcodeblock %}

##子视图与父视图
OS X 10.5之前，如果A视图不是B视图的子视图，则A视图的任何部分都不能出现在B视图的区域内。
Apple在OS X 10.5后采用更加灵活的规则，在iOS中允许视图之间互相覆盖(overlap), 无论
是否有父子关系。如图：


{% img /images/overlap.png %} 

因为overlap有多种可能的情形，我们并无法直接观察出以上三个视图的继承关系。事实上，
中间的视图和左边的视图是兄弟视图(siblings)，而右边视图是中间视图的子视图。
如果views是通过nib文件创建的，我们可以通过nib编辑器的outline区域查看它们之间的关系。
如图: 

{% img /images/views.png %} 

另外，假设someView是根视图，则可以通过以下代码在控制台输出视图的层次结构：
{% codeblock lang:objc %}
NSLog(@“%@“, [someView performSelector:@selector(recursiveDescription)]);
{% endcodeblock %}
addSubview:方法使得一个视图成为另一视图的子视图，当addSubview:被调用时，传入参数的
视图对象会成为父视图subviews数组的最后一个元素，也就是最后一个绘制在屏幕上的子视图，
覆盖在所有子视图的最顶层。 为了更好地理解overlap的规则和视图之间的关系，我们可以新建
一个iOS Empty Application 项目, 通过以下代码实现上面三个视图的效果。

{% codeblock lang:objc %}
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    // Override point for customization after application launch.
    self.window.rootViewController = [UIViewController new];

    UIView *mainView = self.window.rootViewController.view;
    UIView *v1 = [[UIView alloc] initWithFrame:CGRectMake(113, 111, 132, 194)];
    v1.backgroundColor = [UIColor colorWithRed:1 green:0.4 blue:1 alpha:1];
    UIView *v2 = [[UIView alloc] initWithFrame:CGRectMake(41, 56, 132, 194)];
    v2.backgroundColor = [UIColor colorWithRed:0.5 green:1 blue:0 alpha:1];
    UIView *v3 = [[UIView alloc] initWithFrame:CGRectMake(43, 197, 160, 230)];
    v3.backgroundColor = [UIColor colorWithRed:1 green:0 blue:0 alpha:1];

    [mainView addSubview:v1];
    [v1 addSubview:v2];
    [mainView addSubview:v3];
    
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    return YES;
}

{% endcodeblock %}

##未完待续


