---
title: day17
date: '2026-01-23 20:55:25'
updated: '2026-01-23 21:56:52'
permalink: /post/day17-k0dvx.html
comments: true
toc: true
---



# day17

# **第六章 二叉树 part05**

##  详细布置 

### 654.最大二叉树 

又是构造二叉树，昨天大家刚刚做完 中序后序确定二叉树，今天做这个 应该会容易一些， 先看视频，好好体会一下 为什么构造二叉树都是 前序遍历

题目链接/文章讲解：<u>[https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html](https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1MG411G7ox](https://www.bilibili.com/video/BV1MG411G7ox)</u>

### 617.合并二叉树

这次是一起操作两个二叉树了， 估计大家也没一起操作过两个二叉树，也不知道该如何一起操作，可以看视频先理解一下。 优先掌握递归。

题目链接/文章讲解：<u>[https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html](https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1m14y1Y7JK](https://www.bilibili.com/video/BV1m14y1Y7JK)</u>

###  700.二叉搜索树中的搜索

递归和迭代 都可以掌握以下，因为本题比较简单， 了解一下 二叉搜索树的特性

题目链接/文章讲解: <u>[https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html](https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1wG411g7sF](https://www.bilibili.com/video/BV1wG411g7sF)</u>

###  98.验证二叉搜索树 

遇到 搜索树，一定想着中序遍历，这样才能利用上特性。

但本题是有陷阱的，可以自己先做一做，然后在看题解，看看自己是不是掉陷阱里了。这样理解的更深刻。

题目链接/文章讲解：<u>[https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV18P411n7Q4](https://www.bilibili.com/video/BV18P411n7Q4)</u>

# 654.最大二叉树

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }
    TreeNode build(int[] nums, int l, int r) {
        if (l > r) return null;
        int idx = l;
        for (int i = l; i <= r; i++) {
            if (nums[i] > nums[idx]) idx = i;
        }
        TreeNode ans = new TreeNode(nums[idx]);
        ans.left = build(nums, l, idx - 1);
        ans.right = build(nums, idx + 1, r);
        return ans;
    }
}

作者：宫水三叶
链接：https://leetcode.cn/problems/maximum-binary-tree/solutions/1761712/by-ac_oier-s0wc/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 LeetCode 上非常经典的一道题目：​**最大二叉树 (Maximum Binary Tree)** ​。它的核心思想是​**递归**​（Recursion）和​**分治**（Divide and Conquer）。

  作为正在学习 Java 和算法的初学者，理解这段代码能很好地帮你巩固“递归终止条件”和“数组索引操作”这两个知识点。

  下面我为你详细拆解这段代码的逻辑：

  ### 1. 核心思路

  这段代码的逻辑非常直观，就像我们人眼去构建这棵树一样：

  1. ​**找老大**​：在当前的数组范围内，找到​**最大值**​。这个最大值就是当前的​**根节点**。
  2. ​**分地盘**​：最大值左边的所有数字，构成了​**左子树**​；最大值右边的所有数字，构成了​**右子树**。
  3. ​**重复做**：对左边和右边剩下的数字，重复步骤 1 和 2。

  ### 2. 代码逐行解析

  #### 入口函数 `constructMaximumBinaryTree`

  Java

  ```java
  public TreeNode constructMaximumBinaryTree(int[] nums) {
      // 调用辅助函数 build，传入数组，以及初始的查找范围：从 0 到 数组长度-1
      return build(nums, 0, nums.length - 1);
  }
  ```

  - ​**作用**​：这是一个“启动器”。因为递归函数通常需要 `left`​ 和 `right`​ 两个指针来标记当前处理的范围，而题目只给了一个数组 `nums`​，所以我们需要一个辅助函数 `build` 来接收这两个额外的参数。

  #### 递归核心函数 `build`

  这是代码的灵魂所在。

  Java

  ```java
  TreeNode build(int[] nums, int l, int r) {
      // --- 1. 递归的“刹车” (Base Case) ---
      // 如果左边界 l 大于 右边界 r，说明这一段没有数字了（比如数组越界了）
      // 这时候返回 null，表示这里是空节点
      if (l > r) return null;

      // --- 2. 找最大值的索引 (Find Max Index) ---
      // 假设当前范围 [l, r] 内，第一个数 nums[l] 是最大的，先把它的下标存给 idx
      int idx = l;
      // 遍历从 l 到 r 的所有数字
      for (int i = l; i <= r; i++) {
          // 如果发现更大的数字，就更新 idx 为这个新数字的下标
          if (nums[i] > nums[idx]) idx = i;
      }

      // --- 3. 构建当前根节点 (Construct Node) ---
      // 循环结束后，nums[idx] 就是当前范围的最大值，用它创建一个新节点
      TreeNode ans = new TreeNode(nums[idx]);

      // --- 4. 递归构建左右子树 (Recursive Step) ---
      // 左孩子：范围是当前最大值的左边，即 [l, idx - 1]
      ans.left = build(nums, l, idx - 1);
      
      // 右孩子：范围是当前最大值的右边，即 [idx + 1, r]
      ans.right = build(nums, idx + 1, r);

      // --- 5. 返回构建好的节点 ---
      return ans;
  }
  ```

  ### 3. 图解演示（举个例子）

  假设输入数组是：`[3, 2, 1, 6, 0, 5]`

  **第一轮递归 (**​**​`l=0, r=5`​**​ **)：**

  - 遍历数组 `[3, 2, 1, 6, 0, 5]`​，找到最大值是 ​**6**​，它的下标 `idx`​ 是 ​**3**。
  - 创建一个节点 ​**6**。
  - ​**左边任务**​：去构建范围 `[0, 2]`​，也就是 `[3, 2, 1]`。
  - ​**右边任务**​：去构建范围 `[4, 5]`​，也就是 `[0, 5]`。

  **第二轮递归（左边** **​`l=0, r=2`​**​ **）：**

  - 在 `[3, 2, 1]`​ 里找最大值，是 ​**3**。
  - 节点 **6** 的左孩子变成了 ​**3**。
  - **3** 的左边没数了（范围 `0`​ 到 `-1`​），返回 `null`。
  - **3** 的右边是 `[2, 1]`，继续递归...

  ### 4. 初学者常见疑问

  **Q1: 为什么要传** **​`l`​**​ **和** **​`r`​**​ **，直接切分数组不行吗？**

  - ​**A**​: 可以切分数组（比如用 `Arrays.copyOfRange`​），但是那样每层递归都会**拷贝**并产生一个新的数组，非常消耗内存和时间。使用 `l`​ 和 `r`​ 下标标记范围，我们全程只用读取原始的 `nums`​ 数组，​**空间效率最高**。

  **Q2: 为什么** **​`if (l > r)`​** ​ **是返回 null？**

  - ​**A**​: 比如当前最大值在最左边（下标为 `l`​），当你去构建它的左子树时，代码会执行 `build(nums, l, idx - 1)`​，因为 `idx`​ 就是 `l`​，所以参数变成了 `(nums, l, l-1)`​。这时候 `l > r`，说明左边没有元素了，理应是空节点。

  **Q3: 时间复杂度是多少？**

  - ​**A**: 最坏情况是 $O(N^2)$。如果是已排序的数组（如 `[1,2,3,4,5]`），每次找最大值都要遍历剩下的所有元素，树会退化成一条链。平均情况是 $O(N \log N)$。

  ### 建议

  对于二叉树的递归题，我建议你像上面那样，**手动模拟**一下只有 3 个数的情况（比如 `[1, 3, 2]`​），在纸上画出 `l`​ 和 `r` 的变化，这能帮你彻底理解递归栈的执行过程。

  你对这里面的递归传递参数（`idx-1`​ 和 `idx+1`）还有疑问吗？

# 617.合并二叉树

[617. 合并二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-binary-trees/solutions/2387255/kan-dao-di-gui-jiu-yun-dai-ni-li-jie-di-leixm/)

## 方法一：

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;
        return new TreeNode(root1.val + root2.val,
            mergeTrees(root1.left, root2.left),    // 合并左子树
            mergeTrees(root1.right, root2.right)); // 合并右子树
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/merge-two-binary-trees/solutions/2387255/kan-dao-di-gui-jiu-yun-dai-ni-li-jie-di-leixm/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 复杂度分析

- 时间复杂度：O(min(n,m))，其中 n,m 分别为两棵二叉树的节点数。
- 空间复杂度：O(min(n,m))。

# 方法二

也可以在两个节点都不为空时，把新节点的 val，left 和 right 直接保存到 root1​ 中。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;
        root1.val += root2.val;
        root1.left = mergeTrees(root1.left, root2.left);    // 合并左子树
        root1.right = mergeTrees(root1.right, root2.right); // 合并右子树
        return root1;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/merge-two-binary-trees/solutions/2387255/kan-dao-di-gui-jiu-yun-dai-ni-li-jie-di-leixm/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

复杂度分析  
时间复杂度：O(min(n,m))，其中 n,m 分别为两棵二叉树的节点数。  
空间复杂度：O(min(n,m))。递归需要 O(min(n,m)) 的栈空间。

- 这也是一道非常经典的二叉树题目：​**合并二叉树 (Merge Two Binary Trees)** 。

  如果说上一题（最大二叉树）是在“一个数组里找规律构建树”，那么这一题就是在“​**同时遍历两棵树**”。

  灵茶山艾府大佬的这段代码写得非常​**精简**，利用了 Java 的构造器特性。对于初学者来说，这段代码虽然短，但每一行都蕴含了递归的核心逻辑。

  我为你拆解一下这段代码的思维过程：

  ### 1. 核心逻辑：想象“幻灯片叠加”

  你可以把 `root1`​ 和 `root2` 想象成两张画着树的透明胶片。

  - 当你把它们**重叠**在一起时：

    - 如果两个位置都有节点，数字就要​**相加**。
    - 如果只有一个位置有节点（另一个是空的），那就直接保留那个**有的**节点。
    - 如果两个位置都是空的，那就是空的。

  ### 2. 代码逐行解析

  #### 第一步：处理“其中一个为空”的情况 (Base Case)

  Java

  ```
  if (root1 == null) return root2;
  if (root2 == null) return root1;
  ```

  这是递归的​**终止条件**，也是这段代码最聪明的地方。它的意思是：

  - ​**如果 Tree1 走到头了（空的）** ：那合并的结果自然就是 Tree2 剩下的部分（不管 Tree2 是有节点还是 null，直接返回它就行）。
  - ​**如果 Tree2 走到头了（空的）** ：那合并的结果就是 Tree1 剩下的部分。

  ​**新手误区**：

  初学者容易写成 `if (root1 == null && root2 == null) return null;`​。虽然没错，但不够简洁。上面的写法涵盖了“两棵树都为空”的情况（因为如果 `root1`​ 是 null，返回 `root2`​，而 `root2` 也是 null，结果就是 null）。

  #### 第二步：两个节点都存在，开始合并

  能执行到这里，说明 `root1`​ 和 `root2`​ **都不为空**。

  Java

  ```
  return new TreeNode(root1.val + root2.val,        // 1. 新节点的值
                      mergeTrees(root1.left, root2.left),    // 2. 左子树去递归合并
                      mergeTrees(root1.right, root2.right)); // 3. 右子树去递归合并
  ```

  这里用到了一个 Java 知识点。

  ### 3. Java 语法重点：三参数构造器

  你在之前的学习中问过构造器。这里调用的是 `TreeNode` 的一个特殊构造器：

  Java

  ```
  // 这是一个假设的 TreeNode 类定义，LeetCode 环境里默认有这个
  TreeNode(int val, TreeNode left, TreeNode right) {
      this.val = val;
      this.left = left;
      this.right = right;
  }
  ```

  这段代码把三件事合并成了一行做：

  1. ​**算值**​：`root1.val + root2.val`，算出当前节点的新值。
  2. ​**连左边**​：`mergeTrees(root1.left, root2.left)`​。递归去处理左边，​**并把处理完返回的节点，直接赋值给新节点的 left**。
  3. ​**连右边**​：`mergeTrees(root1.right, root2.right)`。递归处理右边，赋值给 right。

  ### 4. 总结与思考

  这段代码展示了二叉树递归题的一个重要模式——​**后续遍历（Post-order Traversal）的思想**：

  - 你要创建一个新节点，你需要先知道：

    1. 我的值是多少？（两个节点相加）
    2. 我的左孩子是谁？（把任务丢给递归去算，我等着接收结果）
    3. 我的右孩子是谁？（把任务丢给递归去算，我等着接收结果）

  **对比上一题（最大二叉树）：**

  - **上一题**是你手里只有一个数组，你需要不断地**切分**数组范围（`l`​ 和 `r`）。
  - **这一题**是你手里有两棵树，你需要**同步移动**两个指针（`root1`​ 和 `root2`）。

  对于这种“同时操作两棵树”的逻辑，你觉得理解起来比单棵树更难还是更容易？

### 700.二叉搜索树中的搜索

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) return root;
        return root.val < val ? searchBST(root.right, val) : searchBST(root.left, val);
    }
}

