---
title: day04
date: '2026-01-04 17:34:02'
updated: '2026-01-04 19:49:42'
permalink: /post/day04-z1uoqih.html
comments: true
toc: true
---



# day04

#### 

●  链表理论基础

●  203.移除链表元素

●  707.设计链表

●  206.反转链表

## ** 详细布置 **

### ** 链表理论基础 **

建议：了解一下链表基础，以及链表和数组的区别

文章链接：<u>[https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)</u>

---

### [** 203.移除链表元素  **]()

[建议： 本题最关键是要理解 虚拟头结点的使用技巧，这个对链表题目很重要。]()

[题目链接/文章讲解/视频讲解：：]()​<u>[https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html](https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html)</u>​[]()

### [\*\* 707.设计链表  \*\*]()

[建议： 这是一道考察 链表综合操作的题目，不算容易，可以练一练 使用虚拟头结点]()

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html)</u>​[]()

### [\*\* 206.反转链表 \*\*]()

[建议先看我的视频讲解，视频讲解中对 反转链表需要注意的点讲的很清晰了，看完之后大家的疑惑基本都解决了。]()

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html)</u>

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
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode (0,head);//哨兵节点
        ListNode cur = dummy;//循环指针
        while (cur.next!=null){
            if(cur.next.val ==val){
                cur.next = cur.next.next;//指向后一个节点
            }else{
            cur = cur.next;
//当删除节点后，跳出if循环，重新检查是否符合不为空。
//注意不能在if中删除完后直接向后循环，此时新的拼接上来的节点会被掉检查，如果等于val值
//则会答案出错
            }
        }
        return dummy.next;
        }
        
    
}
```

# 思路总结：

- 想要优雅的删除节点，最好在头结点前人为的预置一个dummy节点，也就是哨兵节点，这样头节点就与其他中间节点是一个处理方法了

‍

# 707.设计链表

‍

```java
class MyLinkedList {
    /**
     * 内部类：定义链表节点的结构（图纸）
     * 属于自引用结构：在一个类中引用自身类型
     */
    class ListNode {
        int val;       // 数据域：存储实际数值
        ListNode next; // 指针域：存储下一个节点的堆内存地址

        // 构造函数：用于初始化节点
        ListNode(int val) {
            this.val = val; // 将传入的局部变量 val 赋值给当前对象的成员变量 val
        }//THIS.   核心作用：区分成员变量（Field）和局部变量（Parameter/Local Variable）。
//语法含义：this 是一个指向当前对象实例的引用（java中的指针，等于c语言中的指针 this点就是解引用）。指向我成员变量的val， int
//必用场景：当局部变量与成员变量同名时，必须使用 this. 来显式指定成员变量，否则会产生“变量遮蔽（Variable Shadowing）”现象。
    }

    private int size;          // 记录链表中有效元素的个数
    private ListNode dummyhead; // 虚拟头节点（哨兵），不存储有效数据，简化首部操作

    /**
     * 初始化链表
     */
    public MyLinkedList() {
        this.size = 0;               // 初始化长度为0
        this.dummyhead = new ListNode(0); // 在堆内存中创建一个哨兵节点，由 JVM 自动管理内存
    }

    /**
     * 获取第 index 个节点的值
     */
    public int get(int index) {
        // 1. 检查 index 合法性：不能为负，也不能超出当前元素个数
        if (index < 0 || index >= size) {
            return -1; // 卫语句：非法索引直接提前返回
        }

        // 2. 查找逻辑
        // 因为 dummyhead 是第 -1 个，我们要找第 index 个，需要向后移动 index + 1 次
        ListNode cur = dummyhead;
        int i = 0;
        while (i <= index) { // 循环 index+1 次
            cur = cur.next;  // 沿着 next 指针向后“解引用”
            i++;
        }
        return cur.val;
    }

    /**
     * 在链表最前面插入一个节点
     */
    public void addAtHead(int val) {
        // 直接复用 addAtIndex，index 为 0 即表示在头部插入
        addAtIndex(0, val);
    }

    /**
     * 在链表最后面插入一个节点
     */
    public void addAtTail(int val) {
        // index 为 size 即表示在末尾插入
        addAtIndex(size, val);
    }

    /**
     * 在第 index 个节点前插入新节点
     */
    public void addAtIndex(int index, int val) {
        // 1. 如果 index 大于 size，无法插入，直接返回
        if (index > size) {
            return;
        }

        // 2. 如果 index 小于 0，根据题目惯例，通常视为在头部插入
        index = Math.max(0, index);

        // 3. 准备工作
        size++; // 有效元素个数增加
        ListNode pred = dummyhead; // 找到前驱节点（Predecessor）
        
        // 4. 寻找前驱：要在 index 插入，必须停在 index-1 的位置
        // 循环 index 次即可到达目标位置的前一个节点
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }

