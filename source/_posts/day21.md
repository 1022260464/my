---
title: day21
date: '2026-01-27 21:26:08'
updated: '2026-01-27 21:41:14'
permalink: /post/day21-z1rcxpc.html
comments: true
toc: true
---



# day21

# **第六章 二叉树part08**

### 669. 修剪二叉搜索树

这道题目比较难，比 添加增加和删除节点难的多，建议先看视频理解。

题目链接/文章讲解： <u>[https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html](https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)</u>

视频讲解： <u>[https://www.bilibili.com/video/BV17P41177ud](https://www.bilibili.com/video/BV17P41177ud)</u>

### 108.将有序数组转换为二叉搜索树

本题就简单一些，可以尝试先自己做做。

<u>[https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.htm](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)</u>

### 538.把二叉搜索树转换为累加树

本题也不难，在 求二叉搜索树的最小绝对差 和 众数 那两道题目 都讲过了 双指针法，思路是一样的。

<u>[https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html](https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1d44y1f7wP](https://www.bilibili.com/video/BV1d44y1f7wP)</u>

### 总结篇  

[好了，二叉树大家就这样刷完了，做一个总结吧]()

<u>[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E6%80%BB%E7%BB%93%E7%AF%87.html](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E6%80%BB%E7%BB%93%E7%AF%87.html)</u>

‍

# 669. 修剪二叉搜索树

## 递归法：

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return null;
        if (root.val < low) return trimBST(root.right, low, high);
        else if (root.val > high) return trimBST(root.left, low, high);
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}

作者：宫水三叶
链接：https://leetcode.cn/problems/trim-a-binary-search-tree/solutions/1814532/by-ac_oier-help/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是一个非常经典的**修剪二叉搜索树 (Trim a Binary Search Tree)**  的递归解法（LeetCode 669 题）。

  这段代码充分利用了 **二叉搜索树 (BST)**  的特性（左子节点 \< 根节点 \< 右子节点）来高效地决定保留或丢弃哪些部分。

  以下是代码逻辑的详细解析：

  ### 1. 核心思路

  我们的目标是移除所有小于 `low`​ 或者大于 `high` 的节点。

  由于 BST 的有序性，我们不需要遍历整棵树，而是可以根据当前节点的值来**剪掉**整个无效的子树。

  ### 2. 代码逻辑拆解

  这段代码主要处理三种情况：

  #### 情况 A：当前节点值太小 (`root.val < low`)

  Java

  ```
  if (root.val < low) return trimBST(root.right, low, high);
  ```

  - **原理：**  如果当前节点的值小于 `low`​，那么它的**整个左子树**肯定都小于 `low`​（因为左 \< 根）。
  - **操作：**  当前节点和左子树都必须被“修剪”掉。但是，**右子树**中可能还有符合范围 `[low, high]`​ 的节点（因为右 \> 根）。
  - **结果：**  我们直接放弃当前节点，递归处理右子树，并返回右子树的处理结果来顶替当前节点的位置。

  #### 情况 B：当前节点值太大 (`root.val > high`)

  Java

  ```
  else if (root.val > high) return trimBST(root.left, low, high);
  ```

  - **原理：**  如果当前节点的值大于 `high`​，那么它的**整个右子树**肯定都大于 `high`​（因为右 \> 根）。
  - **操作：**  当前节点和右子树都必须被“修剪”掉。但是，**左子树**中可能还有符合范围的节点。
  - **结果：**  我们直接放弃当前节点，递归处理左子树，并返回左子树的处理结果来顶替当前节点的位置。

  #### 情况 C：当前节点在范围内 (`low <= root.val <= high`)

  Java

  ```
  root.left = trimBST(root.left, low, high);
  root.right = trimBST(root.right, low, high);
  return root;
  ```

  - **原理：**  当前节点是合法的，我们需要保留它。
  - **操作：虽然当前节点没问题，但它的孩子节点**可能越界。所以我们需要分别对左右子树进行递归修剪 (`trimBST`​)，并将修剪后的结果重新赋值给 `root.left`​ 和 `root.right`。
  - **结果：**  重连完孩子后，返回当前节点 `root`。

  ---

  ### 3. 复杂度分析

  - **时间复杂度：**  $O(N)$

    - 其中 $N$ 是树中的节点数。在最坏的情况下（例如我们需要访问每一个节点来判断），我们需要遍历整棵树。
  - **空间复杂度：**  $O(H)$

    - 其中 $H$ 是树的高度。这部分空间消耗主要来自于递归调用栈。最坏情况（树退化成链表）为 $O(N)$，平均情况为 $O(\log N)$。

  ### 总结

  它没有使用额外的存储空间，而是直接在原树结构上通过修改指针来移除节点，完美体现了递归在树形结构处理上的优势。

  **您想看这个算法的“迭代法”（非递归）写法来进行对比吗？**

# 迭代法：

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        while (root != null && (root.val < low || root.val > high)) root = root.val < low ? root.right : root.left;
        TreeNode ans = root;
        while (root != null) {
            while (root.left != null && root.left.val < low) root.left = root.left.right;
            root = root.left;
        }
        root = ans;
        while (root != null) {
            while (root.right != null && root.right.val > high) root.right = root.right.left;
            root = root.right;
        }
        return ans;
    }
}

