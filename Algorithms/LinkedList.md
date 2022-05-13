# LinkedList

[toc]

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

### [Leetcode 92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

这个题目无法化为更小情况，因此无法使用递归方法。

##### 特殊案例

1. 无需翻转：翻转链表长度为0
2. 输入链表长度为0或者1

##### 解题思路

1. 找到调换部分链表的起始位置
2. 对该部分链表进行调换
3. 将调换链表左边、右边以及调换部分进行连接

##### 复杂度

- 时间复杂度：O(N)，所有链表节点只遍历一遍
- 空间复杂度：O(1)，无需开辟新的空间

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // dummy head
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        // left part
        ListNode prev = null, cur = dummy; // 为什么使用prev是因为，可能m是0
        for (int i = 0; i < left; i++) {
            prev = cur;
            cur = cur.next;
        }
        
        // reverse nodes
        ListNode prev2 = prev;
        ListNode cur2 = cur;
        
        for (int i = left; i <= right; i++) {
            ListNode newHead = cur2.next;
            cur2.next = prev2;
            prev2 = cur2;
            cur2 = newHead;
        }
        
        prev.next = prev2;
        cur.next = cur2;
        
        return dummy.next;
    }
}
```



### [Leetcode 234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

##### 特殊案例

链表长度为0或者1的时候，该链表为回文。

##### 解题思路

- 使用额外空间：将链表复制到列表中，然后使用两个指针分别从头和尾向中间扫描，通过比较每次扫描的节点判断是否回文。
- 不使用额外空间：将后半部分列表进行翻转，然后比较前半部分和后半部分列表。
  1. 找到链表中间位置，注意：当链表中节点为偶数个的时候，中间节点是在左半部分的最后一个节点；当为奇数个的时候，则为正当中那个。
  2. 定义后半部分：中间节点之后开始
  3. 翻转后半部分节点
  4. 分别从前半部分和翻转后的后半部分开始遍历，判断是否回文

##### 复杂度

- 时间复杂度：O(N)，**找中点**遍历所有节点一次，**翻转**遍历一半节点一次，**比较**遍历所有节点一次
- 空间复杂度：O(1)，无需额外空间。

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
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        
        // get mid
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode mid = findMid(head);
        
        ListNode rightPart = mid.next;
        mid.next = null;
        
        ListNode newHead = reverseList(rightPart);
        
        while (newHead != null) {
            if (head.val != newHead.val) {
                return false;
            }
            
            head = head.next;
            newHead = newHead.next;
        }
        
        return true;
    }
    
    private ListNode findMid(ListNode head) {
        ListNode fast = new ListNode(0);
        ListNode slow = new ListNode(0);
        
        fast.next = head;
        slow.next = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
    }
    
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        
        while (cur != null) {
            ListNode nextNode = cur.next;
            cur.next = prev;
            prev = cur;
            cur = nextNode;
        }
        
        return prev;
    }
}
```

## 2 链表中环的检测

## 3 两个有序链表的合并

## 4 删除链表倒数第N各节点

## 5 求链表的中间节点







[^1]: The base case is a way to **return without making a recursive call**.
[^2]: Space complexity of recursive algorithm is proportinal to **maximum depth** of recursion tree generated
[^3]: The dummy head, that will point to your **final answer** or list that you will return.

