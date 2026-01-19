---
title: day 14
date: '2026-01-19 16:34:01'
updated: '2026-01-19 16:52:34'
permalink: /post/day-14-z1clix0.html
comments: true
toc: true
---



# day 14

# **第六章  二叉树 part02**

### 226.翻转二叉树 （优先掌握递归）

这道题目 一些做过的同学 理解的也不够深入，建议大家先看我的视频讲解，无论做过没做过，都会有很大收获。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html](https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html)</u>

### 101. 对称二叉树 （优先掌握递归）

先看视频讲解，会更容易一些。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html](https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html)</u>

### 104.二叉树的最大深度 （优先掌握递归）

什么是深度，什么是高度，如何求深度，如何求高度，这里有关系到二叉树的遍历方式。

大家 要先看视频讲解，就知道以上我说的内容了，很多录友刷过这道题，但理解的还不够。

题目链接/文章讲解/视频讲解： <u>[https://programmercarl.com/0104.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.html](https://programmercarl.com/0104.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.html)</u>

### 111.二叉树的最小深度 （优先掌握递归）

先看视频讲解，和最大深度 看似差不多，其实 差距还挺大，有坑。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.html](https://programmercarl.com/0111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.html)</u>

### 226.翻转二叉树 （优先掌握递归）

[226. 翻转二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/invert-binary-tree/description/)

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode left = invertTree(root.left); // 翻转左子树
        TreeNode right = invertTree(root.right); // 翻转右子树
        root.left = right; // 交换左右儿子
        root.right = left;
        return root;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/invert-binary-tree/solutions/2713610/shi-pin-shen-ru-li-jie-di-gui-pythonjava-zhqh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 写法二

也可以先交换左右儿子，然后递归处理左子树，递归处理右子树。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode tmp = root.left; // 交换左右儿子
        root.left = root.right;
        root.right = tmp;
        invertTree(root.left); // 翻转左子树
        invertTree(root.right); // 翻转右子树
        return root;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/invert-binary-tree/solutions/2713610/shi-pin-shen-ru-li-jie-di-gui-pythonjava-zhqh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

‍

# 101. 对称二叉树 （优先掌握递归）

[101. 对称二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/symmetric-tree/solutions/2015063/ru-he-ling-huo-yun-yong-di-gui-lai-kan-s-6dq5/)

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isSameTree(root.left, root.right);
    }

    // 100. 相同的树（改成镜像判断）
    private boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null || q == null) {
            return p == q;
        }
        return p.val == q.val && isSameTree(p.left, q.right) && isSameTree(p.right, q.left);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/symmetric-tree/solutions/2015063/ru-he-ling-huo-yun-yong-di-gui-lai-kan-s-6dq5/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 104.二叉树的最大深度 （优先掌握递归）

[104. 二叉树的最大深度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)

# 方法一：自底向上

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int lDepth = maxDepth(root.left);
        int rDepth = maxDepth(root.right);
        return Math.max(lDepth, rDepth) + 1;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/maximum-depth-of-binary-tree/solutions/2010612/kan-wan-zhe-ge-shi-pin-rang-ni-dui-di-gu-44uz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 方法二：自顶向下

```java
class Solution {
    private int ans;

    public int maxDepth(TreeNode root) {
        dfs(root, 0);
        return ans;
    }

    private void dfs(TreeNode node, int depth) {
        if (node == null) {
            return;
        }
        depth++;
        ans = Math.max(ans, depth);
        dfs(node.left, depth);
        dfs(node.right, depth);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/maximum-depth-of-binary-tree/solutions/2010612/kan-wan-zhe-ge-shi-pin-rang-ni-dui-di-gu-44uz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 111.二叉树的最小深度 （优先掌握递归）

[111. 二叉树的最小深度 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)

# 方法一：自顶向下「递」

```java
class Solution {
    private int ans = Integer.MAX_VALUE;

    public int minDepth(TreeNode root) {
        dfs(root, 0);
        return root != null ? ans : 0;
    }

