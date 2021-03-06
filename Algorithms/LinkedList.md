# LinkedList

[toc]

![LinkedList](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/LinkedList.png)

## 1 单链表翻转

### [Leetcode 206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

#### 递归方法

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



#### 迭代方法

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

### [Leetcode 141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

##### 特殊案例

输入链表为空或者只有一个节点

##### 解题思路

使用快、慢指针，判断这两个指针能否相遇。

##### 代码

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }
        
        ListNode slow = head, fast = head;
        
        // 如果fast.next == null 了就表明无循环
        while (fast.next != null && fast.next.next != null) {
            
            slow = slow.next;
            fast = fast.next.next;
            
            if (slow == fast) {
                return true;
            }
        }
        
        return false;
    }
}
```

### [Leetcode 142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

##### 特殊案例

输入链表为空或者只有一个节点

##### 解题思路

1. 使用快、慢指针
2. 数学推导：
   1. 快指针路程：2v * t = a + b + N * c
   2. 慢指针路程：v * t = a + b
   3. a + b = N * c (此时b为慢指针所在位置)
   4. 慢指针从b点移动a步，可以到达环的入口

<img src="https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/142_fig1.png" alt="142_fig1" style="zoom: 25%;" />

##### 复杂度

- 时间复杂度：O(N)，慢指针不会超过链表的总长度

- 空间复杂度：O(1)，没有使用额外的空间

## 3 两个有序链表的合并

### [Leetcode 160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

##### 特殊案例

1. 两个链表不相交：不需要单独列出
2. 两个链表为空

均不需要单独列出判断，因为不相交和为空最终都会变为`Null`跳出循环。

##### 解题思路

1. 让节点a，b分别从链表1,2出发，当到达尾部时，a继续从2开始，b继续从1开始。
2. 数学证明：链表1长度为N，链表2长度为M。那么，两个节点相遇的时候，就一定是两个链表相交的点，因为M+N = N+M。

##### 复杂度

- 时间复杂度：O(M+N)，M+N为两个链表节点总和。
- 空间复杂度：O(1)，没有开辟额外空间。

##### 代码

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA, b = headB;
        
        while (a != b) {
            a = a == null ? headB : a.next;
            b = b == null ? headA : b.next;
        }
        
        return a;
    }
}
```

### [Leetcode 21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

#### 递归方法

##### 最小情况

当输入节点为`Null`的时候返回

##### 解题思路

1. 递归每一层做的事情：找到下一个节点，返回的时候将该节点连接到要输出结果的后面。
2. 递归每一层会有以下三种情况：
   1. 当链表1已经为`Null`时，那么就直接把链表2的节点接上去，
   2. 当链表2已经为`Null`时，那么就直接把链表1的节点接上去。
   3. 当两个链表都有节点时，则进行比较两个链表的节点，并且将更小值的节点接上去。

##### 复杂度

- 时间复杂度：O(N + M)，两个链表所有节点都遍历一遍
- 空间复杂度：O(N + M)，递归N + M次

##### 代码

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) {
            return list2;
        } else if (list2 == null) {
            return list1;
        } else if (list1.val < list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

### 迭代方法

##### 解题思路

分别遍历两个链表，将更小的加到结果链表尾部

##### 复杂度

- 时间复杂度：O(N + M)
- 空间复杂度：O(1)

##### 代码

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode curr = new ListNode(0);
        ListNode dummy = curr;
        
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                curr.next = list1;
                list1 = list1.next;
            } else {
                curr.next = list2;
                list2 = list2.next;
            }
            
            curr = curr.next;
        }
        
        if (list1 != null) {
            curr.next = list1;
        }
        
        if (list2 != null) {
            curr.next = list2;
        }
        
        return dummy.next;
    }
}
```

### [Leetcode 23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

##### 解题思路

使用`PriorityQueue`将这些链表排序，按照头结点的值。然后每次取出最顶部（最小）的头结点，放入到结果链表中，并将该节点从原节点去除掉后，放回`PriorityQueue`。直到将所有节点按照从小到大的顺序存入到结果链表中

##### 复杂度

- 时间复杂度：O(N * log(K))，N是所有节点数，K是链表个数。log(K)是每次将链表放回`PriorityQueue`时候排序所消耗的时间。
- 空间复杂度：O(N)

##### 代码

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
        
        for (ListNode node : lists) {
            if (node != null) {
                pq.offer(node);
            }
        }
        
        // pop top list and put head into final list
        // check if it is null, if so do not add it back to pq
        ListNode res = new ListNode(0);
        ListNode dummy = res;
        
        while (!pq.isEmpty()) {
            ListNode temp = pq.poll();
            res.next = new ListNode(temp.val);
            temp = temp.next;
            res = res.next;
            
            if (temp == null) {
                continue;
            } else {
                pq.offer(temp);
            }
        }
        
        return dummy.next;
    }
}
```

## 4 删除链表倒数第N各节点

## 5 求链表的中间节点







[^1]: The base case is a way to **return without making a recursive call**.
[^2]: Space complexity of recursive algorithm is proportinal to **maximum depth** of recursion tree generated
[^3]: The dummy head, that will point to your **final answer** or list that you will return.

