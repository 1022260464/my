---
title: day22
date: '2026-01-29 18:01:09'
updated: '2026-01-29 18:51:42'
permalink: /post/day22-212z4l.html
comments: true
toc: true
---



# day22

# 第七章 回溯算法part01

### 理论基础

其实在讲解二叉树的时候，就给大家介绍过回溯，这次正式开启回溯算法，大家可以先看视频，对回溯算法有一个整体的了解。

题目链接/文章讲解：<u>[https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1cy4y167mM](https://www.bilibili.com/video/BV1cy4y167mM)</u>

### 77. 组合

对着 在 回溯算法理论基础 给出的 代码模板，来做本题组合问题，大家就会发现 写回溯算法套路。

在回溯算法解决实际问题的过程中，大家会有各种疑问，先看视频介绍，基本可以解决大家的疑惑。

本题关于剪枝操作是大家要理解的重点，因为后面很多回溯算法解决的题目，都是这个剪枝套路。

题目链接/文章讲解：<u>[https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html](https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1ti4y1L7cv](https://www.bilibili.com/video/BV1ti4y1L7cv)</u>

剪枝操作：<u>[https://www.bilibili.com/video/BV1wi4y157er](https://www.bilibili.com/video/BV1wi4y157er)</u>

### 216.组合总和III

如果把 组合问题理解了，本题就容易一些了。

题目链接/文章讲解：<u>[https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html](https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1wg411873x](https://www.bilibili.com/video/BV1wg411873x)</u>

###  17.电话号码的字母组合

本题大家刚开始做会有点难度，先自己思考20min，没思路就直接看题解。

题目链接/文章讲解：<u>[https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html](https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html)</u>

视频讲解：<u>[https://www.bilibili.com/video/BV1yV4y1V7Ug](https://www.bilibili.com/video/BV1yV4y1V7Ug)</u>

# 理论基础：

### 回溯法解决的问题

回溯法，一般可以解决如下几种问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等

**相信大家看着这些之后会发现，每个问题，都不简单！**

另外，会有一些同学可能分不清什么是组合，什么是排列？

​**组合是不强调元素顺序的，排列是强调元素顺序**。

例如：{1, 2} 和 {2, 1} 在组合上，就是一个集合，因为不强调顺序，而要是排列的话，{1, 2} 和 {2, 1} 就是两个集合了。

记住组合无序，排列有序，就可以了。

### [#](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%A6%82%E4%BD%95%E7%90%86%E8%A7%A3%E5%9B%9E%E6%BA%AF%E6%B3%95)如何理解回溯法

​**回溯法解决的问题都可以抽象为树形结构**，是的，我指的是所有回溯法的问题都可以抽象为树形结构！

因为回溯法解决的都是在集合中递归查找子集，​**集合的大小就构成了树的宽度，递归的深度就构成了树的深度**。

递归就要有终止条件，所以必然是一棵高度有限的树（N叉树）。

这块可能初学者还不太理解，后面的回溯算法解决的所有题目中，我都会强调这一点并画图举相应的例子，现在有一个印象就行。

### [#](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%A8%A1%E6%9D%BF)回溯法模板

这里给出Carl总结的回溯算法模板。

在讲[二叉树的递归 (opens new window)](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)中我们说了递归三部曲，这里我再给大家列出回溯三部曲。

- 回溯函数模板返回值以及参数

在回溯算法中，我的习惯是函数起名字为backtracking，这个起名大家随意。

回溯算法中函数返回值一般为void。

再来看一下参数，因为回溯算法需要的参数可不像二叉树递归的时候那么容易一次性确定下来，所以一般是先写逻辑，然后需要什么参数，就填什么参数。

但后面的回溯题目的讲解中，为了方便大家理解，我在一开始就帮大家把参数确定下来。

回溯函数伪代码如下：

```text
void backtracking(参数)
```

- 回溯函数终止条件

既然是树形结构，那么我们在讲解[二叉树的递归 (opens new window)](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)的时候，就知道遍历树形结构一定要有终止条件。

所以回溯也有要终止条件。

什么时候达到了终止条件，树中就可以看出，一般来说搜到叶子节点了，也就找到了满足条件的一条答案，把这个答案存放起来，并结束本层递归。

所以回溯函数终止条件伪代码如下：

```text
if (终止条件) {
    存放结果;
    return;
}
```

- 回溯搜索的遍历过程

在上面我们提到了，回溯法一般是在集合中递归搜索，集合的大小构成了树的宽度，递归的深度构成的树的深度。

```java


void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}

```

# 77. 组合：

[77. 组合 - 力扣（LeetCode）](https://leetcode.cn/problems/combinations/description/)

## 方法一：枚举下一个数选哪个

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(n, k, path, ans); // 从 n 开始倒着枚举，这样 dfs 中就不需要 n 了，少传一个参数
        return ans;
    }

    // 枚举选哪个：在 1 到 i 中选一个数，加到 path 末尾
    private void dfs(int i, int k, List<Integer> path, List<List<Integer>> ans) {
        int d = k - path.size(); // 还要选 d 个数
        if (d == 0) { // 选好了
            ans.add(new ArrayList<>(path));
            return;
        }

        // 枚举的数不能太小，否则后面没有数可以选
        for (int j = i; j >= d; j--) {
            path.add(j);
            dfs(j - 1, k, path, ans);
            path.removeLast(); // path.remove(path.size() - 1);
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combinations/solutions/2071017/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-65lh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是“灵茶山艾府”（0x3F）大佬非常经典的\*\*回溯（Backtracking）\*\*模板代码。他的代码风格以简洁、极致优化著称。

  这段代码的核心在于\*\*“倒序枚举”​**加上了**​“极致剪枝”\*\*。我来为你拆解其中的奥妙，特别是那个让初学者摸不着头脑的 `j >= d`。

  ---

  ### 1. 核心思路图解

  这道题本质上是在一颗“决策树”上做深度优先搜索（DFS）。

  - **题目：**  从 $1$ 到 $n$ 中选 $k$ 个数。
  - **策略：**  我们倒着选（从大到小），比如从 $n$ 开始尝试。

    - 选了 $j$，那么下一个数只能在 $1$ 到 $j-1$ 之间选（保证不重复且有序）。
    - 不选 $j$，就看 $j-1$。

  ### 2. 这里的“剪枝” (`j >= d`) 是什么意思？

  这是这段代码的**灵魂**所在。

  Java

  ```
  // d = k - path.size(); // 还需要选 d 个数
  for (int j = i; j >= d; j--) { ... }
  ```

  普通的回溯可能会写成 `j >= 1`​（只要大于1都试一下）。但这里写的是 `j >= d`。

  **通俗解释（数人头原理）：**

  假设你还需要选 `d`​ 个数，而你当前手里最大能选的牌只有 `j` 了（意味着你只能从 $1, 2, ..., j$ 里选）。

  **如果** **​`j < d`​**​ **，就算你把** **$1$** **到** **$j$** **所有的牌全拿走，凑出来的数量也不够** **​`d`​**​ **个！**

  **举个例子：**

  假设 $n=4, k=4$（要在 4 个数里选 4 个）。

  - 一开始 `path`​ 是空，还需要选 `d=4` 个。
  - **第一轮循环：**

    - ​`j = 4`​：`4 >= 4`，条件成立。选了 4，剩下还需要选 3 个，去 $1,2,3$ 里找。​**可行**。
    - ​`j = 3`​：`3 >= 4`​，**条件不成立！循环直接结束！**

      - **为什么？**  因为如果你第一个数选了 3，剩下只有 $1, 2$ 可选。你总共最多只能凑出 $\{3, 2, 1\}$ 这 3 个数，永远凑不够 4 个。所以从 3 开始的分支完全不用看，直接剪掉。

  这就是​**剪枝**，它极大地减少了无效的搜索。

  ---

  ### 3. 代码细节逐行解析

  #### A. 为什么倒着枚举？(`dfs` 少传一个参数)

  Java

  ```
  dfs(n, k, path, ans); 
  // 签名：dfs(int i, ...)
  ```

  - **正序枚举（1 到 n）：**  你通常需要传 `startIndex`​（当前从哪开始）和 `n`​（最大到哪结束）。例如 `dfs(start, n, k, ...)`。
  - **倒序枚举（n 到 1）：**  我们只需要传当前最大能选的数 `i`。因为我们默认最小只能选到 1。

    - 当我们处理 `dfs(i)` 时，意思就是“请在 $1$ 到 $i$ 之间选”。
    - 下一层递归就是 `dfs(j-1)`，意思是“请在 $1$ 到 $j-1$ 之间选”。
    - 参数 `n` 不需要一直透传，代码更简洁。

  #### B. 递归终止条件

  Java

  ```
  int d = k - path.size(); // 还需要选 d 个
  if (d == 0) { ... }
  ```

  - 当还需要选的个数为 0 时，说明选够了。
  - ​`new ArrayList<>(path)`​：**注意！**  这里必须 `new`​ 一个新的 List。因为 `path` 是引用传递，如果不拷贝，后续的回溯操作（remove）会把结果集里的数据也删光，最后你会得到一堆空的 List。

  #### C. 回溯的“现场恢复”

  Java

  ```
  path.add(j);            // 1. 做选择：把 j 加进去
  dfs(j - 1, k, ...);     // 2. 递归：去 j-1 的范围里找剩下的
  path.removeLast();      // 3. 撤销选择：把 j 拿出来，换下一个数试试
  ```

  这是回溯的标准三部曲。注意 `removeLast()`​ 是 Java 21 引入的 `List`​ 接口方法（或者 `LinkedList`​ 的方法）。在老版本 Java 或 `ArrayList`​ 中，通常写成 `path.remove(path.size() - 1)`。

  ---

  ### 4. 总结

  这段代码好在哪里？

  1. **极简参数：**  利用倒序枚举，省去了 `start_index`​ 和 `n`​ 两个边界参数的维护，只用一个 `i` 代表当前可选范围的上界。
  2. **极致性能：**  `j >= d` 这一行代码，把所有不可能成功的路径在根部就切断了，运行速度比普通暴力回溯快得多。
  3. **逻辑清晰：**  每一行代码都没有废话。

  如果你面试能写出这种带剪枝的回溯，面试官通常会非常满意。

- **复杂度分析**

  时间复杂度：分析回溯问题的时间复杂度，有一个简易公式：路径长度×搜索树的叶子数。对于本题，路径长度始终为 k，叶子个数为 C(n,k)，所以时间复杂度为 O(k⋅C(n,k))。空间复杂度：O(k)。返回值不计入。

# 方法二：选或不选：

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(n, k, path, ans); // 从 n 开始倒着枚举，这样 dfs 中就不需要 n 了，少传一个参数
        return ans;
    }

    // 选或不选：讨论 i 是否加入 path
    private void dfs(int i, int k, List<Integer> path, List<List<Integer>> ans) {
        int d = k - path.size(); // 还要选 d 个数
        if (d == 0) { // 选好了
            ans.add(new ArrayList<>(path));
            return;
        }

        // 不选 i
        if (i > d) {
            dfs(i - 1, k, path, ans);
        }

        // 选 i
        path.add(i);
        dfs(i - 1, k, path, ans);
        path.removeLast(); // path.remove(path.size() - 1);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combinations/solutions/2071017/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-65lh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这也是灵茶山艾府大佬的代码，但它使用的是完全不同的回溯策略。

  上一段代码叫 **“枚举选哪个”** （用 `for`​ 循环），而这一段代码叫 **“选或不选”** （用 `if` 分支）。

  这种思路更像是在做一道道 **“是非题”** ：对于数字 `i`，我是要放入袋子？还是不要？

  以下是详细解析：

  ### 1. 核心逻辑：二叉树决策

  这就好比你在遍历每一个数字 `i`​（从 `n`​ 到 `1`），对于每一个数字，你只有两条路：

  1. **左路（不选** **​`i`​**​ **）：**  直接跳过，去看 `i-1`。
  2. **右路（选** **​`i`​**​ **）：**  把 `i`​ 放进包里，然后去看 `i-1`。

  因为每个节点只有 2 个分支（选 vs 不选），所以这棵递归树是一棵标准的**二叉树**。而上一段代码（for 循环）是一棵**多叉树**。

  ---

  ### 2. 这里的“剪枝” (`if (i > d)`) 是什么意思？

  这行代码非常关键，它出现在 **“不选”** 的分支里：

  ```java
  // 不选 i
  if (i > d) {
      dfs(i - 1, k, path, ans);
  }

  ```

  **通俗解释（库存 vs 需求）：**

  - ​**​`d`​**​  **(需求)：**  你还差 `d` 个数没选。
  - ​**​`i`​**​  **(库存)：**  你当前面前只剩下 `1`​ 到 `i`​ 这 `i` 个数字可以考虑了。

  **逻辑推导：**   
  如果你决定 **“不选”** 当前的数字 `i`​，那么你的库存就变少了 1 个（只剩下 `i-1`​ 个）。  
  你只有在  **“剩下的库存仍然足够满足需求”**  的时候，才有资格说“我不选”。

  - 如果 `i > d`​：意味着哪怕扔掉现在的 `i`​，剩下的 `i-1`​ 个数也比 `d`​ 多（或相等），够凑数的。**允许不选**。
  - 如果 `i == d`​：意味着剩下的数字刚好等于你缺的数字。你**必须**全选！**不允许不选**（所以这里没有 `else`​，如果 `i == d`，程序只会走下面的“选 i”逻辑）。

  **例子：**   
  还需要选 3 个数 (`d=3`​)，现在轮到数字 3 (`i=3`​)。  
  你敢不选 3 吗？不敢。因为如果不选 3，剩下只有 1 和 2，怎么也凑不够 3 个。所以这时候 `if (3 > 3)` 不成立，强制进入下方的“选 i”分支。

  ---

  ### 3. 代码对比：这两种写法哪种好？

  我们将这两种写法做一个对比：

  |特性|**枚举选哪个 (For 循环)**|**选或不选 (递归分支)**|
  | ------| ------------------------------------------------| -----------------------------------------------|
  |**思维模型**|**多叉树**：我在当前这一层，一次性把所有能选的都试一遍。|**二叉树**：我只盯着当前的数字 `i`，只思考它是留还是丢。|
  |**代码结构**|使用 `for` 循环横向遍历。|没有 `for`​ 循环，只有两条 `dfs` 调用。|
  |**剪枝方式**|在 `for`​ 循环的终止条件 `j >= d` 里剪枝。|在进入分支前的 `if (i > d)` 里剪枝。|
  |**适用场景**|**组合、排列**问题更直观。|**子集、背包**问题更直观。|

  ### 4. 详细流程演示 (Dry Run)

  假设 `n=4, k=2`。

  1. ​**​`dfs(4)`​** ​ **：**  还需要 2 个。

  - **不选 4？**  `4 > 2`​，可以。 -> 递归 `dfs(3)`（还需要 2 个）。
  - **选 4？**  加入 4。 -> 递归 `dfs(3)`（还需要 1 个）。

  2. **假设走了“不选 4” ->**  **​`dfs(3)`​** ​ **：**  还需要 2 个。

  - **不选 3？**  `3 > 2`​，可以。 -> 递归 `dfs(2)`。
  - **选 3？**  加入 3。 -> 递归 `dfs(2)`。

  3. **假设走了“不选 3” ->**  **​`dfs(2)`​** ​ **：**  还需要 2 个。

  - **不选 2？**  `2 > 2`​ **不成立！**  (库存只有 1,2，扔了 2 就剩一个 1，不够凑 2 个)。**这条路被剪枝了！**
  - **选 2？**  只能走这条路。加入 2。 -> 递归 `dfs(1)`（还需要 1 个）。

  ### 5. 总结

  这段代码虽然看起来没有 `for` 循环，但本质上是一样的。

  - **不选 i** = 相当于上一段代码中，`for`​ 循环还没轮到 `i`，继续往下看。
  - **选 i** = 相当于上一段代码中，`for`​ 循环选中了 `i`。

  **哪种写法更好？**   
  在面试中，**第一种（For 循环枚举）** 更为通用，因为它可以轻松适配“排列”问题（顺序重要）或“组合”问题。  
  **第二种（选或不选）** 是 0-1 背包问题的核心思想，对于理解动态规划（DP）非常有帮助。如果你想为以后学 DP 打基础，这段代码值得细细品味。

# 216.组合总和III：

[216. 组合总和 III - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iii/description/)

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>(k);
        dfs(9, n, k, ans, path); // 从 i=9 开始倒着枚举
        return ans;
    }

    private void dfs(int i, int leftSum, int k, List<List<Integer>> ans, List<Integer> path) {
        int d = k - path.size(); // 还要选 d 个数
        if (leftSum < 0 || leftSum > (i * 2 - d + 1) * d / 2) { // 剪枝
            return;
        }
        if (d == 0) { // 找到一个合法组合
            ans.add(new ArrayList<>(path));
            return;
        }
        // 枚举的数不能太小，否则后面没有数可以选
        for (int j = i; j >= d; j--) {
            path.add(j);
            dfs(j - 1, leftSum - j, k, ans, path);
            path.removeLast(); // path.remove(path.size() - 1);
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combination-sum-iii/solutions/2071013/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-feme/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 LeetCode 216 题  **“组合总和 III”**  的极致优化解法。

  相比于上一题单纯的“选满 k 个数”，这道题多了一个限制条件：​**数字之和必须等于 n**。

  灵茶山艾府（0x3F）大神的这段代码，最令人拍案叫绝的地方在于他将  **“数学剪枝”**  和  **“递归终止条件”**  完美地融合在了一起。

  以下是深度解析：

  ### 1. 核心公式：数学剪枝的魔法

  这段代码最难懂的只有这一行：

  Java

  ```
  if (leftSum < 0 || leftSum > (i * 2 - d + 1) * d / 2) { // 剪枝
      return;
  }
  ```

  这行代码做了两个判断，只要满足任何一个，就直接回头（剪枝），不再浪费时间往下找。

  #### A. 下界剪枝 (`leftSum < 0`)

  - **含义：**  如果剩余需要的和 `leftSum` 已经是负数了，说明选的数字太大，超标了。这很简单。

  #### B. 上界剪枝 (`leftSum > ...`) —— 这里的数学推导

  这是代码的精华。

  - **背景：**  我们当前最大能选的数字是 `i`​，还需要选 `d` 个数。
  - **思考：**  在这种情况下，我们能凑出的**最大可能的和**是多少？

    - 当然是选最大的 `d`​ 个数：即 `i, i-1, i-2, ..., i-d+1`。
  - **等差数列求和：**   
    这是一个首项为 `i-d+1`​，末项为 `i`​，项数为 `d` 的等差数列。

    $$
    \text{Sum} = \frac{(\text{首项} + \text{末项}) \times \text{项数}}{2}
    $$

  $$
  \text{Sum} = \frac{((i - d + 1) + i) \times d}{2}
  $$

  $$
  \text{Sum} = \frac{(2i - d + 1) \times d}{2}
  $$

  - **结论：**  假如你需要的 `leftSum`​ 比这个**理论最大值**还要大，那你怎么选都不可能凑够，直接放弃。

  ---

  ### 2. 隐形的终止条件：为什么 `d==0` 时不检查 sum？

  通常我们会这样写终止条件：

  Java

  ```
  if (d == 0) {
      if (leftSum == 0) ans.add(...); // 显式检查和是否为0
      return;
  }
  ```

  但 0x3F 的代码里：

  Java

  ```
  if (d == 0) { // 居然直接添加了？？
      ans.add(new ArrayList<>(path));
      return;
  }
  ```

  **为什么他敢直接添加？**

  秘密就在刚才那个剪枝公式里！

  当 `d == 0` 时（还需要选 0 个数）：

  1. 代入公式 `(i * 2 - d + 1) * d / 2`​，因为乘以 `d`​ (0)，结果为 ​**0**。
  2. 剪枝条件变成了：`if (leftSum < 0 || leftSum > 0)`。

     - 这意味着：只要 `leftSum`​ 不等于 0，就会被剪枝 return 掉，根本走不到 `if (d == 0)` 这一行。
  3. **结论：**  能活着走到 `if (d == 0)`​ 这一行，`leftSum`​ **一定** 是 0。

  这就是高手的代码：**逻辑闭环，没有废话。**

  ---

  ### 3. 代码全流程图解

  假设 `k=3, n=7`​，从 `9` 开始倒序枚举。

  1. **开始：**  `dfs(9, 7, 3)`。需要 3 个数，和为 7。

     - **剪枝检查：**  最大能凑 `9+8+7 = 24`​。`7 < 24`，继续。
  2. **尝试选 9：**  `dfs(8, -2, 2)`。

     - **剪枝：**  `leftSum (-2) < 0`​，​**剪掉！** （9 太大了，不可能）。
  3. ... (中间跳过 8, 7, 6, 5) ...
  4. **尝试选 4：**  `dfs(3, 3, 2)`。需要 2 个数，和为 3，最大能选 3。

     - **最大可能和：**  `3+2 = 5`​。`3 <= 5`，有戏。
     - **继续递归...**  最终找到 `4, 2, 1`。

  ### 4. 总结与复习

  这段代码融合了三个技巧，建议直接背诵这个模板：

  1. **倒序枚举 (**​**​`i`​**​ **从 9 到 1)：**  参数少，逻辑顺。
  2. **数量剪枝 (**​**​`j >= d`​**​ **)：**  也就是上一题讲的“如果剩下的数字不够凑满 k 个，就不选了”。
  3. **Sum 剪枝 (等差数列公式)：**  如果剩下的数字全选最大的，也凑不够 `leftSum`，或者已经超了，就不选了。

  **对比普通写法：**

  普通写法可能要跑 1ms，加上这个数学剪枝后，通常能跑进 0ms（击败 100%）。这就是数学在算法中的力量。

## 方法二：选或不选（dp）

暂缺

# 17.电话号码的字母组合：

[17. 电话号码的字母组合 - 力扣（LeetCode）](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)

```java
class Solution {
    private static final String[] MAPPING = new String[]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public List<String> letterCombinations(String digits) {
        int n = digits.length();
        if (n == 0) {
            return List.of();
        }

        List<String> ans = new ArrayList<>();
        char[] path = new char[n]; // 注意 path 长度一开始就是 n，不是空数组
        dfs(0, ans, path, digits.toCharArray());
        return ans;
    }

    private void dfs(int i, List<String> ans, char[] path, char[] digits) {
        if (i == digits.length) {
            ans.add(new String(path));
            return;
        }
        String letters = MAPPING[digits[i] - '0'];
        for (char c : letters.toCharArray()) {
            path[i] = c; // 直接覆盖
            dfs(i + 1, ans, path, digits);
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/letter-combinations-of-a-phone-number/solutions/2059416/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-3orv/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是灵茶山艾府（0x3F）针对 LeetCode 第 17 题  **“电话号码的字母组合”**  给出的一种**极致优化**的回溯解法。

  相比于传统的使用 `StringBuilder`​ 或 `List<Character>`​ 进行回溯，这段代码最大的亮点在于使用了 ​**固定长度的字符数组 (**​**​`char[]`​** ​ **)** 。

  我为你详细拆解其中的奥妙，特别是为什么它不需要显式的“撤销选择”（Backtracking）。

  ---

  ### 1. 核心亮点：固定长度数组 (`char[] path`)

  通常大家做回溯题，习惯用 `StringBuilder path`，然后：

  1. ​`path.append(c)` （做选择）
  2. ​`dfs(...)`
  3. ​`path.deleteCharAt(...)` （撤销选择/回溯）

  **灵神的思路：**

  - **已知长度：**  我们非常确定，最终生成的字符串长度一定等于输入数字的长度 `n`。比如输入 "23"，结果肯定是 2 个字母（如 "ad", "ae"）。
  - **直接占位：**  既然长度固定，不如直接开一个 `char[n]` 数组。
  - **覆盖即回溯：**  这一点非常精妙，后面详细讲。

  ---

  ### 2. 代码逐行深度解析

  #### A. 映射表 (Mapping)

  Java

  ```
  private static final String[] MAPPING = new String[]{
      "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
  };
  ```
  - 这是一个查找表。
  - ​`MAPPING[2]`​ 对应 "abc"，`MAPPING[9]` 对应 "wxyz"。
  - 0 和 1 留空，因为电话按键上 0 和 1 没有对应的字母。

  #### B. 主方法初始化

  Java

  ```
  int n = digits.length();
  if (n == 0) return List.of(); // 特判空情况

  char[] path = new char[n]; // 关键：直接创建一个长度为 n 的空数组
  dfs(0, ans, path, digits.toCharArray());
  ```
  - 这里直接申请了最终需要的内存空间，避免了 `StringBuilder` 动态扩容的开销。

  #### C. DFS 与 隐形回溯 (核心逻辑)

  Java

  ```
  private void dfs(int i, List<String> ans, char[] path, char[] digits) {
      // 1. 终止条件：填满了 n 个位置
      if (i == digits.length) {
          ans.add(new String(path)); // 将 char[] 转为 String 放入结果集
          return;
      }

      // 2. 获取当前按键对应的字母集合
      // digits[i] - '0' 将字符 '2' 转为整数 2
      String letters = MAPPING[digits[i] - '0'];

      // 3. 遍历每一个可选字母
      for (char c : letters.toCharArray()) {
          path[i] = c; // 【直接覆盖】 这一步同时完成了“做选择”和“回溯”
          dfs(i + 1, ans, path, digits);
      }
  }
  ```
  ---

  ### 3. 为什么不需要 `removeLast`？（图解覆盖逻辑）

  这是初学者最容易困惑的地方。让我们通过图解来看看 `path[i] = c` 是如何工作的。

  假设输入 `digits = "23"`​，`n=2`​。`path`​ 数组长度为 2 `[?, ?]`。

  **执行流程模拟：**

  1. ​**​`dfs(0)`​** ​  **(处理 '2' ->**   **"abc")**

     - 循环第 1 次：`c = 'a'`。
     - 执行 `path[0] = 'a'`​。此时 path 是 `['a', ?]`。
     - 递归调用 `dfs(1)`。
  2. ‍
  3. ​**​`dfs(0)`​** ​ **回到循环**

     - ​`dfs(1)`​ 全部跑完，函数返回到 `dfs(0)`。
     - 循环第 2 次：`c = 'b'`。
     - 执行 `path[0] = 'b'`​。此时 path 是 `['b', 'e']` (第二个位置是脏数据，但无所谓)。
     - **关键点：**  `'b'`​ 直接覆盖了之前的 `'a'`​。我们不需要手动把 `'a'`​ 删掉，因为 `path[0]` 这个位置反正要放新东西。
     - 递归调用 `dfs(1)`... 后面会把第二个位置覆盖为 'd', 'e', 'f'。

  **总结：**

  - **传统的** **​`List/StringBuilder`​** 是一个变长的容器，加一个必须删一个，否则长度会无限增长。
  - **固定数组** **​`char[]`​**  是一个定长的容器（像填空题）。我在“第一空”填了 A，下次循环想填 B 时，直接把 B 写在 A 上面就行了（擦除并重写），自然就不需要额外的“删除”操作。

  ### 4. 优缺点分析

  - **优点：**

    - **速度极快：**  数组操作是所有数据结构中最快的。
    - **内存最省：**  没有 `StringBuilder` 对象创建的开销，没有动态扩容开销。
    - **代码简洁：**  省去了回溯的那一行代码。
  - **缺点：**

    - 只适用于**结果长度固定**的回溯问题（比如排列、定长组合）。如果是“子集”问题（结果长度不固定），就不能用这种直接覆盖的方法了，还是得用 `path.add`​ 和 `path.remove`。

  这段代码是标准的“工业级”算法写法，非常值得背诵。