    private void dfs(TreeNode node, int cnt) {
        if (node == null) {
            return;
        }
        cnt++;
        if (node.left == null && node.right == null) { // node 是叶子
            ans = Math.min(ans, cnt);
            return;
        }
        dfs(node.left, cnt);
        dfs(node.right, cnt);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 优化

##### 如果递归中发现 cnt≥ans，由于继续向下递归也不会让 ans 变小，直接返回。

##### 这一技巧叫做「最优性剪枝」。

```java
class Solution {
    private int ans = Integer.MAX_VALUE;

    public int minDepth(TreeNode root) {
        dfs(root, 0);
        return root != null ? ans : 0;
    }

    private void dfs(TreeNode node, int cnt) {
        if (node == null || ++cnt >= ans) { // 最优性剪枝
            return;
        }
        if (node.left == null && node.right == null) { // node 是叶子
            ans = cnt;
            return;
        }
        dfs(node.left, cnt);
        dfs(node.right, cnt);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- #### 复杂度分析

  - 时间复杂度：O(n)，其中 n 为二叉树的节点个数。
  - 空间复杂度：O(n)。最坏情况下，二叉树退化成一条链，递归需要 O(n) 的栈空间。

# 方法二：自底向上「归」

定义 dfs(node) 表示以节点 node 为根的**子树**的最小深度。\

- 写法一  
  分类讨论：
- 如果 node 是空节点，由于没有节点，返回 0。  
  如果 node 没有右儿子，那么深度就是左子树的深度加一，即 dfs(node)=dfs(node.left)+1。  
  如果 node 没有左儿子，那么深度就是右子树的深度加一，即 dfs(node)=dfs(node.right)+1。  
  如果 node 左右儿子都有，那么分别递归计算左子树的深度，以及右子树的深度，二者取最小值再加一，即  
  dfs(node)=min(dfs(node.left),dfs(node.right))+1  
  注意：并不需要特判 node 是叶子的情况，因为在没有右儿子的情况下，我们会递归 node.left，如果它是空节点，递归的返回值是 0，加一后得到 1，这正是叶子节点要返回的值。
- 答案：dfs(root)。
- 代码实现时，可以直接递归调用 minDepth。
- 答疑  
  问：本题和 104. 二叉树的最大深度 的区别是什么？为什么本题代码要更复杂一些？
- 答：对于非叶节点，把握一个共同原则：如果一个儿子是空节点，另一个儿子不是空节点，那么答案只能来自非空的那一侧。
- 求最大深度，空节点返回 0，直接计算 max，一定会取到有节点的那一侧（因为深度比 0 大）。  
  求最小深度，空节点返回 0，直接计算 min，会取到空节点，不符合「答案只能来自非空的那一侧」。所以求最小深度必须多写一些逻辑。
- 作者：灵茶山艾府  
  链接：https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/  
  来源：力扣（LeetCode）  
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

‍

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (root.right == null) {
            return minDepth(root.left) + 1;
        }
        if (root.left == null) {
            return minDepth(root.right) + 1;
        }
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 写法二  
  分类讨论：

  如果 node 是空节点，返回 0。  
  如果 node 是叶子节点，返回 1。  
  否则，计算 leftDepth，如果左儿子不是空节点，那么 leftDepth=minDepth(node.left)，否则 leftDepth=∞，这样后面计算 min 不会取到 ∞。对于右儿子也同理，计算出 rightDepth。最后返回 min(leftDepth,rightDepth)+1。

  作者：灵茶山艾府  
  链接：https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/  
  来源：力扣（LeetCode）  
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (root.left == null && root.right == null) { // root 是叶子
            return 1;
        }
        int leftDepth = root.left != null ? minDepth(root.left) : Integer.MAX_VALUE;
        int rightDepth = root.right != null ? minDepth(root.right) : Integer.MAX_VALUE;
        return Math.min(leftDepth, rightDepth) + 1;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/minimum-depth-of-binary-tree/solutions/2730984/liang-chong-fang-fa-zi-ding-xiang-xia-zi-0sxz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
