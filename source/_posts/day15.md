---
title: day 15
date: '2026-01-20 19:32:13'
updated: '2026-01-20 20:18:17'
permalink: /post/day-15-zov3ve.html
comments: true
toc: true
---



# day 15

# **第六章  二叉树part03**

迭代法，大家可以直接过，二刷有精力的时候 再去掌握迭代法。

### 110.平衡二叉树 （优先掌握递归）

再一次涉及到，什么是高度，什么是深度，可以巩固一下。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html](https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html)</u>

### 257. 二叉树的所有路径 （优先掌握递归)

这是大家第一次接触到回溯的过程， 我在视频里重点讲解了 本题为什么要有回溯，已经回溯的过程。

如果对回溯 似懂非懂，没关系， 可以先有个印象。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html](https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html)</u>

### 404.左叶子之和 （优先掌握递归）

其实本题有点文字游戏，搞清楚什么是左叶子，剩下的就是二叉树的基本操作。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html](https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html)</u>

### 222.完全二叉树的节点个数（优先掌握递归）

需要了解，普通二叉树 怎么求，完全二叉树又怎么求

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html](https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html)</u>

‍

# 110.平衡二叉树 （优先掌握递归）

[110. 平衡二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/balanced-binary-tree/solutions/2015068/ru-he-ling-huo-yun-yong-di-gui-lai-kan-s-c3wj/)

## 写法一:

```java
private int getHeight(TreeNode node) {
    // 1. 递归终止条件（Base Case）
    // 空节点的高度为 0，且空树本身是平衡的。
    if (node == null) {
        return 0;
    }

    // 2. 递归计算左子树高度
    // 这里使用的是后序遍历（左右根），先处理子节点。
    int leftH = getHeight(node.left);
    
    // 3. 递归计算右子树高度
    int rightH = getHeight(node.right);

    // 4. 核心判断逻辑（剪枝与标记）
    // - leftH == -1: 说明左子树已经不平衡了，直接向上返回 -1。
    // - rightH == -1: 说明右子树已经不平衡了，直接向上返回 -1。
    // - Math.abs(leftH - rightH) > 1: 说明当前节点的左右子树高度差超过 1，当前节点也不平衡。
    if (leftH == -1 || rightH == -1 || Math.abs(leftH - rightH) > 1) {
        return -1; // 标记为不平衡
    }

    // 5. 如果通过了上面的检查，说明当前子树是平衡的
    // 返回当前节点的真实高度：左右子树最大高度 + 1（节点本身）
    return Math.max(leftH, rightH) + 1;
}
```

# 写法二（提前减枝）

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) != -1;
    }

    private int getHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        int leftH = getHeight(node.left);
        if (leftH == -1) {
            return -1; // 提前退出，不再递归，提前减枝
        }
        int rightH = getHeight(node.right);
        if (rightH == -1 || Math.abs(leftH - rightH) > 1) {
            return -1;
        }
        return Math.max(leftH, rightH) + 1;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/balanced-binary-tree/solutions/2015068/ru-he-ling-huo-yun-yong-di-gui-lai-kan-s-c3wj/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 257. 二叉树的所有路径 （优先掌握递归)

