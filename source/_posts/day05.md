---
title: day05
date: '2026-01-05 16:14:52'
updated: '2026-01-29 17:38:54'
permalink: /post/day05-z4ejpj.html
comments: true
toc: true
---



# day05

## 今日任务

●  24. 两两交换链表中的节点

●  19.删除链表的倒数第N个节点

●  面试题 02.07. 链表相交

●  142.环形链表II

●  总结

## 详细布置

### 4. 两两交换链表中的节点

用虚拟头结点，这样会方便很多。

本题链表操作就比较复杂了，建议大家先看视频，视频里我讲解了注意事项，为什么需要temp保存临时节点。

题目链接/文章讲解/视频讲解： <u>[https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)</u>

###  19.删除链表的倒数第N个节点  

双指针的操作，要注意，删除第N个节点，那么我们当前遍历的指针一定要指向 第N个节点的前一个节点，建议先看视频。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)</u>

### 面试题 02.07. 链表相交  

本题没有视频讲解，大家注意 数值相同，不代表指针相同。

[题目链接/文章讲解：]()​<u>[https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)</u>

### 142.环形链表II 

算是链表比较有难度的题目，需要多花点时间理解 确定环和找环入口，建议先看视频。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)</u>​[]()

### 总结

对于链表的题目，大家最大的困惑可能就是 什么使用用虚拟头结点，什么时候不用虚拟头结点？

一般涉及到 增删改操作，用虚拟头结点都会方便很多， 如果只能查的话，用不用虚拟头结点都差不多。

当然大家也可以为了方便记忆，统一都用虚拟头结点。

<u>[https://www.programmercarl.com/%E9%93%BE%E8%A1%A8%E6%80%BB%E7%BB%93%E7%AF%87.html](https://www.programmercarl.com/%E9%93%BE%E8%A1%A8%E6%80%BB%E7%BB%93%E7%AF%87.html)</u>

# 4. 两两交换链表中的节点

[24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/solutions/2374872/tu-jie-die-dai-di-gui-yi-zhang-tu-miao-d-51ap/)

# 方法一：

```java
//迭代法
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0, head); // 用哨兵节点简化代码逻辑
        ListNode node0 = dummy;
        ListNode node1 = head;
        while (node1 != null && node1.next != null) { // 至少有两个节点
            ListNode node2 = node1.next;
            ListNode node3 = node2.next;

            node0.next = node2; // 0 -> 2
            node2.next = node1; // 2 -> 1
            node1.next = node3; // 1 -> 3

            node0 = node1; // 下一轮交换，0 是 1， 0作为哨兵节点
            node1 = node3; // 下一轮交换，1 是 3，1作为头节点
        }
        return dummy.next; // 返回新链表的头节点
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/swap-nodes-in-pairs/solutions/2374872/tu-jie-die-dai-di-gui-yi-zhang-tu-miao-d-51ap/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

![1691121590-SWAYuj-lc24-c](/source/images/1691121590-SWAYuj-lc24-c-20260105162314-4rjrhvd.png)

# 解题思路：

- 初始化一个哨兵节点，把node0作为哨兵节点，node1作为head节点，之后每轮循环，先将// 0 -> 2 // 2 -> 1 // 1 -> 3

## 方法二：

```java
class Solution {//递归方法
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode node1 = head;
        ListNode node2 = head.next;
        ListNode node3 = node2.next;

        node1.next = swapPairs(node3); // 1 指向递归返回的链表头
        node2.next = node1; // 2 指向 1

        return node2; // 返回交换后的链表头节点
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/swap-nodes-in-pairs/solutions/2374872/tu-jie-die-dai-di-gui-yi-zhang-tu-miao-d-51ap/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 19.删除链表的倒数第N个节点

[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 由于可能会删除链表头部，用哨兵节点简化代码
        ListNode dummy = new ListNode(0, head);
        ListNode left = dummy;
        ListNode right = dummy;
        while (n-- > 0) {
            right = right.next; // 右指针先向右走 n 步
        }
        while (right.next != null) {
            left = left.next;
            right = right.next; // 左右指针一起走
        }
        left.next = left.next.next; // 左指针的下一个节点就是倒数第 n 个节点
        return dummy.next;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/remove-nth-node-from-end-of-list/solutions/2004057/ru-he-shan-chu-jie-dian-liu-fen-zhong-ga-xpfs/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 解题思路：

- 通过快慢指针，初始化左右指针，以及哨兵节点。核心是通过右指针先向前走n步来指向倒数第n个节点，通过left指针指向第n个节点的前一个节点。

# 面试题 02.07. 链表相交

[面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

## 方法一 ：

```java
public class Solution {//双指针法（浪漫相遇法）🌹
//指针a先遍历完a再去遍历b，指针b相反。两指针走的步数是相同的
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode A = headA, B = headB;
        while (A != B) {
            A = A != null ? A.next : headB;//A = ( A != null ? A.next : headB );赋值优先级非常低，实际先执行右边
            B = B != null ? B.next : headA;
        }
        return A;//此时返回a或b都是相同的
    }
}

作者：Krahets
链接：https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/solutions/1190240/mian-shi-ti-0207-lian-biao-xiang-jiao-sh-b8hn/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- a指针先走完a在走b，b指针相反。两个指针最终走过的步数是相同的，如果两个节点最终在node节点相遇，而最后指向null则说明没用公共节点。反之则相反
- 时间复杂度 O(a+b) ： 最差情况下（即 ∣a−b∣=1 , c=0 ），此时需遍历 a+b 个节点。  
  空间复杂度 O(1) ： 节点指针 A , B 使用常数大小的额外空间。

‍

## 方法二：

```java
public class Solution {//哈希集合，后面有详细讲解，先做了解
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> visited = new HashSet<ListNode>();
        ListNode temp = headA;
        while (temp != null) {
            visited.add(temp);
            temp = temp.next;
        }
        temp = headB;
        while (temp != null) {
            if (visited.contains(temp)) {
                return temp;
            }
            temp = temp.next;
        }
        return null;
    }
}

作者：力扣官方题解
链接：https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/solutions/1395092/lian-biao-xiang-jiao-by-leetcode-solutio-2kne/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 142.环形链表II（未详细解释）

[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

```java
class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow) { // 相遇
                while (slow != head) { // 再走 a 步
                    slow = slow.next;
                    head = head.next;
                }
                return slow;
            }
        }
        return null;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/linked-list-cycle-ii/solutions/1999271/mei-xiang-ming-bai-yi-ge-shi-pin-jiang-t-nvsq/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
