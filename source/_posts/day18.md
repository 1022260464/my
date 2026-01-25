---
title: day18
date: '2026-01-25 21:48:02'
updated: '2026-01-25 22:09:29'
permalink: /post/day18-znpxwp.html
comments: true
toc: true
---



# day18

# **第六章 二叉树part06**

##  详细布置

### 530.二叉搜索树的最小绝对差 

需要领悟一下二叉树遍历上双指针操作，优先掌握递归

题目链接/文章讲解：<u>[https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html](https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1DD4y11779](https://www.bilibili.com/video/BV1DD4y11779)</u>

### 501.二叉搜索树中的众数

和 530差不多双指针思路，不过 这里涉及到一个很巧妙的代码技巧。

可以先自己做做看，然后看我的视频讲解。

<u>[https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html](https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1fD4y117gp](https://www.bilibili.com/video/BV1fD4y117gp)</u>

### 236. 二叉树的最近公共祖先

本题其实是比较难的，可以先看我的视频讲解

<u>[https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html](https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1jd4y1B7E2](https://www.bilibili.com/video/BV1jd4y1B7E2)</u>

# 530.二叉搜索树的最小绝对差 

[530. 二叉搜索树的最小绝对差 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/description/)

```java
class Solution {
    private int ans = Integer.MAX_VALUE;
    private int pre = Integer.MIN_VALUE / 2; // 防止减法溢出

    public int getMinimumDifference(TreeNode root) {
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        dfs(node.left);
        ans = Math.min(ans, node.val - pre);
        pre = node.val;
        dfs(node.right);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/minimum-absolute-difference-in-bst/solutions/3038197/zhong-xu-bian-li-jian-ji-xie-fa-pythonja-3pwf/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 LeetCode 上解决 **二叉搜索树（BST）最小绝对差**（如题目 530 或 783）的一个非常精简且高效的解法。

  作者是 **灵茶山艾府**（EndlessCheng），他是知名的算法大佬，代码风格通常非常精炼且注重细节优化。

  以下是这段代码的详细中文解析：

  ### 1. 核心思想：中序遍历 (In-Order Traversal)

  解决此题的关键在于二叉搜索树的一个重要性质：

  > **二叉搜索树的中序遍历（左 -> 根 -> 右）得到的节点值序列是单调递增的有序序列。**
  >

  在一个有序数组中，最小的差值一定出现在 **相邻** 的两个数之间。因此，我们不需要两两比较所有节点，只需要在遍历过程中，比较 **当前节点值** 和 **上一个节点值 (**​**​`pre`​**​ **)**  即可。

  ### 2. 代码细节解析

  #### **变量定义**

  - ​`ans`​: 记录当前的最小差值。初始化为 `Integer.MAX_VALUE`，确保第一次计算出的差值能覆盖它。
  - ​`pre`​: 记录中序遍历中 **前一个访问节点** 的值。

  #### **巧妙的初始化：防止溢出**

  ```java
  private int pre = Integer.MIN_VALUE / 2; // 防止减法溢出

  ```

  这是一个非常有经验的竞技编程技巧（Trick）：

  - **常规做法**：通常我们会用一个 `boolean`​ 标记是否是第一个节点，或者将 `pre`​ 设为 `-1`​，然后在计算前判断 `if (pre != -1)`。
  - **本题做法**：
  - 将 `pre`​ 初始化为一个很小的负数（`Integer.MIN_VALUE / 2`，大约是 -10 亿）。
  - **为什么除以 2？**  如果直接使用 `Integer.MIN_VALUE`​，当 `node.val`​ 是正数（比如 0 或 1）时，`node.val - Integer.MIN_VALUE`​ 会发生 **整数溢出**，变成一个负数，导致 `Math.min` 错误地更新。
  - **效果**：当访问树的第一个节点（假设值为 `x`​）时，计算 `x - pre`​ 会得到一个 **巨大的正数**。因为 `ans`​ 是取最小值，这个巨大的值会被忽略。这样就省去了 `if` 判断，让代码更简洁，且不影响结果。

  #### **DFS 递归函数**

  代码遵循标准的中序遍历流程：

  1. ​**​`dfs(node.left)`​** ：递归左子树（寻找更小的值）。
  2. **处理当前节点**：

  - ​`ans = Math.min(ans, node.val - pre);`：计算当前值与前一个值的差，尝试更新最小值。
  - ​`pre = node.val;`​：更新 `pre`，当前节点将成为下一个节点的“前驱”。

  3. ​**​`dfs(node.right)`​** ：递归右子树（寻找更大的值）。

  ### 3. 示例推演

  假设树的结构是 `root = [4, 2, 6]`：

  1. **进入 DFS(4) -> DFS(2)** 。
  2. **处理节点 2**：

  - 计算 `2 - (-10亿)`​  `10亿`。
  - ​`ans`​ 保持 `Integer.MAX_VALUE`（或更新为这个 10 亿，反正很大）。
  - 更新 `pre = 2`。

  3. **回到 DFS(4)** ：

  - 处理节点 4。
  - 计算 `4 - 2 = 2`。
  - ​`ans`​ 更新为 **2**。
  - 更新 `pre = 4`。

  4. **进入 DFS(6)** ：

  - 处理节点 6。
  - 计算 `6 - 4 = 2`。
  - ​`ans`​ 保持 **2**。
  - 更新 `pre = 6`。

  5. **结束**，返回 `ans` (2)。

  ### 4. 复杂度分析

  |指标|复杂度|说明|
  | ------| --------| -----------------------------------------------------------------|
  |**时间复杂度**||我们对树进行了完整的中序遍历，每个节点访问一次。|
  |**空间复杂度**||递归栈的深度取决于树的高度 。最坏情况（链状）为 ，平均情况为 。|

  ---

  **总结**：这是一个利用 BST 排序特性，结合整数运算技巧避免边界判断的优雅解法。

  **接下来这一步可以为您做得更好：**   
  如果您担心递归导致栈溢出，您想看看这个算法的 **非递归（迭代栈）版本** 怎么写吗？

# 501.二叉搜索树中的众数

[501. 二叉搜索树中的众数 - 力扣（LeetCode）](https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/)

```java
class Solution {
    TreeNode pre; // 记录上一个遍历的结点
    int result = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        if (root == null)
            return 0;
        traversal(root);
        return result;
    }

    public void traversal(TreeNode root) {
        if (root == null)
            return;
        // 左
        traversal(root.left);
        // 中
        if (pre != null) {
            result = Math.min(result, root.val - pre.val);
        }
        pre = root;
        // 右
        traversal(root.right);
    }
}
```

- 这段代码写得非常标准，是解决这类 BST 问题最**经典、通用**的写法。

  它和上一段代码的核心思路完全一样（都是利用 **中序遍历** 的有序性），但在实现细节上采用了更传统、更易读的方式。

  以下是代码分析和对比：

  ### 1. 代码核心逻辑分析

  这段代码用 `TreeNode pre`​（对象引用）代替了上一版中的 `int pre`（数值）。

  - ​**​`TreeNode pre`​**​ **的作用**​：  
    它是一个指针，始终指向 ​ **“中序遍历中，上一个访问过的节点”** 。
  - ​**​`if (pre != null)`​** ​ **的作用**​：  
    这是处理  **“第一个节点”**  的关键逻辑。

    1. 当递归访问到整棵树 **最左下角** 的那个节点（也就是最小值的节点）时，它是第一个被处理的。
    2. 此时 `pre`​ 还是初始值 `null`。
    3. ​`if (pre != null)` 条件不满足 $\rightarrow$ ​**跳过计算**（因为第一个节点没有“前驱”可以跟它做减法）。
    4. 执行 `pre = root` $\rightarrow$ `pre` 指向了这个最小节点。
    5. 当访问 **第二个节点** 时，`pre`​ 已经不为空了，`result = root.val - pre.val` 才会开始计算。

  ### 2. 与上一版（灵茶山艾府版）的对比

  |**特性**|**你的版本 (Standard)**|**上一版 (Magic Number)**||||
  | ------| ---------------------------------------------------------------| ----------------------------------------------------| --| --| --|
  |​**​`pre`​**​**类型**|​`TreeNode`(对象引用)|​`int`(整数数值)||||
  |**第一步处理**|用`if (pre != null)`显式判断是否是第一个节点|用`Integer.MIN_VALUE / 2`这种“魔术数”让第一次计算结果极大，从而被忽略||||
  |**可读性**|​**高**。逻辑非常直观，适合初学者理解|​**中**。需要理解那个魔术数的技巧||||
  |**通用性**|​**高**。适用于所有需要记录前驱节点的题目（如将 BST 转为链表等）|主要用于求差值等特定数值计算场景||||

  ### 3. 图解执行流程

  假设树还是：

  Plaintext

  ```
      4
     / \
    2   6
  ```

  1. ​**​`traversal(4)`​**  $\rightarrow$ **​`traversal(2)`​**  (去左边)。
  2. ​**​`traversal(2)`​** ：

     - 左孩子为空，返回。
     - **【中】：处理节点 2**

       - 此时 `pre`​ 是 `null`。
       - ​`if` 不成立，不计算差值。
       - ​**​`pre`​**​ **指向节点 2**。
     - 右孩子为空，返回。
  3. ​**回到** **​`traversal(4)`​** ：

     - **【中】：处理节点 4**

       - 此时 `pre` 指向节点 2（不为 null）。
       - 计算 `4 - 2 = 2`​，更新 `result`。
       - ​**​`pre`​**​ **更新指向节点 4**。
     - ​**​`traversal(6)`​**  (去右边)。
  4. ​**​`traversal(6)`​** ：

     - 左孩子为空。
     - **【中】：处理节点 6**

       - 此时 `pre` 指向节点 4。
       - 计算 `6 - 4 = 2`​，`result` 不变。
       - ​**​`pre`​**​ **更新指向节点 6**。

  ### 总结

  你的这段代码非常棒，属于\*\*“教科书级”\*\*的写法。

  - ​**优点**：逻辑清晰，没有使用奇技淫巧，面试时写这种代码最稳妥，不容易出错。
  - ​**适用场景**：以后遇到任何“在二叉树中需要比较前后节点”的题目（比如验证 BST、BST 转双向链表），都可以背这个模板：

  这一段代码完全没问题，您可以放心使用！

# 236. 二叉树的最近公共祖先

[236. 二叉树的最近公共祖先 - 力扣（LeetCode）](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root; // 找到 p 或 q 就不往下递归了，原因见上面答疑
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) { // 左右都找到
            return root; // 当前节点是最近公共祖先
        }
        // 如果只有左子树找到，就返回左子树的返回值
        // 如果只有右子树找到，就返回右子树的返回值
        // 如果左右子树都没有找到，就返回 null（注意此时 right = null）
        return left != null ? left : right;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solutions/2023872/fen-lei-tao-lun-luan-ru-ma-yi-ge-shi-pin-2r95/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是解决 **二叉树最近公共祖先 (LCA)**  问题最经典、最通用的递归解法（适用于所有二叉树，不局限于 BST）。

  灵神的这段代码非常凝练，采用了  **“后序遍历 (Post-order Traversal)”**  的思想（即：先搜左右子树，最后在根节点做决定）。

  我们可以把它想象成一个  **“自底向上汇报”**  的过程。

  ### 1. 核心逻辑大白话

  想象根节点（Root）是一个老板，他派了两个员工（Left 和 Right）分别去左子树和右子树寻找目标人物 $p$ 和 $q$。

  员工回来的汇报结果只有三种情况：

  1.  **“没找到”**  (返回 `null`)。
  2.  **“找到了其中一个”**  (返回那个节点，比如 $p$)。
  3.  **“两个都在我这边的下面”**  (返回 LCA 节点)。

  ### 2. 代码逐行拆解

  #### 第一步：终止条件（Base Case）

  Java

  ```
  if (root == null || root == p || root == q) {
      return root; 
  }
  ```
  这是递归的出口，有两层含义：

  1. ​**​`root == null`​**​：走到叶子节点的下一层了也没找到，返回 `null`。
  2. ​**​`root == p || root == q`​**​：**这是最关键的一步！**

     - 一旦遇到了 $p$ 或者 $q$，**立刻返回**当前节点，不再继续往下找了。
     - **为什么？**

       - 如果 $p$ 和 $q$ 分散在两边，这里返回 $p$，就是告诉上层“我找到 $p$ 了”。
       - ​**特例**：如果 $p$ 是 $q$ 的祖先（例如 $q$ 在 $p$ 的左子树里），当我们遇到 $p$ 时直接返回 $p$。虽然我们没有去遍历 $p$ 的子树找 $q$，但根据定义，$p$ 和 $q$ 的 LCA 就是 $p$ 自己。这个逻辑完美覆盖了包含关系。

  #### 第二步：递归搜索（递）

  Java

  ```
  TreeNode left = lowestCommonAncestor(root.left, p, q);
  TreeNode right = lowestCommonAncestor(root.right, p, q);
  ```
  这是标准的 ​**后序遍历**。先让左子树去搜，再让右子树去搜。

  - ​`left` 变量存的是：左边搜到的结果（可能是 $p$，$q$，`null`，或者已经找到的公共祖先）。
  - ​`right` 变量同理。

  #### 第三步：处理汇报结果（归）

  这里是逻辑最密集的地方，我们分情况讨论：

  **情况 A：左右都有结果**

  Java

  ```
  if (left != null && right != null) {
      return root; 
  }
  ```
  - ​**含义**：左边汇报“我找到 $p$ 了”，右边汇报“我找到 $q$ 了”（或者反过来）。
  - ​**结论**​：既然一左一右，那么 **当前节点** **​`root`​** 一定是他们的最近交汇点（LCA）。
  - ​**动作**​：向上返回 `root`。

  **情况 B：只有一边有结果**

  Java

  ```
  return left != null ? left : right;
  ```
  这句三元运算符包含了剩下的逻辑：

  1. ​**左边没找到 (**​**​`left == null`​**​ **)，右边找到了 (**​**​`right != null`​**​ **)** ：

     - 说明 $p$ 和 $q$ 都在右子树里（或者只找到了其中一个，另一个在更下面）。
     - 我们把右边的结果 `right` 继续向上抛。
  2. ​**左边找到了，右边没找到**：

     - 说明目标都在左边，把 `left` 向上抛。
  3. ​**两边都没找到 (**​**​`left == null && right == null`​**​ **)** ：

     - 返回 `null`。

  ---

  ### 3. 图解演示

  假设树如下，我们要找 **5** 和 **6** 的 LCA：

  Plaintext

  ```
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
  ```
  1. **根节点 3** 派人去左边找 `(5, 6)`​，派人去右边找 `(5, 6)`。
  2. ​**左边（节点 5）** ：

     - 一进门，发现 `root == 5`​（也就是 `root == p`）。
     - 触发 `if (root == p) return root;`
     - ​**直接返回 5**。（注意：它甚至没去遍历下面的 6 和 2，因为找到了 5 就足够说明这边的结果了）。
  3. ​**右边（节点 1）** ：

     - 左子树找 0 -\> 返回 null。
     - 右子树找 8 -\> 返回 null。
     - 左右都是 null -\> ​**返回 null**。
  4. ​**回到根节点 3**：

     - ​`left`​ 拿到了 ​**5**。
     - ​`right`​ 拿到了 ​**null**。
     - 执行 `return left != null ? left : right;`​ -\> ​**返回 5**。

  ​**最终结果是 5**。（正确，因为 5 是 6 的祖先，5 和 6 的 LCA 是 5）。

  ---

  ### 4. 总结

  这段代码之所以经典，是因为它利用 **后序遍历** 的特性，巧妙地处理了两种情况：

  1. ​**分散在两侧**：当前节点是 LCA。
  2. ​**包含关系（一个是另一个的祖先）** ：遇到的第一个节点就是 LCA。

  **对比之前的 BST 题目：**

  - ​**BST**：因为有序，我们可以通过值的大小判断该往左走还是往右走（剪枝）。
  - ​**普通二叉树（本题）** ：无序，必须全树搜索（最坏 $O(N)$），直到找到为止。
