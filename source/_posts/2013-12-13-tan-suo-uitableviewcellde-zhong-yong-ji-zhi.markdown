---
layout: post
title: "探索UITableViewCell的重用机制"
date: 2013-12-13 12:53
comments: true
categories: iOS
---
{% img  /images/reuseCell.png %}

为了充分利用iOS设备有限的内存，UITableView这个API的设计巧妙地利用了这样一个事实：
尽管一个table可能需要显示成千上万个cell的数据, 但在有限的屏幕空间上，用户只能看见当前显示的cell。因此，UITableView采用按需创建cell的策略，随着table的滑动，前面不再可见的cell会被加入一个缓存队列, 当后面新的一行数据需要显示时，可以重用队列中的旧的cell内存来存放数据，如果队列为空，则分配内存新创建一个UITableViewCell对象。

假设一个UITableView有100000行数据，实际上需要创建的UITableViewCell对象有多少个呢？
下面的代码让您更直观地理解cell的重用机制。您可以自己创建一个UITableViewController
并实现相应的方法来验证。

首先在MyViewController.h中声明一个cellCount属性。
{% codeblock lang:objc %}

@interface MyViewController : UITableViewController

@property (nonatomic, strong) NSMutableArray *myData;
@property (nonatomic) NSUInteger cellCount; // 记录新创建的cell的数量

@end

{% endcodeblock %}

然后在MyViewController.m中实现如下方法：

{% codeblock lang:objc %}
#import “MyViewController.h"

@implementation MyViewController
@synthesize myData, cellCount;

- (void)viewDidLoad
{
    [super viewDidLoad];
    cellCount = 0;
    myData = [[NSMutableArray alloc] init];
    
    for (NSUInteger i = 0; i < 100000; ++i) {
        NSString *dataString = [NSString stringWithFormat:@"Item %lu", i];
        [myData addObject:dataString];
    }

    NSLog(@"The array contains %lu items", [myData count]);
}


-(NSInteger)tableView:(UITableView *)tableView
    numberOfRowsInSection:(NSInteger)section
{
    return [myData count];
}


- (UITableViewCell *)tableView:(UITableView *)tableView
    cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *cellIndentifier = @"Cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell"];
    if (cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault
                                       reuseIdentifier:cellIndentifier];
        ++self.cellCount;
    }
    cell.textLabel.text = [myData objectAtIndex:indexPath.row];
    NSLog(@"There are actually %lu new cells now”, cellCount);
    return cell;
}
{% endcodeblock %}

假设一个tableView视图最多只能完整显示10个cell, 以上代码的输出如图：
{% codeblock %}
The array contains 100000 items
There are actually 1 new cells now
There are actually 2 new cells now
There are actually 3 new cells now
There are actually 4 new cells now
There are actually 5 new cells now
There are actually 6 new cells now
There are actually 7 new cells now
There are actually 8 new cells now
There are actually 9 new cells now
There are actually 10 new cells now
There are actually 11 new cells now
There are actually 11 new cells now
……
{% endcodeblock %}
我们观察到无论怎样向下滑动tableView，至多只有11个cell被创建！最开始分配的cell内存被后来的数据行反复利用。
这种机制的确很好地利用了内存空间。
为什么是11个而不是10个呢？如下图所示，在刚开始向下滑动的某一瞬间, 第1行数据Item0的cell仍有部分可见，
此时缓存队列为空，为了显示第11行，UITableView创建了第11个cell对象。这也是最后一个新建的cell。
当第1行数据完全不可见时，第1个cell入队列, 接下来需要显示的第12行则重用了队列中第一个cell的空间。
依此类推，无论table中有多少行数据，无论接下来如何滑动，每一个数据行都可以循环利用这11个cell对象的内存。

{% img  /images/tableView.png %}

