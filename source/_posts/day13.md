---
title: day13
date: '2026-01-12 18:43:10'
updated: '2026-01-12 19:43:10'
permalink: /post/day13-qhjxo.html
comments: true
toc: true
---



# day13

# **第六章 二叉树part01**

### 理论基础

需要了解 二叉树的种类，存储方式，遍历方式 以及二叉树的定义

文章讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)</u>

###  递归遍历 （必须掌握）

二叉树的三种递归遍历掌握其规律后，其实很简单

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)</u>

### 迭代遍历 （基础不好的录友，迭代法可以放过）

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%BF%AD%E4%BB%A3%E9%81%8D%E5%8E%86.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%BF%AD%E4%BB%A3%E9%81%8D%E5%8E%86.html)</u>

###  统一迭代   （基础不好的录友，迭代法可以放过）

这是统一迭代法的写法， 如果学有余力，可以掌握一下

题目链接/文章讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html)</u>

### 层序遍历

看完本篇可以一口气刷十道题，试一试， 层序遍历并不难，大家可以很快刷了十道题。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html)</u>

# 递归遍历 （必须掌握）深度优先 DFS

```java
// 前序遍历·递归·LC144_二叉树的前序遍历
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        preorder(root, result);
        return result;
    }

    public void preorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);//result一直在内存当中
        preorder(root.left, result);//函数自调用
        preorder(root.right, result);
    }
}
// 中序遍历·递归·LC94_二叉树的中序遍历
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inorder(root, res);
        return res;
    }

    void inorder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        inorder(root.left, list);
        list.add(root.val);             // 注意这一句
        inorder(root.right, list);
    }
}
// 后序遍历·递归·LC145_二叉树的后序遍历
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        postorder(root, res);
        return res;
    }

    void postorder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        postorder(root.left, list);
        postorder(root.right, list);
        list.add(root.val);             // 注意这一句
    }
}
```

