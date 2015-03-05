--- 
layout: post
title: "关于Linked List的奇技淫巧"
date: 2014-1-05 21:16
comments: true
categories: Data Structures
---

{% img /images/linkedlist.png %}

单链表是一种十分简单的数据结构，然而基本操作的组合可以衍生出不少tricky的问题。
定义单链表的结构如下(C++11):
{% codeblock lang:cpp %}
template <typename T>
class node {
	public:
		T data;
		shared_ptr<node<T>> next;
};
{% endcodeblock %}

##Reverse Linked List
一种常见的操作是链表的反转。最自然的实现方式是使用递归：
{% codeblock lang:cpp %}
template <typename T>
shared_ptr<node<T>> reversed_linked_list(shared_ptr<node<T>> & head) {
	if (!head || !head->next)
		return head;
	shared_ptr<node<T>> new_head = reversed_linkded_list(head->next);
	head->next->next = head;
	head->next = nullptr;
	return new_head;	
}
{% endcodeblock %}
然而这种方法的缺点是需要O(n)的栈空间。因为不是尾递归，编译器无法把代码优化成迭代的形式。
迭代的方法只需O(1)的额外空间，利用了prev和curr两个指针：
{% codeblock lang:cpp %}
template <typename T>
shared_ptr<node<T>> reversed_linked_list(shared_ptr<node<T>> & head) {
	shared_ptr<node<T>> prev = nullptr, curr = head;
	while (curr) {
		shared_ptr<node<T>> temp = curr->next;
		curr->next = prev;
		prev = curr;
		curr = temp;
 	}
 	return prev;
}
{% endcodeblock %}

##Find the Middle Point of LinkedList
求一个单链表的中间结点利用了pre_slow, slow和fast三个指针:
{% codeblock lang:cpp %}
slow = fast = L, pre_slow = nullptr;
while (fast) {
	fast = fast->next;
	if (fast) {
		pre_slow = slow;
		slow = slow->next;
		fast = fast->next;
	}
}
{% endcodeblock %}

##Zipping A Linked List
更难的问题是求链表的"zip"。
给定一个单链表(L0, L1,...Ln-1)，定义链表的“zip"为(L0, Ln-1, L1, Ln-2...)。
求zip的过程由三个基本操作组合而成，除了上面提到的两个，还有一个基本操作是
将一个链表表头结点连接到另一个链表，可以抽象成如下的子过程：
{% codeblock lang:cpp %}
template <typename T>
void connect_a_next_to_b_advance_a(shared_ptr<node<T>> & a,
								   const shared_ptr<node<T>> & b) {
	shared_ptr<node<T>> temp = a->next;
	a->next = b;
	a = temp;
}
{% endcodeblock %}

完整的Zipping过程如下:

{% codeblock lang:cpp %}
template <typename T>
shared_ptr<node<T>> zipping_linked_list(const shared_ptr<node<T>> & L) {
	// Find the middle point of the linked list
	shared_ptr<node<T>> slow = L, fast = L, pre_slow = nullptr;
	while (fast) {
		fast = fast->next;
		if (fast) {
			pre_slow = slow;
			slow = slow->next;
			fast = fast->next;
		}
	}
	// The list only contains one node
	if (!pre_slow) return L;
	// Split the list into two lists
	pre_slow->next = nullptr;
	shared_ptr<node<T>> reverse = reversed_linked_list(slow), curr = L;
	// Zipping the list
	while (curr && reverse) {
		connect_a_next_to_b_advance_a(curr, reverse);
		if (curr) connect_a_next_to_b_advance_a(reverse, curr);
	}
	return L;
}
{% endcodeblock %}

关于链表还有许多变式的问题，解决的关键在于把原问题分解成子过程，还要考虑各种边界情况：
比如空链表，长度为1的链表，奇偶长度的链表等等。