作者：宫水三叶
链接：https://leetcode.cn/problems/trim-a-binary-search-tree/solutions/1814532/by-ac_oier-help/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是 **修剪二叉搜索树 (Trim BST)**  的 **迭代法 (Iterative)**  版本。

  与递归版本相比，迭代法的最大优势在于​**不需要使用栈空间**，因此空间复杂度是常数级 $O(1)$，避免了在树极深时可能出现的栈溢出问题。

  这段代码的逻辑可以清晰地分为三个步骤：

  ### 1. 寻找合法的“新根节点” (Find New Root)

  Java

  ```
  while (root != null && (root.val < low || root.val > high)) 
      root = root.val < low ? root.right : root.left;
  TreeNode ans = root;
  ```

  - **目的：**  原来的 `root`​ 可能本身就不在 `[low, high]` 范围内。我们首先需要找到这棵树中第一个落在区间内的节点，它将成为新树的根。
  - **逻辑：**

    - 如果当前节点 `< low`​：说明它和它的左子树都太小，往右走 (`root = root.right`)。
    - 如果当前节点 `> high`​：说明它和它的右子树都太大，往左走 (`root = root.left`)。
    - 一旦找到一个在 `[low, high]`​ 之间的节点，或者是 `null`​，循环结束。我们将这个节点记为 `ans`，最后返回它。

  ### 2. 修剪左子树 (Trim Left Subtree)

  Java

  ```
  while (root != null) {
      while (root.left != null && root.left.val < low) 
          root.left = root.left.right;
      root = root.left;
  }
  ```

  - **当前状态：**  此时的 `root` 必然是合法的（即 $\ge low$ 且 $\le high$）。
  - **只处理下限：**  对于左子树，我们只需要关心它是否 ​**小于** **​`low`​**。

    - *为什么不用管* *​`high`​*​ *？*  因为在 BST 中，左节点必然小于父节点。既然父节点 `root` 已经是 $\le high$ 了，那么 `root.left` 必然也 $\le high$，所以左边不可能越过上限。
  - **操作：**

    - 如果左孩子太小 (`root.left.val < low`​)：说明左孩子及其左子树都废了。我们直接把左孩子的**右子树**接上来（`root.left = root.left.right`）。
    - **注意内层** **​`while`​**​ **：**  接上来的新节点可能依然太小，所以需要一直循环判断，直到左孩子合法或者为空。
    - 当左孩子合法后，指针下移 (`root = root.left`) 继续处理下一层。

  ### 3. 修剪右子树 (Trim Right Subtree)

  Java

  ```
  root = ans; // 重置指针回到合法的头节点
  while (root != null) {
      while (root.right != null && root.right.val > high) 
          root.right = root.right.left;
      root = root.right;
  }
  ```

  - **重置：**  将 `root`​ 指针重新指回 `ans`，开始处理右边。
  - **只处理上限：**  对于右子树，我们只需要关心它是否 ​**大于** **​`high`​**。

    - *为什么不用管* *​`low`​*​ *？*  同理，右节点必然大于父节点。父节点 $\ge low$，则右节点必然 $\ge low$。
  - **操作：**

    - 如果右孩子太大 (`root.right.val > high`​)：说明右孩子及其右子树都废了。我们把右孩子的**左子树**接上来（`root.right = root.right.left`）。
    - 同样使用内层 `while` 确保接上来的新节点满足要求。
    - 处理完当前层，指针下移。

  ---

  ### 复杂度对比

  |**维度**|**递归法 (Recursive)**|**迭代法 (Iterative)**||||
  | --| --------------------| --------------------| --| --| --|
  |**时间复杂度**|$O(N)$- 需遍历节点|$O(N)$- 需遍历节点||||
  |**空间复杂度**|$O(H)$- 递归栈深度|**$O(1)$**- 仅需几个指针变量||||
  |**代码可读性**|极高，逻辑自然|稍复杂，需分步处理||||
  |**健壮性**|树极深时可能栈溢出|**不会栈溢出**||||

  ### 总结

  这段迭代代码非常巧妙。它不像递归那样“自底向上”地构建树，而是\*\*“自顶向下”地修改指针\*\*。

  1. 先定头 (Find Head)。
  2. 站在合法的父节点上，剔除不合法的左孩子 (Trim Left)。
  3. 站在合法的父节点上，剔除不合法的右孩子 (Trim Right)。

  这是一个非常优秀的**链表/树指针操作**范例。