[257. 二叉树的所有路径 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-paths/)

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        dfs(root, "", ans);
        return ans;
    }

    // 把 String 改成 StringBuilder 更快，见右边的【Java 写法二】
    private void dfs(TreeNode node, String path, List<String> ans) {
        if (node == null) {
            return;
        }
        path += node.val;
        if (node.left == null && node.right == null) { // 叶子节点
            ans.add(path);
            return;
        }
        path += "->";
        dfs(node.left, path, ans);
        dfs(node.right, path, ans);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/binary-tree-paths/solutions/3038189/liang-chong-xie-fa-lu-jing-wei-can-shu-h-q2wz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 写法二：

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        dfs(root, new StringBuilder(), ans);
        return ans;
    }

    private void dfs(TreeNode node, StringBuilder path, List<String> ans) {
        if (node == null) {
            return;
        }
        int len = path.length(); // 记录当前长度，用于回溯
        path.append(node.val);
        if (node.left == null && node.right == null) { // 叶子节点
            ans.add(path.toString());
        } else {
            path.append("->");
            dfs(node.left, path, ans);
            dfs(node.right, path, ans);
        }
        path.setLength(len); // 回溯，恢复现场
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/binary-tree-paths/solutions/3038189/liang-chong-xie-fa-lu-jing-wei-can-shu-h-q2wz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这是一段非常清晰的 **DFS（深度优先搜索）**  代码，采用的是  **“前序遍历”**  的思路（根 -> 左 -> 右）。

这段代码最巧妙的地方在于它利用了 Java 中 **​`String`​**​ **的不可变性（Immutability）**  来简化回溯逻辑。

以下是详细的代码逻辑与设计思路分析：

### 1. 核心逻辑：携带路径向下走 (Top-Down)

不同于上一题“计算高度”是把值从下往上传（Bottom-Up），这道题是把当前的路径信息 `path`​ 作为参数，一层一层**从上往下传**。

**代码流程拆解：**

1. **当前层要做的事：**   
   ​`path += node.val;`​  
   把当前节点的值拼接到路径字符串的末尾。此时 `path` 变成了 "之前的路径 + 当前节点"。
2. **判断是否到终点（叶子节点）：**

```java
if (node.left == null && node.right == null) {
    ans.add(path); // 找到一条完整路径，加入结果集
    return;
}

```

如果是叶子节点，说明这条路走到底了，保存并结束当前分支。  
3. **准备下一层：**   
​`path += "->";`​  
如果不是叶子节点，说明后面还有节点，需要加上箭头。  
4. **递归进入下一层：**   
​`dfs(node.left, path, ans);`​  
​`dfs(node.right, path, ans);`​  
带着最新的 `path` 继续往下找。

### 2. 难点解析：为什么不需要“显式回溯”？

通常在使用 DFS 寻找路径时（特别是使用 `List`​ 或 `StringBuilder` 时），我们需要手动“弹出”最后一个元素，这叫“回溯” (Backtracking)。但在本解法中，你看不到任何“删除/撤销”操作。

**原因在于** **​`String`​**​ **是不可变的。**

- **传值机制：**  当执行 `dfs(node.left, path, ans)`​ 时，传入的是当前 `path`​ 的一个**副本**（或者说引用的值）。
- **互不干扰：**
- 左子树递归时，它会在**它自己的** `path`​ 变量上拼接东西，这不会影响到当前层（父节点）的 `path` 变量。
- 当左子树递归结束返回时，当前层的 `path` 依然保持着调用左子树之前的样子（即包含 "根->" 的状态）。
- 紧接着调用 `dfs(node.right, path, ans)`​，右子树拿到的依然是干净的、未被左子树污染的 `path`。

**隐式回溯示例：**   
假设树是 `root(1)`​ -> `left(2)`​, `right(3)`。

1. **Root:**  path = "1->"。传入左边。
2. **Left:**  接收到 "1->"。变成 "1->2"。是叶子，保存。**结束返回**。
3. **Root:**  此时 Root 这一层的 `path`​ 变量**依然是 "1->"** （并没有变成 "1->2"）。
4. **Right:**  Root 将 "1->" 传入右边。
5. **Right:**  接收到 "1->"。变成 "1->3"。保存。

### 3. 代码注释中的“StringBuilder 更快”是指什么？

作者灵神在注释里提到了：

> ​`// 把 String 改成 StringBuilder 更快`

**原因：**

- **String 的代价：**  每次执行 `path += ...`​，Java 实际上都会创建一个**全新**的 String 对象（涉及内存分配和字符数组拷贝）。在节点很多、路径很长时，这会产生大量的临时对象，增加 GC（垃圾回收）压力，且字符串拼接的时间复杂度较高。
- **StringBuilder 的优势：**  `StringBuilder` 是可变的，所有操作都在同一个对象上进行。
- **StringBuilder 的代价（需要显式回溯）：**  如果用 `StringBuilder`​，因为大家共用一个对象，左子树改了 `sb`​，右子树就会看到改后的结果。所以必须在递归返回后，**手动删除**刚刚添加的字符（回溯）。

### 4. 总结

|特性|本解法 (String)|优化解法 (StringBuilder)|
| ------| ------------------------------------| --------------------------|
|**代码复杂度**|**低** (无需手动回溯，逻辑简单)|**中** (需要手动 `setLength` 回溯)|
|**内存/时间效率**|**较低** (产生大量临时 String 对象)|**高** (复用同一个对象)|
|**适用场景**|面试手写（不易出错）、节点数较少时|追求极致性能、工业级代码|

**一句话评价：**   
这是一个非常适合**面试手写**的解法，因为它利用 Java 语言特性规避了复杂的“回溯”逻辑，代码极其简洁，逻辑一目了然。

**你想看看如果要用** **​`StringBuilder`​**​ **来实现“显式回溯”，代码应该长什么样吗？**

# 回溯写法

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        List<String> path = new ArrayList<>();
        dfs(root, ans, path);
        return ans;
    }

    private void dfs(TreeNode node, List<String> ans, List<String> path) {
        if (node == null) {
            return;
        }
        path.add(String.valueOf(node.val));
        if (node.left == null && node.right == null) { // 叶子节点
            ans.add(String.join("->", path));
        } else {
            dfs(node.left, ans, path);
            dfs(node.right, ans, path);
        }
        // 恢复现场，撤销上面的 path.add(String.valueOf(node.val));
        path.removeLast(); // Java 旧版本请用 path.remove(path.size() - 1);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/binary-tree-paths/solutions/3038189/liang-chong-xie-fa-lu-jing-wei-can-shu-h-q2wz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是灵神提供的**第二种写法**，也是最标准的**回溯法（Backtracking）** 模板。

  如果说上一版代码是利用 `String`​ 的不可变性“偷懒”避免了回溯，那么这一版则是**显式回溯**的教科书级演示。

  ### 1. 核心差异：可变容器 `List<String> path`

  这里不再传递一个每次都全新的 `String`​，而是整棵树的遍历过程**共用同一个** `ArrayList` 对象。

  - **好处：**  节省内存。不需要在每个节点都创建新的字符串副本。
  - **代价：**  必须手动维护“现场”。既然大家共用一个列表，你用完了必须把它清理干净，否则会影响后续的操作。

  ### 2. 什么是“恢复现场”（Backtracking）？

  让我们用一个生活中的例子来理解 `path.add`​ 和 `path.removeLast`：

  > **场景：**  你是一个探险家（递归函数），手里拿着一个笔记本（`path` List），你要探索一个迷宫（二叉树）。
  >
  > 1. **进房间（递归前）：**  你走进一个房间（节点），把房间号写在笔记本上（`path.add`）。
  > 2. **探索（递归中）：**  你继续往深处走，去下一个房间...
  > 3. **离开房间（递归后）：**  当你探索完这个分支，准备退回到上一个房间时，你**必须把笔记本上刚刚写的那个房间号擦掉**（`path.removeLast`）。
  >

  **如果你不擦掉会发生什么？**   
  假设树是 `1 -> 2`​ 和 `1 -> 3`​。  
  如果不擦掉 `2`​，当你回退到 `1`​ 再去访问 `3`​ 时，笔记本上会写着 `[1, 2, 3]`​。但这显然是错的，因为 `2`​ 和 `3` 并不是父子关系，它们是兄弟，不应该出现在同一条路径里。

  ### 3. 代码执行流程图解

  假设二叉树如下：

  ```text
      1
     / \
    2   3

  ```

  1. **DFS(1):**  `path`​ = `[1]`。不是叶子。
  2. **DFS(2):**  `path`​ = `[1, 2]`。是叶子。

  - 生成结果 `"1->2"`​ 加入 `ans`。
  - 函数即将结束。
  - **执行 removeLast:**  `path`​ 变回 `[1]`​。 **（关键！恢复了！）**
  - DFS(2) 结束返回。

  3. 回到 **DFS(1):**  此时 `path`​ 是 `[1]`。继续执行右分支。
  4. **DFS(3):**  `path`​ = `[1, 3]`。是叶子。

  - 生成结果 `"1->3"`​ 加入 `ans`。
  - **执行 removeLast:**  `path`​ 变回 `[1]`。
  - DFS(3) 结束返回。

  5. 回到 **DFS(1):**  左右都走完了。

  - **执行 removeLast:**  `path`​ 变回 `[]`。
  - DFS(1) 结束。

  ### 4. 性能分析

  - **时间复杂度：**  。虽然避免了中间节点的字符串拼接，但在叶子节点使用 `String.join`​ 依然需要遍历路径长度 。但整体上，Java 的 `ArrayList`​ 操作通常比频繁创建 `String` 对象要快。
  - **空间复杂度：**  （递归栈 + 路径列表）。相比上一版，减少了大量的临时 `String` 对象，对垃圾回收（GC）更友好。

  ### 5. 总结与对比

  |特性|写法一 (String 参数)|写法二 (List 参数 + 回溯)|
  | ------| ---------------------------| ---------------------------|
  |**核心机制**|利用不可变性，隐式回溯|修改共享变量，显式回溯|
  |**代码难度**|简单，无需手动撤销|稍难，忘记 `removeLast` 会出错|
  |**内存开销**|高 (产生大量 String 垃圾)|**低 (复用同一个 List)**|
  |**适用场景**|简单路径问题，字符串较短|**复杂回溯问题 (如求所有排列/组合)**|

  **编程思维升华：**   
  这一版代码是通往  **“回溯算法”**  大门的钥匙。如果你能理解这里的 `add`​ 和 `removeLast`​ 成对出现的原因，那么你以后解决 **LeetCode 113. 路径总和 II** 或者 **全排列** 等问题时，就会发现它们的代码结构和这个是一模一样的。

  **想试一试“路径总和 II”吗？那道题要求你返回所有路径（List<List<Integer>>），而不是字符串，那里必须使用这种 List + 回溯的方法。**

# 404.左叶子之和 （优先掌握递归）

##### [404. 左叶子之和 - 力扣（LeetCode）](https://leetcode.cn/problems/sum-of-left-leaves/description/)

```java
class Solution {
    private int ans = 0;

    public int sumOfLeftLeaves(TreeNode root) {
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode node) {
        if (node == null) {
            return;
        }

        // 递归，寻找左叶子
        dfs(node.left);
        dfs(node.right);

        TreeNode left = node.left; // 当前节点的左儿子
        if (left != null && left.left == null && left.right == null) { // 当前节点的左儿子是叶子
            ans += left.val; // 累加节点值
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/sum-of-left-leaves/solutions/3751068/jian-dan-ti-jian-dan-zuo-pythonjavaccgoj-w8st/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

‍

# 写法二

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) { // 空节点
            return 0;
        }

        // 递归左右子树，收集左右子树中的「节点的左儿子是叶子」的叶子节点值之和
        int sum = sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);

        TreeNode left = root.left; // 当前节点的左儿子
        if (left != null && left.left == null && left.right == null) { // 当前节点的左儿子是叶子
            sum += left.val; // 累加节点值
        }

        return sum;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/sum-of-left-leaves/solutions/3751068/jian-dan-ti-jian-dan-zuo-pythonjavaccgoj-w8st/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这是一个非常典型的 **DFS（深度优先搜索）**  题目，但灵神在这里采用了一个非常关键的视角：​ **“父节点视角”** 。

让我们来拆解一下为什么这段代码要这样写，以及它巧妙在哪里。

### 1. 核心难点：节点“不知道”自己是谁

在二叉树遍历中，当你递归访问到一个节点（比如 `node`​）时，这个 `node` 只知道：

1. 我是谁（`val`）。
2. 我的孩子是谁（`left`​, `right`）。

但是，node 不知道自己是父节点的左孩子还是右孩子。

如果你只判断 if (node.left \=\= null && node.right \=\= null)，你只能确定它是一个叶子，却无法确定它是左叶子。

### 2. 解决方案：站在父节点的肩膀上

为了解决这个问题，灵神的这段代码采用了  **“父节点检查子节点”**  的策略。

我们不问当前节点“你是左叶子吗？”，而是问当前节点： **“你的左孩子是叶子吗？”**

### 3. 代码逻辑逐行解析

Java

```
private void dfs(TreeNode node) {
    if (node == null) {
        return;
    }

    // 1. 遍历整棵树
    // 无论是左子树还是右子树，都要进去看看，因为右子树里也可能藏着左叶子
    dfs(node.left);
    dfs(node.right);

    // 2. 核心判断逻辑（父节点视角）
    TreeNode left = node.left; // 拿到左孩子
    
    // 判断三连：
    // (1) left != null: 首先得有左孩子
    // (2) left.left == null: 左孩子的左边是空的
    // (3) left.right == null: 左孩子的右边是空的
    // (2) + (3) 说明这个左孩子就是一个“叶子节点”
    if (left != null && left.left == null && left.right == null) { 
        ans += left.val; // 找到了！累加它的值
    }
}
```

### 4. 另一种思路（对比增强理解）

除了这种“父节点视角”，还有一种常见的写发是 “传参标记法”。

即在递归时多传一个布尔值参数 boolean isLeft：

Java

```
// 伪代码示例
void dfs(TreeNode node, boolean isLeft) {
    if (node == null) return;
    // 如果我是叶子，且我是从左边传下来的
    if (node.left == null && node.right == null && isLeft) {
        ans += node.val;
    }
    dfs(node.left, true);  // 往左走，标记为 true
    dfs(node.right, false); // 往右走，标记为 false
}
```

- **传参法**是让节点知道“我来自哪里”。
- \*\*灵神的写法（父节点视角）\*\*是让节点知道“我的左孩子是什么”。
- ​**灵神写法的优势**：不需要修改递归函数的签名（参数列表），代码看起来更加干净，直接利用了二叉树本身的结构特性。

### 5. 总结

|**关注点**|**描述**|||
| --| ----------------------------------------------------------------------------------| --| --|
|**遍历方式**|后序遍历（其实前中后序在这里不影响结果，只要遍历到每个节点即可）|||
|**判断时机**|在处理“父节点”的时候，去偷看它的“左孩子”|||
|**易错点**|很多人只判断了`node`​是叶子，忘了判断它是`left`；或者只遍历左边，忘了右子树里也有左叶子。|||

> 编程思维提炼：
>
> 在树的问题中，如果处理当前节点的信息不足以做出判断（例如不知道父子关系），试着退后一步，站在父节点的角度去审视子节点。

接下来：

你已经掌握了计算高度、路径遍历、以及左叶子之和。我们要不要尝试挑战一下 “路径总和” (Path Sum) 系列的问题？它需要结合我们之前学过的“递归传值”技巧。

‍

# 222.完全二叉树的节点个数（优先掌握递归）

```java
class Solution {
    // 通用递归解法
    public int countNodes(TreeNode root) {
        if(root == null) {
            return 0;
        }
        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}

class Solution {
    // 迭代法
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int result = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            while (size -- > 0) {
                TreeNode cur = queue.poll();
                result++;
                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
        }
        return result;
    }
}

class Solution {
    /**
     * 针对完全二叉树的解法
     *
     * 满二叉树的结点数为：2^depth - 1
     */
    public int countNodes(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftDepth = getDepth(root.left);
        int rightDepth = getDepth(root.right);
        if (leftDepth == rightDepth) {// 左子树是满二叉树
            // 2^leftDepth其实是 （2^leftDepth - 1） + 1 ，左子树 + 根结点
            return (1 << leftDepth) + countNodes(root.right);
        } else {// 右子树是满二叉树
            return (1 << rightDepth) + countNodes(root.left);
        }
    }

    private int getDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            root = root.left;
            depth++;
        }
        return depth;
    }
}