        // 5. 核心：三步连线法
        // 
        ListNode toadd = new ListNode(val); // 在堆中创建新节点
        toadd.next = pred.next;             // 第一步：新节点指向原本的后继
        pred.next = toadd;                  // 第二步：前驱节点指向新节点
    }

    /**
     * 删除第 index 个节点
     */
    public void deleteAtIndex(int index) {
        // 1. 检查 index 合法性
        if (index < 0 || index >= size) {
            return;
        }

        // 2. 准备工作
        size--; // 有效元素个数减少
        ListNode pred = dummyhead;

        // 3. 寻找前驱：停在待删除节点的前一个
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }

        // 4. 核心：跳过目标节点
        // 
        // 让前驱直接指向“孙子节点”，中间的节点失去引用，将被 JVM 回收
        pred.next = pred.next.next;
    }
}
```

# 解惑专题：

---

### 知识点专题：单链表节点类的定义与内存模型

#### 1. 代码标准范式

```java
/**
 * Definition for singly-linked list.
 * 单链表节点的类定义
 */
public class ListNode {
    // [数据域]
    int val;
    
    // [指针域 / 引用域]
    ListNode next;//作为对象存储在堆上
    
    // [构造器]//当调用new进行实体话的时候，会在堆山开辟一片新的内存
    ListNode(int x) {
        val = x;
        next = null; // 显式初始化（Java中对象引用默认为null，可省略）
    }
}

