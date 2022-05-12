# LinkedList

## 单链表翻转

### [Leetcode 206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

#### Recursive

##### 最小情况[^1]

递归的核心是将大问题转化为小问题，一直找到最小的情况后，向上层返回。在这个题目中，我们可以将问题一直向下一个（`head.next`）转移，将范围缩小。一直到当节点下一个为空（`head.next == null`）的时候就想上返回。

##### 调换节点

当我们获得了来自下一层倒序的链表后，第二步就是要将当层的节点放入恰当的位置（进行调序）。

##### 复杂度分析

- 时间复杂度：O(log(N)) 每个节点只进行一边操作

- 空间复杂度：O(N) 递归需要栈[^2]来存储信息，递归N层

##### 例子分析

![ReverseList Recursive](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/ReverseList%20Recursive%20(1).png)

#### Iteration





Leetcode 92

Leetcode 234



[^1]: The base case is a way to **return without making a recursive call**.
[^2]: space complexity of recursive algorithm is proportinal to **maximum depth** of recursion tree generated

## 链表中环的检测

## 两个有序链表的合并

## 删除链表倒数第N各节点

## 求链表的中间节点