### 108.将有序数组转换为二叉搜索树

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return dfs(nums, 0, nums.length - 1);
    }

    private TreeNode dfs(int[] nums, int lo, int hi) {
        if (lo > hi) {
            return null;
        } 
        // 以升序数组的中间元素作为根节点 root。
        int mid = lo + (hi - lo) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        // 递归的构建 root 的左子树与右子树。
        root.left = dfs(nums, lo, mid - 1);
        root.right = dfs(nums, mid + 1, hi); 
        return root;
    }
}

```

- 这段代码是 **将有序数组转换为高度平衡的二叉搜索树 (Convert Sorted Array to Binary Search Tree)**  的标准解法（LeetCode 108 题）。

  它的核心思想利用了 **分治法 (Divide and Conquer)**  和 ​**二叉搜索树的中序遍历特性**。

  ### 1. 核心逻辑解析

  **为什么选中间元素？**

  - **有序数组**其实就是二叉搜索树的**中序遍历**结果。
  - 题目要求构建的是**高度平衡**的 BST（即左右子树高度差不超过 1）。
  - 为了保证平衡，我们必须让根节点的左右两边包含**数量大致相等**的节点。
  - 因此，**数组的中间位置 (Middle)**  是作为根节点的最佳选择。

  ### 2. 代码步骤详解

  这是一个典型的递归过程：

  1. **确定区间：**  `dfs(nums, lo, hi)`​ 负责处理数组中从索引 `lo`​ 到 `hi` 的部分。
  2. **终止条件：**   
     Java

     ```
     if (lo > hi) { return null; }
     ```

     当左边界超过右边界时，说明区间为空，无法构成节点，返回 `null`。
  3. **选取根节点：**

     - 计算中间索引 `mid`。
     - **注意细节：**  写法 `lo + (hi - lo) / 2`​ 比 `(lo + hi) / 2`​ 更安全，可以防止 `lo + hi`​ 超过 `Integer.MAX_VALUE` 导致的溢出（虽然在数组索引场景下很少见，但是个好习惯）。
     - 创建当前子树的根节点。
  4. **递归构建左右子树：**

     - ​`root`​ 的左孩子，由数组 `[lo, mid - 1]` 这一段构建而成。
     - ​`root`​ 的右孩子，由数组 `[mid + 1, hi]` 这一段构建而成。

  ### 3. 举例可视化

  假设输入数组：`[-10, -3, 0, 5, 9]`

  1. **第一层：**  `lo=0, hi=4`​ -\> `mid=2`​ (值为 ​**0**)。

     - 根节点是 ​**0**。
     - 左边递归 `[-10, -3]`​，右边递归 `[5, 9]`。
  2. **第二层 (左)：**  处理 `[-10, -3]`​。`mid`​ 指向  **-10** (或者 -3，取决于偶数长度取左还是右，这里整数除法通常取左)。

     - 左子节点变成 ​ **-10**（假设取左中位数）。
  3. **第二层 (右)：**  处理 `[5, 9]`。

     - 右子节点变成 ​**5**（假设取左中位数）。
     - ...以此类推。

  最终生成的树是平衡的。

  ### 4. 复杂度分析

  - **时间复杂度：**  $O(N)$

    - 每个元素只会被访问一次用来创建一个节点。
  - **空间复杂度：**  $O(\log N)$

    - 这是递归栈的空间。因为我们每次都取中间，生成的树是**平衡**的，所以树的高度是 $\log N$，递归深度也是 $\log N$。
    - （注：如果生成的树不是平衡的，最坏空间复杂度会是 $O(N)$，但这个算法保证了平衡）。

  ### 总结

  这段代码非常简洁且标准。它完美地展示了如何通过**二分**的思想，把一个线性结构（数组）还原成一个平衡的层级结构（树）。

---

### 538.把二叉搜索树转换为累加树

```java
class Solution {
    private int s = 0;