```

#### 2. 核心概念解析

**2.1 类 (Class) 与 抽象数据类型 (ADT)**

- **定义**：`public class ListNode`​ 定义了一种**用户自定义引用数据类型 (User-Defined Reference Data Type)** 。
- **作用**：它是对链表节点这一实体的**抽象 (Abstraction)** ，封装了节点的数据属性和结构属性。

**2.2 成员变量 (Member Variables / Fields)**   
类中包含两种不同类型的字段，体现了 Java 的内存存储差异：

- ​**​`int val`​**​  **(基本数据类型 / Primitive Type)**
- **存储内容**：直接存储数值本身（Payload）。
- **内存特性**：占用固定的内存空间（int 占用 4 字节），存储在对象的内存布局中。
- **语义**：代表节点的**数据域**。
- ​**​`ListNode next`​**​  **(引用数据类型 / Reference Type)**
- **存储内容**：存储的是**内存地址**（或者说是指向堆内存中另一个对象的句柄/引用），而非对象本身。
- **自引用结构 (Self-Referential Structure)** ：
- **定义**：一个类在其内部包含了一个指向自身类型对象的引用。
- **原理**：这是链表、树、图等递归数据结构的基础。因为 `next`​ 仅仅是一个引用（在 64 位 JVM 上通常占用 8 字节），它不会导致内存无限递归分配。只有在运行时通过 `new` 实例化新对象时，才会分配实际的节点内存。
- **语义**：代表节点的**指针域**，用于维护线性表的逻辑顺序。

**2.3 构造器 (Constructor)**

- **语法**：`ListNode(int x)` 是类的构造方法。
- **作用**：用于对象的**初始化 (Initialization)** 。
- **执行时机**：当使用 `new`​ 关键字进行**实例化 (Instantiation)**  时被调用。
- 过程：`new`​ 指令在**堆内存 (Heap Memory)**  中开辟一块新的空间存放该节点对象，然后调用构造器将传入的参数 `x`​ 赋值给 `val`，完成状态的初始化。

#### 3. 内存模型图解 (Memory Model)

在 Java 中，链表的构建过程实质上是**堆内存**中对象的链接过程。

- **栈 (Stack)** ：存储局部变量（如 `head`​, `cur`​），这些变量保存的是**引用（地址）** 。
- **堆 (Heap)** ：存储实际的 `ListNode`​ **对象实例**。

> **笔记重点（引用传递）：**   
> 当执行 `nodeA.next = nodeB`​ 时，发生的是**值传递 (Pass by Value)** ，但传递的值是 **nodeB 的内存地址**。这使得 `nodeA`​ 的指针域指向了 `nodeB` 所在的堆内存空间。

#### 4. 常见术语速查表 (Glossary)

|术语 (CN)|术语 (EN)|解释|
| -----------| -------------------------| --------------------------------------------------------------|
|**实例化**|Instantiation|通过 `new` 关键字在堆内存创建对象的过程。|
|**引用**|Reference|Java 中的“指针”，指向对象在内存中的位置。|
|**解引用**|Dereference|通过 `.`​ 操作符访问引用指向的对象的属性（如 `cur.val`）。|
|**空指针异常**|NullPointerException|尝试对 `null`​ 引用进行解引用操作（如 `null.next`）。|
|**哨兵节点**|Sentinel / Dummy Node|不存储有效数据，仅作为链表头部占位符的辅助节点。|
|**垃圾回收**|Garbage Collection (GC)|当一个节点没有任何引用指向它时，JVM 会自动回收其占用的内存。|

---

这是一个非常深入的问题，触及了计算机底层架构和程序运行的本质。我们将这三个核心问题拆解开来，作为你 Java 笔记的进阶篇。

---

### 1. 为什么链表节点是在“堆”中创建的？

在 Java 语言规范中，内存管理遵循一个基本准则：**所有的对象（Object）都在堆中分配。**

- **技术原因（生命周期管理）：**

  - **栈（Stack）**  的数据是“随用随丢”的。函数执行结束，栈帧弹出，里面所有的局部变量立刻销毁。
  - **链表节点（Heap）**  需要“长期生存”。如果你在一个函数里创建了链表节点并返回它，这个节点必须在函数结束后依然存在。
  - 堆内存是由 **JVM 垃圾回收器（GC）**  管理的。只要还有一个引用（指针）指向这个节点，它就不会被销毁。
- 如何识别：  
  在 Java 中，凡是看到 new 关键字（例如 new ListNode(5)），就意味着这块内存是在堆上申请的。

---

### 2. 函数“无限调用自己”是在循环中吗？

**不是循环，是压栈。**  虽然效果上看起来都在重复执行一段代码，但底层机制完全不同。

#### 循环 (Loop)

Java

```
while(true) { 
    // 执行代码 
}
```

- ​**内存表现**​：它就在**同一个栈帧**里反复运行。不会产生额外的内存消耗（除非你在循环里 new 对象）。它像是在操场上​**绕圈跑**，跑多少圈都不会累死。

#### 递归 (Recursion / 无限自调用)

Java

```
public void func() {
    func(); // 无限自调用
}
```

- ​**内存表现**：每调用一次自己，系统就会在栈里塞入一个新的“栈帧”。
- ​**比喻**​：它不是绕圈跑，而是​**盖楼**​。每调用一次就是往上盖一层，但地基（栈的总大小）是有限的，楼盖得太高，地基塌了，这就是 `StackOverflowError`。

---

### 3. 这与“递归”有什么关系？

 **“函数调用自身”这种行为，在编程术语中就叫做递归。**

当你无限调用自身时，你就创造了一个​**没有出口的递归**。

#### 递归的三个要素：

1. ​**自调用**：函数内部调用自己。
2. ​**边界条件（出口）** ：什么时候停止（你现在的困惑是如果没有出口会怎样）。
3. **递推关系**：每一层递归都在解决一个更小的问题。

```java
//死递归
public void test() {
    test(); // 没有任何判断，一进函数就调自己
}
//较为安全的递归
public void countdown(int n) {
    if (n == 0) { // 1. 终止条件：如果数字到0了，就停止
        System.out.println("发射！");
        return; 
    }
    System.out.println(n);
    countdown(n - 1); // 2. 推进：每次数字减1，越来越靠近0
}
```

#### 链表与递归的天然联系：

链表本身就是一种**递归定义**的数据结构：

> 什么是链表？
>
> 链表是一个节点，后面跟着另一个链表（或者 null）。

因此，处理链表题（比如反转链表、合并链表）时，我们经常有两种选择：

1. ​**迭代法 (Iterative)** ​：使用 `while` 循环和临时指针（省空间，不费栈）。
2. ​**递归法 (Recursive)** ：使用函数自调用（代码简洁，但要注意栈深度）。

---

### 专业笔记补充：内存与执行对比图

|**特性**|**循环 (Iteration)**|**递归 (Recursion)**||||
| --| ----------------------------------| -------------------------------------| --| --| --|
|**底层机制**|跳转指令（Jump）|压栈与出栈（Push/Pop）||||
|**内存消耗**|恒定（在当前栈帧操作）|随着深度增加（每层一个新栈帧）||||
|**风险**|死循环（CPU 100%，但不一定报错）|栈溢出（直接报 StackOverflowError）||||
|**联系**|任何递归都可以改写为循环|递归更符合人类处理链表/树的思维||||

#### 深度总结

- **堆（Heap）**  是用来存“肉体”（对象数据）的，空间大，归 GC 管。
- **栈（Stack）**  是用来存“动作”（函数调用）的，空间小，自动管理。
- **递归** 是动作的堆叠，一旦没有出口，动作叠得太高，栈就爆了。

你想看一个用“递归”来处理链表的实际代码例子（比如求链表长度），以此来感受一下它是怎么“压栈”的吗？

‍

# 

# *206.反转链表 （未详细分析）

```java
class Solution {
    // 首先「递」到链表末尾，把末尾节点作为新链表的头节点 revHead
    // 然后在「归」的过程中，把经过的节点依次插在新链表的末尾（尾插法）
    public ListNode reverseList(ListNode head) {
        // 判断 head == null 是为了兼容一开始链表就是空的情况
        if (head == null || head.next == null) {
            return head; // 链表末尾，即下面的 revHead
        }
        ListNode revHead = reverseList(head.next); // 「递」到链表末尾，拿到新链表的头节点
        ListNode tail = head.next; // 在「归」的过程中，head.next 就是新链表的末尾
        tail.next = head; // 把 head 插在新链表的末尾
        head.next = null; // 如果不写这行，新链表的末尾两个节点成环，这俩节点互相指向对方
        return revHead;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/reverse-linked-list/solutions/1992225/you-xie-cuo-liao-yi-ge-shi-pin-jiang-tou-o5zy/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