作者：代码随想录
链接：https://leetcode.cn/problems/count-complete-tree-nodes/solutions/1812445/by-carlsun-2-bwlp/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这三段代码分别代表了解决“完全二叉树节点个数”问题的三个不同层次：

1. **通用递归（DFS）** ：最直观，但没利用题目特性。
2. **通用迭代（BFS）** ：层序遍历，也没利用特性。
3. **针对完全二叉树的优化解法**：**这是本题的精髓**，利用了完全二叉树和满二叉树的性质，将时间复杂度从  降低到了 。

下面我们重点分析第三种写法，因为它才是面试中考察的重点。

---

### 1. 前两种通用解法（暴力法）

前两种解法（递归 DFS 和 迭代 BFS）把这棵树当作一棵**普通的二叉树**来处理。

- **思路**：不管是递归还是用队列，本质都是把这  个节点挨个遍历一遍数数。
- **时间复杂度**：。
- **评价**：能通过，但没有拿到这道题作为“完全二叉树”题目的红利。

---

### 2. 第三种：针对完全二叉树的  解法

这种解法利用了完全二叉树的一个核心性质：**它的子树中，一定有一棵是“满二叉树”（Perfect Binary Tree）。**   
满二叉树的节点数是可以直接通过公式计算的：（ 是高度），不需要一个个去数。

