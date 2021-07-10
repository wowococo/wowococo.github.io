---
title: LRU 实现
excerpt: 感觉 tim 的搜索框那里的历史记录展示就是用双向链表做的，不过可能是 MRU
date: 2021-01-31 18:37:12
tags: Leetcode 双向链表
---

思路：

Python dict + 双向链表

```
class Node:
	def __init__(self, k, v):
		self.key = k
		self.val = v
		self.prev = None
		self.next = None
		
class LRUCache:
	def __init__(self, capacity):
		self.capacity = capacity
		self.dic = dict()
		self.head = Node(0, 0)
		self.tail = Node(0, 0)
		self.head.next = self.tail
		self.tail.prev = self.head
	
	def get(self, key):
		if key in self.dic:
			n = self.dic[key]
			self.remove(n)
			self.add(n)
			return n.val
		return -1
		
	def put(self, key, value):
		if key in self.dic:
			n = self.dic[key]
			self.remove(n)
		n = Node(key, value)
		self.add(n)
		self.dic[key] = n
		if len(self.dic) > self.capacity:
			_n = self.tail.prev
			self.remove(_n)
			del self.dic[_n.key]
	
	# 向头结点后插入结点
	def add(self, node):
		node.next = self.head.next
		self.head.next.prev = node
		
		self.head.next = node
		node.prev = self.head
	
	# 删除链表中某个节点
	def remove(self, node):
		p = node.prev
		n = node.next
		p.next = n
		n.prev = p

```



