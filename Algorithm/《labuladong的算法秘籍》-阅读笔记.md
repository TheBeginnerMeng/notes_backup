开始时间：20220422

完成时间：xx

# 开篇：算法秘籍阅读指南

培养框架思维：跳出细节，从整体上来理解各种算法的底层逻辑。

# 无剑篇：刷题心法

## 学习算法和刷题的框架思维

### 1. 数据结构的存储方式

数据结构的存储方式有两种：顺序存储（数组）和链式存储（链表）

> 数组

​	优点：可以随机访问，相对节约存储空间

​	缺点：扩容时间复杂度O(N)，插入和删除元素时间复杂度O(N)

> 链表

​	优点：插入和删除元素，时间复杂度O(1)

​	缺点：不能随机访问，消耗更多的存储空间

### 2. 数据结构的基本操作

任何数据结构的操作：遍历+访问—>增删改查

遍历+访问的形式：线性和非线性的

线性：for/while，非线性：递归

数组的遍历：

```python
def traverse(arr):
    for i in range(len(arr)):
        # 迭代访问arr[i]
```

链表的遍历，兼具迭代和递归结构：

```python
class ListNode():
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def traverse(head: ListNode):
    p = head
    while p != Null:
        p = p.next
        # 迭代访问 p.val
        
def traverse(head: ListNode):
    # 递归访问 head.val
    traverse(head.next)
```

二叉树遍历框架，非线性递归遍历结构：

```python
class TreeNode():
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def traverse(root: TreeNode):
    traverse(root.left)
    traverse(root.right)
```

二叉树框架扩展为N叉树遍历框架：

```python
class TreeNode():
    def __init__(self, val=0, children=list()):
        self.val = val
        self.children = children
def traverse(root: TreeNode):
    for child in root.children:
        traverse(child)
```

### 3. 算法刷题指南

建议的刷题顺序：

1. 先学习数组、链表等基本数据结构的常用算法，如：单链表翻转、前缀和数组、二分搜索等；

2. 学会基础算法之后，再来刷二叉树，大部分算法技巧，本质都是树的遍历问题；

   二叉树的题目，都是基于下面这个框架：

   ```python
   def traverse(root):
       # 前序位置
       traverse(root.left)
       # 中序位置
       traverse(root.right)
       # 后序位置
   ```

## 计算机算法的本质

### 算法的本质

算法的本质就是**穷举**。穷举有两个关键难点：**无遗漏**和**无冗余**

思考一道算法题，从两个维度：

1. **如何穷举**？无遗漏的穷举出所有可能的解
2. **如何聪明地穷举**？避免所有冗余的计算

如何穷举的难点：递归类问题，动态规划系列

如何聪明地穷举的难点：非递归算法技巧

## 数组、单链表系列算法

单链表常考的技巧是双指针。如用快慢指针来判断链表是否有环。

数组常用的技巧很大一部分也还是双指针相关的技巧，即教你如何`聪明地穷举`。

## 二叉树系列算法

二叉树的递归分为两类思路：**第一类是遍历一遍二叉树得答案**（回溯），**第二类是通过分解问题计算出答案**（动态规划）。

# 学剑篇：基础数据结构

数组链表代表着计算机最基本的两种存储形式：顺序存储和链式存储。

数组链表的主要算法技巧是**双指针**，双指针又分为中间向两端扩散的双指针、两端向中间收缩的双指针、快慢指针。

此外，数组还有**前缀**和**差分数组**，也属于必知必会的算法技巧。

## 小而美的算法技巧：前缀和数组

前缀和技巧适用于快速、频繁地计算一个索引区间内的元素之和。

# 仗剑篇：进阶数据结构





# 霸剑篇：暴力搜索算法





# 悟剑篇：动态规划





# 朴剑篇：其他经典算法