#### 核心逻辑解析

代码的核心在于如何判断哪一边是“满”的：

```java
// 辅助函数：只往左走计算深度
// 因为完全二叉树的性质，最左边的深度就是该子树的最大深度
private int getDepth(TreeNode root) {
    int depth = 0;
    while (root != null) {
        root = root.left;
        depth++;
    }
    return depth;
}

```

主函数逻辑：

1. 计算**左子树**的深度 `leftDepth`。
2. 计算**右子树**的深度 `rightDepth`​。  
    *(注意：这里的深度都是指“最左路径”的长度)*

#### 情况一：`leftDepth == rightDepth`

这意味着**左子树一定是满二叉树**。

- **为什么？**   
  如果左子树的最左边深度是 ，而右子树的最左边深度也是 ，说明左子树的最后一层已经被填满了（否则右子树不可能开始填第  层）。
- **怎么算？**
- **左子树节点数**（满二叉树公式）：
- **根节点**：
- **右子树**：不知道是不是满的，递归去算 `countNodes(root.right)`
- **总数**：
- **代码对应**：`(1 << leftDepth) + countNodes(root.right)`​  
   *(位运算* *​`1 << k`​*​ *等价于 )*

#### 情况二：`leftDepth != rightDepth`​ (即 `left > right`)