    public TreeNode convertBST(TreeNode root) {
        dfs(root);
        return root;
    }

    private void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        dfs(node.right); // 递归右子树
        s += node.val;
        node.val = s; // 此时 s 就是 >= node.val 的所有数之和
        dfs(node.left); // 递归左子树
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/convert-bst-to-greater-tree/solutions/2552803/jian-ji-xie-fa-li-yong-er-cha-sou-suo-sh-lz3i/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是一个非常巧妙且简洁的解法，用于解决 ​**把二叉搜索树转换为累加树 (Convert BST to Greater Tree)** （LeetCode 538 或 1038 题）。

  这段代码的核心在于利用了二叉搜索树（BST）的特性，进行了一次 ​ **“反向中序遍历”** 。

  ### 1. 核心原理：反向中序遍历

  我们知道，对于二叉搜索树：

  - **标准中序遍历** (`Left -> Root -> Right`​) 会得到一个**升序**的序列（从小到大）。
  - **反向中序遍历** (`Right -> Root -> Left`​) 则会得到一个**降序**的序列（从大到小）。

  题目要求是将每个节点的值修改为“大于或等于该节点值的所有节点之和”。

  如果我们**从大到小**遍历节点，那么对于当前访问的节点，**之前访问过的所有节点**都比它大。

  ### 2. 代码逻辑解析

  Java

  ```
  private int s = 0; // 全局累加器
  ```
  - ​`s`​ 用来记录​**当前已经遍历过的所有节点值的总和**。

  Java

  ```
  private void dfs(TreeNode node) {
      if (node == null) return;
      
      // 1. 先递归右子树（去寻找比当前节点大的值）
      dfs(node.right); 
      
      // 2. 处理当前节点
      s += node.val;   // 把当前节点原来的值加到累加器 s 中
      node.val = s;    // 更新当前节点的值为 s (此时 s = 原值 + 所有比它大的值)
      
      // 3. 再递归左子树（去寻找比当前节点小的值）
      dfs(node.left); 
  }
  ```
  ### 3. 举例推演

  假设有一棵二叉搜索树：

  Plaintext

  ```
      2
     / \
    0   3
  ```
  这棵树的节点值大小顺序是：`0 < 2 < 3`。

  **执行流程 (**​**​`dfs(root)`​** ​ **)：**

  1. **向右走到底：**  访问节点 ​**3**。

     - ​`dfs(node.right)` (3 的右边是 null，返回)
     - ​`s += 3` $\rightarrow$ `s` 变为 3。
     - ​`node.val = 3` (节点 3 更新为 3)。
     - ​`dfs(node.left)` (3 的左边是 null，返回)
     - **此时树的状态：**  3 已经处理完。
  2. **回到根节点：**  访问节点 ​**2**。

     - ​`s += 2` $\rightarrow$ `s` 变为 $3 + 2 = 5$。
     - ​`node.val = 5` (节点 2 更新为 5)。
     - **逻辑验证：**  节点 2 的新值应该是 $2 + 3 = 5$。正确。
  3. **向左走：**  访问节点 ​**0**。

     - ​`dfs(node.right)` (0 的右边是 null，返回)
     - ​`s += 0` $\rightarrow$ `s` 变为 $5 + 0 = 5$。
     - ​`node.val = 5` (节点 0 更新为 5)。
     - **逻辑验证：**  节点 0 的新值应该是 $0 + 2 + 3 = 5$。正确。

  最终树变为：

  Plaintext

  ```
      5
     / \
    5   3
  ```
  ### 4. 复杂度分析

  - **时间复杂度：**  $O(N)$

    - 我们需要遍历树中的每一个节点一次。
  - **空间复杂度：**  $O(H)$

    - $H$ 是树的高度，代表递归栈的深度。平均情况 $O(\log N)$，最坏情况 $O(N)$。

  ### 总结

  灵茶山艾府（EndlessCheng）的这段代码极其精炼。它并没有创建一个新列表来存数，也没有重复计算，而是直接利用递归的顺序，通过一个外部变量 `s` 维护前缀和（准确说是“后缀和”），一步到位完成了转换。
