---
title: day20
date: '2026-01-25 22:12:53'
updated: '2026-01-25 22:26:50'
permalink: /post/day20-1y4dno.html
comments: true
toc: true
---



# day20

# **第六章 二叉树part07**

### 235. 二叉搜索树的最近公共祖先

相对于 二叉树的最近公共祖先 本题就简单一些了，因为 可以利用二叉搜索树的特性。

题目链接/文章讲解：<u>[https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1Zt4y1F7ww](https://www.bilibili.com/video/BV1Zt4y1F7ww)</u>

### 701.二叉搜索树中的插入操作

本题比想象中的简单，大家可以先自己想一想应该怎么做，然后看视频讲解，就发现 本题为什么比较简单了。

题目链接/文章讲解：<u>[https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html](https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1Et4y1c78Y](https://www.bilibili.com/video/BV1Et4y1c78Y)</u>

### 450.删除二叉搜索树中的节点

相对于 插入操作，本题就有难度了，涉及到改树的结构

题目链接/文章讲解：<u>[https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html](https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)</u>​[  ]()

视频讲解：<u>[https://www.bilibili.com/video/BV1tP41177us](https://www.bilibili.com/video/BV1tP41177us)</u>

# 235. 二叉搜索树的最近公共祖先

[235. 二叉搜索树的最近公共祖先 - 力扣（LeetCode）](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/solutions/2023873/zui-jin-gong-gong-zu-xian-yi-ge-shi-pin-8h2zc/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        int x = root.val;
        if (p.val < x && q.val < x) { // p 和 q 都在左子树
            return lowestCommonAncestor(root.left, p, q);
        }
        if (p.val > x && q.val > x) { // p 和 q 都在右子树
            return lowestCommonAncestor(root.right, p, q);
        }
        return root; // 其它
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/solutions/2023873/zui-jin-gong-gong-zu-xian-yi-ge-shi-pin-8h2zc/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是专门针对 **二叉搜索树 (BST)**  的最近公共祖先（LCA）解法（对应 LeetCode 235 题）。

  它比上一题（普通二叉树 LCA）要简单得多，因为它利用了 BST  **“左小右大”**  的黄金特性，不需要像普通二叉树那样“盲搜”全树。

  ### 1. 核心逻辑：寻找“分岔点”

  在 BST 中，寻找最近公共祖先，本质上就是在找 **第一个** 满足以下条件的节点：

  **该节点的值位于** **$p$** **和** **$q$** **之间（包含** **$p$** **或** **$q$** **本身）。**

  - 如果当前节点的值比 $p$ 和 $q$ **都大** $\rightarrow$ 说明 $p$ 和 $q$ 都在左边，往左走。
  - 如果当前节点的值比 $p$ 和 $q$ **都小** $\rightarrow$ 说明 $p$ 和 $q$ 都在右边，往右走。
  - **一旦出现“一左一右”或者“等于其中一个”**  $\rightarrow$ 说明当前节点就是分岔点（LCA），直接返回。

  ### 2. 代码详解

  Java

  ```
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
      int x = root.val;
      
      // 情况 1: 都在左边
      // 如果 p 和 q 的值都小于当前节点 x
      // 说明公共祖先一定在左子树里，递归向左找
      if (p.val < x && q.val < x) { 
          return lowestCommonAncestor(root.left, p, q);
      }
      
      // 情况 2: 都在右边
      // 如果 p 和 q 的值都大于当前节点 x
      // 说明公共祖先一定在右子树里，递归向右找
      if (p.val > x && q.val > x) { 
          return lowestCommonAncestor(root.right, p, q);
      }
      
      // 情况 3: 找到了分岔点 (LCA)
      // 此时只有三种可能：
      // 1. p < x < q (一个在左，一个在右，x 是分界点)
      // 2. p == x (x 就是 p，且 q 在 x 的下面，x 是祖先)
      // 3. q == x (x 就是 q，且 p 在 x 的下面，x 是祖先)
      // 无论哪种，x 都是最近公共祖先，直接返回。
      return root; 
  }
  ```

  ### 3. 举例演示

  假设 BST 如下，找 **0** 和 **5** 的 LCA：

  Plaintext

  ```
         6
       /   \
      2     8
     / \   / \
    0   4 7   9
         \
          5
  ```

  1. ​**当前 root**  **=**  **6**。

     - 目标是 0 和 5。
     - $0 < 6$ 且 $5 < 6$。
     - ​**判定**​：两个都在左边，于是调用 `lowestCommonAncestor(2, ...)`。
  2. ​**当前 root**  **=**  **2**。

     - 目标是 0 和 5。
     - $0 < 2$，但 $5 > 2$。
     - ​**判定**：一个在左，一个在右。
     - 代码逻辑：前两个 `if` 都不满足（不全小，也不全大）。
     - 执行 `return root;`（返回 2）。

  ​**结果**：LCA 是 2。

  ### 4. 与上一题（普通二叉树）的区别

  这是一个非常经典的面试考点，面试官经常会先让你写普通二叉树的，再问你“如果是 BST 怎么优化？”

  |**特性**|**普通二叉树 (上一题)**|**二叉搜索树 BST (本题)**||||
  | --| -----------------------------------------------| ------------------------------------------------------| --| --| --|
  |**搜索方式**|​**后序遍历**（DFS）。必须先搜完子树，根据返回值判断。|​**前序遍历**（类似二分查找）。从上往下，根据值直接决定去向。||||
  |**是否回溯**|需要回溯（自底向上汇报）。|​**不需要回溯**（一路向下，找到即停）。||||
  |**时间复杂度**|$O(N)$- 最坏情况要遍历所有节点。|$O(\log N)$- 只需要走一条路径（树高）。||||
  |**空间复杂度**|$O(N)$- 递归栈深度。|$O(1)$- 如果改写成迭代写法（不需要栈）。||||

  ### 5. 迭代版（拓展）

  因为 BST 不需要回溯，这道题可以很容易地改成 `while` 循环，空间复杂度直接降为 $O(1)$。这是相比普通二叉树最大的优势。

  Java

  ```
  // 迭代写法：极其高效，面试加分项
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
      while (true) {
          if (root.val > p.val && root.val > q.val) {
              root = root.left; // 都在左边，往左走
          } else if (root.val < p.val && root.val < q.val) {
              root = root.right; // 都在右边，往右走
          } else {
              return root; // 找到了分岔点
          }
      }
  }
  ```

# 701.二叉搜索树中的插入操作

[701. 二叉搜索树中的插入操作 - 力扣（LeetCode）](https://leetcode.cn/problems/insert-into-a-binary-search-tree/solutions/)

## 迭代法：

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        TreeNode newRoot = root;
        TreeNode pre = root;
        while (root != null) {
            pre = root;
            if (root.val > val) {
                root = root.left;
            } else if (root.val < val) {
                root = root.right;
            } 
        }
        if (pre.val > val) {
            pre.left = new TreeNode(val);
        } else {
            pre.right = new TreeNode(val);
        }

        return newRoot;
    }}
```

- 这段代码写得​**完全正确**​，是一个非常标准的 ​**BST 插入操作的迭代（非递归）写法**。

  相比于递归写法，迭代写法最大的优势是​**省内存**（不需要系统栈空间，空间复杂度 $O(1)$）。

  下面为您详细拆解这段代码的逻辑亮点：

  ### 1. 核心思路：双指针法 (`pre`​ 和 `root`)

  在二叉树的插入操作中，我们面临一个核心矛盾：

  - 我们需要遍历找到那个 `null`​ 的空位（这是 `root` 指针的任务）。
  - 但是，当我们找到 `null`​ 时，我们必须操作 **它的父节点** 才能把新节点接上去（单单拿着一个 `null` 是没法操作的）。

  所以，这里必须使用 ​**双指针**：

  - ​**​`root`​**​  **(当前指针)** ​：负责探路，一直往下走，直到走到 `null` 为止。
  - ​**​`pre`​**​  **(父节点指针)** ​：紧紧跟在 `root`​ 后面，记录“我从哪里来”。当 `root`​ 掉进 `null`​ 坑里时，`pre` 正好站在悬崖边，负责把新节点拉住。

  ### 2. 代码逐行解析

  Java

  ```
  // 特判：如果树是空的，直接把新节点作为根返回
  if (root == null) return new TreeNode(val);

  TreeNode newRoot = root; // 因为下面 root 变量会被用来遍历（会变），所以先存一下原始根节点，用于最后返回
  TreeNode pre = root;     // 双指针之父指针

  // 1. 寻找插入位置
  while (root != null) {
      pre = root; // pre 先跟上，记录当前节点为父节点
      if (root.val > val) {
          root = root.left;  // 目标值小，往左找
      } else if (root.val < val) {
          root = root.right; // 目标值大，往右找
      } 
  }

  // 此时 loop 结束，root 变成了 null，pre 指向了叶子节点（也就是待插入位置的父节点）

  // 2. 执行插入动作
  // 这里需要再判断一次，因为不知道是 pre 的左边空了还是右边空了
  if (pre.val > val) {
      pre.left = new TreeNode(val);
  } else {
      pre.right = new TreeNode(val);
  }

  return newRoot;
  ```

  ### 3. 一个小小的变量命名建议

  虽然您的逻辑完全正确，但直接复用参数 `root` 作为遍历变量有时候会让人（尤其是初学者）感到困惑：“诶？root 怎么变了？最后返回的还是原来的树吗？”

  通常我们会习惯重新定义一个变量 `cur` (current) 来遍历，这样代码语义更清晰：

  Java

  ```
  class Solution {
      public TreeNode insertIntoBST(TreeNode root, int val) {
          if (root == null) return new TreeNode(val);
          
          // 使用 cur 代替修改 root，这样就不需要 newRoot 变量了，最后直接 return root 即可
          TreeNode cur = root; 
          TreeNode pre = root;
          
          while (cur != null) {
              pre = cur; // 记录父节点
              if (cur.val > val) {
                  cur = cur.left;
              } else {
                  cur = cur.right;
              }
          }
          
          // 链接新节点
          if (pre.val > val) {
              pre.left = new TreeNode(val);
          } else {
              pre.right = new TreeNode(val);
          }
          
          return root;
      }
  }
  ```

  ### 4. 复杂度分析

  - ​**时间复杂度**：$O(N)$。最坏情况下（树退化成链表）需要从头走到尾。如果是平衡树则是 $O(\log N)$。
  - ​**空间复杂度**：$O(1)$。这是**迭代版**最大的优势！它只用了两个指针变量，没有递归产生的调用栈开销。

  ### 总结

  您的代码逻辑非常清晰，成功运用了**双指针**解决“寻找父节点”的问题。这就是工程中处理链表和树结构非常实用的技巧！

## 递归法:

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) // 如果当前节点为空，也就意味着val找到了合适的位置，此时创建节点直接返回。
            return new TreeNode(val);
            
        if (root.val < val){
            root.right = insertIntoBST(root.right, val); // 递归创建右子树
        }else if (root.val > val){
            root.left = insertIntoBST(root.left, val); // 递归创建左子树
        }
        return root;
    }}
```

- 这段代码写得**非常漂亮且简练**。这是二叉搜索树（BST）插入操作最经典的**递归写法**。

  相比于您刚才发的迭代（循环）版本，这个递归版本最大的特点就是利用了 **“函数返回值”** 来自动完成父子节点的连接，从而省去了那个 `pre` 指针。

  以下是这段代码的核心逻辑解析，特别是一个初学者容易因“想不通”而卡住的地方：

  ### 1. 核心魔法：为什么要写 `root.right = ...`？

  很多初学者能理解 `insertIntoBST(root.right, val)`​ 是去右边找位置，但很难理解为什么要把结果**重新赋值**给 `root.right`：

  ```java
  root.right = insertIntoBST(root.right, val);

  ```

  **这里是递归的精髓：**

  - **对于普通的节点（路人甲）** ：  
    比如当前 `root`​ 是 10，我们要去它的右边插入 15。它的右孩子原本是 12。  
    递归调用 `insertIntoBST(node(12), 15)`​ 会返回 `node(12)`​（因为它只负责往下传导，自己没变）。  
    所以 `root.right = node(12)`​，相当于**自己指向自己原来的孩子**，链接关系没有改变。
  - **对于也就是这一层的父节点（关键时刻）** ：  
    当递归走到 `null`​ 时（找到了空位），`if (root == null)`​ 返回了 `new TreeNode(val)`​。  
    **此时回到上一层**，上一层的 `root.left`​ 或 `root.right`​ 接收到的返回值，就不再是 `null`​ 了，而是这个**新创建的节点**。  
    **一句话总结：**  这行代码的意思是 **“我去让我的子树处理插入任务，处理完后，它返回什么，我就认谁做我的孩子。”** （大部分时候认的还是原来的孩子，只有在尽头时认了新孩子）。

  ### 2. 递归 vs 迭代（对比您上一段代码）

  这两段代码是完美的对比教材：

  |特性|**递归版 (Recursive)**  (本段代码)|**迭代版 (Iterative)**  (上一段代码)|
  | ------| ------------------------------------------------| --------------------------------------|
  |**代码量**|极少，逻辑清晰。|较多，需要维护 `pre`​ 和 `root` 两个指针。|
  |**如何连接新节点**|利用 **返回值** (`return new`) 接龙。|利用 **​`pre`​**​ **指针** 手动 `pre.left = new`。|
  |**空间复杂度**|****。需要系统栈空间（树有多高，栈就有多深）。|****。只用了几个变量，**更省内存**。|
  |**思维难度**|需要理解“递归的返回值接龙”。|符合直觉的“走到死胡同然后造房子”。|

  ### 3. 图解“接龙”过程

  假设向 `4 -> 7`​ (7是叶子) 的树中插入 `9`：

  1. **Level 1 (**​**​`root=4`​**​ **)** :

  - ​`4 < 9`​，执行 `root.right = insertIntoBST(7, 9)`。
  - 等待 Level 2 返回结果...

  2. **Level 2 (**​**​`root=7`​**​ **)** :

  - ​`7 < 9`​，执行 `root.right = insertIntoBST(null, 9)`。
  - 等待 Level 3 返回结果...

  3. **Level 3 (**​**​`root=null`​**​ **)** :

  - 触发 `root == null`。
  - **返回 `new Node(9)**`。

  4. **回到 Level 2**:

  - 接收到返回值。`root.right`​ (原本是 null) 现在变成了 `Node(9)`。
  - **链接建立！**  (7 指向了 9)。
  - Level 2 任务结束，**返回自己 (**​**​`Node(7)`​** ​ **)** 。

  5. **回到 Level 1**:

  - 接收到返回值。`root.right`​ 被赋值为 `Node(7)` (原本就是 7，没有变)。
  - Level 1 任务结束，**返回自己 (**​**​`Node(4)`​** ​ **)** 。

  ### 总结

  您的这段递归代码是**完全正确且规范**的。

  - **面试建议**：如果面试官没有限制空间复杂度，或者让你“快速写出来”，用**递归版**（不容易写错）。
  - **工程建议**：如果树特别特别深（比如几万层），为了防止栈溢出（Stack Overflow），或者为了极致的性能，用**迭代版**（您上一条发的那个）。

# 450.删除二叉搜索树中的节点

## 解法一：

```java
// 解法1(最好理解的版本)
class Solution {
  public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return root;
    if (root.val == key) {
      if (root.left == null) {
        return root.right;
      } else if (root.right == null) {
        return root.left;
      } else {
        TreeNode cur = root.right;
        while (cur.left != null) {
          cur = cur.left;
        }
        cur.left = root.left;
        root = root.right;
        return root;
      }
    }
    if (root.val > key) root.left = deleteNode(root.left, key);
    if (root.val < key) root.right = deleteNode(root.right, key);
    return root;
  }
}
```

- 这是一个非常经典且直观的解法！这段代码确实如你所说，是  **“最好理解”**  的版本。

  它最大的特点在于处理  **“左右孩子都不为空”**  这种情况时，采用了一种  **“整体搬迁”**  的策略，而不是去交换数值。

  我为您详细拆解一下这段代码的逻辑，特别是那个最难理解的 `else` 分支。

  ### 1. 核心逻辑图解

  这段代码主要处理了三种情况：

  #### 情况 A：没找到删除目标

  ​`if (root == null) return root;`  
  直接返回，不做任何操作。

  #### 情况 B：找到了目标，且它“缺胳膊少腿”（只有 0 个或 1 个孩子）

  这是最简单的情况，直接用剩下的那个孩子（或者 null）顶替自己的位置即可。

  - ​`if (root.left == null)`: 左边没人，右孩子上位。
  - ​`if (root.right == null)`: 右边没人，左孩子上位。

  #### 情况 C：找到了目标，且它“儿女双全”（左右孩子都有）

  这是这段代码的精华所在。

  **策略：**   
  既然要删除当前节点 `root`​，我们需要从它的 **右子树** 中找一个新老大（继任者）来顶替它。

  1. **找继任者**：右子树里 **最小** 的那个节点（也就是右子树里最左边的节点）。
  2. **搬家**：把 `root`​ 的 **左子树** 整个搬过去，作为那个“继任者”的左孩子。
  3. **上位**：让 `root` 的右孩子直接变成新的根。

  ### 2. 代码逐行注释解析

  ```java
  class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
      // 1. 没找到，或者是空树，直接返回
      if (root == null) return root;

      // 2. 找到了要删除的节点
      if (root.val == key) {
        
        // 情况 1: 如果左孩子为空（包含叶子节点的情况），直接返回右孩子
        // 相当于让右孩子接替了自己的位置
        if (root.left == null) {
          return root.right;
        } 
        // 情况 2: 如果右孩子为空，直接返回左孩子
        else if (root.right == null) {
          return root.left;
        } 
        // 情况 3: 左右都有孩子 (难点)
        else {
          // 第一步：去右子树找“接盘侠”
          // 我们要找右子树中最小的节点，因为它比左子树所有节点都大，
          // 只有它能当左子树的新父亲。
          TreeNode cur = root.right;
          while (cur.left != null) {
            cur = cur.left;
          }
          
          // 第二步：嫁接
          // 把在这个节点原本的左子树，整个挂到“接盘侠”的左边
          cur.left = root.left;
          
          // 第三步：上位
          // 删除原来的 root，直接返回 root.right 作为新的根
          root = root.right;
          return root;
        }
      }

      // 3. 还没找到，利用 BST 特性继续递归
      if (root.val > key) root.left = deleteNode(root.left, key);
      if (root.val < key) root.right = deleteNode(root.right, key);
      
      return root;
    }
  }

  ```

  ### 3. 这个解法的优缺点分析

  **优点：**

  - **逻辑直观**：不需要进行复杂的旋转或者值的交换，就是单纯的“指针修改”。
  - **一次递归**：在处理“双子节点”情况时，它没有再次递归调用 `deleteNode`​，而是用 `while` 循环找到位置后直接拼接，代码效率在某些场景下不错。

  **缺点（需要注意的地方）：**

  - **树的平衡性破坏**：  
    这种方法把原来的左子树直接挂到了右子树的最底下。这会增加树的 **深度**。如果频繁删除，树可能会变得越来越“瘦长”，导致后续查找效率从  退化。
  - *对比*：另一种标准解法（交换值法）是把右子树最小节点的值**复制**给当前节点，然后递归删掉那个最小节点，这样能更好地保持树原本的形态。

  ### 总结

  作为面试或者理解 BST 删除逻辑的入门，这个解法是 **完美** 的。它清晰地展示了 BST 删除节点时如何保持“左小右大”的性质（即：把比根小的左子树，挂在比根大的右子树中最小的那个节点下面）。

# 解法二：

```java

```

- 这段代码是 BST 删除节点的另一种\*\*更通用、更“正统”\*\*的解法（通常被称为 **Hibbard Deletion** 的变体）。

  相比于上一版（“整体搬迁”法），这一版采用的是  **“偷天换日”（值交换）**  的策略。

  ### 核心区别

  - ​**上一版（搬迁法）** ：把整个左子树搬到右子树最小节点的下面。

    - ​*后果*：会改变树的拓扑结构，容易让树变得“很高很瘦”（不平衡）。
  - ​**这一版（交换法）** ：保留当前节点的位置，只修改它的值，然后去下面把那个提供值的节点删掉。

    - ​*后果*​：​**更好地保留了树原本的形态**，不会大幅增加树的高度。

  ---

  ### 代码逻辑详解

  重点看 `else`​ 分支中处理  **“左右都有孩子”**  的逻辑：

  Java

  ```
  /* 情况 3: 左右都有孩子 */
  else {
      // 1. 也是先判空，类似上一版（但这部分其实可以合并到下面的逻辑，不过这样写更清晰）
      if (root.left == null) return root.right;
      if (root.right == null) return root.left;
      
      // 2. 寻找继任者 (Successor)
      // 也就是右子树中最小的那个节点
      TreeNode tmp = root.right;
      while (tmp.left != null) {
          tmp = tmp.left;
      }
      
      // 3. 【核心操作】 偷天换日 (Value Swap)
      // 我们不删除 root 这个节点对象本身，而是把它的值改成继任者的值。
      // 现在，root 变成了 继任者，但是树里出现了两个值为 tmp.val 的节点。
      root.val = tmp.val;
      
      // 4. 【递归删除】
      // 既然 root 的值已经变了，旧的 root 算是“消失”了。
      // 我们现在的任务变成了：去右子树里，把那个原本的 tmp 节点删掉。
      root.right = delete(root.right, tmp.val);
  }
  ```

  ### 图解对比

  假设要删除根节点 ​**5**：

  Plaintext

  ```
        5
       / \
      3   7
         / \
        6   8
  ```

  **上一版（搬迁法）的做法：**

  找到右边最小的 6。把左子树 (3) 整个挂到 6 的左边。

  Plaintext

  ```
        7
       / \
      6   8
     /
    3
  ```

  ​*结果*：树变深了，结构变了。

  **这一版（交换法）的做法：**

  1. 找到右边最小的 ​**6**。
  2. 把根节点的 5 改成 6。

  Plaintext

  ```
        6  <-- 变成了6
       / \
      3   7
         / \
        6   8  <-- 这里还有一个6
  ```

  3. 递归调用 `delete(root.right, 6)`，把下面那个 6 删掉（它是叶子节点，很容易删）。

  Plaintext

  ```
        6
       / \
      3   7
           \
            8
  ```

  ​*结果*​：树的结构基本没动，只是原本的 5 变成了 6，下面的 6 没了。**这更加稳定。**

  ### 总结

  - ​**工程首选**：这一版（交换法）。因为它对树的平衡性破坏较小。AVL 树或红黑树的删除操作，底层也是基于这个“值交换”逻辑进行的。
  - ​**理解首选**：上一版（搬迁法）。因为它不需要再次递归，逻辑是一条线走到底的指针操作，更容易脑补。

  您的这段代码写得非常好，逻辑严密，是标准的教科书式写法！

‍

# 递归法：

```java
class Solution {
  public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null){
      return null;
    }
    //寻找对应的对应的前面的节点，以及他的前一个节点
    TreeNode cur = root;
    TreeNode pre = null;
    while (cur != null){
      if (cur.val < key){
        pre = cur;
        cur = cur.right;
      } else if (cur.val > key) {
        pre = cur;
        cur = cur.left;
      }else {
        break;
      }
    }
    if (pre == null){
      return deleteOneNode(cur);
    }
    if (pre.left !=null && pre.left.val == key){
      pre.left = deleteOneNode(cur);
    }
    if (pre.right !=null && pre.right.val == key){
      pre.right = deleteOneNode(cur);
    }
    return root;
  }

  public TreeNode deleteOneNode(TreeNode node){
    if (node == null){
      return null;
    }
    if (node.right == null){
      return node.left;
    }
    TreeNode cur = node.right;
    while (cur.left !=null){
      cur = cur.left;
    }
    cur.left = node.left;
    return node.right;
  }}
```

- 这段代码写得​**非常扎实**！

  这是上一版“整体搬迁法（Grafting/Move Strategy）”的​**迭代（Iterative）版本**。

  它结合了两个优点：

  1. ​**迭代搜索**​：利用 `while` 循环寻找节点，避免了递归查找带来的系统栈开销，空间复杂度降到了极致的 $O(1)$。
  2. ​**逻辑解耦**​：将“查找节点”和“删除动作”拆分开，`deleteOneNode` 专门负责处理那三种删除情况，结构很清晰。

  ### 代码逻辑分析

  #### 1. 查找逻辑（主函数 `deleteNode`）

  这一部分写得很像链表操作，使用了经典的 ​**双指针法 (**​**​`cur`​**​ **和** **​`pre`​**​ **)** 。

  - ​**​`pre`​**​ **的作用**​：因为是单向树，当 `cur`​ 找到目标时，我们需要操作它的 **父节点** 才能把它移除。所以 `pre`​ 紧跟在 `cur` 后面。
  - ​**根节点特判**​：`if (pre == null)`​ 这一步处理得很关键。如果循环结束后 `pre`​ 还是空，说明要删的是 ​**根节点**，此时没有父节点，直接调用删除函数并返回新根。
  - ​**父子相认**：  
    Java

    ```
    // 你的写法：通过值重新确认是左孩子还是右孩子
    if (pre.left !=null && pre.left.val == key) ...
    ```
    这里其实可以稍微优化（见下文建议），但逻辑是完全正确的。

  #### 2. 删除逻辑（辅助函数 `deleteOneNode`）

  这部分逻辑和您最早发的那个递归版本（解法1）的核心逻辑 ​**完全一致**。

  - ​**情况 A**​：`node.right == null`。直接返回左孩子（包含叶子节点情况）。
  - ​**情况 B**：左右都有。

    1. ​`TreeNode cur = node.right;` (进入右子树)
    2. ​`while (cur.left != null)...` (找到右子树最小节点)
    3. ​`cur.left = node.left;` (把旧的左子树嫁接到右边最小节点的左下角)
    4. ​`return node.right;` (右孩子上位)

  ---

  ### 两点小小的优化建议

  您的代码已经通过了 LeetCode，功能上没有问题。这里有两个细节可以让代码更严谨、更高效：

  #### 建议 1：处理“找不到 Key”的情况

  虽然您的代码在 key 不存在时不会报错（因为下面的 `if`​ 进不去，最后返回 `root`），但最好显式判断一下，逻辑更清晰：

  Java

  ```
  while (cur != null) {
      // ... 你的查找逻辑 ...
      if (cur.val == key) break; // 找到了，跳出
  }
  // 【建议新增】：如果跑完循环 cur 还是 null，说明没找到，直接返回
  if (cur == null) {
      return root; 
  }
  ```
  #### 建议 2：利用指针直接判断方向

  在循环结束后，`cur`​ 就是要删除的节点，`pre`​ 是它的父节点。我们不需要再比对一次 `val`​，直接比对 **对象地址** 即可：

  Java

  ```
  // 你的写法
  if (pre.left !=null && pre.left.val == key) ...

  // 优化写法：直接判断 cur 是 pre 的左还是右
  if (pre.left == cur) {
      pre.left = deleteOneNode(cur);
  } else if (pre.right == cur) {
      pre.right = deleteOneNode(cur);
  }
  ```
  ### 最终优化后的代码概览

  Java

  ```
  class Solution {
      public TreeNode deleteNode(TreeNode root, int key) {
          if (root == null) return null;
          
          TreeNode cur = root;
          TreeNode pre = null;
          
          // 1. 迭代查找
          while (cur != null) {
              if (cur.val == key) break; // 找到了
              pre = cur;
              if (cur.val < key) {
                  cur = cur.right;
              } else {
                  cur = cur.left;
              }
          }
          
          // 2. 没找到的情况
          if (cur == null) return root;
          
          // 3. 删除的是根节点
          if (pre == null) {
              return deleteOneNode(cur);
          }
          
          // 4. 删除的是普通节点 (利用 cur 指针直接判断)
          if (pre.left == cur) {
              pre.left = deleteOneNode(cur);
          } else {
              pre.right = deleteOneNode(cur);
          }
          return root;
      }

      // 这个辅助函数逻辑完美，保持不变
      public TreeNode deleteOneNode(TreeNode node) {
          if (node == null) return null;
          if (node.right == null) return node.left;
          
          TreeNode cur = node.right;
          while (cur.left != null) {
              cur = cur.left;
          }
          cur.left = node.left;
          return node.right;
      }
  }
  ```
  ### 总结

  这是 **空间复杂度最优 (**​**$O(1)$**​ **)**  的解法。

  - 如果您在面试中被要求“不能使用递归”或者“优化空间”，这就是满分答案。
  - 它的删除策略是“整体搬迁”，虽然可能增加树高，但代码编写难度适中，非常实用。
