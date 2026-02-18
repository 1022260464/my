---
title: day27
date: '2026-02-07 19:42:22'
updated: '2026-02-18 18:37:48'
permalink: /post/day26-1bewst.html
comments: true
toc: true
---



# day27

# 第八章 贪心算法 part01

贪心算法其实就是没有什么规律可言，所以大家了解贪心算法 就了解它没有规律的本质就够了。

不用花心思去研究其规律， **没有思路就立刻看题解**。

基本贪心的题目 有两个极端，要不就是特简单，要不就是死活想不出来。

学完贪心之后再去看动态规划，就会了解贪心和动规的区别。

# 详细布置

## 理论基础

<u>[https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)</u>

## 455.分发饼干

<u>[https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html](https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html)</u>

## 376. 摆动序列

<u>[https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html](https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html)</u>

## 53. 最大子序和

<u>[https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html)</u>

# 455.分发饼干

[455. 分发饼干 - 力扣（LeetCode）](https://leetcode.cn/problems/assign-cookies/description/)

```java
class Solution {
    int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int i = 0;
        for (int x : s) {
            if (i < g.length && g[i] <= x) {
                i++;
            }
        }
        return i;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/assign-cookies/solutions/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这段代码是 **LeetCode 455. 分发饼干 (Assign Cookies)**  的解法。

  这道题是 **贪心算法 (Greedy Algorithm)**  的入门级经典题目。

  灵茶山艾府（EndlessCheng）的这段代码写得非常​**精简**​，利用了 Java 的增强 `for` 循环混搭双指针的思想。下面我为你详细拆解。

  ---

  ### 1. 核心策略：贪心 (Greedy)

  ​**生活常识类比**：

  假设你有一堆饼干和一堆孩子。

  - 为了让最多的孩子开心，你应该怎么分？
  - ​**策略**​：把**最小**的饼干，分给**胃口最小**的孩子。

    - 如果你把大饼干给了胃口小的孩子，那胃口大的孩子就没有大饼干吃了（浪费了尺寸）。
    - 如果你把小饼干给胃口大的孩子，他吃不饱（浪费了饼干）。

  所以，核心在于：​ **“小饼干喂小胃口，大饼干喂大胃口”** 。

  ### 2. 代码逻辑图解

  #### A. 排序 (Sorting)

  Java

  ```
  Arrays.sort(g); // g: 孩子的胃口 (Greed factor)
  Arrays.sort(s); // s: 饼干的尺寸 (Size)
  ```

  - 这一步是必须的。只有排好序，我们才能从“最小”的开始匹配。

  #### B. 遍历饼干 (Iterating Cookies)

  这是灵神写法的精髓：**他选择遍历饼干，而不是遍历孩子。**

  Java

  ```
  int i = 0; // i 指向当前胃口最小的、还没吃饱的孩子
  for (int x : s) { // x 是当前拿到的这块饼干
      // 逻辑：
      // 1. i < g.length: 还有孩子没吃饱
      // 2. g[i] <= x: 当前这块饼干 x 能满足当前这个孩子 g[i]
      if (i < g.length && g[i] <= x) {
          i++; // 这个孩子满足了，指向下一个胃口稍大的孩子
      }
      // 隐藏逻辑 (Else):
      // 如果 g[i] > x，说明这块饼干太小了，连胃口最小的孩子都喂不饱。
      // 那这块饼干就是废弃的，循环继续，拿下一块更大的饼干试试。
  }
  ```

  ### 3. 为什么要这样写？

  通常的双指针写法是这样的：

  Java

  ```
  // 普通写法
  int child = 0;
  int cookie = 0;
  while (child < g.length && cookie < s.length) {
      if (g[child] <= s[cookie]) {
          child++;
      }
      cookie++; // 无论是否满足，饼干都要消耗掉（或跳过）
  }
  return child;
  ```

  ​**灵神的写法优势**：

  1. ​**代码更短**​：利用 `for (int x : s)`​ 省去了 `cookie++` 的手动操作。
  2. ​**逻辑清晰**：

     - 我们拿着每一块饼干去问：“能不能喂饱当前排队的第一个孩子？”
     - 能 -\> `i++`。
     - 不能 -\> 饼干扔掉（循环自动处理），下一块。

  ### 4. 复杂度分析

  - ​**时间复杂度**：$O(N \log N + M \log M)$。

    - $N$ 和 $M$ 分别是孩子和饼干的数量。
    - 排序消耗了绝大部分时间。
    - 遍历过程是线性的 $O(N + M)$（双指针各走一遍），比起排序可以忽略不计。
  - ​**空间复杂度**：$O(\log N + \log M)$。

    - Java 的 `Arrays.sort` 使用快排/双轴快排，需要一定的栈空间。

  ### 5. 总结

  这道题是  **“排序 + 双指针 + 贪心”**  的完美结合。

  - **排序**是为了让贪心策略（局部最优 \= 全局最优）成立。
  - **双指针**是为了在线性时间内完成匹配。

  **下一步：**

  既然你已经看过了 **贪心** 和 ​**回溯**。

  你想挑战一道稍微难一点的贪心题目吗？比如 ​**LeetCode 55. 跳跃游戏 (Jump Game)** ？那道题也是贪心的经典，而且不需要排序，是对思维的另一种锻炼。

## 376. 摆动序列

[376. 摆动序列 - 力扣（LeetCode）](https://leetcode.cn/problems/wiggle-subsequence/solutions/518651/376-bai-dong-xu-lie-tan-xin-jing-dian-ti-vyxt/)

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length <= 1) {
            return nums.length;
        }
        //当前差值
        int curDiff = 0;
        //上一个差值
        int preDiff = 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            //得到当前差值
            curDiff = nums[i] - nums[i - 1];
            //如果当前差值和上一个差值为一正一负
            //等于0的情况表示初始时的preDiff
            if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
                count++;
                preDiff = curDiff;
            }
        }
        return count;
    }
}

// DP
class Solution {
    public int wiggleMaxLength(int[] nums) {
        // 0 i 作为波峰的最大长度
        // 1 i 作为波谷的最大长度
        int dp[][] = new int[nums.length][2];

        dp[0][0] = dp[0][1] = 1;
        for (int i = 1; i < nums.length; i++){
            //i 自己可以成为波峰或者波谷
            dp[i][0] = dp[i][1] = 1;

            for (int j = 0; j < i; j++){
                if (nums[j] > nums[i]){
                    // i 是波谷
                    dp[i][1] = Math.max(dp[i][1], dp[j][0] + 1);
                }
                if (nums[j] < nums[i]){
                    // i 是波峰
                    dp[i][0] = Math.max(dp[i][0], dp[j][1] + 1);
                }
            }
        }

        return Math.max(dp[nums.length - 1][0], dp[nums.length - 1][1]);
    }
}

作者：代码随想录
链接：https://leetcode.cn/problems/wiggle-subsequence/solutions/518651/376-bai-dong-xu-lie-tan-xin-jing-dian-ti-vyxt/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这两段代码分别展示了 **LeetCode 376. 摆动序列 (Wiggle Subsequence)**  的两种经典解法：**贪心算法 (Greedy)**  和 ​**动态规划 (Dynamic Programming)** 。

  这道题的核心目标是：在一个序列中，找到最长的子序列，使得该子序列中相邻元素的差值正负交替（即“一上一下”的摆动）。

  下面我为你详细解析这两种解法的思路和优劣。

  ---

  ### 解法一：贪心算法 (Greedy Algorithm) - 推荐

  这是本题最高效、最巧妙的解法。

  #### 1. 核心思想：找“峰”和“谷”

  摆动序列最长的子序列，其实就是原序列波动的​**转折点**（峰值和谷值）的总数。

  贪心策略很简单：​**只统计“峰”和“谷”的数量**，忽略单调递增或单调递减中间的元素。

  #### 2. 代码逻辑解析

  Java

  ```
  // 关键变量
  int curDiff = 0;  // 当前坡度 (nums[i] - nums[i-1])
  int preDiff = 0;  // 上一次记录的坡度
  int count = 1;    // 结果计数，默认序列最右端有一个峰/谷，或者只有1个元素时为1
  ```

  - ​**遍历逻辑**​：从第 2 个元素开始遍历 (`i = 1`)。
  - ​**判断条件**​：  
    ​`if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0))`

    - ​`curDiff > 0`: 当前是上升趋势。
    - ​`preDiff <= 0`: 之前是下降趋势（或者是初始状态 0）。
    - ​**组合起来**​：这就形成了一个 ​ **“谷” (Valley)** ​。反之则形成 ​ **“峰” (Peak)** 。
  - ​**​`preDiff`​**​ **的更新位置（重点！）** ：  
    Java

    ```
    if (...) {
        count++;
        preDiff = curDiff; // 注意：只在出现摆动变化时才更新 preDiff
    }
    ```

    **为什么要写在 if 里面？**

    为了处理 **平坡** 的情况。

    例如 `[1, 2, 2, 2, 3, 1]`。

    - 当遇到 `2, 2`​ 时，`curDiff`​ 是 0，不进 if，`preDiff` 保持为正（之前的上升趋势）。
    - 当遇到 `2, 3`​ 时，`curDiff`​ 是正，但 `preDiff` 也是正，不进 if。
    - 直到遇到 `3, 1`​，`curDiff`​ 是负，`preDiff`​ 依然保持着最初的“正”（代表之前的上升趋势），此时判定为“峰”，`count++`。
    - 这样就成功忽略了中间平坦和单调的部分，直接连接了起点和终点。

  #### 3. 复杂度

  - ​**时间复杂度**：$O(N)$，只需遍历一次数组。
  - ​**空间复杂度**：$O(1)$，只用了几个变量。

  ---

  ### 解法二：动态规划 (Dynamic Programming)

  虽然代码随想录提供的这个 DP 版本是 $O(N^2)$ 的（类似最长递增子序列 LIS 的写法），但它更符合动态规划的标准推导思维。

  #### 1. 状态定义

  使用二维数组 `dp[i][2]`：

  - ​`dp[i][0]`​: 以 `nums[i]`​ 结尾，且 `nums[i]`​ 是 ​**波峰**​（即最后一步是上升 `nums[j] < nums[i]`）的最长摆动子序列长度。
  - ​`dp[i][1]`​: 以 `nums[i]`​ 结尾，且 `nums[i]`​ 是 ​**波谷**​（即最后一步是下降 `nums[j] > nums[i]`）的最长摆动子序列长度。

  #### 2. 状态转移

  我们需要两层循环。外层 `i`​ 遍历当前元素，内层 `j`​ 遍历 `0`​ 到 `i-1` 的所有元素。

  - ​**如果是波峰 (**​**​`nums[j] < nums[i]`​** ​ **)** ​：  
    说明 `i`​ 是从 `j`​ “升”上来的。为了构成摆动，`j`​ 必须是一个波谷。  
    ​`dp[i][0] = Math.max(dp[i][0], dp[j][1] + 1);`
  - ​**如果是波谷 (**​**​`nums[j] > nums[i]`​** ​ **)** ​：  
    说明 `i`​ 是从 `j`​ “降”下来的。为了构成摆动，`j`​ 必须是一个波峰。  
    ​`dp[i][1] = Math.max(dp[i][1], dp[j][0] + 1);`

  #### 3. 复杂度

  - ​**时间复杂度**：$O(N^2)$。因为有双重循环。
  - ​**空间复杂度**：$O(N)$（如果只存长度）或 $O(N^2)$（如代码所示）。

  #### 4. 优化提示

  其实 DP 写法也可以优化到 $O(N)$。

  我们不需要回顾 `j`，只需要记录“当前为止最大的波峰长度”和“当前为止最大的波谷长度”。

  Java

  ```
  // O(N) 的 DP 写法（实际上逻辑等同于贪心）
  int up = 1;
  int down = 1;
  for (int i = 1; i < nums.length; i++) {
      if (nums[i] > nums[i-1]) {
          up = down + 1; // 既然上升了，就在之前“下降”的基础上+1
      } else if (nums[i] < nums[i-1]) {
          down = up + 1; // 既然下降了，就在之前“上升”的基础上+1
      }
  }
  return Math.max(up, down);
  ```

  ---

  |**特性**|**解法一：贪心**|**解法二：DP**||||
  | --| ------------------------------------| -------------------------| --| --| --|
  |**思维难度**|较高（需要理解波峰波谷和差值逻辑）|中等（标准的 LIS 变体）||||
  |**时间复杂度**|O(N)|O(N^2)||||

   **|** 实际应用 **|贪心首选**效率极高|DP次选，适合数据量较小的情况|

  ​**建议**​：面试或解题时，优先掌握 ​**贪心解法**​。它不仅代码短，而且能体现出你对“局部最优推导全局最优”的理解。对于平坡（`preDiff` 更新时机）的处理是贪心解法的唯一的坑。

# 53. 最大子序和

[53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/solutions/2361770/53-zui-da-zi-shu-zu-he-dong-tai-gui-hua-bvkq9/)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int res = nums[0];
        for(int i = 1; i < nums.length; i++) {
            nums[i] += Math.max(nums[i - 1], 0);
            res = Math.max(res, nums[i]);
        }
        return res;
    }
}

作者：Krahets
链接：https://leetcode.cn/problems/maximum-subarray/solutions/2361770/53-zui-da-zi-shu-zu-he-dong-tai-gui-hua-bvkq9/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这是 **LeetCode 53. 最大子数组和 (Maximum Subarray)**  的一种非常极致的解法。

  作者 **Krahets (K神)**  以代码精炼著称。这段代码通过 **原地修改数组 (In-place Modification)**  的方式，实现了 ​**动态规划 (Dynamic Programming)** ​，也就是大名鼎鼎的 **Kadane 算法 (Kadane's Algorithm)**  的变体。

  我为你拆解这段代码的每一个细节。

  ---

  ### 1. 核心思想：要么加入前任，要么自立门户

  这道题的核心在于：**遍历到第** **​`i`​**​ **个数时，我要不要带上前面的累加和？**

  设 $dp[i]$ 为 **以第** **​`i`​**​ **个元素结尾** 的最大子数组和。

  状态转移方程通常是：

  $$
  dp[i] = \max(nums[i], \quad dp[i-1] + nums[i])
  $$

  Krahets 的代码把这个方程简化为了：

  ​`nums[i] += Math.max(nums[i-1], 0)`

  **逻辑翻译：**

  1. ​**如果前面的累加和 (**​**$nums[i-1]$**​ **) 是正数**：

     - 也就是 `nums[i-1] > 0`。
     - ​**决策**​：“以前的收益是正的，对我这个新来的有增益，那我就**加入**前面的子数组。”
     - ​**操作**​：`nums[i] = nums[i] + nums[i-1]`。
  2. ​**如果前面的累加和 (**​**$nums[i-1]$**​ **) 是负数或 0**：

     - 也就是 `nums[i-1] <= 0`。
     - ​**决策**​：“以前的收益是负资产，带上它只会拖累我。那我就**抛弃**前面，从我这里**重新开始**一个新的子数组。”
     - ​**操作**​：`nums[i] = nums[i] + 0` （即保持原样，自己当头）。

  ---

  ### 2. 代码执行演示

  假设输入 `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`

  |**索引 i**|**当前值 nums[i]**|**前一个累加和 nums[i-1]**|**决策 (加还是不加?)**|**更新后的 nums[i] (即 dp[i])**|**当前最大 res**|||||||
  | -----| -----| -----------| --------| --------------| -----------| --| --| --| --| --| --|
  |0|-2|N/A|(初始)|-2|-2|||||||
  |1|1|-2 (负数)|**不加**(加0)|**1**(从这重开)|1|||||||
  |2|-3|1 (正数)|**加**|-3 + 1 \= **-2**|1|||||||
  |3|4|-2 (负数)|**不加**(加0)|**4**(从这重开)|4|||||||
  |4|-1|4 (正数)|**加**|-1 + 4 \=**3**|4|||||||
  |5|2|3 (正数)|**加**|2 + 3 \=**5**|5|||||||
  |6|1|5 (正数)|**加**|1 + 5 \=**6**|**6**(最大值!)|||||||
  |...|...|...|...|...|...|||||||

  最终返回 `res = 6`。

  ---

  ### 3. 代码细节解析

  Java

  ```
  public int maxSubArray(int[] nums) {
      int res = nums[0]; // 初始化结果为第一个元素（防止全是负数的情况返回0）
      
      // 从第二个元素开始遍历
      for(int i = 1; i < nums.length; i++) {
          
          // 【核心代码】
          // 直接修改 nums[i] 来存储以 i 结尾的最大和
          // 如果 nums[i-1] > 0，就加进来；否则加 0 (相当于重置)
          nums[i] += Math.max(nums[i - 1], 0);
          
          // 每次算出一个新的 nums[i] (即 dp[i])，都更新全局最大值
          res = Math.max(res, nums[i]);
      }
      return res;
  }
  ```
  ### 4. 复杂度分析

  - ​**时间复杂度**: $O(N)$。只遍历了一次数组。
  - ​**空间复杂度**: $O(1)$。

    - 通常动态规划需要开一个 $O(N)$ 的 `dp` 数组。
    - Krahets 这里直接复用了 `nums` 数组（原地算法），省去了额外空间。

  ### 5. 面试提示

  虽然这个解法非常巧妙且节省空间，但在面试中需要注意一点：

  - ​**修改了输入数据**​：这段代码执行完后，原数组 `nums` 的内容就被破坏了（变成了前缀和/DP数组）。
  - ​**沟通技巧**​：如果面试官问“能不能不修改原数组？”，你就需要说：“如果不允许修改原数组，我们可以定义两个变量 `pre`​ 和 `maxAns` 来滚动更新，空间依然是 $O(1)$。”

  **不修改原数组的版本 (滚动变量)：**

  Java

  ```
  public int maxSubArray(int[] nums) {
      int pre = 0, maxAns = nums[0];
      for (int x : nums) {
          // pre 表示以“前一个数”结尾的最大子数组和
          pre = Math.max(pre + x, x); 
          maxAns = Math.max(maxAns, pre);
      }
      return maxAns;
  }
  ```
  ​**总结**​：Krahets 的这个写法是 **Kadane 算法** 最直观的“原地 DP”实现，逻辑非常霸道且高效。理解了“正数增益，负数拖累”，就理解了这道题。