- 没问题！针对你贴出的这几段代码，我们把其中涉及到的**核心 Java 语法**拆解开来，像剥洋葱一样讲清楚。

  对于算法初学者，这段代码里最需要理解的其实是以下 ​**5 个语法点**：

  ---

  ### 1. `List<Integer>`​ 和 `ArrayList<>` —— 那个“装结果的袋子”

  代码里有一句很长：

  Java

  ```
  List<Integer> result = new ArrayList<Integer>();
  ```

  - List (列表)：  
    你可以把它理解为一个\*\*“弹性的数组”\*\*。普通的数组（如 int[]）长度是死的，但 List 是活的，想装多少装多少。
  - \<Integer\> (泛型)：  
    尖括号 \< \> 像是给这个袋子贴了个标签：“只许装整数”。

    - ​*注意*​：这里不能写 `int`​，必须写 `Integer`​。因为 `List`​ 是高级容器，只能装“对象”（引用类型），`Integer`​ 就是 `int` 的包装盒版本。
  - ​**​`new ArrayList<...>()`​** ：

    - ​`List`​ 只是一个​**标准**（接口），相当于说“我要一个列表”。
    - ​`ArrayList`​ 是这个标准的​**具体实现**（品牌），相当于说“给我来个‘数组牌’的列表”。
    - ​`new`​：就是在内存里**真正造出**这个袋子。

  **通俗总结：**  这句话的意思是  **“给我造一个新的、专门装整数的弹性袋子，名字叫 result。”**

  ---

  ### 2. `TreeNode root` —— 那个“节点对象”

  虽然你的代码里没写 `TreeNode` 的定义，但我们在用它：

  Java

  ```
  public void preorder(TreeNode root, ...)
  ```

  在 Java 里，`TreeNode`​ 是一个​**自定义的类（Class）** 。它长得大概是这样：

  Java

  ```
  public class TreeNode {
      int val;         // 这里的 val 就是节点存的数字
      TreeNode left;   // 这是左手牵着的孩子节点
      TreeNode right;  // 这是右手牵着的孩子节点
  }
  ```

  - root.val：  
    中间的 点 (.) 是 Java 最重要的符号，意思是\*\*“的”\*\*。  
    root.val \= root 的 数值。
  - root.left：  
    root 的 左孩子。拿到依然是一个 TreeNode 对象。

  ---

  ### 3. `void`​ vs `List<Integer>` —— “老板”与“打工仔”的区别

  仔细看，你有两个函数，它们的“头衔”不一样：

  **函数 A（老板）：**

  Java

  ```
  public List<Integer> preorderTraversal(TreeNode root)
  ```

  - ​**​`public`​**：公开的，谁都能调。
  - ​**​`List<Integer>`​** ​：​**这是返回值类型**​。意味着这个函数执行完，**必须**交出一个装满整数的列表。它是对外负责的接口。

  **函数 B（打工仔/递归函数）：**

  Java

  ```
  public void preorder(TreeNode root, List<Integer> result)
  ```

  - ​**​`void`​**​：意思是\*\*“空”\*\*。
  - 这意味着这个函数​**不需要返回任何东西**。
  - **为什么？**  因为它手里拿着老板给的袋子（`result`​），它只需要往袋子里塞东西（`result.add`）就行了，不需要把袋子交还回去（因为袋子一直在内存里，大家共用）。

  ---

  ### 4. `null` —— “空”的概念

  Java

  ```
  if (root == null) {
      return;
  }
  ```

  - ​**​`null`​**​：在 Java 里表示\*\*“什么都没有”\*\*。
  - 当递归走到叶子节点（比如之前例子里的 2）的下面时，它的左孩子就是 `null`。
  - ​**​`return;`​** ​：在 `void`​ 的函数里，`return`​ 单独出现，意思就是\*\*“停！函数立刻结束，退回到上一层”\*\*。这在递归里叫“递归出口”。

  ---

  ### 5. 方法的调用 —— “传参”

  Java

  ```
  preorder(root.left, result);
  ```

  - 这叫​**调用函数**。
  - ​**​`root.left`​**：把当前节点的左孩子，当作下一层的“根节点”传进去。
  - ​**​`result`​**​：**关键点！**  这里把那个“袋子”也传进去了。

    - 在 Java 里，这种对象传递是\*\*“传引用”\*\*（相当于给了袋子的钥匙）。
    - 也就是说，​**不管递归多少层，大家手里拿的都是同一个** **​`result`​**​ **袋子**。这也是为什么最后能把所有数字都收集起来的原因。

  ---

  ### 总结一下这几行代码的剧本

  1. ​**​`new ArrayList`​**：主函数先买了个空袋子。
  2. ​**​`preorder(...)`​** ​：主函数把**树的根**和**袋子**交给递归函数（打工仔）。
  3. ​**​`if (root == null)`​** ：打工仔先看一眼，如果是空的（null），直接收工（return）。
  4. ​**​`list.add(root.val)`​** ：如果不空，先把手头这个数字扔进袋子。
  5. ​**​`preorder(root.left...)`​** ：打工仔喊自己的分身去处理左边，把袋子钥匙也给分身。
  6. ​**​`return result`​**：等所有分身都干完活了，主函数把装满的袋子交上去。

  **这下对 Java 的这些写法有没有清晰一点？如果还有哪个具体的单词不认识，随时问我！**

# 迭代遍历 （基础不好的录友，迭代法可以放过）

‍

# 统一迭代   （基础不好的录友，迭代法可以放过）

‍

# 层序遍历

