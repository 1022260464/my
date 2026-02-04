---
title: day23
date: '2026-02-04 21:48:47'
updated: '2026-02-04 22:14:47'
permalink: /post/day23-1wbsfv.html
comments: true
toc: true
---



# day23

# **第七章 回溯算法part02**

### 39. 组合总和

本题是 集合里元素可以用无数次，那么和组合问题的差别 其实仅在于 startIndex上的控制

题目链接/文章讲解：<u>[https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html](https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1KT4y1M7HJ](https://www.bilibili.com/video/BV1KT4y1M7HJ)</u>

---

### 40.组合总和II

本题开始涉及到一个问题了：去重。

注意题目中给我们 集合是有重复元素的，那么求出来的 组合有可能重复，但题目要求不能有重复组合。

题目链接/文章讲解：<u>[https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html](https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV12V4y1V73A](https://www.bilibili.com/video/BV12V4y1V73A)</u>

---

### 131.分割回文串

本题较难，大家先看视频来理解 分割问题，明天还会有一道分割问题，先打打基础。

<u>[https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html](https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1c54y1e7k6](https://www.bilibili.com/video/BV1c54y1e7k6)</u>

---

### 39. 组合总和

[39. 组合总和 - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum/description/)

## 方法一：选或不选

- 用 dfs(i,left) 来回溯，设当前枚举到 candidates[i]，剩余要选的元素之和为 left，按照选或不选分类讨论：不选 candidates[i]：递归到 dfs(i+1,left)。  
  选 candidates[i]：递归到 dfs(i,left−candidates[i])。注意 i 不变，表示在下次递归中可以继续选 candidates[i]。  
  注：这个思路类似 完全背包。
- 如果递归中发现 left=0 则说明找到了一个合法组合，复制一份 path 加入答案。
- 递归边界：如果 i=n 或者 left<0 则返回。
- 递归入口：dfs(0,target)。

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(0, target, candidates, ans, path);
        return ans;
    }

    private void dfs(int i, int left, int[] candidates, List<List<Integer>> ans, List<Integer> path) {
        if (left == 0) {
            // 找到一个合法组合
            ans.add(new ArrayList<>(path));
            return;
        }

        if (i == candidates.length || left < 0) {
            return;
        }

        // 不选
        dfs(i + 1, left, candidates, ans, path);

        // 选
        path.add(candidates[i]);
        dfs(i, left - candidates[i], candidates, ans, path);
        path.remove(path.size() - 1); // 恢复现场
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combination-sum/solutions/2747858/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-mhf9/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 第 39 题 "组合总和" (Combination Sum)**  的一种经典解法。

  作者 **灵茶山艾府 (EndlessCheng)**  是 LeetCode 社区非常知名的题解作者。这段代码使用的是  **“选或不选” (Pick or Skip)**  的深度优先搜索 (DFS) / 回溯思路。

  下面我为你详细拆解这段代码的逻辑。

  ---

  ### 1. 核心思路：选还是不选？

  通常解决组合问题有两种常见的 DFS 写法：

  1. ​**枚举选哪个**​：在当前步骤中使用 `for` 循环枚举所有可能的下一个数字。
  2. ​**选或不选 (本题解法)** ​：对于当前数字 `candidates[i]`，我们在递归树的每一个节点只有两个分支：

     - **不选**这个数字：直接跳过，考虑下一个数字。
     - **选**这个数字：放入路径，并继续考虑当前数字（因为题目允许重复选择）。

  这种视角将多叉树转化为了​**二叉树**，逻辑非常清晰。

  ### 2. 代码逐行详解

  #### 主函数 `combinationSum`

  Java

  ```
  public List<List<Integer>> combinationSum(int[] candidates, int target) {
      List<List<Integer>> ans = new ArrayList<>();
      List<Integer> path = new ArrayList<>();
      // 从第 0 个索引开始，剩余目标值为 target
      dfs(0, target, candidates, ans, path);
      return ans;
  }
  ```

  - ​**​`ans`​**: 用于存储最终所有合法的组合列表。
  - ​**​`path`​**: 记录当前递归路径上已经选择的数字。
  - ​**入口**​: `dfs(0, target, ...)` 表示从数组第 0 个数开始做决策。

  ---

  #### 递归函数 `dfs`

  **参数含义：**

  - ​`i`​: 当前正在考虑 `candidates`​ 数组中的第 `i` 个数字。
  - ​`left`​: 距离目标值 `target` 还需要凑多少（剩余数值）。

  **第一步：递归出口 (Base Cases)**

  Java

  ```
  if (left == 0) {
      // 找到一个合法组合
      ans.add(new ArrayList<>(path));
      return;
  }

  if (i == candidates.length || left < 0) {
      return;
  }
  ```

  1. ​**成功**​: 如果 `left == 0`，说明路径上的数字之和刚好等于 target。

     - ​**注意**​: 必须使用 `new ArrayList<>(path)`​。因为 `path` 在内存中只有一份，如果不拷贝，后续的回溯操作（remove）会把结果里的数据也删掉。
  2. ​**失败**:

     - ​`i == candidates.length`: 所有数字都考虑完了，还没凑齐。
     - ​`left < 0`: 当前选择的数字之和已经超过 target 了，剪枝。

  **第二步：分支 1 —— 不选当前数字**

  Java

  ```
  // 不选
  dfs(i + 1, left, candidates, ans, path);
  ```

  - 直接跳过当前数字 `candidates[i]`。
  - 索引变为 `i + 1`​，剩余目标值 `left` 不变。

  **第三步：分支 2 —— 选当前数字**

  Java

  ```
  // 选
  path.add(candidates[i]);
  dfs(i, left - candidates[i], candidates, ans, path);
  path.remove(path.size() - 1); // 恢复现场
  ```

  1. ​**做出选择**​: 把 `candidates[i]`​ 加入 `path`。
  2. ​**递归**:

     - 注意索引依然是 ​**​`i`​**​（不是 `i+1`​）。这是因为题目规定​**同一个数字可以无限制重复被选取**。如果不跳到下一位，下层递归依然有机会再次选择这个数。
     - 剩余目标值变为 `left - candidates[i]`。
  3. ​**回溯 (Backtracking)** :

     - ​`path.remove(...)`​: 这是回溯算法的精髓。当上面的递归结束（无论是因为找到结果了，还是因为失败返回了），我们需要把刚才加入的那个数字拿出来，​**恢复现场**，以便回到上一层状态去走其他的逻辑分支（即去走“不选”的那个分支）。

  ---

  ### 3. 示例演示

  假设 `candidates = [2, 3]`​, `target = 3`。

  1. **DFS(0, 3)**  (考虑数字2):

     - ​**分支 A (不选 2)** ​: 递归 `DFS(1, 3)` (考虑数字3)。

       - ​**分支 A1 (不选 3)** ​: `DFS(2, 3)`​ -\> `i`​越界 -\> 返回。
       - ​**分支 A2 (选 3)** ​: `path`​\=[3], 递归 `DFS(1, 0)`。

         - ​`left`​为0 -\> **找到答案 [3]**  -\> 返回。
         - 回溯: `path` 变回 []。
     - ​**分支 B (选 2)** ​: `path`​\=[2], 递归 `DFS(0, 1)` (继续考虑数字2, 剩余1)。

       - ​**分支 B1 (不选 2)** ​: 递归 `DFS(1, 1)` (考虑数字3, 剩余1)。

         - ​**分支 ... (选 3)** ​: `DFS(1, -2)`​ -\> `left < 0`​ -\> 返回。
       - ​**分支 B2 (选 2)** ​: `path`​\=[2, 2], 递归 `DFS(0, -1)`。

         - ​`left < 0`​ -\> 返回。
         - 回溯: `path` 变回 [2]。
     - 回溯: `path` 变回 []。

  ### 4. 总结与复杂度

  这种写法的优点是逻辑非常像 **完全背包问题** 的思路。

  - ​**时间复杂度**: $O(S)$，其中 S 是所有可行解的长度之和。最坏情况下是指数级别的（例如 target 很大，candidates 包含 1 时）。
  - ​**空间复杂度**: $O(\text{target} / \min(\text{candidates}))$。这是递归栈的最大深度，取决于最坏情况下我们要选多少个最小的数字才能凑到 target。

  **你可以这样优化：**

  如果在进入 DFS 之前先对 `candidates`​ 进行​**排序**​，那么在“选”的分支中，如果发现 `left - candidates[i] < 0`，甚至可以直接跳出循环或不再递归，进一步剪枝提高效率。

  **下一步：**

  是否需要我展示另一种常见的解法（使用 `for` 循环枚举）来对比一下两者的区别？

# 剪枝优化

- 把 candidates 从小到大排序，如果递归中发现 left<candidates[i]，由于后面的数字只会更大，所以无法把 left 减小到 0，可以直接返回。
- ```java
  class Solution {
      public List<List<Integer>> combinationSum(int[] candidates, int target) {
          Arrays.sort(candidates);
          List<List<Integer>> ans = new ArrayList<>();
          List<Integer> path = new ArrayList<>();
          dfs(0, target, candidates, ans, path);
          return ans;
      }

      private void dfs(int i, int left, int[] candidates, List<List<Integer>> ans, List<Integer> path) {
          if (left == 0) {
              // 找到一个合法组合
              ans.add(new ArrayList<>(path));
              return;
          }

          if (i == candidates.length || left < candidates[i]) {
              return;
          }

          // 不选
          dfs(i + 1, left, candidates, ans, path);

          // 选
          path.add(candidates[i]);
          dfs(i, left - candidates[i], candidates, ans, path);
          path.remove(path.size() - 1); // 恢复现场
      }
  }

  作者：灵茶山艾府
  链接：https://leetcode.cn/problems/combination-sum/solutions/2747858/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-mhf9/
  来源：力扣（LeetCode）
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  ```

### 复杂度分析：

- 由如下完全背包代码可知，在 candidates=[2,3,4,⋯,31], target=40 的极端数据下，搜索次数的上界为 37271。换句话说，即使题目不保证答案个数 ≤150，我们也能很快地找到所有答案。
- ```java
  f = [1] + [0] * 40
  for i in range(2, 32):
      for j in range(i, 41):
          f[j] += f[j - i]
  print(sum(f))  # 37271

  ```

- 这段复杂度分析写得非常硬核，因为它不仅仅是在分析算法的逻辑步骤，而是引入了\*\*数论（Number Theory）**中的**整数拆分（Integer Partition）\*\*理论来界定算法的上界。

  这在普通的算法面试中属于“超纲”内容，但对于理解回溯算法在极端情况下的表现非常有帮助。

  下面我为你深度解析这段分析背后的数学含义。

  ---

  ### 1. 为什么要引用 Python 代码和 "37271"？

  #### 背景：回溯算法的痛点

  回溯（DFS）算法的时间复杂度通常很难精确计算，因为很大程度上取决于数据的“长相”。

  - ​**最好情况**：找不到组合，很快剪枝返回。
  - ​**最坏情况**​：`target`​ 很大，`candidates` 很密集（比如 1, 2, 3...），导致解非常多。

  #### Python 代码的作用：模拟最坏情况

  作者构造了一个密集的 `candidates = [2, 3, ..., 31]`​ 和一个适中的 `target = 40`。

  那段 Python 代码实际上是一个**完全背包问题的动态规划**解法（`f[j]`​ 表示凑出和为 `j` 的方案数）。

  - ​**目的**​：它算出在这种密集输入下，总的方案数（或者说搜索树的有效节点规模）大约是 ​**37271**。
  - ​**结论**：3.7 万次运算对于现代 CPU（每秒处理 $10^8$ 级别指令）来说是**瞬间完成**的。这有力地证明了：即使在题目约束并未极其严格（题目说答案数 $\le 150$，但这里模拟了更坏的情况）的前提下，DFS 依然能高效通过，不会超时（TLE）。

  ---

  ### 2. 那个复杂的数学公式是什么？

  公式：

  $$
  O(n \log n + \text{target} \cdot e^{\pi \sqrt{\frac{2}{3} \cdot \text{target}}})
  $$

  这是著名的 **哈代-拉马努金公式 (Hardy-Ramanujan Formula)**  的变形。

  #### 数学来源：整数拆分 (Integer Partitions)

  如果 `candidates`​ 包含 `[1, 2, 3, ..., target]`​ 中的所有数字，那么本题就变成了纯粹的数学问题：**把整数** **​`target`​**​ **拆分成若干个正整数之和，有多少种拆法？**  这个数量被称为划分函数 $p(n)$。

  哈代和拉马努金证明了 $p(n)$ 的增长速度近似于：

  $$
  p(n) \sim \frac{1}{4n\sqrt{3}} \exp\left(\pi \sqrt{\frac{2n}{3}}\right)
  $$

  #### 公式拆解

  1. **$n \log n$**​: 这是对 `candidates` 数组进行排序的时间（如果不排序，剪枝效果差，且无法直接套用某些逻辑，但这里主要是为了预处理）。
  2. **$e^{\pi \sqrt{\dots}}$**: 这一项就是 $p(\text{target})$ 的主导项。

     - 它告诉我们，随着 `target`​ 的增加，组合方案的数量是呈\*\*亚指数级（Sub-exponential）\*\*增长的。
     - 虽然不是纯粹的 $2^n$ 这种指数爆炸，但也非常快。
  3. **$\text{target}$**​: 前面的乘数系数，代表在递归过程中构建路径、复制列表（`new ArrayList<>(path)`）的开销。

  ​**人话翻译**：

  这个公式是在说：“在这个题目允许的最坏情况下（数字很密、target 很大），我们需要找到的**答案数量**会按照这个指数级公式增长。因为 DFS 必须把所有答案都找出来，所以 DFS 的运行时间至少也得是这么长。”

  ---

  ### 3. 空间复杂度分析

  > **空间复杂度：O(target)**
  >

  这个比较好理解：

  1. ​**递归栈深度**​：最坏情况下，我们每次都选最小的数字（假设是 1）。那么递归深度就是 `target / 1 = target` 层。
  2. ​**Path 数组**​：`path`​ 列表中最多也就存 `target` 个数字。
  3. ​**返回值不计入**​：按照算法分析的惯例，用于存储最终结果的 `ans` 列表所占空间通常不计入空间复杂度（除非题目专门问 Output Sensitivity）。

  ---

  ### 4. 面试贴士 (Key Takeaways)

  在面试中，你不需要背诵哈代-拉马努金公式，但可以借鉴这个分析思路来回答面试官关于“​**时间复杂度**”的追问：

  1. ​**常规回答**：

     - “这是一个 NP-Hard 问题，最坏时间复杂度是指数级的 $O(S)$，其中 $S$ 是所有可行解的长度之和。”
  2. ​**进阶回答 (加分项)** ：

     - “具体来说，其复杂度取决于​**解的数量**​。这本质上和**整数拆分 (Integer Partition)**  问题相关。”
     - “如果 `candidates`​ 数字很小且密集，解的数量会随着 `target`​ 增长非常快（亚指数级）；如果 `candidates` 数字稀疏，解会少很多，DFS 就会很快。”

  **下一步：**

  需要我为你展示这道题的 **剪枝优化 (Pruning)**  版本吗？加上排序和剪枝后，代码在实际运行中会快非常多。

## 方法二：枚举选哪个

- 类似 视频 中的「答案视角」。同样用 dfs(i,left) 来回溯，设当前枚举到 candidates[i]，剩余要选的元素之和为 left，考虑枚举下个元素是谁：

  在 [i,n−1] 中枚举要填在 path 中的元素 candidates[j]，然后递归到 dfs(j,left−candidates[j])。注意这里是递归到 j 不是 j+1，表示 candidates[j] 可以重复选取。

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(0, target, candidates, ans, path);
        return ans;
    }

    private void dfs(int i, int left, int[] candidates, List<List<Integer>> ans, List<Integer> path) {
        if (left == 0) {
            // 找到一个合法组合
            ans.add(new ArrayList<>(path));
            return;
        }

        // 枚举选哪个
        for (int j = i; j < candidates.length && candidates[j] <= left; j++) {
            path.add(candidates[j]);
            dfs(j, left - candidates[j], candidates, ans, path);
            path.remove(path.size() - 1); // 恢复现场
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combination-sum/solutions/2747858/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-mhf9/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 方法三：完全背包预处理 + 可行性剪枝

暂缺

```java

```

---

# 40.组合总和II

[40. 组合总和 II - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-ii/solutions/3036036/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-a7be/)

# 方法一：选或不选

- 前置题目：78. 子集

  视频讲解：回溯算法套路①子集型回溯【基础算法精讲 14】

  为了方便跳过相同元素和剪枝，在递归前，把 candidates 从小到大排序。

  用 dfs(i,left) 来回溯，设当前枚举到 candidates[i]，剩余要选的元素之和为 left，按照选或不选分类讨论：

  选 candidates[i]：递归到 dfs(i+1,left−candidates[i])。  
  不选 candidates[i]：跳过后续所有等于 candidates[i] 的数，递归到 dfs(i′,left)，其中 [i,i′−1] 中的数都相同。如果不跳过这些数，设 x=candidates[i], x′=candidates[i+1]，那么「选 x 不选 x′」和「不选 x 选 x′」这两种情况都会加到答案中，这就重复了。  
  递归边界：

  如果 left=0，说明所选元素之和恰好等于 target，把 path 加入答案，返回。  
  如果 i=n，没有可以选的数字，返回。  
  如果 left<candidates[i]，由于后面的数都比 left 大，所以 left 无法减成 0，返回。  
  递归入口：dfs(0,target)。

  如果 left=0，说明所选元素之和恰好等于 target，把 path 加入答案，返回。  
  如果 i=n，没有可以选的数字，返回。  
  如果 left<candidates[i]，由于后面的数都比 left 大，所以 left 无法减成 0，返回。  
  递归入口：dfs(0,target)。

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(0, target, candidates, ans, path);
        return ans;
    }

    private void dfs(int i, int left, int[] candidates, List<List<Integer>> ans, List<Integer> path) {
        // 所选元素之和恰好等于 target
        if (left == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }

        // 没有可以选的数字
        if (i == candidates.length) {
            return;
        }

        // 所选元素之和无法恰好等于 target
        int x = candidates[i];
        if (left < x) {
            return;
        }

        // 选 x
        path.add(x);
        dfs(i + 1, left - x, candidates, ans, path);
        path.remove(path.size() - 1); // 恢复现场

        // 不选 x，那么后面所有等于 x 的数都不选
        // 如果不跳过这些数，会导致「选 x 不选 x'」和「不选 x 选 x'」这两种情况都会加到 ans 中，这就重复了
        i++;
        while (i < candidates.length && candidates[i] == x) {
            i++;
        }
        dfs(i, left, candidates, ans, path);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combination-sum-ii/solutions/3036036/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-a7be/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 第 40 题 "组合总和 II" (Combination Sum II)**  的解法。

  相比于上一题（第 39 题），这道题有两个关键约束变化，直接导致代码逻辑的改变：

  1. ​**每个数字只能使用一次**​（`candidates` 中的每个元素在每个组合中只能使用一次）。
  2. ​**解集不能包含重复的组合**​（`candidates`​ 本身可能包含重复数字，如 `[1, 1, 2]`）。

  为了解决这两个问题，代码引入了 **排序 (Sorting)**  和 ​**去重逻辑 (De-duplication)** 。

  ---

  ### 1. 核心变化点详解

  #### A. 为什么要先排序？

  Java

  ```
  Arrays.sort(candidates);
  ```

  排序有两个目的：

  1. ​**剪枝优化**​：如果当前数字已经大于剩余目标值 `left`，因为后面数字更大，肯定也不行，可以直接停止。
  2. ​**方便去重**​：这是最关键的。通过排序，相同的数字会排在一起（例如 `[1, 1, 2, 5]`​）。这使得我们可以在“不选”的分支中，通过简单的 `while` 循环跳过所有相同的数字，从而避免重复组合。

  #### B. 选当前数字：索引变为 `i + 1`

  Java

  ```
  // 选 x
  path.add(x);
  dfs(i + 1, left - x, candidates, ans, path); // 注意这里是 i + 1
  path.remove(path.size() - 1);
  ```

  - ​**第 39 题**​：传入 `i`，表示当前数字可以无限重复选。
  - ​**第 40 题（本题）** ​：传入 `i + 1`，表示当前这个位置的数字一旦选了（或决定不选），以后都不能再用了，必须看下一个位置。

  #### C. 不选当前数字：核心去重逻辑

  Java

  ```
  // 不选 x，那么后面所有等于 x 的数都不选
  i++;
  while (i < candidates.length && candidates[i] == x) {
      i++;
  }
  dfs(i, left, candidates, ans, path);
  ```

  这是这段代码的​**灵魂所在**。

  ​**场景模拟**：

  假设 `candidates = [2, 2, 2, 5]`​，`target = 5`​，当前我们在考虑第一个 `2` (索引 0)。

  我们面临两个分支：

  1. ​**选第一个 2**：我们会得到以“第一个 2”开头的组合。
  2. ​**不选第一个 2**​：这意味着我们决定​**放弃使用“第一个 2”** 。

     - 如果我们紧接着在下一步选择了“第二个 2”，这就逻辑重复了！因为“第二个 2”和“第一个 2”数值一样。
     - ​**结论**​：如果我们决定“不选”当前这个数值 `x`​，那么后面紧挨着的、所有等于 `x`​ 的数字，我们都不能选。否则就会产生重复的子集（比如 `[第一个2, 5]`​ 和 `[第二个2, 5]` 是一样的）。

  ---

  ### 2. 代码逻辑流程图

  假设输入 `candidates = [1, 2, 2]`​, `target = 3`​。排序后依然是 `[1, 2, 2]`。

  1. **DFS(0, 3)**  (考虑 index 0, value 1):

     - ​**选 1**​: Path\=[1], 剩下 target\=2。进入 `DFS(1, 2)`。

       - (内部逻辑处理完 `[1, 2]` 等结果...)
     - ​**不选 1**​: `i`​ 变为 1。没有重复的 1，直接进入 `DFS(1, 3)` (考虑 index 1, value 2)。
  2. **DFS(1, 3)**  (考虑 index 1, value 2):

     - ​**选 2**​: Path\=[2], 剩下 target\=1。进入 `DFS(2, 1)`。

       - (会尝试选 index 2 的那个 2，发现 2 \> 1，return)。
     - ​**不选 2**:

       - ​**关键点**​: 当前 `x = 2`。
       - 代码执行 `while`​ 循环：检查 `candidates[2]` 也是 2，跳过！
       - ​`i` 变成 3 (越界)。
       - 进入 `DFS(3, 3)`​ -\> return。
     - ​**结果**: 避免了“不选第一个 2 但选了第二个 2”的情况。

  ---

  ### 3. 复杂度分析

  - ​**时间复杂度**: $O(2^n \cdot n)$。

    - 最坏情况下，每个位置都有“选”或“不选”两种可能，且因为每个数字只能用一次，树的深度最大为 $n$。
    - 排序需要 $O(n \log n)$。
    - 实际上，由于剪枝（`left < x`​）和去重（`while` 循环），实际运行速度远快于 $2^n$。
  - ​**空间复杂度**: $O(n)$。

    - 递归栈深度最深为 $n$。
    - ​`path` 列表最长为 $n$。

  ### 4. 总结：两道题的对比

  |**特性**|**组合总和 (39题)**|**组合总和 II (40题 - 本题)**||||
  | --| --------------------| ------| --| --| --|
  |**重复元素**|输入数组无重复|**输入数组有重复**||||
  |**使用次数**|单个数字可用无数次|**单个数字只能用一次**||||
  |**递归参数**|​`dfs(i, ...)`|​**​`dfs(i + 1, ...)`​** ||||
  |**去重策略**|不需要|**排序 +**​**​`while`​**​**循环跳过重复项**||||

- ### **去重逻辑**和**复杂度上界**的推导：

  这一段的核心在于解释 ​ **“为什么在‘不选’分支中要跳过所有相同的数字”** ​，以及\*\*“为什么这个回溯算法不会超时”\*\*。

  ---

  ### 1. 核心难点：去重逻辑的数学原理

  作者提到了：

  > “选 $x$ 不选 $x'$” 和 “不选 $x$ 选 $x'$” 这两种情况都会加到答案中，这就重复了。
  >

  让我们用图解的方式具体化这句话。

  假设 `candidates = [2, 2, 2]`​, `target = 2`。我们将这三个 2 分别标记为 $2_a, 2_b, 2_c$。

  如果不跳过重复数字，标准的“选或不选”会产生以下重复路径：

  1. ​**路径 A (选** **$2_a$**​ **)** :

     - 选择 $2_a$ -\> `path = [2]`​ -\> 达到 target -\> ​**记录答案**  **​`[2]`​** 。
  2. ​**路径 B (不选** **$2_a$**​ **, 选** **$2_b$**​ **)** :

     - 跳过 $2_a$ -\> 选择 $2_b$ -\> `path = [2]`​ -\> 达到 target -\> ​**记录答案**  **​`[2]`​** 。

  ​**问题出现**​：答案里会有两个 `[2]`，这是重复的。

  ​**作者的解决方案**：

  一旦你决定\*\*“不选”\*\* $2_a$，这意味着你在当前的决策树分支中彻底放弃了数值为 2 的这个选项。那么紧跟在后面的 $2_b, 2_c$（因为已经排过序，它们肯定挨在一起）也必须全部跳过。

  - ​**逻辑**：要么在第一次遇到 2 的时候就选（覆盖了所有包含 2 的情况）；要么就彻底不碰 2。

  ---

  ### 2. 复杂度分析：为什么是 28629？

  这段复杂度分析是在回答一个常见的疑问： **“回溯算法会不会因为指数爆炸而超时？”**

  #### 极端情况构建

  作者构建了一个极其恶心的测试用例：

  - 30 个 1
  - 15 个 2
  - ...
  - 1 个 30

  这个用例的特点是：对于 `target = 30` 来说，每种数字的数量都足够多，多到仿佛有“无限个”。

  例如，我们只需要 30 个 1 就能凑成 30，而数组里刚好有 30 个 1。这实际上就把问题退化成了 ​**无限制的整数拆分问题 (Integer Partition)** ，也就是上一题（Combination Sum I）的最坏情况。

  #### 动态规划验证 (Python 代码)

  Python

  ```
  f = [1] + [0] * 30
  for i in range(1, 31):
      for j in range(i, 31):
          f[j] += f[j - i]
  print(sum(f))  # 28629
  ```

  这段代码计算的是：把整数 30 拆分成若干正整数之和，一共有多少种不同的拆分方案（即 Partition Function $p(30)$ 的累加）。结果是 28,629。

  #### 结论

  即使在数据最密集、解最多的极端情况下，搜索树的节点数（约 3 万）对于计算机来说也是微不足道的。

  - $30,000$ 次操作 $\times$ 每次操作的常数时间 $\ll$ LeetCode 的超时限制（通常 $10^8$ 次操作）。
  - 所以，虽然理论复杂度写起来很吓人（带 $\exp$ 和 $\pi$ 的公式），但实际运行​**非常快**。

  ---

  ### 3. 代码与逻辑的对应 (Review)

  回顾你上一条发出的代码，现在逻辑完全对应上了：

  Java

  ```
  // 1. 选 candidates[i]
  path.add(candidates[i]);
  dfs(i + 1, left - candidates[i], ...); 
  path.remove(...);

  // 2. 不选 candidates[i]
  // 核心去重逻辑：跳过后续所有等于 candidates[i] 的数
  i++;
  while (i < candidates.length && candidates[i] == candidates[i-1]) { // 注意这里比较的是 i 和 i-1 (或者用变量x记录)
      i++;
  }
  dfs(i, left, ...);
  ```

  ### 4. 总结

  1. ​**前置题目 78 (子集)** ：这道题本质上就是在求子集的过程中，筛选出和为 target 的子集。
  2. ​**排序**​：是去重和剪枝的基础，没有排序就无法通过简单的 `while` 循环去重。
  3. ​**剪枝**​：`left < candidates[i]` 极其重要，它保证了在数组后半段大数时直接截断递归，大幅提升效率。

  **下一步：**

  这道题还有一种\*\*“枚举选哪个” (For-loop 递归)\*\* 的写法，它的去重逻辑是 `if (i > start && candidates[i] == candidates[i-1]) continue;`。

  这种写法在很多人的认知中更直观（横向去重），你想看看这两种写法的对比吗？

# 方法二：枚举选哪个

- 同样地，把 candidates 从小到大排序，用 dfs(i,left) 来回溯。

  在 [i,n−1] 中枚举要加到 path 中的数 candidates[j]，然后递归到 dfs(j+1,left−candidates[j])。注意，如果 j>i，说明我们没选 candidates[j−1]，那么根据方法一的跳过规则，如果此时 candidates[j]=candidates[j−1]，则跳过，枚举下一个 j。

  剪枝：在枚举 j 的过程中，如果 left<candidates[j]，由于后面的数都比 left 大，所以 left 无法减成 0，退出循环。

  注意不需要判断 i=n，因为此时一定不会进入循环。

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(0, target, candidates, ans, path);
        return ans;
    }

    private void dfs(int i, int left, int[] candidates, List<List<Integer>> ans, List<Integer> path) {
        // 所选元素之和恰好等于 target
        if (left == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }

        // 在 [i, n-1] 中选一个 candidates[j]
        // 注意选 candidates[j] 意味着 [i,j-1] 中的数都没有选
        for (int j = i; j < candidates.length && candidates[j] <= left; j++) {
            // 如果 j>i，说明 candidates[j-1] 没有选
            // 同方法一，所有等于 candidates[j-1] 的数都不选
            if (j > i && candidates[j] == candidates[j - 1]) {
                continue;
            }
            path.add(candidates[j]);
            dfs(j + 1, left - candidates[j], candidates, ans, path);
            path.remove(path.size() - 1); // 恢复现场
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combination-sum-ii/solutions/3036036/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-a7be/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码展示了 **LeetCode 40. 组合总和 II** 的另一种经典写法：​ **“枚举选哪个” (Enumeration / For-Loop Recursion)** 。

  相比于上一各方法（“选或不选”的二叉树视角），这种写法构建的是一棵 ​**多叉树**​。这也是很多人直觉上更容易理解的写法： **“我当前这一步，可以在剩下的数字里选哪一个？”**

  我为你详细拆解这段代码，特别是那个困扰无数人的 `j > i` 去重逻辑。

  ---

  ### 1. 核心逻辑：横向枚举 vs 纵向递归

  这段代码的结构是：

  - ​**外层** **​`for`​**​ **循环**​：负责  **“横向”**  广度遍历。表示在当前位置（递归层级），我可以尝试把 `candidates[i]`​, `candidates[i+1]`, ... 依次作为当前路径的下一个数字。
  - ​**内层** **​`dfs`​**​ **递归**​：负责  **“纵向”**  深度挖掘。一旦选定了 `candidates[j]`​，就带着剩余的 `left` 进入下一层。

  ### 2. 代码细节逐行解析

  #### A. 剪枝优化 (Pruning)

  Java

  ```
  // 在循环条件中直接判断：candidates[j] <= left
  for (int j = i; j < candidates.length && candidates[j] <= left; j++)
  ```

  - 因为数组已经 **排序** (`Arrays.sort`)。
  - 如果当前的 `candidates[j]`​ 已经比 `left` 大了，那么后面的数字肯定更大，完全没必要继续循环了。直接退出循环，效率极高。

  #### B. 核心去重逻辑 (The Tricky Part)

  这是本题最大的难点：

  Java

  ```
  if (j > i && candidates[j] == candidates[j - 1]) {
      continue;
  }
  ```

  - ​**​`j > i`​**​: 这个条件保证了我们 **不是** 在处理当前这一层的第一个可选数字。

    - 如果 `j == i`​，说明这是父节点传下来的第一个能选的数，​**必须选**（或者说有资格尝试选）。
    - 如果 `j > i`​，说明 `candidates[i]`​ (或者之前的某个数) 已经被考察过了，现在 `j` 往后移了一位。
  - ​**​`candidates[j] == candidates[j-1]`​** : 如果当前考察的数字和前一个考察的数字一样。

  ​**人话翻译**：

  > “在​**同一层**（同一个父节点下），如果我刚才已经试过把 '2' 当作当前位置的数字去跑了一遍 DFS，那么现在遇到的这个新的 '2' 就不要再试了，否则结果肯定重复。”
  >

  这就叫：​**树层去重 (Horizontal Pruning)** 。

  ---

  ### 3. 图解演示 (Visualizing Deduplication)

  假设 `candidates = [1, 2, 2']`​, `target = 3`。

  我们把两个 2 分别标记为 `2`​ 和 `2'` 以示区分。

  **DFS(0, 3) - 也就是第一层递归：**

  1. ​**​`j = 0`​**​  **(对应数字 1)** :

     - 选 1。
     - 递归 `DFS(1, 2)`​ ... 最终找到 `[1, 2]`。
     - 回溯。
  2. ​**​`j = 1`​**​  **(对应数字 2)** :

     - 判断 `j > i`​? 是 (1 \> 0)。
     - 判断 `cand[1] == cand[0]`​? No (2 !\= 1)。
     - ​**选 2**。
     - 递归 `DFS(2, 1)`​ -\> 剩下 `2'`​ -\> 选 `2'`​ -\> sum\=4 (超了) -\> 回溯。
     - 回溯。
  3. ​**​`j = 2`​**​  **(对应数字 2')** :

     - 判断 `j > i`​? 是 (2 \> 0)。
     - 判断 `cand[2] == cand[1]`​? ​**YES (2**  **==**  **2)** 。
     - **触发** **​`continue`​**​ **！跳过！**
     - ​**原因**​：上一轮 (`j=1`​) 我们已经以 `2`​ 为开头尝试过找答案了。现在 `j=2`​ 又是 `2`，如果再以它开头，找到的组合和上一轮一模一样。

  ---

  ### 4. 为什么要用 `j + 1`？

  Java

  ```
  dfs(j + 1, left - candidates[j], ...);
  ```

  - ​**题目要求**：每个数字在每个组合中只能使用一次。
  - ​**Method 1 (选/不选)** ​：我们在“选”的分支用了 `i + 1`​，表示当前这个索引 `i`​ 用过了，下次看 `i+1`。
  - ​**Method 2 (枚举)** ​：我们在 `for`​ 循环中选中了 `candidates[j]`​，那么递归到下一层时，当然只能从 `j + 1`​ 开始挑，不能再回头选 `j`。

  ---

  ### 5. 两种方法的终极对比

  |**特征**|**方法一：选或不选 (Pick/Skip)**|**方法二：枚举选哪个 (For-Loop)**||||
  | --| ------------------------------| ----------------------------| --| --| --|
  |**思维模型**|**二叉树**(对于每个数：要么选，要么扔)|**多叉树**(对于当前位置：填哪个数？)||||
  |**去重逻辑**|**纵向/不选分支去重**<br /><br />`while(i<n && cand[i]==x) i++`|**横向/同层去重**<br /><br />`if(j>i && cand[j]==cand[j-1])`||||
  |**代码直观度**|逻辑较严密，稍微抽象|符合人类填空直觉，较好理解||||
  |**时间复杂度**|一样$O(2^n \cdot n)$|一样$O(2^n \cdot n)$||||
  |**空间复杂度**|一样|一样||||

  ### 6. 总结建议

  - 对于 **组合 (Combination)**  和 **子集 (Subset)**  问题， **“枚举选哪个” (方法二)**  通常是面试中的主流写法，因为那个 `if (j > i && ...)` 的模板非常通用，背下来就能解决所有“不可重复选取的去重”问题。
  - 对于 **0-1 背包** 理论分析， **“选或不选” (方法一)**  更贴合动态规划的状态转移方程。

---

# 131.分割回文串

[131. 分割回文串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-partitioning/description/)

# 方法一：选或不选

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> ans = new ArrayList<>();
        List<String> path = new ArrayList<>();
        dfs(0, 0, s, path, ans);
        return ans;
    }

    // 考虑 i 后面的逗号怎么选
    // start 表示当前这段回文子串的开始位置
    private void dfs(int i, int start, String s, List<String> path, List<List<String>> ans) {
        if (i == s.length()) { // s 分割完毕
            ans.add(new ArrayList<>(path)); // 复制 path
            return;
        }

        // 不分割，不选 i 和 i+1 之间的逗号
        if (i < s.length() - 1) { // i=n-1 时只能分割
            // 考虑 i+1 后面的逗号怎么选
            dfs(i + 1, start, s, path, ans);
        }

        // 分割，选 i 和 i+1 之间的逗号（把 s[i] 作为子串的最后一个字符）
        if (isPalindrome(s, start, i)) {
            path.add(s.substring(start, i + 1));
            // 考虑 i+1 后面的逗号怎么选
            // start=i+1 表示下一个子串从 i+1 开始
            dfs(i + 1, i + 1, s, path, ans);
            path.removeLast(); // path.remove(path.size() - 1);
        }
    }

    private boolean isPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/palindrome-partitioning/solutions/2059414/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-fues/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 131. 分割回文串 (Palindrome Partitioning)**  的解法。

  这依然是灵茶山艾府（EndlessCheng）标志性的  **“选或不选”**  思路，但这里选的对象非常有意思：不是选数字，而是 ​ **“选逗号”（切分点）** 。

  我为你深度解析这种独特的\*\*“逗号视角”\*\*。

  ---

  ### 1. 核心视角：切西瓜（选逗号）

  通常我们做这道题，想法是：“我枚举第一段切在哪儿，然后递归切剩下的”。

  但在灵神的这个解法中，他把视角转换成了：

  > 一个长度为 $N$ 的字符串，字符之间有 $N-1$ 个空隙（可以理解为逗号的位置）。
  >
  > 对于每一个空隙，我们都有两种选择：
  >
  > 1. ​**切一刀 (Split)** ：前面的部分形成一个子串。
  > 2. ​**不切 (Don't Split)** ：让当前的子串继续变长。
  >

  #### 例子演示：`s = "aab"`

  索引：`0 1 2`

  字符：`a a b`

  空隙： `^ ^`

  (空隙 0 在第一个 a 后面，空隙 1 在第二个 a 后面)

  ### 2. 代码参数详解

  Java

  ```
  // i: 当前正在考虑第 i 个字符后面的空隙（或者说正在考虑字符 s[i]）
  // start: 当前正在累积的这段子串是从哪里开始的
  private void dfs(int i, int start, String s, ...)
  ```

  - ​**​`start`​**: 记录当前正在“生长”的子串的起始位置。
  - ​**​`i`​**​: 当前走到哪个字符了。我们要决定的是：**在** **​`s[i]`​** ​ **后面要不要切一刀？**

  ---

  ### 3. 逻辑逐行拆解

  #### A. 递归出口 (Base Case)

  Java

  ```
  if (i == s.length()) { // s 分割完毕
      ans.add(new ArrayList<>(path));
      return;
  }
  ```

  - 当我们处理完所有字符（`i` 走到了末尾），说明切分结束。
  - ​**注意**：这里能进来，隐含意味着最后一个字符后面“必须切了一刀”（或者说默认结束），且之前的切分都是合法的回文。

  #### B. 分支一：不切（不选逗号）

  Java

  ```
  // 不分割，不选 i 和 i+1 之间的逗号
  if (i < s.length() - 1) { // i=n-1 时必须结束，不能选择“不切”
      dfs(i + 1, start, s, path, ans);
  }
  ```

  - ​**含义**​：当前子串 `s[start...i]`​ 还没结束，我要把它延长到 `i+1`。
  - ​**操作**：

    - ​`i`​ 变成 `i+1` (继续看下一个字符)。
    - ​`start`​ **保持不变** (因为子串还是从同一个地方开始的)。
  - ​**限制**​：`i < s.length() - 1`。如果是最后一个字符，你不能选择“不切”，因为字符串已经没了，必须结算。

  #### C. 分支二：切（选逗号）

  Java

  ```
  // 分割，选 i 和 i+1 之间的逗号
  if (isPalindrome(s, start, i)) {
      path.add(s.substring(start, i + 1)); // 1. 放入当前合法回文串
      
      // 2. 递归：start 变成了 i + 1
      dfs(i + 1, i + 1, s, path, ans);
      
      path.removeLast(); // 3. 回溯
  }
  ```

  - ​**前提**​：只有当 `s[start...i]` 确实是回文串时，我们才有资格在这里切一刀。如果是乱码，切下来也不符合题目要求，直接剪枝。
  - ​**操作**：

    - 记录答案：把 `s.substring(start, i+1)`​ 加入 `path`。
    - 递归：`i`​ 变成 `i+1`​；**​`start`​**​ **也变成** **​`i+1`​**​ (因为这一段切完了，下一段从 `i+1` 开始)。
    - 回溯：撤销选择。

  ---

  ### 4. 图解对比：两种写法

  为了让你彻底明白，我对比一下  **“枚举分割点 (For-loop)”**  和  **“选逗号 (Pick/Skip)”**  的区别。

  假设 `s = "aab"`。

  #### 写法一：常规 For 循环 (枚举分割点)

  > "我现在站在 start\=0，我尝试切出 'a', 'aa', 'aab'..."
  >

  - **DFS(0)**

    - ​`for j = 0`​ (切出 "a"): 递归 `DFS(1)`
    - ​`for j = 1`​ (切出 "aa"): 递归 `DFS(2)`
    - ​`for j = 2` (切出 "aab"): 不是回文，跳过。

  #### 写法二：本题写法 (选或不选逗号)

  > "我现在看着字符 index 0 ('a')。切不切？"
  >

  - ​**DFS(i=0, start=0)** : 看着第一个 'a'

    - ​**不切**​: `DFS(i=1, start=0)`​ -\> 子串变成 "aa"，看着第二个 'a'。
    - ​**切**​: 检查 "a" 是回文吗？是。加入 "a"。递归 `DFS(i=1, start=1)`​ -\> 新子串从第二个 'a' 开始。

  ### 5. 复杂度分析

  - ​**时间复杂度**: $O(n \cdot 2^n)$。

    - 字符串长 $n$，有 $n-1$ 个逗号。每个逗号选或不选，状态空间是 $2^{n-1}$。
    - 每次判断回文 `isPalindrome` 需要 $O(n)$ (最坏)。
    - 虽然有剪枝（必须是回文才切），但最坏情况（如 "aaaaa"）确实能达到这个界。
  - ​**空间复杂度**: $O(n)$。

    - 递归栈深度最大为 $n$。

  ### 6. 优缺点评价

  - ​**优点**:

    - ​**视角统一**：如果你一直在练习灵神的“选或不选”套路（子集、组合），这种写法能让你保持思维的一致性。
    - ​**逻辑清晰**：把“切分”动作具象化了。
  - ​**缺点**:

    - ​**判断回文重复计算**​：`isPalindrome` 会被反复调用。

      - ​*优化点*​：可以使用 **动态规划 (DP)**  预处理出一个 `boolean[][] check` 表，这样判断回文就是 $O(1)$ 了。
    - ​**Substring 开销**​：Java 的 `substring` 是拷贝操作，有一定开销（不过这道题无论哪种写法都避免不了）。

  ### 总结

  这段代码通过 **DFS(index, start)**  维护了两个状态：

  1. ​**​`index`​**: 所有的字符我都看一遍。
  2. ​**​`start`​**: 记录还没被切下来的“暂存区”的开头。

  如果你理解了上一题（组合总和）的“选或不选”，这道题就是\*\*“在字符串空隙中做子集问题”\*\*。

---

# 方法二：答案的视角（枚举子串结束位置）

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> ans = new ArrayList<>();
        List<String> path = new ArrayList<>();
        dfs(0, s, path, ans);
        return ans;
    }

    // 考虑 s[i] ~ s[n-1] 怎么分割
    private void dfs(int i, String s, List<String> path, List<List<String>> ans) {
        if (i == s.length()) { // s 分割完毕
            ans.add(new ArrayList<>(path)); // 复制 path
            return;
        }
        for (int j = i; j < s.length(); j++) { // 枚举子串的结束位置
            if (isPalindrome(s, i, j)) {
                path.add(s.substring(i, j + 1)); // 分割！
                // 考虑剩余的 s[j+1] ~ s[n-1] 怎么分割
                dfs(j + 1, s, path, ans);
                path.removeLast(); // path.remove(path.size() - 1);
            }
        }
    }
    
    private boolean isPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/palindrome-partitioning/solutions/2059414/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-fues/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是 **LeetCode 131. 分割回文串** 的第二种解法，也是最主流、最符合直觉的  **“枚举选哪个” (Enumeration)**  写法。

  如果说上一各解法（选或不选）是  **“二叉树”**  视角（切 vs 不切），那么这一段代码就是标准的  **“多叉树”**  视角（这一刀切多长？）。

  我为你详细拆解这段代码的逻辑，并对比两种写法的差异。

  ---

  ### 1. 核心逻辑：切多长？

  **代码结构**：

  - ​**​`dfs(i, ...)`​** ​: 表示当前我们已经处理完了字符串的前 `i`​ 个字符，现在要从 `s[i]` 开始，切出下一段回文串。
  - ​**​`for (int j = i; j < s.length(); j++)`​** :
  - 这是一次 **横向枚举**。
  - 我们尝试让当前的子串长度不同：`s[i...i]`​, `s[i...i+1]`​, `s[i...i+2]` ... 一直到字符串末尾。
  - ​`j` 代表当前这一刀切在哪个位置（子串的结束索引）。

  ---

  ### 2. 代码执行流程 (以 "aab" 为例)

  ​**​`dfs(0, "aab")`​**  —— 从头开始切。

  1. ​**​`j = 0`​**​  **(切出 "a")** :

  - ​`isPalindrome("a")`​? -> **是**。
  - ​`path`​ = `["a"]`。
  - **递归 `dfs(1)**` (处理剩下的 "ab")。
  - (内部会切出 "a", "b" -> 得到结果 `["a", "a", "b"]`)
  - 回溯：`path`​ 变回 `[]`。

  2. ​**​`j = 1`​**​  **(切出 "aa")** :

  - ​`isPalindrome("aa")`​? -> **是**。
  - ​`path`​ = `["aa"]`。
  - **递归 `dfs(2)**` (处理剩下的 "b")。
  - (内部会切出 "b" -> 得到结果 `["aa", "b"]`)
  - 回溯：`path`​ 变回 `[]`。

  3. ​**​`j = 2`​**​  **(切出 "aab")** :

  - ​`isPalindrome("aab")`​? -> **否**。
  - 不递归，直接跳过。

  **结果**：收集到 `[["a", "a", "b"], ["aa", "b"]]`。

  ---

  ### 3. 两套解法的深度对比

  这两种写法（选或不选 vs 枚举子串）是回溯算法的“任督二脉”。理解它们的转换关系至关重要。

  |特性|**解法一：选或不选 (Pick/Skip)**|**解法二：枚举子串 (Enumeration)**|
  | ------| -----------------------------------------| --------------------------|
  |**关注点**|**空隙 (Gap)**|**片段 (Chunk)**|
  |**决策方式**|“这个逗号切不切？”|“这一段切到哪儿？”|
  |**树的形状**|**二叉树** (深度大)|**多叉树** (宽度大)|
  |**参数状态**|需要 `index`​ (当前字符) 和 `start` (当前子串起始)|只需要 `i` (当前子串起始)|
  |**代码直觉**|逻辑稍绕，但符合 DP 状态转移|**非常直观**，符合人类切割逻辑|
  |**去重/剪枝**|依赖回文判断来剪枝|**For 循环天然剪枝** (不是回文就不往下走)|

  **代码对应关系：**

  - 解法一的 `dfs(i+1, start)`​ (不切) 对应解法二中 `j`​ 继续向后移 (`j++`)。
  - 解法一的 `dfs(i+1, i+1)`​ (切) 对应解法二中 `dfs(j+1)` (进入下一层)。

  ---

  ### 4. 复杂度与优化 (DP 预处理)

  这段代码的痛点依然在 `isPalindrome`。

  - **当前复杂度**：。每次循环都要跑一次  的回文检测。
  - **优化方案**：**动态规划预处理 (DP Pre-computation)** 。

  我们可以先花费  的时间算出一个表格 `f[i][j]`​，表示 `s[i...j]`​ 是否是回文串。  
  这样在 DFS 的 `if`​ 判断中，`isPalindrome` 就变成了  的查表操作。

  **优化后的 DFS 片段：**

  ```java
  // 预处理代码 (DP)
  boolean[][] f = new boolean[n][n];
  for (int i = n - 1; i >= 0; i--) {
      for (int j = i; j < n; j++) {
          // 如果首尾字符相同，且中间部分也是回文 (或长度小于2)
          f[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 2 || f[i + 1][j - 1]);
      }
  }

  // DFS 中直接查表
  private void dfs(int i, ...) {
      // ...
      for (int j = i; j < s.length(); j++) {
          if (f[i][j]) { // O(1) 查表
              path.add(s.substring(i, j + 1));
              dfs(j + 1, ...);
              path.removeLast();
          }
      }
  }

  ```

  ### 5. 总结

  - **面试建议**：这种 **For 循环枚举** 的写法是**最通用、最推荐**的写法。它代码更短，变量更少（不需要 `start`），逻辑也更容易解释。
  - **核心思想**：站在位置 `i`​，枚举这一刀切在 `j`。如果是回文，就切下来，把剩下的问题交给递归。

  **下一步：**   
  这道题是“分割”问题。  
  还有一类非常相似的题目是 **IP 地址复原 (LeetCode 93)** ，也是切割字符串，但它限制了切出来的段必须符合 IP 规则，且只能切成 4 段。你要试试看吗？思路和这个代码模板几乎一模一样。

‍
