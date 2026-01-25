---
title: day16
date: '2026-01-21 22:50:57'
updated: '2026-01-21 23:05:38'
permalink: /post/day16-1d3j2x.html
comments: true
toc: true
---



# day16

# **第六章 二叉树 part04**

### 找树左下角的值

本题递归偏难，反而迭代简单属于模板题， 两种方法掌握一下

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html](https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html)</u>

### *路径总和

本题 又一次涉及到回溯的过程，而且回溯的过程隐藏的还挺深，建议先看视频来理解

112. 路径总和，和 113. 路径总和ii 一起做了。 优先掌握递归法。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html](https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html)</u>

### 从中序与后序遍历序列构造二叉树 

本题算是比较难的二叉树题目了，大家先看视频来理解。

106.从中序与后序遍历序列构造二叉树，105.从前序与中序遍历序列构造二叉树 一起做，思路一样的

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html](https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html)</u>

# 找树左下角的值:

[513. 找树左下角的值 - 力扣（LeetCode）](https://leetcode.cn/problems/find-bottom-left-tree-value/)

# 迭代法：

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        TreeNode node = root;
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        while (!q.isEmpty()) {
            node = q.poll();
            if (node.right != null) q.add(node.right);
            if (node.left != null)  q.add(node.left);
        }
        return node.val;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/find-bottom-left-tree-value/solutions/2049776/bfs-wei-shi-yao-yao-yong-dui-lie-yi-ge-s-f34y/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 复杂度分析  
  时间复杂度：O(n)，其中 n 为二叉树的节点个数。  
  空间复杂度：O(n)。满二叉树（每一层都填满）最后一层有大约 n/2 个节点，因此队列中最多有 O(n) 个元素，所以空间复杂度是 O(n) 的。

- 这份代码非常经典，出自算法大神“灵茶山艾府”之手。它的核心在于\*\*“逆向思维”\*\*，将标准的 BFS（广度优先搜索）顺序稍微改动一下，就极大地简化了代码逻辑。

  以下是为您整理的详细学习笔记：

  ---

  # 学习笔记：找树左下角的值 (LC 513) - 极简 BFS 法

  ### 1. 核心思路：逆向 BFS

  通常的层序遍历（BFS）是 ​ **“从上到下，从左到右”** 。如果我们按照这个顺序遍历，最后一个访问的节点是“右下角”的节点。

  题目要求找到 “最后一行、最左边” 的值。

  如果我们把遍历顺序改为 “从上到下，从右到左”，那么：

  1. 每一层都是先访问右边的节点，再访问左边的节点。
  2. 整棵树遍历结束时，​**最后访问的一个节点，一定是最后一层的最左边那个节点**。

  ### 2. 代码解析

  Java

  ```
  class Solution {
      public int findBottomLeftValue(TreeNode root) {
          // node 用来记录当前遍历到的节点，循环结束后，它就是最后一个节点
          TreeNode node = root;
          
          // 使用 Queue 进行 BFS。ArrayDeque 是 Java 中比 LinkedList 更快的队列实现
          Queue<TreeNode> q = new ArrayDeque<>();
          q.add(root);

          while (!q.isEmpty()) {
              // 1. 取出队头元素
              node = q.poll();

              // 2. 关键点：先加入右子节点
              if (node.right != null) q.add(node.right);
              
              // 3. 再加入左子节点
              if (node.left != null)  q.add(node.left);
          }
          
          // 循环结束时，node 停留在最后一次 poll 出来的节点
          // 根据“右 -> 左”的入队顺序，这个就是全树最底层的最左节点
          return node.val;
      }
  }
  ```

  ### 3. 图解推演 (Dry Run)

  假设二叉树如下：

  Plaintext

  ```
        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7
  ```

  **目标：**  找到最底层最左边的值，即 ​**7**。

  **执行流程：**

  1. **初始：**  队列 `[1]`。
  2. **Poll 1：**  `node`​ \= 1。先加右(3)，再加左(2)。队列 `[3, 2]`。
  3. **Poll 3：**  `node`​ \= 3。先加右(6)，再加左(5)。队列 `[2, 6, 5]`​。 *(注意顺序)*
  4. **Poll 2：**  `node`​ \= 2。先加右(null)，再加左(4)。队列 `[6, 5, 4]`。
  5. **Poll 6：**  `node`​ \= 6。无子节点。队列 `[5, 4]`。
  6. **Poll 5：**  `node`​ \= 5。先加右(null)，再加左(7)。队列 `[4, 7]`。
  7. **Poll 4：**  `node`​ \= 4。无子节点。队列 `[7]`。
  8. **Poll 7：**  `node`​ \= 7。无子节点。队列 `[]`。
  9. **结束：**  队列为空，最后一次赋值给 `node`​ 的是 ​**7**。返回 7。

  ### 4. 为什么这个写法更优？

  **对比传统 BFS 写法：**

  - **传统写法：**  需要用 `for (int i = 0; i < size; i++)`​ 的循环来分层，并且每一层都要判断 `if (i == 0)` 来记录该层的第一个元素（最左元素），最后不断更新结果。
  - **灵神写法：**

    - 不需要记录层级 (`depth`)。
    - 不需要记录每层的节点数量 (`size`)。
    - 不需要判断是否是该层的第一个节点。
    - 利用**队列先进先出**的特性，配合**右进左进**的顺序，直接让答案“沉底”。

  ### 5. Java 细节补充

  - ArrayDeque vs LinkedList：  
    在作为队列（Queue）使用时，推荐使用 ArrayDeque。因为它基于数组实现，缓存局部性更好（Cache Locality），且没有链表节点的额外内存开销（不需要存 prev/next 指针），通常比 LinkedList 更快。
  - 变量作用域：  
    代码中 TreeNode node \= root; 定义在 while 循环外部，这是为了让 node 变量在循环结束后依然存活，从而能 return node.val。

  ---

  总结：

  这道题是 BFS 变种 的教科书级案例。它教会我们：不要死记硬背算法模板，根据目标（找最左），灵活调整遍历顺序（先右后左），往往能起到四两拨千斤的效果。

# 递归法：

```java
// 递归法
class Solution {
    private int Deep = -1;
    private int value = 0;
    public int findBottomLeftValue(TreeNode root) {
        value = root.val;
        findLeftValue(root,0);
        return value;
    }

    private void findLeftValue (TreeNode root,int deep) {
        if (root == null) return;
        if (root.left == null && root.right == null) {
            if (deep > Deep) {
                value = root.val;
                Deep = deep;
            }
        }
        if (root.left != null) findLeftValue(root.left,deep + 1);
        if (root.right != null) findLeftValue(root.right,deep + 1);
    }
}
```

- 这段代码使用的是 **DFS（深度优先搜索）**  的思路。

  如果不理解其中的奥妙，很容易觉得它只是在瞎跑。其实它的核心逻辑非常严谨，依靠的是  **“先左后右”**  的遍历顺序和  **“严格大于”**  的判断条件。

  我们通过三个关键点来深度拆解这份代码：

  ### 1. 核心战术：谁先到达最深处？

  代码中最关键的两行是：

  Java

  ```
  if (root.left != null) findLeftValue(root.left, deep + 1);  // 先去左边
  if (root.right != null) findLeftValue(root.right, deep + 1); // 后去右边
  ```

  这就是 前序遍历（中左右）或者是 回溯 的一种体现。

  因为代码总是优先向左钻。这意味着：

  - 如果第 4 层有 3 个节点，最左边那个节点一定会被**最先**访问到。
  - 右边的那些同层节点，会**后**访问到。

  ### 2. 核心判断：`deep > Deep` 的秘密

  Java

  ```
  if (deep > Deep) { // 注意这里是大于，不是大于等于
      value = root.val;
      Deep = deep;
  }
  ```

  这里的逻辑配合上面的“先左后右”顺序，简直是天作之合：

  1. **抢占先机：**  当我们第一次到达树的“新深度”（比如第 3 层）时，因为是先左后右，所以到达的​**一定是该层最左边的节点**。
  2. **只有更深才更新：**  此时 `deep (3) > Deep (2)` 成立，记录下这个值。
  3. **同层忽略：**  接着我们遍历到第 3 层右边的节点。此时 `deep (3) > Deep (3)`​ **不成立**。
  4. **结果：**  `value`​ 始终保留的是\*\*最深层、最先被访问到（即最左侧）\*\*的那个值。

  > 思考： 如果把条件改成 deep \>\= Deep 会发生什么？
  >
  > 答案：结果会变成“最后一行、最右边的值”。因为同层后面的节点会覆盖前面的节点。
  >

  ### 3. 隐形的回溯 (Backtracking)

  Java

  ```
  findLeftValue(root.left, deep + 1);
  ```

  注意这里传参是 deep + 1，而不是 deep++ 或者修改全局变量。

  这利用了函数调用栈的特性：

  - 当你进入下一层递归时，参数变成了 `deep + 1`。
  - 但是当你​**递归归来（回溯）** ​，回到当前函数层级时，本地变量 `deep` 依然是原来的值。

  这保证了我们在“探路”时，不需要手动去减深度，系统自动帮我们复原了现场。

  ---

  ### 图解模拟 (Dry Run)

  假设树结构如下：

  Plaintext

  ```
        1
       / \
      2   3
     /
    4
  ```

  **初始状态：**  `Deep = -1`​, `value = 1`

  1. **访问 1 (deep=0)：**  非叶子，向左递归。
  2. **访问 2 (deep=1)：**  非叶子，向左递归。
  3. **访问 4 (deep=2)：**

     - 是叶子节点。
     - 判断：`2 > -1` 成立！
     - **更新：**  `Deep = 2`​, `value = 4`。
     - 4 无子节点，函数返回。
  4. **回到 2：**  左边跑完了，跑右边（无），函数返回。
  5. **回到 1：**  左边跑完了，向右递归。
  6. **访问 3 (deep=1)：**

     - 是叶子节点。
     - 判断：`1 > 2`​ **不成立**！
     - ​**不更新**（这一步保住了 4 这个“最左”的地位）。
  7. **结束：**  返回 `value` (4)。

  ---

  ### 代码微调建议

  你的代码逻辑完全正确，符合开发规范。只有两个小建议让它更专业：

  1. 变量命名规范：  
     Java 中成员变量通常首字母小写。Deep 建议改为 maxDepth。大写开头的通常是类名。
  2. 初始值优化：  
     Deep 初始化为 -1 是没问题的。不过既然 findBottomLeftValue 第一行已经把 value \= root.val 了，其实相当于默认第 0 层已经处理过了（虽然逻辑上不完全等价，但如果不赋值也没事，递归里会覆盖）。

  **优化后的写法（仅供参考）：**

  Java

  ```
  class Solution {
      // 记录最大深度
      private int maxDepth = -1;
      // 记录结果值
      private int resultValue = 0;

      public int findBottomLeftValue(TreeNode root) {
          // 从第 0 层开始递归
          traversal(root, 0);
          return resultValue;
      }

      private void traversal(TreeNode root, int depth) {
          if (root == null) return;

          // 如果是叶子节点
          if (root.left == null && root.right == null) {
              // 只有当发现“更深”的层级时才更新
              // 因为先左后右，所以第一次达到新层级的节点必是该层最左
              if (depth > maxDepth) {
                  maxDepth = depth;
                  resultValue = root.val;
              }
              return;
          }

          // 依然保持先左后右
          if (root.left != null) traversal(root.left, depth + 1);
          if (root.right != null) traversal(root.right, depth + 1);
      }
  }
  ```

  ### 总结

  递归法的精髓在于：**利用“先左后右”的遍历顺序，配合“深度严格递增”的条件，让第一个到达最深层的节点（即左侧节点）成为最终赢家。**

‍

# 路径总和：

[112. 路径总和 - 力扣（LeetCode）](https://leetcode.cn/problems/path-sum/solutions/2731531/jian-ji-xie-fa-pythonjavacgojsrust-by-en-icwe/)

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        targetSum -= root.val;
        if (root.left == null && root.right == null) { // root 是叶子
            return targetSum == 0;
        }
        return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/path-sum/solutions/2731531/jian-ji-xie-fa-pythonjavacgojsrust-by-en-icwe/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码同样出自“灵茶山艾府”，依然保持了他一贯的**极简、精准**风格。

  这道题是 ​**LeetCode 112. 路径总和 (Path Sum)** 。

  与其说这是一段代码，不如说它展示了一种\*\*“减法思维”\*\*。相比于“带着累加和往下跑，看是否等于目标值”，这段代码选择“带着目标值往下跑，不断做减法，看最后是否归零”。

  下面我们来深度拆解这段代码的 3 个精妙之处：

  ### 1. 核心逻辑：倒计时（减法思维）

  Java

  ```
  targetSum -= root.val;
  ```

  - **常规思维：**  传入一个 `currentSum = 0`​，每到一个节点就 `currentSum + node.val`​，最后看 `currentSum == targetSum`。
  - **灵神思维：**  不需要额外的变量。每经过一个节点，就把这个节点的“债”还掉（从 `targetSum` 里减去）。

    - 到了叶子节点时，如果 `targetSum`​ 刚好变成了 `0`，说明债还清了（路径和符合要求）。

  ### 2. 终止条件的区分（易错点）

  代码中有两个 `if`，初学者很容易搞混它们的职责：

  **A. 空节点检查 (Base Case 1)**

  Java

  ```
  if (root == null) {
      return false;
  }
  ```

  - 这不仅是防止空指针异常，也是逻辑判断：​**空节点不等于路径结束**​。即便此时 `targetSum`​ 是 0，但如果 `root`​ 是 null，说明这条路走断了还没到叶子，或者树本身就是空的，所以返回 `false`。

  **B. 叶子节点检查 (Base Case 2)**

  Java

  ```
  if (root.left == null && root.right == null) { // 只有到了叶子节点
      return targetSum == 0; // 这里的 targetSum 已经被减去了当前节点的值
  }
  ```

  - **题目要求：**  路径必须是从根节点到​**叶子节点**。
  - 只有当且仅当：**我是叶子** 且 ​**减去我之后余额为 0**，才算找到了路径。

  ### 3. 隐形的回溯与短路

  Java

  ```
  return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
  ```

  这里有两个编程语言层面的细节：

  1. **利用局部变量做“隐式回溯”：**

     - ​`targetSum`​ 是 Java 的基本数据类型 (`int`​)，它是**值传递**的。
     - 当你调用 `root.left`​ 时，传入的是减去当前值后的 `targetSum`。
     - 当你递归返回，准备调用 `root.right`​ 时，当前函数栈帧里的 `targetSum`​ **并没有被左边的递归修改**。
     - 因此，你不需要手动写 `targetSum += root.val` 这种回溯代码。Java 的函数调用栈自动帮你完成了“现场恢复”。
  2. **短路运算 (Short-circuit Evaluation)：**

     - 操作符 `||` 是短路的。
     - 如果 `hasPathSum(root.left, ...)`​ 返回了 `true`​，Java **根本不会**去执行右边的递归。这在找到路径时能极大地节省时间。

  ---

  ### 图解推演 (Dry Run)

  假设目标 ​**Target**  **=**  **10**，树结构如下：

  Plaintext

  ```
        5
       / \
      4   8
     /
    1
  ```

  1. **节点 5：**

     - ​`targetSum`​ 变为 10 - 5 \= ​**5**。
     - 不是叶子，递归左子树 (4)。
  2. **节点 4：**

     - ​`targetSum`​ 变为 5 - 4 \= ​**1**。
     - 不是叶子，递归左子树 (1)。
  3. **节点 1：**

     - ​`targetSum`​ 变为 1 - 1 \= ​**0**。
     - **是叶子！**  检查 `targetSum == 0`​？ -\> ​**True**。
     - 返回 True。
  4. **回到节点 4：**  收到左儿子的 True，返回 True。
  5. **回到节点 5：**  收到左儿子的 True，`||` 短路，不再看右边的 8，直接返回 True。

  ### 总结

  这段代码是 DFS（递归） 的标准模板之一。

  它教会我们：在递归函数中，利用参数传递（减法）往往比维护全局变量或累加变量更干净、更不容易出错。

# 从中序与后序遍历序列构造二叉树 

[106. 从中序与后序遍历序列构造二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solutions/2647794/tu-jie-cong-on2-dao-onpythonjavacgojsrus-w8ny/)

# 写法一：

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int n = postorder.length;
        if (n == 0) { // 空节点
            return null;
        }
        int leftSize = indexOf(inorder, postorder[n - 1]); // 左子树的大小
        int[] in1 = Arrays.copyOfRange(inorder, 0, leftSize);
        int[] in2 = Arrays.copyOfRange(inorder, leftSize + 1, n);
        int[] post1 = Arrays.copyOfRange(postorder, 0, leftSize);
        int[] post2 = Arrays.copyOfRange(postorder, leftSize, n - 1);
        TreeNode left = buildTree(in1, post1);
        TreeNode right = buildTree(in2, post2);
        return new TreeNode(postorder[n - 1], left, right);
    }

    // 返回 x 在 a 中的下标，保证 x 一定在 a 中
    private int indexOf(int[] a, int x) {
        for (int i = 0; ; i++) {
            if (a[i] == x) {
                return i;
            }
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solutions/2647794/tu-jie-cong-on2-dao-onpythonjavacgojsrus-w8ny/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这道题是数据结构面试中的\*\*“超级高频题”\*\*：**从中序与后序遍历序列构造二叉树** (LeetCode 106)。

  灵神的这份代码采用了\*\*“最直观”**的写法。虽然它因为频繁拷贝数组导致性能不是最优（**​**$O(N^2)$**​ **），但它**最符合人类的思维逻辑\*\*，非常适合用来理解算法的核心原理。

  ---

  ### 1. 核心解题逻辑

  解这道题的钥匙在于理解两种遍历方式的特性：

  1. **后序遍历 (Postorder):**  `[ 左子树 | 右子树 | 根节点 ]`

     - **关键点：**  数组的**最后一位**一定是当前的​**根节点**。
  2. **中序遍历 (Inorder):**  `[ 左子树 | 根节点 | 右子树 ]`

     - **关键点：**  一旦我们在中序遍历中找到了“根节点”，由于中序是“左-根-右”，根节点左边的所有元素都属于​**左子树**​，右边的所有元素都属于​**右子树**。

  ---

  ### 2. 代码深度拆解

  这份代码其实就在做三件事：**找根** -\> **切分** -\> ​**递归**。

  #### 第一步：确定根节点

  Java

  ```
  int n = postorder.length;
  // 后序遍历的最后一个元素，必然是当前的根
  int rootVal = postorder[n - 1]; 
  ```

  #### 第二步：在中序中定位，计算左子树大小

  我们需要知道左子树有多少个节点，才能去切割数组。

  Java

  ```
  // 在中序数组中找到根节点的位置
  // 比如 inorder = [9, 3, 15, 20, 7], rootVal = 3
  // 找到 3 的下标是 1。这意味着左边有 1 个元素（9），也就是左子树大小为 1。
  int leftSize = indexOf(inorder, postorder[n - 1]); 
  ```

  #### 第三步：切割数组 (Arrays.copyOfRange)

  这是代码最核心、也最容易晕的地方。利用 `leftSize` 把两个大数组切成四份。

  **切割逻辑图示：**

  - **中序 (Inorder):**

    - ​`in1`​ (左子树): `0`​ 到 `leftSize`
    - ​`in2`​ (右子树): `leftSize + 1`​ 到 `最后` (跳过根节点)
  - **后序 (Postorder):**

    - ​`post1`​ (左子树): `0`​ 到 `leftSize`​ (​**长度必须和中序的左子树一样！** )
    - ​`post2`​ (右子树): `leftSize`​ 到 `n - 1` (排除掉最后的根节点)

  Java

  ```
  // 这里的 range 是 [from, to)，左闭右开
  int[] in1 = Arrays.copyOfRange(inorder, 0, leftSize);
  int[] in2 = Arrays.copyOfRange(inorder, leftSize + 1, n);

  int[] post1 = Arrays.copyOfRange(postorder, 0, leftSize);
  int[] post2 = Arrays.copyOfRange(postorder, leftSize, n - 1);
  ```

  #### 第四步：构建与递归

  Java

  ```
  TreeNode left = buildTree(in1, post1);   // 用切出来的左半部分去建左树
  TreeNode right = buildTree(in2, post2);  // 用切出来的右半部分去建右树
  return new TreeNode(postorder[n - 1], left, right); // 组装
  ```

  ---

  ### 3. 代码中的细节分析

  1. ​**​`indexOf`​**​ **中的死循环** **​`for (int i = 0; ; i++)`​** 

     - 这里为什么敢写死循环？因为题目保证了输入是有效的。也就是说，后序遍历里的那个根节点，**一定**能在中序遍历里找到。所以不需要判断边界，直接跑直到 `return`。
  2. ​**​`leftSize`​**​ **的双重含义**

     - 它既是“根节点在中序数组中的下标”（如果起始是0）。
     - 它也是“左子树的节点数量”。
     - 利用“数量相等”这个性质，我们才能准确地去切分 `postorder` 数组。

  ### 4. 优缺点评估

  - **优点（清晰）：**  代码逻辑完全对应这一题的数学定义，没有复杂的索引计算，非常容易看懂。
  - **缺点（性能）：**

    - **时间复杂度** **$O(N^2)$**​ **：**  每次递归都调用 `Arrays.copyOfRange`​ 复制数组，且 `indexOf` 也是线性查找。在树退化成链表时，性能较差。
    - **空间复杂度** **$O(N^2)$**​ **：**  产生了大量的临时数组对象。

  ### 5. 进阶优化思路 (面试加分项)

  在面试中，写出上面这个版本通常能及格，但面试官可能会问：“能优化吗？”

  **优化方向：**

  1. **去拷贝：**  不要在递归中复制数组，而是直接传递**下标范围** (例如 `build(int inStart, int inEnd, int postStart, int postEnd)`).
  2. **哈希表加速：**  `indexOf`​ 每次都要遍历寻找。可以在开始前把 `inorder`​ 的元素和下标存入 `HashMap`，这样查找根节点位置就变成了 $O(1)$。

  **优化后的复杂度：**

  - 时间复杂度：$O(N)$
  - 空间复杂度：$O(N)$

  灵神的这段代码是为了教学演示逻辑而简化的，如果要看 $O(N)$ 的版本，通常需要写一个带 4 个指针参数的辅助函数。

# 写法二：

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int n = inorder.length;
        Map<Integer, Integer> index = new HashMap<>(n, 1); // 预分配空间
        for (int i = 0; i < n; i++) {
            index.put(inorder[i], i);
        }
        return dfs(0, 0, n, postorder, index); // 左闭右开区间
    }

    private TreeNode dfs(int inL, int postL, int postR, int[] postorder, Map<Integer, Integer> index) {
        if (postL == postR) { // 空节点
            return null;
        }
        int leftSize = index.get(postorder[postR - 1]) - inL; // 左子树的大小
        TreeNode left = dfs(inL, postL, postL + leftSize, postorder, index);
        TreeNode right = dfs(inL + leftSize + 1, postL + leftSize, postR - 1, postorder, index);
        return new TreeNode(postorder[postR - 1], left, right);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solutions/2647794/tu-jie-cong-on2-dao-onpythonjavacgojsrus-w8ny/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 写法二  
  上面的写法有两个优化点：

  用一个哈希表（或者数组）预处理 inorder 每个元素的下标，这样就可以 O(1) 查到 postorder[n−1] 在 inorder 的位置，从而 O(1) 知道左子树的大小。  
  把递归参数改成子数组下标区间（左闭右开区间）的左右端点，从而避免复制数组。

  作者：灵茶山艾府  
  链接：https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solutions/2647794/tu-jie-cong-on2-dao-onpythonjavacgojsrus-w8ny/  
  来源：力扣（LeetCode）  
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

- 这正是我们刚才提到的​**进阶优化版本**！

  相比于上一版不停地拷贝数组（$O(N^2)$），这一版通过 **HashMap** 和 **下标指针** 将时间复杂度降到了完美的 ​**$O(N)$**。

  这也是面试中面试官最想看到的“满分回答”。让我们来拆解一下这份代码的高明之处。

  ---

  ### 1. 核心优化点：空间换时间

  #### A. HashMap 的妙用

  Java

  ```
  Map<Integer, Integer> index = new HashMap<>(n, 1);
  for (int i = 0; i < n; i++) {
      index.put(inorder[i], i);
  }
  ```

  - **痛点解决：**  上一版代码中，为了找到根节点在中序遍历中的位置，我们每次都要遍历数组 (`indexOf` 方法)，这很慢。
  - **优化方案：**  在递归开始前，先把中序遍历中 `数值 -> 下标` 的映射存起来。
  - **效果：**  以后查找“根节点在哪”只需要 $O(1)$ 的时间。

  #### B. 指针代替拷贝

  代码中不再使用 Arrays.copyOfRange，而是传递三个整数变量：inL、postL、postR。

  这就像是：与其把房子拆了建成两个小房子传给孩子，不如直接给孩子一张地图，告诉他“从这儿到那儿归你管”。

  ---

  ### 2. 难点解析：为什么参数变少了？

  通常这种解法需要 4 个指针（`inLeft`​, `inRight`​, `postLeft`​, `postRight`​）。但灵神的代码只用了 ​**3 个**：

  - ​`inL`: 中序遍历的左边界。
  - ​`postL`: 后序遍历的左边界。
  - ​`postR`​: 后序遍历的右边界（​**注意：左闭右开区间**）。

  为什么不需要 inR（中序右边界）？

  因为我们只需要算出 左子树的大小 (leftSize)，就可以推导出左右子树在 postorder 数组中的所有分界点。inR 在计算逻辑中是可以被省去的。

  ---

  ### 3. 核心逻辑图解 (关键数学推导)

  假设我们当前的子树范围如下：

  - **Postorder (后序):**  `[ 左子树 | 右子树 | 根 ]`

    - 范围：`[postL, postR)`​   *(注意：*​*​`postR`​*​ *是越界的，根在* *​`postR-1`​*​ *)*
  - **Inorder (中序):**  `[ 左子树 | 根 | 右子树 ]`

    - ​`inL` 指向左子树的开头。

  #### 第一步：找根节点

  Java

  ```
  // 后序遍历的最后一个元素是根
  int rootVal = postorder[postR - 1]; 
  ```

  #### 第二步：计算左子树大小 (核心)

  Java

  ```
  // 1. 从 Map 拿出根在中序里的位置
  int rootIndex = index.get(rootVal); 

  // 2. 左子树大小 = 根的位置 - 中序起始位置
  int leftSize = rootIndex - inL;
  ```

  #### 第三步：递归拆分 (最烧脑的部分)

  我们需要根据 `leftSize` 去切割后序数组。

  **A. 左子树 (**​**​`left`​**​ **)**

  - **Inorder:**  既然是左子树，起始点 `inL` 不变。
  - **Postorder:**

    - 起始点：`postL` (不变)
    - 结束点：`postL + leftSize`​ (向后数 `leftSize` 个)

  Java

  ```
  dfs(inL, postL, postL + leftSize, ...)
  ```

  **B. 右子树 (**​**​`right`​**​ **)**

  - **Inorder:**  跳过“左子树”和“根”。

    - 新起点 \= `inL + leftSize + 1`
  - **Postorder:**  紧接着左子树后面，直到原来的根之前。

    - 起始点：`postL + leftSize` (接上)
    - 结束点：`postR - 1` (原来的区间去掉最后一位的根)

  Java

  ```
  dfs(inL + leftSize + 1, postL + leftSize, postR - 1, ...)
  ```

  ---

  ### 4. 细节：左闭右开区间 `[L, R)`

  代码注释中提到了 `// 左闭右开区间`​。这是编程中处理数组范围的一种常见习惯（类似于 Java 的 `substring` 或 Python 的切片）。

  - ​**包含 L，不包含 R**。
  - 所以区间长度 \= `R - L`。
  - 所以最后一个元素 \= `R - 1`。

  这就是为什么代码里全是 `postR - 1`​ 以及判断空节点用 `postL == postR` 的原因。

  ### 总结

  这份代码是构建二叉树问题的​**标准工业级写法**：

  1. **预处理 Map** 消除查找开销。
  2. **指针传递** 消除空间开销。
  3. **精简参数** 逻辑极其紧凑。

  如果你在面试中能写出这一版，并能解释清楚 `leftSize` 是怎么算出来的，面试官基本上就没有理由扣分了。

‍
