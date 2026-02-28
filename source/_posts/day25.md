---
title: day25
date: '2026-02-07 19:29:37'
updated: '2026-02-07 19:41:50'
permalink: /post/day25-z2vvtcz.html
comments: true
toc: true
---



# day25

# **第七章 回溯算法 part04**

## 491.递增子序列

本题和大家刚做过的 90.子集II 非常像，但又很不一样，很容易掉坑里。

<u>[https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html](https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1EG4y1h78v](https://www.bilibili.com/video/BV1EG4y1h78v)</u>

# 46.全排列

本题重点感受一下，排列问题 与 组合问题，组合总和，子集问题的区别。 为什么排列问题不用 startIndex

<u>[https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html](https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV19v4y1S79W](https://www.bilibili.com/video/BV19v4y1S79W)</u>

## 47.全排列 II

本题 就是我们讲过的 40.组合总和II 去重逻辑 和 46.全排列 的结合，可以先自己做一下，然后重点看一下 文章中 我讲的拓展内容： used[i - 1] \=\= true 也行，used[i - 1] \=\= false 也行

<u>[https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1R84y1i7Tm](https://www.bilibili.com/video/BV1R84y1i7Tm)</u>

下面这三道题都非常难，建议大家一刷的时候 可以适当选择跳过。

因为 一刷 也不求大家能把这么难的问题解决，大家目前能了解一下题目的要求，了解一下解题思路，不求能直接写出代码，先大概熟悉一下这些题，二刷的时候，随着对回溯算法的深入理解，再去解决如下三题。

## 332. 重新安排行程（建议跳过，力扣修改后台数据，应该不能用回溯了）

本题没有录制视频，当初录视频是按照 《代码随想录》出版的目录来的，当时没有这道题所以就没有录制。

<u>[https://programmercarl.com/0332.%E9%87%8D%E6%96%B0%E5%AE%89%E6%8E%92%E8%A1%8C%E7%A8%8B.html](https://programmercarl.com/0332.%E9%87%8D%E6%96%B0%E5%AE%89%E6%8E%92%E8%A1%8C%E7%A8%8B.html)</u>

## 51. N皇后（适当跳过）

N皇后这道题目还是很经典的，一刷的录友们建议看看视频了解了解大体思路 就可以 （如果没时间本次就直接跳过） ，先有个印象，二刷的时候重点解决。

<u>[https://programmercarl.com/0051.N%E7%9A%87%E5%90%8E.html](https://programmercarl.com/0051.N%E7%9A%87%E5%90%8E.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1Rd4y1c7Bq](https://www.bilibili.com/video/BV1Rd4y1c7Bq)</u>

## 37. 解数独（适当跳过）

同样，一刷的录友们建议看看视频了解了解大体思路（如果没时间本次就直接跳过），先有个印象，二刷的时候重点解决。

。

<u>[https://programmercarl.com/0037.%E8%A7%A3%E6%95%B0%E7%8B%AC.html](https://programmercarl.com/0037.%E8%A7%A3%E6%95%B0%E7%8B%AC.html)</u>

[视频讲解：]()​<u>[https://www.bilibili.com/video/BV1TW4y1471V](https://www.bilibili.com/video/BV1TW4y1471V)</u>

## 总结

刷了这么多回溯算法的题目，可以做一做总结了！

<u>[https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E6%80%BB%E7%BB%93.html](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E6%80%BB%E7%BB%93.html)</u>

‍

## 491.递增子序列

[491. 非递减子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/non-decreasing-subsequences/description/)

```java
class Solution {
    private List<Integer> path = new ArrayList<>();
    private List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backtracking(nums,0);
        return res;
    }

    private void backtracking (int[] nums, int start) {
        if (path.size() > 1) {
            res.add(new ArrayList<>(path));
        }

        int[] used = new int[201];
        for (int i = start; i < nums.length; i++) {
            if (!path.isEmpty() && nums[i] < path.get(path.size() - 1) ||
                    (used[nums[i] + 100] == 1)) continue;
            used[nums[i] + 100] = 1;
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.remove(path.size() - 1);
        }
    }
}

//法二：使用map
class Solution {
    //结果集合
    List<List<Integer>> res = new ArrayList<>();
    //路径集合
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        getSubsequences(nums,0);
        return res;
    }
    private void getSubsequences( int[] nums, int start ) {
        if(path.size()>1 ){
            res.add( new ArrayList<>(path) );
            // 注意这里不要加return，要取树上的节点
        }
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=start ;i < nums.length ;i++){
            if(!path.isEmpty() && nums[i]< path.getLast()){
                continue;
            }
            // 使用过了当前数字
            if ( map.getOrDefault( nums[i],0 ) >=1 ){
                continue;
            }
            map.put(nums[i],map.getOrDefault( nums[i],0 )+1);
            path.add( nums[i] );
            getSubsequences( nums,i+1 );
            path.removeLast();
        }
    }
}

作者：代码随想录
链接：https://leetcode.cn/problems/non-decreasing-subsequences/solutions/387905/491-di-zeng-zi-xu-lie-shen-sou-hui-su-xiang-jie-by/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这两段代码是 **LeetCode 491. 非递减子序列 (Non-decreasing Subsequences)**  的两种经典解法。

这道题是回溯算法中的一个​**特例**​，也是一道由于**去重逻辑**独特而容易做错的题目。它与你之前学的“子集 II” (Subsets II) 最大的不同在于：**这道题不能排序！**

我为你详细解析这两段代码的核心逻辑，特别是那个让很多人困惑的 `used` 数组/Map。

---

### 1. 核心难点：为什么不能排序？

在“子集 II”中，我们要去重 `[1, 2, 2]`​，我们先排序，然后用 `if (i > start && nums[i] == nums[i-1])` 来去重。

但在本题中，题目要求找​**子序列 (Subsequence)** ，子序列要求保留原数组的相对顺序。

- 原数组：`[4, 7, 6, 7]`
- 如果你排序变成：`[4, 6, 7, 7]`
- 你就会错误地找到 `[4, 6]`​ 这个组合，但在原数组中 `6`​ 在 `7`​ 后面，`[4, 7]`​ 是合法的，`[4, 6]` 也是合法的（如果原数组里6在4后面）。
- ​**结论**​：**不能改变原数组的顺序，所以不能用排序去重法。**

---

### 2. 解决方案：同层去重 (Level Deduplication)

既然不能排序，我们如何避免重复呢？比如 `[4, 7(a), 6, 7(b)]`。

当我们站在 `4` 这个节点下，准备选下一个数：

1. 选 `7(a)`​ -\> 递归下去找到 `[4, 7(a), ...]`。
2. 选 `6`​ -\> 递归下去。
3. 轮到 `7(b)`​ -\> **停！**  前面已经选过一个 `7`​ 了。既然数值都是 7，`7(a)`​ 能引出的所有结果，`7(b)` 肯定也能引出，再算一遍就是重复。

所以，我们需要一个机制来记录：**在当前这一层（当前递归节点），哪些数字已经使用过了。**

---

### 3. 法一：数组哈希 (Array Hashing) - 性能最优

Java

```
private void backtracking(int[] nums, int start) {
    // 只要长度 > 1，就是合法答案，加入结果集
    if (path.size() > 1) {
        res.add(new ArrayList<>(path));
    }
    
    // 关键点：used 数组定义在递归函数内部！
    // 这意味着它只负责“当前这一层”的去重。
    // 每一层递归都有一个独立的 used 数组。
    int[] used = new int[201]; 
    
    for (int i = start; i < nums.length; i++) {
        // 条件1：!path.isEmpty() && nums[i] < path.get(...) 
        // -> 违反了非递减规则，跳过。
        
        // 条件2：used[nums[i] + 100] == 1 
        // -> 本层已经用过这个数值了，跳过（去重）。
        if (!path.isEmpty() && nums[i] < path.get(path.size() - 1) ||
            (used[nums[i] + 100] == 1)) {
                continue;
        }
        
        used[nums[i] + 100] = 1; // 标记：本层这个数用过了
        path.add(nums[i]);
        backtracking(nums, i + 1);
        path.remove(path.size() - 1);
        
        // 注意：这里不需要回溯 used 数组！
        // 因为 used 只是为了记录“本层”是否选过。下一轮循环（i++）依然还在“本层”。
    }
}
```

**为什么是** **​`int[201]`​** ​ **？**

- 题目约束：`-100 <= nums[i] <= 100`。
- 为了把负数映射到数组索引，我们需要加上偏移量 `100`。
- ​`-100`​ 对应索引 `0`​，`100`​ 对应索引 `200`。总共需要 201 个空间。
- 这种方法比 `HashMap` 快很多，因为没有哈希冲突和对象创建的开销。

---

### 4. 法二：HashMap 去重

Java

```
// 使用过了当前数字
if ( map.getOrDefault( nums[i],0 ) >=1 ){
    continue;
}
map.put(nums[i], map.getOrDefault( nums[i],0 )+1);
```

原理和数组一模一样，只是用 `HashMap` 替代了数组。

- ​**适用场景**​：当 `nums[i]`​ 的范围非常大（比如 `Integer.MAX_VALUE`）或者非常稀疏时，数组存不下，必须用 Map。
- ​**缺点**：在本题特定数据范围下，比数组慢。

---

### 5. 易错点总结

1. ​**不要排序**：一旦排序，虽然去重方便了，但破坏了子序列的相对顺序。
2. ​**​`used`​**​ **数组的位置**：

   - ​**错误写法**​：把 `used` 定义成全局变量。那样会导致不同层级之间互相干扰。
   - ​**正确写法**​：`used`​ 必须定义在 `backtracking`​ 函数​**内部**。每一层递归代表树的一层，每一层需要一个干净的“记事本”来记录这一层选了谁。
3. ​**不需要** **​`return`​**：

   - 我们在收集结果时 `if (path.size() > 1) res.add(...)`​ 之后​**不能 return**。
   - 因为 `[4, 6]`​ 是答案，但往下继续递归可能还有 `[4, 6, 7]` 也是答案。这和“分割问题”不同，这里我们要收集树上的所有节点（除了根节点）。

### 总结

这道题是回溯算法中  **“同层去重”**  的教科书级案例。

- ​**Subsets II (子集 II)** ​：可以排序 -\> 使用 `i > start && nums[i] == nums[i-1]` 去重。
- **Subsequences (本题)** ：不可排序 -\> 使用 `HashSet`​ 或 `int[] used` 在每一层记录已使用的数值。

## 46.全排列

[46. 全排列 - 力扣（LeetCode）](https://leetcode.cn/problems/permutations/description/)

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        int n = nums.length;
        List<Integer> path = Arrays.asList(new Integer[n]); // 所有排列的长度都是一样的 n
        boolean[] onPath = new boolean[n];
        List<List<Integer>> ans = new ArrayList<>();

        dfs(0, nums, ans, path, onPath);
        return ans;
    }

    // 枚举 path[i] 填 nums 的哪个数
    private void dfs(int i, int[] nums, List<List<Integer>> ans, List<Integer> path, boolean[] onPath) {
        if (i == nums.length) {
            ans.add(new ArrayList<>(path));
            return;
        }

        for (int j = 0; j < nums.length; j++) {
            if (!onPath[j]) {
                path.set(i, nums[j]); // 从没有选的数字中选一个
                onPath[j] = true; // 已选上
                dfs(i + 1, nums, ans, path, onPath);
                onPath[j] = false; // 恢复现场
                // 注意 path 无需恢复现场，因为排列长度固定，直接覆盖就行
            }
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/permutations/solutions/2079585/hui-su-bu-hui-xie-tao-lu-zai-ci-jing-que-6hrh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 46. 全排列 (Permutations)**  的解法。

  恭喜你！你已经集齐了回溯算法的“三大金刚”：

  1. **组合/子集** (Combinations/Subsets): 用 `start` 索引控制，不回头。
  2. **分割** (Partitioning): 用 `start` 索引控制，切隔板。
  3. **排列** (Permutations): ​**每一次都从头选，但要跳过已选的**。

  这段代码正是全排列的标准模板，但灵神（EndlessCheng）在这里使用了一个非常巧妙的​**覆盖式写法**​（`path.set`），我来为你详细拆解。

  ---

  ### 1. 核心差异：为什么没有 `start` 索引？

  在之前的“组合”问题中，我们用的都是 `dfs(index, ...)`​ 且循环从 `j = index` 开始。

  - 因为组合 `[1, 2]`​ 和 `[2, 1]` 是一样的，所以选了 1 之后，下次只能选 2，不能回头选 1。

  **但是在排列问题中：**

  - ​`[1, 2]`​ 和 `[2, 1]`​ 是**不同**的。
  - 这意味着：当我们填第二个空位时，我们依然可以考虑数组里的**所有**数字（除了刚才填在第一个位置的那个）。

  所以，代码中的循环是：

  Java

  ```
  for (int j = 0; j < nums.length; j++) // 每次都从 0 开始扫描整个数组
  ```

  ### 2. 核心机制：`onPath` (visited) 数组

  既然每次都从头扫描，怎么防止 `[1, 1, 1]` 这种把同一个数字用了多次的情况出现呢？

  答案就是 `boolean[] onPath`。

  - ​**​`onPath[j] == true`​**​: 表示 `nums[j]`​ 已经在当前的排列路径上了，​**跳过**。
  - ​**​`onPath[j] == false`​**​: 表示 `nums[j]`​ 还没被选，​**可以选**。

  ### 3. 代码细节：独特的“覆盖”优化

  这部分是灵神代码的特色，和普通的 `add/remove` 写法不太一样。

  #### A. 初始化固定长度

  Java

  ```
  // 初始化一个长度为 n 的列表，里面全是 null
  List<Integer> path = Arrays.asList(new Integer[n]); 
  ```

  他预先开辟好了坑位。比如 `n=3`​，`path`​ 初始就是 `[null, null, null]`。

  #### B. 递归逻辑：填空

  Java

  ```
  // i: 当前正在填第几个空位 (0 ~ n-1)
  private void dfs(int i, ...) {
      if (i == nums.length) { ... } // 填满了

      for (int j = 0; j < nums.length; j++) {
          if (!onPath[j]) { // 如果 nums[j] 没被用过
              
              path.set(i, nums[j]); // 【关键】直接覆盖第 i 个位置
              onPath[j] = true;     // 标记占用
              
              dfs(i + 1, ...);      // 填下一个空位 (i + 1)
              
              onPath[j] = false;    // 回溯：释放标记
              // 【注意】这里不需要 path.remove()！
              // 因为下次循环或者回溯回来，会再次调用 path.set(i, ...)
              // 新的值会直接把旧值覆盖掉。
          }
      }
  }
  ```

  ​**对比普通写法**：

  - ​**普通写法**​：`path`​ 是空的 -\> `path.add(val)`​ -\> 递归 -\> `path.removeLast()`。
  - ​**本题写法**​：`path`​ 是满的(null) -\> `path.set(i, val)`​ -\> 递归 -\> (不做操作，等着被覆盖)。

  这种写法稍微省去了一些 `ArrayList` 动态扩容和删除元素的开销，效率略高，但逻辑本质是一样的。

  ---

  ### 4. 复杂度分析

  全排列的复杂度是非常高的，属于阶乘级别。

  - ​**时间复杂度**: $O(n \cdot n!)$。

    - $n!$ 是排列的总数（第一次选 $n$ 个，第二次选 $n-1$ 个...）。
    - $n$ 是每次复制答案 `new ArrayList<>(path)` 的开销。
  - ​**空间复杂度**: $O(n)$。

    - 递归栈深度 $n$。
    - ​`onPath` 数组长度 $n$。

  ---

  ### 5. 总结：回溯算法全家桶

  到这里，你已经掌握了回溯算法的核心分类：

  |**问题类型**|**核心逻辑**|**关键变量**||||
  | --| --------------| ------------| --| --| --|
  |**子集 / 组合**|选后面剩下的|​`start`​(或`i`)||||
  |**分割 (Partition)**|选逗号切分|​`start`​,`i`||||
  |**全排列 (Permutation)**|**选还没被用的**|​**​`boolean[] visited`​**​ **(从头遍历)**||||

  **最后一步挑战：**

  如果 `nums`​ 里包含重复数字，比如 `[1, 1, 2]`，求它的全排列（不能有重复结果）。

  这道题是 ​**LeetCode 47. 全排列 II**。

  它结合了：

  1. 本题的 `boolean[] visited` 逻辑（因为是排列）。
  2. 之前“子集 II”的 **排序 + 去重** 逻辑（因为有重复元素）。

  你需要我展示那道“集大成者”的题目吗？那是回溯算法的**毕业考**。

# 47.全排列 II

[47. 全排列 II - 力扣（LeetCode）](https://leetcode.cn/problems/permutations-ii/description/)

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);

        int n = nums.length;
        List<Integer> path = Arrays.asList(new Integer[n]); // 所有排列的长度都是 n
        boolean[] onPath = new boolean[n]; // onPath[j] 表示 nums[j] 是否已经填入排列
        List<List<Integer>> ans = new ArrayList<>();

        dfs(0, nums, path, onPath, ans);
        return ans;
    }

    // i 表示当前要填排列的第几个数
    private void dfs(int i, int[] nums, List<Integer> path, boolean[] onPath, List<List<Integer>> ans) {
        if (i == nums.length) { // 填完了
            ans.add(new ArrayList<>(path));
            return;
        }

        // 枚举 nums[j] 填入 path[i]
        for (int j = 0; j < nums.length; j++) {
            // 如果 nums[j] 已填入排列，continue
            // 如果 nums[j] 和前一个数 nums[j-1] 相等，且 nums[j-1] 没填入排列，continue
            if (onPath[j] || j > 0 && nums[j] == nums[j - 1] && !onPath[j - 1]) {
                continue;
            }
            path.set(i, nums[j]); // 填入排列
            onPath[j] = true; // nums[j] 已填入排列（注意标记的是下标，不是值）
            dfs(i + 1, nums, path, onPath, ans); // 填排列的下一个数
            onPath[j] = false; // 恢复现场
            // 注意 path 无需恢复现场，因为排列长度固定，直接覆盖 path[i] 就行
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/permutations-ii/solutions/3059690/ru-he-qu-zhong-pythonjavacgojsrust-by-en-zlwl/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 恭喜你！这正是回溯算法的  **“最终 BOSS”**  —— **LeetCode 47. 全排列 II (Permutations II)** 。

  这道题集齐了回溯算法的所有难点：

  1. **排列问题**：需要回头看，每次都从头遍历（不能用 `start` 索引）。
  2. **重复元素**：输入数组 `nums`​ 里有重复数字（如 `[1, 1, 2]`）。
  3. **去重**：输出结果不能包含重复的排列（如 `[1, 1, 2]` 只能出现一次）。

  灵神（EndlessCheng）的这段代码依然采用了经典的  **“排序 + 剪枝”**  策略。我为你详细拆解那行最核心、最让人头秃的去重代码。

  ---

  ### 1. 核心去重逻辑：那行很长的 `if`

  代码中最关键的一行是：

  ```java
  if (onPath[j] || (j > 0 && nums[j] == nums[j - 1] && !onPath[j - 1])) {
      continue;
  }

  ```
  这里包含了两个条件的 **OR (**​ **​`||`​** ​ **)**  关系，我们分开看：

  #### A. `onPath[j]` (基本排列逻辑)

  - **含义**：当前数字 `nums[j]`​ 在 **当前的递归路径上** 已经被用过了。
  - **作用**：防止同一个位置的数字在一个排列中被重复使用（比如 `nums=[1, 2]`​，不能生成 `[1, 1]`）。

  #### B. `j > 0 && nums[j] == nums[j - 1] && !onPath[j - 1]` (核心去重)

  这是全排列去重的灵魂。我们把它拆解成三个子条件：

  1. ​`j > 0`​: 保证 `j-1` 不越界。
  2. ​`nums[j] == nums[j - 1]`: 当前数字和前一个数字相同（说明发生了重复）。
  3. ​ **​`!onPath[j - 1]`​** ​  **(最难理解的点)** : 前一个相同的数字  **“没被用过”** 。

  **人话翻译**：

  > “如果轮到我选 `nums[j]`​ 了，我发现它和前面的 `nums[j-1]`​ 长得一模一样，而且 `nums[j-1]`​ **居然没在当前的路径里**（说明它被跳过或者回溯了），那我也不选。”
  >

  ---

  ### 2. 图解：为什么要 `!onPath[j-1]`？

  假设 `nums = [1, 1, 2]`。我们将两个 1 标记为 **** (索引0) 和 **** (索引1)。

  我们希望生成的排列是 `[1, 1, 2]`​, `[1, 2, 1]`​, `[2, 1, 1]`​。  
  我们**不希望**生成两份 `[1, 1, 2]`（一份是先  后 ，另一份是先  后 ）。

  **为了去重，我们立下一个规矩**：

  > **对于相同的数字，我们强制规定它们的相对顺序：必须先用 ，再用 。**   
  > 如果  还没用，绝不允许先用 ！
  >

  **场景模拟：**

  1. **第一层递归 (填第 0 个空位)** :

  - 尝试选 ****: 合法。标记 `onPath[0]=true`。递归下一层。
  - (回溯回来，`onPath[0]`​ 变回 `false`)
  - 尝试选 ****:
  - ​`nums[1] == nums[0]` (是重复数)。
  - ​`onPath[0]`​ 是 `false` (说明  刚才被撤销了，现在没用它)。
  - **触发**  **​`!onPath[j-1]`​** ​ **剪枝！**  -> `continue`。
  - **含义**：因为  没被用，所以  没资格做“老大”（第一个被选的 1）。

  2. **第二层递归 (填第 1 个空位，假设上一层选了 )** :

  - 当前状态：`path=[1_a]`​, `onPath=[true, false, false]`.
  - 尝试选 ****: `onPath[0]`​ 是 `true` -> 跳过。
  - 尝试选 ****:
  - ​`nums[1] == nums[0]`。
  - ​`onPath[0]`​ 是 `true` (** 正在被使用**)。
  - **不触发剪枝！**  -> 可以选 。
  - **结果**：生成 `[1_a, 1_b, ...]`。这是合法的。

  **结论**：`!onPath[j-1]`​ 实际上是在进行 **树层去重 (Level Deduplication)** 。它保证了在同一层级（同一个父节点下），如果前面有个一样的数没选，那你也不能选。

  ---

  ### 3. 代码中的其他细节

  - ​**​`Arrays.sort(nums)`​** :
  - 这是去重的前提。只有排序了，重复的元素才会挨在一起，`nums[j] == nums[j-1]` 才能生效。
  - ​**​`path`​**​ **的覆盖操作**:
  - 和上一题（无重复全排列）一样，灵神使用了 `path.set(i, nums[j])`​ 覆盖写法，效率略高于 `add/remove`​，且不需要显式回溯 path 的内容（只需要回溯 `onPath` 状态）。

  ### 4. 复杂度分析

  - **时间复杂度**: 。虽然有剪枝，但在最坏情况下（所有元素都不相同），复杂度依然是阶乘级。
  - **空间复杂度**: 。

  ### 5. 总结：回溯算法通关

  至此，你已经完全掌握了回溯算法的核心体系：

  1. **基本子集/组合** (`start`​ 索引): `[1, 2]`​ 和 `[2, 1]` 算一样。
  2. **基本全排列** (`visited`​ 数组): `[1, 2]`​ 和 `[2, 1]` 算不一样。
  3. **去重技巧** (适用于所有类型):

  - **排序** `Arrays.sort`。
  - **剪枝** `if (j > 0 && nums[j] == nums[j-1] && ...)`。
  - 组合/子集去重： **​`...`​** ​ 部分不需要，因为有 `start` 限制自然不会回头。
  - 全排列去重： **​`...`​** ​ 部分是 `!onPath[j-1]`，因为每次从头搜，必须限制相对顺序。

  **最后的最后：**   
  你现在已经具备了解决 90% 回溯面试题的能力。  
  如果想巩固，可以把 **LeetCode 46 (全排列)** , **47 (全排列 II)** , **78 (子集)** , **90 (子集 II)**  这四道题放在一起，用同样的模板（回溯三部曲）默写一遍，体会它们之间微小的差异。

  需要我为你整理一个**回溯算法终极模板对比表**作为结束吗？