```java
// 102.二叉树的层序遍历
class Solution {
    public List<List<Integer>> resList = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        //checkFun01(root,0);
        checkFun02(root);

        return resList;
    }

    //BFS--递归方式
    public void checkFun01(TreeNode node, Integer deep) {
        if (node == null) return;
        deep++;

        if (resList.size() < deep) {
            //当层级增加时，list的Item也增加，利用list的索引值进行层级界定
            List<Integer> item = new ArrayList<Integer>();
            resList.add(item);
        }
        resList.get(deep - 1).add(node.val);

        checkFun01(node.left, deep);
        checkFun01(node.right, deep);
    }

    //BFS--迭代方式--借助队列
    public void checkFun02(TreeNode node) {
        if (node == null) return;
        Queue<TreeNode> que = new LinkedList<TreeNode>();
        que.offer(node);

        while (!que.isEmpty()) {
            List<Integer> itemList = new ArrayList<Integer>();
            int len = que.size();

            while (len > 0) {
                TreeNode tmpNode = que.poll();
                itemList.add(tmpNode.val);

                if (tmpNode.left != null) que.offer(tmpNode.left);
                if (tmpNode.right != null) que.offer(tmpNode.right);
                len--;
            }

            resList.add(itemList);
        }

    }
}
```

- 你好！欢迎来到二叉树的​**层序遍历**（Breadth-First Search, BFS）。

  这道题和之前的“前中后序”遍历完全不同。

  - **前中后序**是“一条道走到黑”（深度优先 DFS），像走迷宫，撞了南墙才回头。
  - **层序遍历**是“一层一层横着扫描”（广度优先 BFS），像切蛋糕，切完第一层，再切第二层。

  我们先重点讲解最经典、也是面试最常考的 ​**​`checkFun02`​**​ **（队列迭代法）** ，然后再看递归法。

  ---

  ### 第一部分：核心语法 —— “俄罗斯套娃”列表

  首先，你要看懂这个返回值类型，很多初学者会懵：

  Java

  ```
  List<List<Integer>> resList
  ```

  - ​**语法**：这是一个“列表的列表”。
  - ​**通俗理解**​：想象这是一个\*\*“公寓楼”\*\*。

    - 外层的 `List`​ 是​**楼层**（第1层、第2层、第3层...）。
    - 内层的 `List<Integer>`​ 是每一层里的​**住户**。
  - ​**操作**：

    - ​`resList.add(item)`：盖了一层新楼。
    - ​`resList.get(0)`：拿到第1层的所有住户名单。

  ---

  ### 第二部分：重点讲解 `checkFun02` (队列迭代法)

  这是层序遍历的​**标准解法**​。核心工具是 ​**队列 (Queue)** 。

  #### 1. 核心道具：Queue (队列)

  Java

  ```
  Queue<TreeNode> que = new LinkedList<TreeNode>();
  ```

  - ​**概念**​：队列就像​**食堂排队打饭**。
  - ​**规则**​：**先进先出** (FIFO)。谁先来，谁先处理；处理完了，把你的孩子（下一层）排到队伍最后面。
  - ​**关键动作（语法点）** ：

    - ​`que.offer(node)`​：​**入队**。排到队伍尾巴去。
    - ​`que.poll()`​：​**出队**。队伍最前面的那个人出来，办事。
    - ​`que.isEmpty()`：队伍空了吗？
    - ​`que.size()`：队伍里现在有几个人？

  #### 2. 代码思路详解（跟着我的逻辑走）

  Java

  ```
  // 1. 先把大Boss（根节点）放进队伍
  que.offer(node);

  // 2. 只要队伍不空，就一直干活
  while (!que.isEmpty()) {
      // 【关键点A】 这一刻队伍的长度，就是“当前这一层”的人数
      int len = que.size(); 
      List<Integer> itemList = new ArrayList<Integer>(); // 准备一个小本子，记录这一层的人

      // 【关键点B】 只处理“当前这一层”的人数
      while (len > 0) {
          // 1. 队头的人出来（出队）
          TreeNode tmpNode = que.poll(); 
          
          // 2. 把他的名字记在本子上
          itemList.add(tmpNode.val);

          // 3. 把他的孩子（下一层的人）赶去排队（入队）
          if (tmpNode.left != null) que.offer(tmpNode.left);
          if (tmpNode.right != null) que.offer(tmpNode.right);
          
          len--; //这一层处理完一个人了
      }
      
      // 这一层处理完了，把小本子放进大结果里
      resList.add(itemList);
  }
  ```

  #### 3. 初学者最容易晕的逻辑点

  **问：为什么要有** **​`int len = que.size()`​** ​ **和** **​`while(len > 0)`​** ​ **？直接循环不可以吗？**

  **答：不可以！**  这是为了\*\*“分层”\*\*。

  - 队列是动态变化的。你在处理第一层（爸爸）的时候，会把第二层（儿子）加进队伍。
  - 如果你不提前记下 `len`（当前层有几个人），你就分不清队伍里哪些是“这一层的老人”，哪些是“刚加进来的下一层新人”。
  - ​**​`len`​**​ **就是一道闸门**。它说：“现在的队伍里这3个人是这一层的，处理完这3个就停，剩下的都是下一层的。”

  ---

  ### 第三部分：简要讲解 `checkFun01` (递归法)

  这个方法的思路其实是\*\*“作弊”\*\*。它本质上还是前序遍历（深度优先），但是它带了一个记号 `deep`（深度）。

  #### 语法点解析

  Java

  ```
  // 如果结果列表的长度 < 当前深度，说明来到了新的一层
  if (resList.size() < deep) {
      // 赶紧新建一层楼
      List<Integer> item = new ArrayList<Integer>();
      resList.add(item);
  }
  // 找到对应的那一层楼，把数字填进去
  resList.get(deep - 1).add(node.val);
  ```

  #### 思路通俗解

  - **迭代法（Method 02）**  是大家排好队，一层一层处理。
  - **递归法（Method 01）**  是一个人跑得飞快（DFS），但他手里拿着个​**对讲机**。

    - 他跑到第3层，对着对讲机喊：“喂！把结果表里的第3行给我拿过来，我填个数字！”
    - 然后他又跑回第2层，喊：“把第2行拿过来！”

  虽然他跑的顺序是乱的（先钻到底），但他填表的时候是根据 `deep` 索引精准填入的，所以最后结果看起来像是层序遍历。

  ---

  ### 给初学者的总结建议

  1. ​**首选** **​`checkFun02`​**​  **(队列)** ：

     - 这是最正统的层序遍历思维。
     - 必须掌握 `Queue`​, `offer`​, `poll`​, `size` 这四个单词。
     - 记住 `len = que.size()` 是为了锁定当前层的节点数量。
  2. ​**关于** **​`LinkedList`​**：

     - 你会发现代码里写的是 `new LinkedList<TreeNode>()`。
     - 这是因为在 Java 里，`LinkedList`​（链表）实现了 `Queue`（队列）的功能接口。你就把它当成一个标准的队列用就行。
  3. ​**视觉想象**：

     - 想象一个管道，根节点进去，然后它消失了，变出了两个孩子排在后面... 就像细胞分裂一样，但是大家都要乖乖排队。