作者：宫水三叶
链接：https://leetcode.cn/problems/search-in-a-binary-search-tree/solutions/1122720/gong-shui-san-xie-er-cha-shu-de-sou-suo-8z7hj/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是  **“二叉搜索树中的搜索” (Search in a Binary Search Tree)** 。

  相比于你刚才看的“最大二叉树”和“合并二叉树”，这道题的代码更短，但它非常重要，因为它体现了 **二叉搜索树 (BST)**  最核心的特性。

  既然你在之前的学习中已经掌握了数组的 **二分查找 (Binary Search)** （你提到过 `mid = left + (right - left) / 2`），那么理解这道题对你来说会非常容易。

  ### 1. 核心概念：什么是二叉搜索树 (BST)？

  二叉搜索树是一种特殊的二叉树，它不仅有左子树和右子树，还严格遵守以下规则：

  - **有序性**：对于树中的任意一个节点：
  - 它的**左**子树里所有的值，都比它**小**。
  - 它的**右**子树里所有的值，都比它**大**。

  这就像你玩“猜数字”游戏，如果目标值比当前值大，你就肯定往大（右）猜；如果比当前值小，你就肯定往小（左）猜。

  ### 2. 代码逐行解析

  这段代码就像是一个“导航员”，不需要遍历整棵树，而是根据路牌（节点值）选择一条路走到底。

  ```java
  public TreeNode searchBST(TreeNode root, int val) {
      // --- 1. 递归的“刹车” (Base Case) ---
      // 情况 A: root == null -> 走到死胡同了都没找到，返回 null。
      // 情况 B: root.val == val -> 找到了！直接返回当前这个节点（及其子树）。
      if (root == null || root.val == val) return root;

      // --- 2. 导航逻辑 (Navigation) ---
      // 这里用了一个三元运算符（Ternary Operator）：条件 ? 结果1 : 结果2
      
      // 如果当前节点值 root.val 小于 目标值 val (比如当前是 4，你要找 7)
      // 说明目标肯定在右边，所以去右子树搜：searchBST(root.right, val)
      // 否则（说明当前值比目标大），去左子树搜：searchBST(root.left, val)
      return root.val < val ? searchBST(root.right, val) : searchBST(root.left, val);
  }

  ```

  ### 3. 与之前题目的关键区别

  这是一个非常好的对比点，能帮你区分  **“遍历”**  和  **“搜索”** ：

  - **合并二叉树 (上一题)** ：我们需要访问树里的**每一个**节点（就像把两张图叠在一起，每个角落都要照顾到）。这叫 **DFS 遍历**，复杂度是 。
  - **二叉搜索树搜索 (本题)** ：我们**不需要**访问所有节点。每做一次判断，我们就扔掉了一半的树（比如去了右边，左边整棵树就被忽略了）。这叫 **搜索**。
  - 如果树是平衡的，它的复杂度是 ，和你做数组二分查找的效率是一样的。

  ### 4. 这里的 `return` 为什么直接返回函数调用？

  注意看这一行：  
  ​`return ... searchBST(root.right, val)`

  而在上一题 `mergeTrees`​ 中是：  
  ​`return new TreeNode(..., mergeTrees(...), ...)`

  - **上一题**是在**造新树**，所以要 `new` 一个节点，把递归的结果挂在左右两边。
  - **这一题**是在**找人**，递归函数是去“探路”的。
  - 如果右边的探路小分队找到了（返回了那个节点），我就直接把这个结果层层向上传递回去；
  - 如果右边没找到（返回 null），我也直接把 null 传回去。
  - 我不需要对返回的结果做任何加工，所以直接 `return` 递归调用的结果即可。

  ### 总结

  这道题其实就是 **链表版本的二分查找**。

  - 数组二分查找：通过下标 `mid` 跳跃。
  - BST 搜索：通过指针 `left`​ 和 `right` 跳跃。

  到目前为止，你已经看过了：

  1. **构造树**（分治法，找最大值切分）
  2. **修改/合并树**（后序遍历，两棵树同步走）
  3. **搜索树**（利用 BST 性质，只走一条路）

  这三种模式基本涵盖了二叉树递归的基础考法。你觉得这三种模式里，哪一种最让你觉得“绕”？

