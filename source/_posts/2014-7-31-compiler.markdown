--- 
layout: post
title: "编译原理入门学习小结"
date: 2014-7-31 19:50
comments: true
categories: compiler
---

{% img /images/loudenbook.png %}

首先，编译原理对程序员而言无疑是非常重要的，有句话说得好：If you don't understand how 
compiler works, you don't really know how computer works. 且不说编译技术在各种
软件中的广泛应用，入门的学习经历中已经让我获益匪浅。在尝试手写scanner和parser过程中温习了
trees，hashtable等重要的数据结构, 又掌握了更多编程语言的细节，学习code generation的过程
可以把以前学过的计算机体系结构，操作系统等知识联系起来，设计grammer和自动机的时候又遇到了
语言学课程上熟悉的Noam Chomsky。此外，编译器构建本身是一项大工程，会引导你去思考如何编写
可维护的代码。

然后，请允许我来安利一下[Compiler Construction: Principles and Practice](http://www.amazon.com/Compiler-Construction-Principles-Kenneth-Louden/dp/0534939724)这本书。
一本出色的入门书可以节省你许多时间，比起偏理论的Dragon Book，Louden的这本书真正做到了
兼顾理论和实践。作者引导你动手实现一个叫做TINY迷你的编译器，感兴趣的读者还可以在此基础上
继续开发新的特性。

最后附上这本书在Amazon上的书评：
{% img /images/AmazonReview.png %}