# 疑惑点：如何保证每一层？

- 这是一个非常关键的问题！这也是很多初学者在写 BFS（广度优先搜索）时最容易晕的地方：**既然队列是“先进先出”的，孩子加进去后，怎么保证不和这一层的父亲混在一起呢？**

  答案就在那句不起眼的 `int len = que.size();` 上。

  为了让你彻底明白，我们用\*\*“快照（Snapshot）”​**的概念来解释，配合一个生动的**​“食堂打饭”\*\*的例子。

  ---

  ### 1. 核心逻辑：两个 while 循环的分工

  你的代码里有两个 `while`，它们的分工极其明确：

  1. ​**外层** **​`while (!que.isEmpty())`​** ：

     - 负责：​ **“只要店里还有人，就别关门”** 。
     - 作用：保证整个遍历过程一直进行，直到处理完所有节点。
  2. ​**内层** **​`while (len > 0)`​** ：

     - 负责：​ **“只处理刚才拍下快照的那一批人”** 。
     - 作用：这就是\*\*“层”\*\*的体现！

  ---

  ### 2. 图解：“快照”是如何把层分开的

  假设我们的树是这样的：

  ```
        3        (第1层)
       / \
      9  20      (第2层)
        /  \
       15   7    (第3层)
  ```

  我们来模拟代码的运行过程，请特别留意 **​`len`​** 这个变量的变化：

  #### **第一轮循环（处理第 1 层）：**

  - ​**状态**​：队列里只有 `[3]`。
  - ​**咔嚓！拍照**​：执行 `int len = que.size();`。

    - 此时 `len = 1`​。这就是计算机对自己说： **“这一层只有 1 个人，我只允许处理 1 次任务。”**
  - ​**开始干活（内层 while）** ：

    - ​`3` 出队。
    - 把 `3`​ 的孩子 `9`​ 和 `20` 加入队列尾部。
    - 此时队列变成了 `[9, 20]`。
    - 但是！因为 `len` 是 1，循环只跑 1 次就结束了。
    - **即便现在队列里有两个人，内层循环也不会继续跑了。**
  - ​**结果**​：第 1 层结束，生成列表 `[3]`。

  #### **第二轮循环（处理第 2 层）：**

  - ​**状态**​：队列里有 `[9, 20]`。
  - ​**咔嚓！拍照**​：执行 `int len = que.size();`。

    - 此时 `len = 2`​。计算机对自己说： **“现在的队伍长度是 2，这 2 个人是这一层的 VIP，后面新来的人先别管。我只处理 2 次。”**
  - ​**开始干活（内层 while）** ：

    - ​**第 1 次**​：`9`​ 出队。它没孩子。`len` 变成 1。
    - ​**第 2 次**​：`20`​ 出队。它的孩子 `15`​ 和 `7`​ 加入队列尾部。此时队列变成 `[15, 7]`​。`len` 变成 0。
    - **停！**  `len`​ 归零了。虽然队列里还有 `15`​ 和 `7`，但内层循环强制结束。
  - ​**结果**​：第 2 层结束，生成列表 `[9, 20]`。

  #### **第三轮循环（处理第 3 层）：**

  - ​**状态**​：队列里有 `[15, 7]`。
  - ​**咔嚓！拍照**​：执行 `int len = que.size();`。

    - 此时 `len = 2`。计算机说：“好，这次的任务量又是 2。”
  - ...（以此类推）

  ---

  ### 3. 在代码中对应这一逻辑

  我把你的代码拿过来，加上详细的注释，你看它是如何体现“分层”的：

  Java

  ```
  // 外层循环：负责宏观的“一直做下去”
  while (!que.isEmpty()) {
      
      // 【关键一步：锁定当前层】
      // 在进入内层循环前，先数一下现在排队的人数。
      // 这时候排在队伍里的，全是“同一辈分”的人！
      // 刚加进来的孩子（下一辈）还在后面，不算在这个 len 里。
      int len = que.size(); 

      List<Integer> itemList = new ArrayList<Integer>();

      // 内层循环：负责“处理完这一批”
      // len > 0 是唯一的控制开关。
      // 一旦 len 减到 0，说明这一层的老前辈都处理完了，
      // 剩下的都是刚才新加进来的孩子，留给下一轮外层循环去处理。
      while (len > 0) {
          TreeNode tmpNode = que.poll();
          itemList.add(tmpNode.val);

          // 这里的 offer 是把下一层的人加到队伍末尾
          // 他们不会影响当前的 len 计数（因为 len 已经在上面固定死了）
          if (tmpNode.left != null) que.offer(tmpNode.left);
          if (tmpNode.right != null) que.offer(tmpNode.right);
          
          len--; // 处理完一个，名额少一个
      }

      resList.add(itemList);
  }
  ```

  ### 总结

  你是如何确定每一层的？

  靠 size() 进行“冻结”。

  在每一轮处理开始前，记录下的那个 `queue.size()`​，就是这一层和下一层的​**分界线**。

  - ​`len`​ 就像是一张​**限量的票**。
  - 如果有 2 个节点在这一层，我就发 2 张票。
  - 处理过程中产生的新节点（孩子），虽然进了队伍，但他们​**没有这一轮的票**，只能等下一轮外层循环重新发票时，才能轮到他们。

‍
