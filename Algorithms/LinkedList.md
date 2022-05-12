# LinkedList

__TOC__



## 1 单链表翻转

### [Leetcode 206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

#### Recursive

##### 特殊案例

输入节点为空或只有一个节点。

##### 最小情况[^1]

递归的核心是将大问题转化为小问题，一直找到最小的情况后，向上层返回。在这个题目中，我们可以将问题一直向下一个（`head.next`）转移，将范围缩小。一直到当节点下一个为空（`head.next == null`）的时候就想上返回。

##### 调换节点

当我们获得了来自下一层倒序的链表后，第二步就是要将当层的节点放入恰当的位置（进行调序）。

##### 复杂度分析

- 时间复杂度：O(log(N)) 每个节点只进行一边操作

- 空间复杂度：O(N) 递归需要栈[^2]来存储信息，递归N层

##### 例子分析

![ReverseList Recursive](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/ReverseList%20Recursive%20(1).png)

##### 代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        
        return newHead;
    }
}
```



#### Iteration

##### 特殊案例

输入节点为空或只有一个节点。

##### 哨兵节点

创建哨兵节点[^3]来返回最后结果

##### 调换节点

使用循环来判断是否已经调换完成，在调换过程中需要注意指针的变换。

##### 复杂度

- 时间复杂度：O(N)，每个节点只需要遍历一次

- 空间复杂度：O(1)，不需要额外创建空间

##### 代码

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        //先判断边界条件
        if(head==null || head.next==null) return head;
        //新建一个尾部 null
        ListNode pre=null;
        while(head != null){
            //1. 保存剩下一段链表的起始点
            ListNode temp = head.next;
            //2. 把head的next进行更改
            head.next = pre;
            //3. 把pre和head进行后移
            pre = head;
            head = temp;
        }
        // 因为head最后为null了，所以返回pre
        return pre;
    }
}
```

Leetcode 92

Leetcode 234



## 2 链表中环的检测

## 3 两个有序链表的合并

## 4 删除链表倒数第N各节点

## 5 求链表的中间节点







[^1]: The base case is a way to **return without making a recursive call**.
[^2]: Space complexity of recursive algorithm is proportinal to **maximum depth** of recursion tree generated
[^3]: The dummy head, that will point to your **final answer** or list that you will return.