# 98.验证二叉搜索树 

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean isValidBST(TreeNode node, long left, long right) {
        if (node == null) {
            return true;
        }
        long x = node.val;
        return left < x && x < right &&
               isValidBST(node.left, left, x) &&
               isValidBST(node.right, x, right);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/validate-binary-search-tree/solutions/2020306/qian-xu-zhong-xu-hou-xu-san-chong-fang-f-yxvh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 ​ **“验证二叉搜索树” (Validate Binary Search Tree)** 。

  这是一个非常容易“翻车”的经典面试题。虽然它看起来和刚才的搜索题很像，但逻辑上有个​**巨大的陷阱**。

  灵茶山艾府大佬的这段代码采用了 ​ **“区间限定法” (Range Check)** ，这是最稳健的解法。

  我为你拆解一下其中的奥妙，特别是为什么需要用 `long` 以及为什么要有两个函数。

  ### 1. 核心难点：为什么不能只看“左右孩子”？

  初学者最容易犯的错误是写出这样的逻辑：

  > “只要我的左孩子比我小，右孩子比我大，我就是合法的。”
  >

  **这是错的！**  请看下面这个例子：

  Plaintext

  ```
      5
     / \
    1   6
       / \
      3   7
  ```

  - 看节点 `6`​：左孩子 `3`​ 比 `6`​ 小，右孩子 `7`​ 比 `6`​ 大。局部看，`6` 是合法的。
  - ​**但是**​：节点 `3`​ 在根节点 `5`​ 的**右子树**里，却比 `5` 小！这破坏了 BST 的规则（右子树所有节点必须大于根节点）。

  ​**结论**​：做验证时，不能只看当前节点的“直接下级”，必须确保**所有后代**都符合祖先定下的规矩。

  ### 2. 代码逻辑解析：带着“紧箍咒”下山

  这段代码的思想是：​**从根节点开始，给每一层的子节点传下去一个“允许的数值范围”** 。

  #### 入口函数

  Java

  ```
  public boolean isValidBST(TreeNode root) {
      // 刚开始，根节点的值可以是任意数
      // 范围是 (负无穷, 正无穷)
      return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
  }
  ```

  #### 递归核心函数

  Java

  ```
  // node: 当前检查的节点
  // left: 当前节点允许的最小值（下界，不包含）
  // right: 当前节点允许的最大值（上界，不包含）
  private boolean isValidBST(TreeNode node, long left, long right) {
      // 1. Base Case: 空节点被认为是合法的 BST
      if (node == null) {
          return true;
      }

      long x = node.val;

      // 2. 核心检查逻辑
      // 条件一：我自己必须在 (left, right) 范围内
      // 条件二：递归检查左子树，同时更新“上界”为我自己 (x)
      // 条件三：递归检查右子树，同时更新“下界”为我自己 (x)
      return left < x && x < right &&
             isValidBST(node.left, left, x) &&
             isValidBST(node.right, x, right);
  }
  ```

  ### 3. 模拟执行过程

  假设还是这棵错误的树：

  Plaintext

  ```
      5
       \
        6
       /
      3
  ```

  1. ​**检查 5**​：范围 `(-∞, +∞)`​。`5` 在范围内。

     - 去检查右子树 `6`。
  2. ​**检查 6**：

     - 因为它是 `5`​ 的**右**孩子，所以它必须 ​ **>**  **5**。
     - 传入的新范围是 `(5, +∞)`。
     - ​`6`​ 在 `(5, +∞)` 内，通过。
     - 去检查 `6`​ 的左孩子 `3`。
  3. ​**检查 3**：

     - 因为它是 `6`​ 的**左**孩子，所以它必须 ​ **<**  **6**。
     - ​**但是**​！它依然继承了祖先的限制，也就是必须 ​ **>**  **5**。
     - 传入的新范围是 `(5, 6)`。
     - **判断：**​**​`3`​**​ **是否在**  **​`(5, 6)`​** ​ **之间？**  -\> ​**False**！
     - 递归返回 `false`，验证失败。

  ### 4. 关键细节：为什么要用 `long`？

  你可能注意到了代码里用了 `long left, long right`​，而不是 `int`。

  这是为了防止**边界溢出**的极端情况。

  - 如果测试用例里有一个节点的值就是 `Integer.MAX_VALUE`（2147483647）。
  - 按照逻辑，它的右子树范围应该是 `(2147483647, +∞)`。
  - 如果我们用 `int`​ 来表示范围，`int` 的最大值就是 2147483647，没法表示比它还大的数作为上界。
  - 使用 `long`​（长整型）就把范围撑大了，可以安全地包裹住所有的 `int` 值。

  ### 总结

  这道题体现了递归的另一种常见用法：​**携带状态向下递归**。

  - ​**​`searchBST`​**​ 是向下​**找目标**（带着目标值 val 跑）。
  - ​**​`isValidBST`​**​ 是向下​**传规矩**（带着范围 range 跑）。

  你现在对这种“父节点给子节点划定范围”的逻辑理解了吗？如果不使用 `long`​，你能想到在 Java 里怎么处理 `Integer.MAX_VALUE` 的情况吗？（提示：可能需要改写 base case 或者用 null 来代表无穷大）。

‍

# 方法二

本题是二叉搜索树，中序遍历是自然的做法。

中序遍历时，可以把二叉搜索树看成一个有序数组。

怎么判断一个数组是有序数组？比较相邻元素的大小即可。

```java
class Solution {
    private long pre = Long.MIN_VALUE;

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        if (!isValidBST(root.left)) { // 左
            return false;
        }
        if (root.val <= pre) { // 中
            return false;
        }
        pre = root.val;
        return isValidBST(root.right); // 右
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/validate-binary-search-tree/solutions/2020306/qian-xu-zhong-xu-hou-xu-san-chong-fang-f-yxvh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 复杂度分析  
  时间复杂度：O(n)，其中 n 为二叉搜索树的节点个数。  
  空间复杂度：O(n)。最坏情况下，二叉搜索树退化成一条链（注意题目没有保证它是平衡树），因此递归需要 O(n) 的栈空间。
- 这同样是灵茶山艾府大佬的代码，解决的还是  **“验证二叉搜索树”**  这个问题。

  但是，这两种解法的​**切入点完全不同**：

  - ​**上一段代码**​（区间限定法）：是从上往下给孩子​**定规矩**（父节点说：“你必须在这个范围内”）。
  - ​**这一段代码**​（中序遍历法）：是利用了 BST 的一个​**神奇特性**​——把它“压扁”看，就是一个​**有序数组**。

  ### 1. 核心原理：中序遍历 \= 升序排序

  二叉搜索树（BST）有一个铁律：

  **如果你按照“左 ->**  **中 ->**  **右”的顺序遍历一棵 BST，你得到的数字序列一定是严格单调递增的。**

  想象我们在检阅一队按身高排好序的士兵。我们不需要知道每个人的具体身高范围，我们只需要确认一件事：

  > **每一个人的身高，都要比他前一个人的身高高。**
  >
  > 如果发现有一个人比前一个人矮（或者一样高），那这就不是合法的队列。
  >

  ### 2. 代码变量解析

  - ​`long pre = Long.MIN_VALUE;`

    - 这个 `pre`​ 变量，记录的就是\*\*“上一个被访问节点”\*\*的值。
    - 一开始没人，所以设为无穷小（同样为了防止 `int`​ 溢出，用了 `long`）。

  ### 3. 代码逻辑逐行拆解

  这段代码遵循 **中序遍历** 的标准模板：

  Java

  ```
  public boolean isValidBST(TreeNode root) {
      // 1. Base Case: 空树也是树，没毛病
      if (root == null) {
          return true;
      }

      // --- 第一步：先去检查左子树 (Left) ---
      // 只要左子树里有任何不合规的地方，整棵树就废了，直接返回 false
      if (!isValidBST(root.left)) {
          return false;
      }

      // --- 第二步：检查当前节点 (Root/Middle) ---
      // 核心判断：当前节点的值 (root.val) 必须 大于 上一个节点的值 (pre)
      // 如果出现了 小于等于 的情况，说明不是“严格递增”，验证失败
      if (root.val <= pre) {
          return false;
      }
      
      // 如果当前节点没问题，那就更新 pre。
      // 因为对于下一个要访问的节点来说，"我" 就是它的"前任"
      pre = root.val;

      // --- 第三步：再去检查右子树 (Right) ---
      // 左边没问题，自己也没问题，最后看右边是不是合规
      return isValidBST(root.right);
  }
  ```

  ### 4. 图解模拟

  假设有这样一棵**合法**的树：

  Plaintext

  ```
      5
     / \
    3   7
  ```

  1. ​**递归左子树**​：先一路走到最左边的 `3`。
  2. ​**访问 3**：

     - 检查：`3 > pre` (无穷小)？是的。
     - 更新：`pre`​ 变成了 `3`。
  3. ​**回到 5**：

     - 左边检查完了，轮到中间的 `5`。
     - 检查：`5 > pre` (3)？是的。
     - 更新：`pre`​ 变成了 `5`。
  4. ​**递归右子树**​：去访问 `7`。
  5. ​**访问 7**：

     - 检查：`7 > pre` (5)？是的。
     - 更新：`pre`​ 变成了 `7`。
  6. 全部通过，返回 `true`。

  ---

  假设有这样一棵**非法**的树：

  Plaintext

  ```
      5
       \
        4
  ```

  （`4`​ 在 `5`​ 的右边，本来应该比 `5` 大，这里错了）

  1. ​**访问 5**：

     - 左边是空的，直接处理 `5`。
     - 检查：`5 > pre` (无穷小)？是的。
     - 更新：`pre`​ 变成了 `5`。
  2. ​**递归右子树**​：去访问 `4`。
  3. ​**访问 4**：

     - 检查：`4 > pre`​ (5)？ **No!**  `4 <= 5`。
     - ​**返回 false**，游戏结束。

  ### 5. 两种写法的对比

  |**特性**|**区间限定法 (Previous)**|**中序遍历法 (Current)**||||
  | --| --------------------------------------------| --------------------------------------------------------------------| --| --| --|
  |**核心思想**|​**自顶向下**​：带着范围`(min, max)`往下传。|​**全局有序**​：记录上一个值`pre`，看是否递增。||||
  |**思维难度**|逻辑比较严密，但需要理解“继承祖先范围”。|逻辑更直观，只要理解“中序遍历”即可。||||
  |**坑点**|容易漏掉`long`处理溢出。|必须使用**全局变量**或类成员变量`pre`来保存状态（因为递归回溯时不能丢掉这个值）。||||

  对于初学者，**中序遍历法**通常更容易记忆，因为它直接利用了 BST 的排序特性。

  你觉得这种“比大小”的方法，是不是比刚才那个“传范围”的方法更好懂一点？

## 方法三：后序遍历

dfs 返回子树的最小值和最大值，供上面的节点判断是否为二叉搜索树。

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return dfs(root)[1] != Long.MAX_VALUE;
    }

    private long[] dfs(TreeNode node) {
        if (node == null) {
            return new long[]{Long.MAX_VALUE, Long.MIN_VALUE};
        }
        long[] left = dfs(node.left);
        long[] right = dfs(node.right);
        long x = node.val;
        // 也可以在递归完左子树之后立刻判断，如果发现不是二叉搜索树，就不用递归右子树了
        if (x <= left[1] || x >= right[0]) {
            return new long[]{Long.MIN_VALUE, Long.MAX_VALUE};
        }
        return new long[]{Math.min(left[0], x), Math.max(right[1], x)};
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/validate-binary-search-tree/solutions/2020306/qian-xu-zhong-xu-hou-xu-san-chong-fang-f-yxvh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 前序遍历在某些数据下不需要递归到叶子节点就能返回（比如根节点左儿子的值大于根节点的值，左儿子就不会继续往下递归了），而中序遍历和后序遍历至少要递归到一个叶子节点。从这个角度上来说，前序遍历是最快的。  
  中序遍历很好地利用了二叉搜索树的性质，使用到的变量最少。  
  后序遍历的思想是最通用的，即自底向上计算子问题的过程。想要学好动态规划的话，请务必掌握自底向上的思想。

  ‍
