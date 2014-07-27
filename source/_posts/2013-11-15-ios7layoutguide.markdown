---
layout: post
title: "iOS 7中的topLayoutGuide与bottomLayoutGuide"
date: 2013-11-15 18:52
comments: true
categories: iOS
---


iOS界面顶部和底部的UI的通常由某种UIKit Bars构成, 比如状态栏(status bar), 
导航条(navigation bar), 工具栏(tool bar)和标签栏(tab bar)。开发中常常需要
在顶部和底部bars之间的区域内布置一些UI元素。在iOS 6以及更早的版本中，一个
UIViewController会自动调整自身view的大小来适配这块中间区域。然而在iOS 7中，
一个UIViewController的view会在竖直方向上扩展至整个屏幕的边界，可能导致原来
view的子视图被顶部或底部的bars所覆盖(如下图)。

{% img /images/beforeguide.png %} 

为了解决这种新布局带来的问题，iOS 7的UIViewController增加了两个新的属性(property):
topLayoutGuide和bottomLayoutGuide。topLayoutGuide和bottomLayoutGuide实际上
位于顶部和底部的两个透明的视图，这两个视图的默认高度正是bars的高度，可以作为子视图嵌入
某个root view。下面以topLayoutGuide为例, 结合Auto layout, 我们可以通过编写代码
解决上述问题。

示例代码如下(其中的vc代表某个navigation controller的rootViewController):


{% codeblock lang:objc %}
    UIView *v1 = vc.view;
    UIView *v2 = [UIView new];
    v2.backgroundColor = [UIColor redColor];
    [v1 addSubview:v2];
    
    // 关闭Autoresizing, 开启Auto layout
    v2.translatesAutoresizingMaskIntoConstraints = NO;
    
    // 获取vc的topLayoutGuide视图
    UIView *tlg = (id)vc.topLayoutGuide;
    NSDictionary *vs = NSDictionaryOfVariableBindings(v2, tlg);
    
    // 使子视图v2的宽度与v1相同
    [v1 addConstraints:
    [NSLayoutConstraint constraintsWithVisualFormat:@"H:|[v2]|"
    	options:0 metrics:nil views:vs]];

    // 把v2放置在topLayoutGuide的下方，并设置v2高度为20
    [v1 addConstraints:
    [NSLayoutConstraint constraintsWithVisualFormat:@"V:[tlg][v2(20)]" 
	options:0 metrics:nil views:vs]];
{% endcodeblock %}


在模拟器运行的得到的效果图如下:

{% img /images/afterguide.png %} 


当然，您也可以在nib编辑器中通过调整各视图的constraints来达到同样的目的。


