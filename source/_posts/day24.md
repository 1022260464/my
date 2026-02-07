---
title: day24
date: '2026-02-07 19:19:28'
updated: '2026-02-07 19:28:22'
permalink: /post/day24-z2mooch.html
comments: true
toc: true
---



# day24

# **第七章 回溯算法part03**

### 93.复原IP地址

本期本来是很有难度的，不过 大家做完 分割回文串 之后，本题就容易很多了

题目链接/文章讲解：<u>[https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html](https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1XP4y1U73i/](https://www.bilibili.com/video/BV1XP4y1U73i/)</u>

### 78.子集

子集问题，就是收集树形结构中，每一个节点的结果。 整体代码其实和 回溯模板都是差不多的。

题目链接/文章讲解：<u>[https://programmercarl.com/0078.%E5%AD%90%E9%9B%86.html](https://programmercarl.com/0078.%E5%AD%90%E9%9B%86.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1U84y1q7Ci](https://www.bilibili.com/video/BV1U84y1q7Ci)</u>

### 90.子集II

大家之前做了 40.组合总和II 和 78.子集 ，本题就是这两道题目的结合，建议自己独立做一做，本题涉及的知识，之前都讲过，没有新内容。

题目链接/文章讲解：<u>[https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html](https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1vm4y1F71J](https://www.bilibili.com/video/BV1vm4y1F71J)</u>

‍

### 93.复原IP地址

[93. 复原 IP 地址 - 力扣（LeetCode）](https://leetcode.cn/problems/restore-ip-addresses/description/)

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> ans = new ArrayList<>();
        int[] path = new int[4]; // path[i] 表示第 i 段的结束位置 + 1（右开区间）
        dfs(0, 0, 0, s, s.length(), path, ans);
        return ans;
    }

    // 分割 s[i] 到 s[n-1]，现在在第 j 段（j 从 0 开始），数值为 ipVal
    private void dfs(int i, int j, int ipVal, String s, int n, int[] path, List<String> ans) {
        if (i == n) { // s 分割完毕
            if (j == 4) { // 必须有 4 段
                int a = path[0], b = path[1], c = path[2];
                ans.add(s.substring(0, a) + "." + s.substring(a, b) + "." + s.substring(b, c) + "." + s.substring(c));
            }
            return;
        }

        if (j == 4) { // j=4 的时候必须分割完毕，不能有剩余字符
            return;
        }

        // 手动把字符串转成整数，这样字符串转整数是严格 O(1) 的
        ipVal = ipVal * 10 + (s.charAt(i) - '0');
        if (ipVal > 255) { // 不合法
            return;
        }

        // 不分割，不以 s[i] 为这一段的结尾
        if (ipVal > 0) { // 无前导零
            dfs(i + 1, j, ipVal, s, n, path, ans);
        }

        // 分割，以 s[i] 为这一段的结尾
        path[j] = i + 1; // 记录下一段的开始位置
        dfs(i + 1, j + 1, 0, s, n, path, ans);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/restore-ip-addresses/solutions/3727037/liang-chong-fang-fa-san-zhong-xun-huan-h-hxak/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 93. 复原 IP 地址 (Restore IP Addresses)**  的解法。

  这依然沿用了灵茶山艾府 (EndlessCheng) 的  **“选或不选”**  核心思想。

  不过在这里，问题从“选数字”变成了 ​ **“选逗号（点）的位置”** 。

  这道题的逻辑比“分割回文串”要稍微复杂一点，因为 IP 地址的每一段都有严格的**数值限制 (0-255)**  和 ​**格式限制 (不能有前导零)** 。

  我为你拆解这段代码的精妙之处。

  ---

  ### 1. 核心视角：累加与决策

  这也是一个 ​**DFS (深度优先搜索)** 。

  我们在遍历字符串的过程中，维护当前这一段 IP 的数值 `ipVal`​。对于每一个字符 `s[i]`，我们面临两个选择：

  1. ​**不分割 (Continue)** ​：把当前字符拼接到当前段的末尾，让 `ipVal` 变大。
  2. ​**分割 (Split)** ​：当前字符作为这一段的结尾，打个点 `.`，开始下一段。

  ---

  ### 2. 代码深度解析

  #### A. 状态定义

  Java

  ```
  // i: 当前处理到字符串的第 i 个字符
  // j: 当前正在处理 IP 地址的第 j 段 (0, 1, 2, 3)
  // ipVal: 当前这一段已经累积的数值 (比如 "25" -> 25)
  private void dfs(int i, int j, int ipVal, String s, int n, int[] path, List<String> ans)
  ```

  - ​**​`ipVal`​**​ **的妙用**​：作者没有每次都用 `substring`​ 去截取字符串然后转 `int`​，而是直接维护一个 `int`​ 类型的 `ipVal`。这使得数值计算变成了 **$O(1)$** 的操作，避免了频繁的字符串拷贝开销。

  #### B. 核心逻辑：累加与剪枝

  Java

  ```
  // 1. 算上当前字符后的新数值
  ipVal = ipVal * 10 + (s.charAt(i) - '0');

  // 2. 剪枝：如果超过 255，说明这一段不合法，直接回溯
  if (ipVal > 255) {
      return;
  }
  ```

  这是 IP 地址的硬性规定：每一段必须 $\le 255$。

  #### C. 分支一：不分割 (Continue)

  Java

  ```
  // 不分割，不以 s[i] 为这一段的结尾
  if (ipVal > 0) { // 关键：前导零检查
      dfs(i + 1, j, ipVal, s, n, path, ans);
  }
  ```

  - ​**前导零限制**：IP 地址不允许 "01", "012" 这样的段，但允许单独的 "0"。

    - 如果 `ipVal == 0`​ (说明当前字符是 '0' 且是这一段的开头)，那么我们**不能**选择“不分割”。因为一旦不分割，后面再来一个数字就会变成前导零（例如 '0' -\> '1' 变成 "01"）。
    - 所以，只有当 `ipVal > 0` 时，我们才有资格选择“继续延长这一段”。
    - ​**特例**​：如果当前就是 '0'，`ipVal`​ 是 0，进不去这个 `if`，只能走下面的“分割”分支。这意味着 '0' 必须单独成为一段。

  #### D. 分支二：分割 (Split)

  Java

  ```
  // 分割，以 s[i] 为这一段的结尾
  path[j] = i + 1; // 记录第 j 段的结束位置（即下一个段的开始位置）
  dfs(i + 1, j + 1, 0, s, n, path, ans); // j+1 进入下一段，ipVal 重置为 0
  ```

  - 记录切分点，并开启新的段 (`j + 1`)，数值清零。

  #### E. 递归出口 (Base Case)

  Java

  ```
  if (i == n) { // 字符串用完了
      if (j == 4) { // 刚好凑够 4 段
          // 拼装答案 (只有找到合法解时才拼装，节省时间)
          // 使用 path 数组里的切分点来 substring
          ...
          ans.add(...);
      }
      return;
  }

  if (j == 4) { // 还没走完字符串，但已经切了 4 段了 -> 失败
      return;
  }
  ```

  ---

  ### 3. 复杂度分析：为什么是 O(1)?

  这道题的复杂度分析非常特殊。

  - ​**时间复杂度**: $O(1)$ (或者说 $O(|S|)$ 用于复制答案)。

    - 你可能会问：回溯不是指数级吗？
    - ​**原因**：IP 地址非常短！

      - 一共只有 ​**4 段**。
      - 每一段最多 ​**3 个字符**。
      - 所以整个字符串长度如果超过 12，直接就没有解了。
      - 递归树的深度最大是 4，每一层的分支最多也就 3 种情况（切1位、切2位、切3位）。
      - 状态总数是一个很小的常数（不超过 $3^4 = 81$ 种路径）。
  - ​**空间复杂度**: $O(1)$。

    - 递归栈深度固定为 4。

  ### 4. 与“分割回文串”的对比

  |**特性**|**131. 分割回文串**|**93. 复原 IP 地址 (本题)**||||
  | --| -----------------| ------------| --| --| --|
  |**分割段数**|不固定|**固定为 4 段**||||
  |**单段限制**|必须是回文|**数值 0-255，无前导零**||||
  |**状态变量**|​`index`​,`start`|​`index`​,`segment_id`​,`current_val`||||
  |**优化技巧**|DP 预处理回文表|**数字累加 (ipVal) 避免字符串转换**||||

  ### 5. 总结

  这段代码通过维护 `ipVal` 变量，巧妙地解决了两个问题：

  1. ​**数值判断**​：直接比较 `ipVal > 255`​，不需要 `Integer.parseInt`。
  2. ​**前导零判断**​：通过 `ipVal > 0`​ 控制是否允许延长，逻辑极其精简。如果当前是 '0'，`ipVal` 为 0，无法进入“不分割”分支，强制切断，完美符合规则。

  **下一步建议：**

  既然你已经掌握了  **“组合”、“分割”、“子集”**  三大类回溯问题。

  接下来可以挑战一下  **“排列 (Permutation)”**  问题（LeetCode 46 全排列），那里的去重逻辑（LeetCode 47 全排列 II）和这里的逻辑（特别是排序去重）是一脉相承的。需要我为你展示吗？

‍

### 78.子集

[78. 子集 - 力扣（LeetCode）](https://leetcode.cn/problems/subsets/description/)

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(0, nums, path, ans);
        return ans;
    }

    // 选或不选：讨论 nums[i] 是否加入 path
    private void dfs(int i, int[] nums, List<Integer> path, List<List<Integer>> ans) {
        if (i == nums.length) { // 子集构造完毕
            ans.add(new ArrayList<>(path)); // 复制 path
            return;
        }

        // 不选 nums[i]
        dfs(i + 1, nums, path, ans); // 考虑下一个数 nums[i+1] 选或不选

        // 选 nums[i]
        path.add(nums[i]);
        dfs(i + 1, nums, path, ans); // 考虑下一个数 nums[i+1] 选或不选
        path.removeLast(); // path.remove(path.size() - 1);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/subsets/solutions/2059409/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-8tkl/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 78. 子集 (Subsets)**  的标准解法。

  这道题是所有回溯问题的\*\*“祖师爷”\*\*。

  你之前练习的 ​ **“组合总和”** ​、​ **“分割回文串”** ​、​ **“复原 IP”** ​，本质上都是在**求子集**的基础上加上了各种限制条件（比如和为 target、必须是回文、数值范围等）。

  而这道题没有任何限制：它要求找出**所有**可能的组合。

  我为你拆解这个最纯粹的  **“选或不选”**  模型。

  ---

  ### 1. 核心模型：二叉决策树

  对于数组 `nums` 中的每一个数字，我们都只有两个选择：

  1. ​**不选 (Skip)** ：这个数字不进入当前子集。
  2. ​**选 (Pick)** ：这个数字进入当前子集。

  假设 `nums = [1, 2, 3]`​，我们在每一个节点（数字）上做二选一的决策，这会形成一棵完美的​**满二叉树**。

  ### 2. 代码深度拆解

  #### A. 递归函数的定义

  Java

  ```
  // i: 当前轮到决策第 i 个数字 (nums[i])
  // path: 记录当前路径上选了哪些数字
  private void dfs(int i, int[] nums, List<Integer> path, List<List<Integer>> ans)
  ```

  #### B. 递归出口 (Base Case)

  Java

  ```
  if (i == nums.length) { // 所有数字都考虑完了
      ans.add(new ArrayList<>(path)); // 记录答案
      return;
  }
  ```

  - **为什么在这里记录答案？**

    - 在“选或不选”的逻辑中，只有当我们要么选、要么不选地处理完**所有**数字后，我们才得到了一个完整的“决策结果”（即一个子集）。
    - 比如 `[1, 2]`​，我们在 `i=0`​ 选了 1，在 `i=1`​ 选了 2，在 `i=2`​（越界）时，我们确定了 `{1, 2}` 是一个子集。

  #### C. 分支一：不选 (Skip)

  Java

  ```
  // 不选 nums[i]
  dfs(i + 1, nums, path, ans);
  ```

  - 直接跳到 `i + 1`​，`path`​ 保持不变。这对应着二叉树的​**左分支**（或者右分支，取决于画法）。

  #### D. 分支二：选 (Pick)

  Java

  ```
  // 选 nums[i]
  path.add(nums[i]);            // 1. 做选择
  dfs(i + 1, nums, path, ans);  // 2. 递归
  path.removeLast();            // 3. 撤销选择 (回溯)
  ```

  - 把当前数字加入 `path`​，然后跳到 `i + 1`​。这对应着二叉树的​**另一条分支**。
  - ​**回溯**​：当这一支递归走到底（到了 base case）返回后，我们需要把刚才加进去的 `nums[i]` 拿出来，以便回退到上一个状态，去走“不选”的分支（或者返回上一层）。

  ---

  ### 3. 复杂度分析

  这道题的复杂度是所有组合类问题的​**基准**。

  - ​**时间复杂度**: $O(n \cdot 2^n)$。

    - **$2^n$**: 长度为 $n$ 的数组，子集总数是 $2^n$ 个（每个元素都有选/不选 2 种状态）。
    - **$n$**​: 每次记录答案时，`new ArrayList<>(path)` 需要 $O(n)$ 的时间来复制列表。
  - ​**空间复杂度**: $O(n)$。

    - 递归栈的深度为 $n$。
    - ​`path` 数组最长也是 $n$。

  ---

  ### 4. 另一种写法对比：枚举选哪个 (For-loop)

  就像之前的题目一样，子集问题也有两种经典写法。

  **写法一（本题解法）：选或不选 (Input View)**

  - 视角：对于 `nums[i]`，我要不要？
  - 答案收集点：​**递归出口 (**​**​`i == n`​**​ **)** 。

  **写法二：枚举选哪个 (Output View)**

  - 视角：`path`​ 的下一个数字，我可以从 `nums[i...n-1]` 里选哪一个？
  - 答案收集点：​**每次进入递归时**（因为每一个节点本身就是一个合法的子集）。

  **写法二的代码模板：**

  Java

  ```
  private void dfs(int i, int[] nums, List<Integer> path, List<List<Integer>> ans) {
      // 进入节点即记录答案 (空集、[1]、[1,2] 都是子集)
      ans.add(new ArrayList<>(path)); 
      
      for (int j = i; j < nums.length; j++) {
          path.add(nums[j]);
          dfs(j + 1, nums, path, ans);
          path.removeLast();
      }
  }
  ```

  ### 5. 总结：回溯算法的三座大山

  到现在为止，你已经掌握了回溯算法中  **“组合” (Combination)**  和  **“子集” (Subset)**  的核心逻辑。

  让我们复盘一下：

  1. ​**子集 (Subsets)** : 无限制，要所有节点。 ($2^n$)
  2. ​**组合总和 (Combination Sum)** :

     - ​**I**​: 元素可重复选 -\> `dfs(i)`
     - ​**II**​: 元素不可重复 + 有重复元素 -\> `dfs(i+1)`​ + `排序去重`
  3. ​**分割回文 (Partition)** : 元素是“逗号”，选逗号切分。

  **下一步挑战：**

  回溯算法的最后一块拼图是 ​ **“全排列” (Permutations)** 。

  - ​**子集/组合**​: `[1, 2]`​ 和 `[2, 1]`​ 是**同一个**东西（顺序不重要）。
  - ​**排列**​: `[1, 2]`​ 和 `[2, 1]`​ 是**不同**的东西（顺序重要）。

  这会导致代码逻辑有一个关键的变化（不再是 `dfs(i+1)`​，而是需要一个 `boolean[] visited` 数组）。你想看看全排列是怎么写的吗？

‍

### 90.子集II

[90. 子集 II - 力扣（LeetCode）](https://leetcode.cn/problems/subsets-ii/solutions/3036436/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-v0js/)

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        dfs(0, nums, ans, path);
        return ans;
    }

    private void dfs(int i, int[] nums, List<List<Integer>> ans, List<Integer> path) {
        int n = nums.length;
        if (i == n) {
            ans.add(new ArrayList<>(path));
            return;
        }

        // 选 x
        int x = nums[i];
        path.add(x);
        dfs(i + 1, nums, ans, path);
        path.remove(path.size() - 1); // 恢复现场

        // 不选 x，那么后面所有等于 x 的数都不选
        // 如果不跳过这些数，会导致「选 x 不选 x'」和「不选 x 选 x'」这两种情况都会加到 ans 中，这就重复了
        i++;
        while (i < n && nums[i] == x) {
            i++;
        }
        dfs(i, nums, ans, path);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/subsets-ii/solutions/3036436/liang-chong-fang-fa-xuan-huo-bu-xuan-mei-v0js/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 90. 子集 II (Subsets II)**  的解法。

  这道题是 **LeetCode 78. 子集** 的进阶版。区别在于：输入数组 `nums`​ **包含重复元素**​（例如 `[1, 2, 2]`​），但要求的输出结果中 ​**不能包含重复的子集**。

  这道题的去重逻辑和 **LeetCode 40. 组合总和 II** 是完全一样的。

  我为你详细拆解这段代码，特别是它是如何巧妙地利用  **“选或不选”**  的逻辑来避开重复子集的。

  ---

  ### 1. 核心逻辑：排序 + 选或不选

  #### A. 为什么要排序？

  Java

  ```
  Arrays.sort(nums);
  ```

  排序是将相同元素“聚拢”在一起的前提。只有 `[1, 2, 2]` 这样排列，我们才能在决定“不选”第一个 2 的时候，轻易地跨过第二个 2。

  #### B. 递归树的视角

  对于当前的数字 `x = nums[i]`，我们有两个决策分支：

  1. ​**选 x (Pick)** ：

     - 把 `x`​ 加入 `path`。
     - 递归处理下一个位置 `dfs(i + 1, ...)`。
     - ​**注意**​：这里不需要跳过重复元素。因为如果我们选了第一个 2，完全可以再选第二个 2（组成 `[2, 2]`），这是合法的。
  2. ​**不选 x (Skip)** ：

     - ​**关键点**​：如果我们决定**不选**当前的这个 2，那么为了避免重复，后面紧接着的所有 2 ​**都不能选**。
     - ​**原因**：参考“组合总和 II”的逻辑。

       - 方案 A：选第一个 2，不选第二个 2 -\> 得到 `[2]`。
       - 方案 B：不选第一个 2，选第二个 2 -\> 得到 `[2]`。
       - A 和 B 重复了。为了去重，我们强制规定：​**一旦不选前面的，后面的相同元素也必须不选**。

  ---

  ### 2. 代码细节逐行解析

  #### 递归出口 (Base Case)

  Java

  ```
  if (i == n) {
      ans.add(new ArrayList<>(path));
      return;
  }
  ```

  - 这也是  **“选或不选”**  模型的一个特征：​**只有当决策完所有数字后（到达叶子节点），才把结果加入答案**。
  - （注：如果是“枚举选哪个”的写法，通常是进入节点就加答案）。

  #### 分支一：选当前数字

  Java

  ```
  // 选 x
  path.add(x);
  dfs(i + 1, nums, ans, path); // 正常递归下一个
  path.remove(path.size() - 1); // 恢复现场
  ```

  这里和普通子集问题一模一样。

  #### 分支二：不选当前数字 (核心去重)

  Java

  ```
  // 不选 x，那么后面所有等于 x 的数都不选
  i++;
  while (i < n && nums[i] == x) { // 跳过所有重复的 x
      i++;
  }
  dfs(i, nums, ans, path);
  ```

  这段代码翻译过来就是：

  > “既然我决定不要这个 `x`​ 了，那么为了防止以后后悔又去选一个一模一样的 `x`​ 造成重复，我把后面所有的 `x`​ 全部跳过去，直接从下一个**不同**的数值开始考虑。”
  >

  ---

  ### 3. 示例演示 `nums = [2, 2]`

  1. **DFS(0)**  (面临第一个 2):

     - ​**选**​: `path=[2]`​. 递归 ​**DFS(1)** .

       - (面临第二个 2)
       - ​**选**​: `path=[2, 2]`​. 递归 DFS(2) -\> ​**记录**  **​`[2, 2]`​** . 回溯。
       - ​**不选**​: 跳过所有 2 (无后续). 递归 DFS(2) -\> ​**记录**  **​`[2]`​** .
       - 回溯。
     - 回溯 `path=[]`.
     - ​**不选**:

       - 当前是 index 0 的 2。
       - ​`while` 循环：发现 index 1 也是 2，跳过。
       - ​`i` 变成 2。
       - 递归 **DFS(2)**  -\> ​**记录**  **​`[]`​** .

  ​**最终结果**​: `[[2, 2], [2], []]`。

  ​**避免的重复项**​: 如果没有 `while`​ 跳过，在“不选”分支里会单独选第二个 2，导致再次记录一个 `[2]`。

  ---

  ### 4. 复杂度分析

  - ​**时间复杂度**: $O(n \cdot 2^n)$。

    - 最坏情况下（例如数组无重复），有 $2^n$ 个子集，每个子集构造需要 $O(n)$。
    - 有重复元素时，实际分支会被 `while` 循环剪掉很多，速度会快很多。
  - ​**空间复杂度**: $O(n)$。

    - 递归栈深度 $n$。

  ---

  ### 5. 两种子集问题的对比总结

  |**题目**|**78. 子集 (Subsets)**|**90. 子集 II (Subsets II)**||||
  | --| ------------------| ----------------------------------------------| --| --| --|
  |**输入**|无重复元素|**有重复元素**||||
  |**排序**|不强制|**必须排序**||||
  |**不选分支**|直接`dfs(i + 1)`|​**​`while`​**​**跳过重复 ->**​**​`dfs(next_i)`​** ||||
  |**核心思想**|纯粹的二叉树枚举|通过剪枝避免“不选A选B(A\=\=B)”的情况||||

  **下一步挑战：**

  恭喜你！到目前为止，你已经掌握了 **组合、分割、子集** 这三大类回溯问题的  **“普通版”**  和 ​ **“去重版”** 。

  这套  **“选或不选” + “排序后跳过重复”**  的组合拳，是解决所有去重类回溯问题的通解。

  接下来，回溯算法只剩最后一个大类：​**全排列 (Permutations)** 。

  - 在排列问题中，`[1, 2]`​ 和 `[2, 1]` 是不同的。
  - 这意味着我们不能简单地用 `dfs(i+1)` 一路往前走，而是需要回头看。
  - 这会引入一个新的变量：`boolean[] visited`。

  你想继续学习 **全排列** 的回溯套路吗？

‍
