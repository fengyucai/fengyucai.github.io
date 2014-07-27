--- 
layout: post
title: "锐捷校园网Mac客户端登录问题的解决方案"
date: 2013-11-01 22:15
comments: true
categories: networking
---
由于多数同学之前没接触过命令行shell，建议下载完客户端后把文件[Ruijie Suppiant.app]拖曳到
Finder的应用程序目录下(如图)，接下来只要直接copy我贴出的命令，并粘贴到终端输入就可以了。

{% img /images/app.png %}  
##打开终端
点击右上角的spotlight搜索并打开应用程序［终端］, 如图所示

{% img /images/terminal.png %} 

在终端输入以下命令, 注意每一行结束都要敲击一下回车[return]
{% codeblock %}
cd /Applications/Ruijie\ Supplicant.app/Contents/MacOS/
sudo chmod 777 Supplicant
{% endcodeblock %}
然后在命令行提示下输入你当前用户的密码，敲击回车确认
(注意，在输入过程中password:后面不会显示任何字符, 保证输入无误并敲击回车即可。
如果发觉已输错,按住delete键清空并再次输入)
再次输入以下命令运行Supplicant二进制文件
{% codeblock %}
./Supplicant
{% endcodeblock %}
这时你应该可以看到如下界面。
正确选择网卡是关键的一个步骤。如何确认你已经选择了正确的网卡呢？

{% img /images/network.png %} 

点击导航条左上角苹果的logo, 点击关于本机-更多信息－系统报告 

{% img /images/BSD.png %} 

如图所示，选中[硬件]下拉列表中的[以太网卡]选项，查看右边的BSD名称，请确认你在客户端下拉菜单
选择的网卡名称与BSD名称一致。然后就可以点击连接了。如果可以成功连上，那么恭喜你，不用继续
往下看了，最后在Dock中保留锐捷的图标就OK了。

##权限的问题
部分同学反映点击连接后会出现找不到网卡的提示，终端界面则出现了permission denied的警告，如图。 

{% img /images/problem.png %} 

原因是一个ChmodBPF的启动项丢失导致了非root用户对文件/dev/bpf没有read/write权限，
由于系统每次重启都会重置/dev下文件的权限，每次登录都要在命令行下为bpf文件添加权限无疑是反人类的，ChmodBPF的作用正是在系统启动时帮你自动完成这个任务。解决方法如下：

##安装Xcode和Git

因为接下来需使用git命令，首先请确认电脑上已经安装了Xcode和Git, Xcode可在App Store免费获取，

OS X的Git安装包传送门[git-1.8.4.2-intel-universal-snow-leopard.dmg](https://code.google.com/p/git-osx-installer/downloads/detail?name=git-1.8.4.2-intel-universal-snow-leopard.dmg&can=3&q=&sort=-uploaded)
##获取ChmodBPF

继续在终端下输入以下命令，从我的Github仓库中获取这个文件
{% codeblock %}
cd ~
sudo git clone git://github.com/cocoalover/ChmodBPF.git
sudo cp -R ChmodBPF /Library/StartUpItems/ChmodBPF 
{% endcodeblock %}
输入用户密码。首次运行git可能需要同意Apple的协议，按照文字提示
按下空格键浏览协议，并输入agree同意该协议, 当看到"Checking Connectivity...Done"
的提示后就可以输入上面的sudo cp命令。
完成后点击屏幕左上角苹果的logo, 点击 关于本机-更多信息－系统报告
选中[软件]菜单下的[启动项], 应该可以看到多了一个名称为ChmodBPF的启动项（如图）

{% img /images/startup.png %} 

##为当前用户添加一个群组
打开系统偏好设置－用户与群组, 点击左下角的＋按钮添加一个名为access_bpf的群组，如图所示，
创建后记得勾选当前的用户名。

{% img /images/group.png %} 

##修改/dev目录下相关文件的群组
继续在终端下输入:
{% codeblock %}
cd /dev/
sudo chgrp access_bpf bpf*
{% endcodeblock %}
重新启动计算机。。。
再次打开终端，输入以下命令运行锐捷客户端
{% codeblock %}
cd /Applications/Ruijie\ Supplicant.app/Contents/MacOS/
./Supplicant
{% endcodeblock %}
确保正确填写认证信息后，应该就可以正常连上校园网了。
在Dock中保留Ruijie的图标，以后登录只需点击图标，不再需要和命令行界面打交道了。
童鞋们如果出现其他问题可以在评论中向我提问，有更好的解决方案也欢迎讨论。