这意味着**右子树一定是满二叉树**（只是比左边稍微矮一层）。

- **为什么？**   
  如果左边深度比右边大，说明最后一层还没填到右子树去，右子树在上一层就已经截止了，且是一个标准的倒数第二层的满二叉树。
- **怎么算？**
- **右子树节点数**（满二叉树公式）：
- **根节点**：
- **左子树**：不确定，递归去算 `countNodes(root.left)`
- **总数**：
- **代码对应**：`(1 << rightDepth) + countNodes(root.left)`

---

### 3. 复杂度对比

|方法|时间复杂度|空间复杂度|说明|
| ------| ------------| ------------| --------------------------|
|**方法一 (DFS)**|||必须访问每一个节点|
|**方法二 (BFS)**|||队列需要存储一层叶子节点|
|**方法三 (优化版)**|****||**最优解**|

**为什么是 ？**

- 每次递归，我们都丢弃了一半的树（因为那一半直接用公式算出来了），只递归另一半。
- 我们总共递归  次（树的高度）。
- 在每一层递归中，我们调用了 `getDepth`，这个函数的复杂度也是 。
- 总耗时：。

### 总结

这份代码随想录的解法非常经典。

- 它教会我们：**不要盲目遍历**。
- 如果是**完全二叉树**，利用其“子树必有满二叉树”的特性，结合**位运算**，可以极大地加速计数过程。

**下一步建议：**   
既然你已经掌握了完全二叉树的计数，要不要尝试一下 **二叉搜索树 (BST)**  的特性应用？比如 **LeetCode 98. 验证二叉搜索树**，看看如何利用 BST 的“中序遍历有序”特性来解题？
