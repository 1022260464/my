---
title: day13
date: '2026-01-29 17:41:08'
updated: '2026-01-29 18:00:13'
permalink: /post/day13-zhvoq8.html
comments: true
toc: true
---



# day13

# **第六章 二叉树part01**

‍

### 理论基础

需要了解 二叉树的种类，存储方式，遍历方式 以及二叉树的定义

文章讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)</u>

### 递归遍历 （必须掌握）

二叉树的三种递归遍历掌握其规律后，其实很简单

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)</u>

###  迭代遍历 （基础不好的录友，迭代法可以放过）

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%BF%AD%E4%BB%A3%E9%81%8D%E5%8E%86.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%BF%AD%E4%BB%A3%E9%81%8D%E5%8E%86.html)</u>

### 统一迭代   （基础不好的录友，迭代法可以放过）

这是统一迭代法的写法， 如果学有余力，可以掌握一下

题目链接/文章讲解：<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html)</u>

### 层序遍历

看完本篇可以一口气刷十道题，试一试， 层序遍历并不难，大家可以很快刷了十道题。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html)</u>

‍

# 递归遍历 （必须掌握）:

- [ ] [144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)
- [ ] [145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)
- [ ] [94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

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
        result.add(root.val);
        preorder(root.left, result);
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

# 迭代遍历:

```java
// 前序遍历顺序：中-左-右，入栈顺序：中-右-左
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.right != null){
                stack.push(node.right);
            }
            if (node.left != null){
                stack.push(node.left);
            }
        }
        return result;
    }
}

// 中序遍历顺序: 左-中-右 入栈顺序： 左-右
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()){
           if (cur != null){
               stack.push(cur);
               cur = cur.left;
           }else{
               cur = stack.pop();
               result.add(cur.val);
               cur = cur.right;
           }
        }
        return result;
    }
}

// 后序遍历顺序 左-右-中 入栈顺序：中-左-右 出栈顺序：中-右-左， 最后翻转结果
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.left != null){
                stack.push(node.left);
            }
            if (node.right != null){
                stack.push(node.right);
            }
        }
        Collections.reverse(result);
        return result;
    }
}
```

# 统一迭代:

```java

```

---

- [ ] [102.二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)
- [ ] [107.二叉树的层次遍历II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)
- [ ] [199.二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)
- [ ] [637.二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)
- [ ] [429.N叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)
- [ ] [515.在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)
- [ ] [116.填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)
- [ ] [117.填充每个节点的下一个右侧节点指针II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)
- [ ] [104.二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)
- [ ] [111.二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

# 102.二叉树的层序遍历

[102. 二叉树的层序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/)

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if (root != null) queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                tmp.add(node.val);
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}

作者：Krahets
链接：https://leetcode.cn/problems/binary-tree-level-order-traversal/solutions/2361604/102-er-cha-shu-de-ceng-xu-bian-li-yan-du-dyf7/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是二叉树 **层序遍历 (Level Order Traversal)**  的教科书级标准写法，也就是广度优先搜索 (BFS)。

  这段代码的核心难点和精髓在于：**如何确保同一层的节点被放在同一个列表中？**

  下面我为你拆解这个逻辑，配合图解让你一次看懂。

  ### 1. 核心逻辑：队列 (Queue) + 锁住长度

  层序遍历的基本思想是“先进先出”，所以必须用 ​**队列 (Queue)** 。

  但普通的 BFS 是一股脑把所有节点都塞进队列里，分不清谁是第一层、谁是第二层。Krahets 大佬这段代码最巧妙的地方在于中间那个 `for` 循环：

  Java

  ```
  // 关键点：在开始遍历当前层之前，先记录当前队列的长度！
  for(int i = queue.size(); i > 0; i--) { 
      // ...
  }
  ```

  - **为什么要这样做？**

    - 当外层 `while`​ 刚开始新的一轮循环时，队列里**只**装着这一层的节点。
    - ​`int i = queue.size()`​ 相当于对当前队列拍了一张\*\*“快照”\*\*。
    - 如果不固定这个 `size`，你在循环里往队列加孩子节点（下一层），队列长度会变长，你就分不清哪些是这层的，哪些是下层的了。

  ---

  ### 2. 代码执行流程模拟 (Dry Run)

  假设二叉树是：

  Plaintext

  ```
      3
     / \
    9  20
      /  \
     15   7
  ```

  **初始状态：**

  - ​`queue`​: `[3]`
  - ​`res`​: `[]`

  **第一轮 while 循环：**

  1. **锁定长度：**  `queue.size()`​ 是 1。`for` 循环要执行 1 次。
  2. **处理节点 3：**

     - ​`poll()`​ 取出 3。放入临时列表 `tmp`​ -\> `[3]`。
     - 如果有左孩子，加 9 进队列。
     - 如果有右孩子，加 20 进队列。
     - **此时队列变了：**  `[9, 20]`。
  3. **循环结束：**  因为只执行 1 次。
  4. **保存结果：**  `res`​ -\> `[[3]]`。

  **第二轮 while 循环：**

  1. **锁定长度：**  此时 `queue`​ 是 `[9, 20]`​，`size`​ 是 2。`for` 循环要执行 2 次（只处理这 2 个老节点，新加的不算）。
  2. **处理节点 9 (第1次)：**

     - 取出 9。`tmp`​ -\> `[9]`。
     - 无孩子。队列 -\> `[20]`。
  3. **处理节点 20 (第2次)：**

     - 取出 20。`tmp`​ -\> `[9, 20]`。
     - 加左孩子 15，加右孩子 7。队列 -\> `[15, 7]`。
  4. **循环结束：**  `res`​ -\> `[[3], [9, 20]]`。

  **第三轮 while 循环：**

  1. **锁定长度：**  `size` 是 2（对应 15 和 7）。
  2. 处理完 15 和 7，它们没有孩子，队列变空。
  3. ​`res`​ -\> `[[3], [9, 20], [15, 7]]`。

  **第四轮：**  `queue` 为空，退出。

  ---

  ### 3. 语法细节解析

  #### A. `List<List<Integer>>` 是什么？

  这是一个​**二维列表**。

  - 你可以把它想象成一个书架（外层 List），每一层书架上放着一排书（内层 `List<Integer>`）。
  - ​`res.add(tmp)` 就是把整理好的一整层（一排书）放进书架。

  #### B. `Queue<TreeNode> queue = new LinkedList<>();`

  - 这里呼应了我们之前聊过的 `Deque`​。`LinkedList`​ 既实现了 `List`​ 接口，也实现了 `Queue`​ 和 `Deque` 接口。
  - 作为队列使用时：

    - ​`queue.add()`​ \= 进队（队尾）
    - ​`queue.poll()`​ \= 出队（队头）
    - ​`queue.isEmpty()`​ \= 判空

  #### C. `for` 循环的写法技巧

  Java

  ```
  for(int i = queue.size(); i > 0; i--)
  ```

  这种写法非常老练。很多新手会写成：

  Java

  ```
  // 错误示范！
  int size = queue.size();
  for(int i = 0; i < size; i++) { ... }
  ```

  或者更糟糕的（逻辑错误）：

  Java

  ```
  // 绝对错误！size() 会在循环中动态变化，导致死循环
  for(int i = 0; i < queue.size(); i++) { ... }
  ```

  Krahets 的写法 `i--`​ 直接在初始化时固定了次数，代码更简洁，且避免了每次循环都调用 `size()` 方法，也避免了逻辑错误。

  ### 总结

  这段代码是**广度优先搜索 (BFS)**  的​**分层模板**。

  以后只要题目要求：

  1. 按层打印
  2. 求二叉树最大深度
  3. 求二叉树每一层的最大值/平均值
  4. 找二叉树最左下角的值

  **不用想，直接背诵并默写这段代码的核心逻辑即可。**

‍
