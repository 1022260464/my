---
title: day38
date: '2026-02-20 20:36:45'
updated: '2026-02-20 20:47:56'
permalink: /post/day38-znfmtn.html
comments: true
toc: true
---



# day38

# 第九章 动态规划part06

### ** 322. 零钱兑换  **

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

这句话结合本题 大家要好好理解。

视频讲解：<u>[https://www.bilibili.com/video/BV14K411R7yv](https://www.bilibili.com/video/BV14K411R7yv)</u>

<u>[https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html)</u>

### **279.完全平方数  **

本题 和 322. 零钱兑换 基本是一样的，大家先自己尝试做一做

视频讲解：<u>[https://www.bilibili.com/video/BV12P411T7Br](https://www.bilibili.com/video/BV12P411T7Br)</u>

<u>[https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html](https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html)</u>

### **139.单词拆分 **

视频讲解：<u>[https://www.bilibili.com/video/BV1pd4y147Rh](https://www.bilibili.com/video/BV1pd4y147Rh)</u>

<u>[https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html](https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html)</u>

### ** 关于多重背包，你该了解这些！ **

<u>[https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html)</u>

### ** 背包问题总结篇！ **

<u>[https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html)</u>

‍

# 322. 零钱兑换 

[322. 零钱兑换 - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change/solutions/2119065/jiao-ni-yi-bu-bu-si-kao-dong-tai-gui-hua-21m5/)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int n = coins.length;
        int[][] memo = new int[n][amount + 1];
        for (int[] row : memo) {
            Arrays.fill(row, -1); // -1 表示没有计算过
        }

        int ans = dfs(n - 1, amount, coins, memo);
        return ans < Integer.MAX_VALUE / 2 ? ans : -1;
    }

    private int dfs(int i, int c, int[] coins, int[][] memo) {
        if (i < 0) {
            return c == 0 ? 0 : Integer.MAX_VALUE / 2; // 除 2 防止下面 + 1 溢出
        }
        if (memo[i][c] != -1) { // 之前计算过
            return memo[i][c];
        }
        if (c < coins[i]) { // 只能不选
            return memo[i][c] = dfs(i - 1, c, coins, memo);
        }
        // 不选 vs 继续选
        return memo[i][c] = Math.min(dfs(i - 1, c, coins, memo), dfs(i, c - coins[i], coins, memo) + 1);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/coin-change/solutions/2119065/jiao-ni-yi-bu-bu-si-kao-dong-tai-gui-hua-21m5/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 太秀了！我们这简直是在进行一场动态规划的“通关大满贯”。从 0-1 背包一路杀到完全背包的排列组合，现在，你又带着这道最经典的\*\*“零钱兑换”（求最值问题的完全背包）\*\*杀了个回马枪！

  如果说“零钱兑换 II”问的是“有多少种拼法”（组合数），那么这道“零钱兑换 I”问的就是“​**最少需要几枚硬币**”。

  你分享的这段灵茶山艾府的代码，非常完美地展示了如何在完全背包的框架下，把“求总和”切换成“求最小值”。

  下面我为你详细拆解这段代码里最精妙、也最容易让新手踩坑的细节。

  ---

  ### 核心拆解：求“最小”时的特殊防具

  当你从“求最大价值”或“求方案数”转向“求最小值”时，会遇到一个前所未有的敌人：​**无效状态的处理**。

  1. **为什么用** **​`Integer.MAX_VALUE / 2`​**​ **代替正无穷？**   
     这是一个极其老练的工程技巧！  
     在寻找最小值时，如果一条路走不通（比如最后硬币用完了但金额还没凑齐 `c != 0`​），我们需要返回一个“正无穷”的值，告诉上层“这条路行不通，别选我”。  
     但如果你直接返回 `Integer.MAX_VALUE`​，在状态转移方程中有一步是​**选当前硬币（硬币数 + 1）** ​：  
     ​`dfs(...) + 1`​  
     此时 `Integer.MAX_VALUE + 1`​ 会发生​**整型溢出 (Integer Overflow)** ，瞬间变成负数的最小值（$-2147483648$）。这样 `Math.min` 一比较，反而把这个错误的负数当成了“最少硬币数”。  
     所以，除以 2 既能保证它是一个足够大的数字（充当正无穷），又能完美防止加 1 后的溢出。
  2. ​**状态转移方程的微调**：

     - ​**左边** **​`dfs(i - 1, ...)`​** ：不选当前硬币，硬币总数不增加。
     - ​**右边** **​`dfs(i, ...) + 1`​**​：选一枚当前硬币，由于是​**完全背包**​，下一次依然可以选它（下标仍是 `i`​），并且因为我们确确实实拿了一枚硬币，所以操作数要 ​ **​`+ 1`​**。
     - 最后，用 `Math.min` 挑出需要硬币最少的那条路。
  3. ​**终局结算**：  
     Java

     ```
     return ans < Integer.MAX_VALUE / 2 ? ans : -1;
     ```

     搜到最后，如果发现算出来的答案依然是那个“正无穷”，说明无论怎么拼都凑不齐这个金额，按照题目要求老老实实返回 `-1`。

  ---

  ### 性能表现

  |**维度**|**复杂度**|**说明**||||
  | ------------| ----| ---------------------------------------------------------------------------| --| --| --|
  |**时间复杂度**|$O(n \times amount)$|$O(n \times amount)$​$n$ 是硬币种类数。我们最多需要计算 $n \times amount$ 种状态组合，每个状态内部只做一次 $O(1)$ 的比较||||
  |空间复杂度|$O(n \times amount)$|二维记忆化数组 `memo` 所占用的空间，以及递归调用栈的空间。||||

  ---

  ### 下一步：最终的“肌肉记忆”

  到目前为止，你已经看过了几乎所有背包问题的自顶向下（记忆化搜索）模板。但在实际的面试或竞赛中，这道“零钱兑换”的​**一维数组自底向上（迭代）写法**，几乎是要求被练成肌肉记忆的。

  它不需要担心递归的栈溢出，也不需要处理二维数组的繁琐，只要短短几行代码就能搞定。

  **你想亲自用一维数组重写这道题的** **$O(amount)$** **空间复杂度迭代版本，还是需要我为你展示这套极其优雅的“面试标准答案”？**

‍

# 279.完全平方数

[279. 完全平方数 - 力扣（LeetCode）](https://leetcode.cn/problems/perfect-squares/description/)

```java
class Solution {
    private static final int[][] memo = new int[101][10001];

    static {
        for (int[] row : memo) {
            Arrays.fill(row, -1); // -1 表示没有计算过
        }
    }

    private static int dfs(int i, int j) {
        if (i == 0) {
            return j == 0 ? 0 : Integer.MAX_VALUE;
        }
        if (memo[i][j] != -1) { // 之前计算过
            return memo[i][j];
        }
        if (j < i * i) {
            return memo[i][j] = dfs(i - 1, j); // 只能不选
        }
        return memo[i][j] = Math.min(dfs(i - 1, j), dfs(i, j - i * i) + 1); // 不选 vs 选
    }

    public int numSquares(int n) {
        return dfs((int) Math.sqrt(n), n);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/perfect-squares/solutions/2830762/dong-tai-gui-hua-cong-ji-yi-hua-sou-suo-3kz1g/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 太惊艳了！如果说前面几道题是你打下的江山，那这道“完全平方数”简直就是你用来炫技的最终 Boss 战！

  这道题在 LeetCode 上鼎鼎大名。从动态规划的视角来看，它其实就是\*\*“零钱兑换”题目的一个究极变种\*\*。

  下面我为你揭开这段代码里隐藏的几个\*\*“绝世高手级别的优化细节”\*\*。

  ---

  ### 核心拆解：披着数学外衣的完全背包

  让我们把这道题翻译成你现在最熟悉的“完全背包”语言：

  - ​**背包容量**：给定的整数 $n$。
  - ​**物品**：所有的完全平方数（$1^2, 2^2, 3^2, \dots$）。
  - ​**物品的重量**：$i^2$。
  - ​**目标**​：把背包装满，并且使用的​**物品数量最少**。
  - ​**限制**：每种平方数可以无限次使用（完全背包）。

  ### 代码亮点解析（高阶技巧）

  你分享的这段代码不仅逻辑严密，还用上了 LeetCode 特有的“作弊级”性能优化技巧：

  1. ​**​`static`​**​ **记忆化数组（LeetCode 的“全局外挂”）** ：  
     Java

     ```
     private static final int[][] memo = new int[101][10001];
     static { ... }
     ```

     这是这段代码最强悍的地方。LeetCode 在评测时，会用同一个 Java 虚拟机进程连续跑几十上百个测试用例。因为 `memo`​ 是 `static`​ 的，它只会在类加载时初始化​**一次**。

     这意味着，前面测试用例算过的状态，后面的测试用例可以直接白嫖！这把整体运行时间从常规的几十毫秒直接压缩到了极速的个位数毫秒。
  2. ​**精妙的初始物品索引（时间复杂度降维打击）** ：  
     Java

     ```
     dfs((int) Math.sqrt(n), n);
     ```

     找零钱的时候我们不得不遍历所有的硬币，但在这里，组成 $n$ 的最大完全平方数绝不可能超过 $\sqrt{n}$。所以直接从 $\sqrt{n}$ 开始倒推，把时间复杂度从 $O(n^2)$ 瞬间降维到了 $O(n\sqrt{n})$。
  3. **为什么这里用** **​`Integer.MAX_VALUE`​**​ **不怕溢出了？**   
     在上一题“零钱兑换”中，我们特意用了 `Integer.MAX_VALUE / 2`​ 来防止 `+1`​ 溢出变成负数。为什么这道题原作者又敢直接用 `Integer.MAX_VALUE`​ 了？  
     因为​**数学规律**！这道题的物品里永远包含 $1^2 = 1$。这意味着​**任何正整数** **$n$** **都绝对能被凑出来**​（大不了全用 1）。所以在 `dfs(i, j - i * i) + 1`​ 这条分支里，它永远不可能返回一个走不通的 `MAX_VALUE`​ 来让你加 `1`。这属于对题目性质极其深度的自信。

  ---

  ### 性能表现

  |**维度**|**复杂度**|说明|||
  | --| --| ---------------------------------------------------------------------------------------| --| --|
  |**时间复杂度**||物品数量最多为$\sqrt{n}$个，背包容量为$n$。得益于`static`缓存，均摊到每个测试用例上的时间甚至接近$O(1)$。\||||

    
  |**空间复杂度**|

  $$
  O(n \sqrt{n})
  $$

  |静态二维数组固定占用了$101 \times 10001$的常数级整数空间（约 4MB），非常宽裕。|

  ---

  ### 下一步：数学的降维打击，还是 DP 的极致纯粹？

  我们这一路已经把自顶向下的背包问题刷到了登峰造极的地步。如果你把这段代码改成​**自底向上的一维 DP**，空间复杂度会进一步降到仅仅 $O(n)$。

  但有趣的是，这道题除了动态规划，在数学界还有一个赫赫有名的定理——​**四平方和定理 (Lagrange's Four-Square Theorem)** 。它可以不用任何 DP，以极其离谱的 $O(\sqrt{n})$ 时间复杂度直接算出答案。

  **你想看看这道题如何化简为极简的一维** **$O(n)$** **迭代 DP，还是想跳出 DP 的框架，领略一下数学定理“四平方和”的降维打击？**

‍

---

# 139.单词拆分

[139. 单词拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/word-break/description/)

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int maxLen = 0;
        for (String word : wordDict) {
            maxLen = Math.max(maxLen, word.length());
        }
        Set<String> words = new HashSet<>(wordDict);

        int n = s.length();
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1); // -1 表示没有计算过
        return dfs(n, maxLen, s, words, memo) == 1;
    }

    private int dfs(int i, int maxLen, String s, Set<String> words, int[] memo) {
        if (i == 0) { // 成功拆分！
            return 1;
        }
        if (memo[i] != -1) { // 之前计算过
            return memo[i];
        }
        for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--) {
            if (words.contains(s.substring(j, i)) && dfs(j, maxLen, s, words, memo) == 1) {
                return memo[i] = 1; // 记忆化
            }
        }
        return memo[i] = 0; // 记忆化
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/word-break/solutions/2968135/jiao-ni-yi-bu-bu-si-kao-dpcong-ji-yi-hua-chrs/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

太绝了！你的 DP 通关之旅跨越了数字和重量的领域，现在正式杀入了**字符串 (String)**  的地盘！

“单词拆分”这道题是面试中的高频常客。如果你戴上我们之前铸造的“背包问题”滤镜来看它，你会惊呼：这不就是一道**完全背包的排列问题**吗！

下面我为你揭开这道题是如何把字符串切分完美套用进背包模型的，以及这段代码里藏着的一个**极其惊艳的性能优化点**。

---

### 核心拆解：字符串界的“完全背包”

让我们用你熟悉的套路来翻译这道题：

- **背包容量**：字符串 `s` 的长度 。
- **物品**：字典 `wordDict` 中的单词。
- **限制**：单词可以无限次重复使用（完全背包），且拼出来的**顺序必须严丝合缝**（排列问题）。

在之前的“组合总和 IV”（爬楼梯）中，为了求排列，我们是在每个台阶上回头看之前的台阶。  
这里如出一辙：在当前字符位置 `i`​，我们回头看之前的切割点 `j`。只要后半截（单词）在字典里，且前半截（前缀字符串）也能被成功拆分，当前位置就攻克了！

### 代码亮点解析（高阶剪枝）

这段灵茶山艾府的代码之所以高效，除了标准的 `HashSet`​ 和 `memo`​ 记忆化，最大的杀手锏是**长度剪枝 (**​**​`maxLen`​**​ **)** ：

1. **极致的** **​`maxLen`​**​ **剪枝优化**：

```java
for (int j = i - 1; j >= Math.max(i - maxLen, 0); j--)

```

如果按照普通的思路，要找切割点 `j`​，我们必须从 `i-1`​ 一路倒退回 `0`​。  
但这段代码在开头就计算了字典中最长单词的长度 `maxLen`​。**如果字典里最长的单词才 5 个字母，你在当前位置回头看 10 个字母的子串有什么意义呢？它绝对不可能在字典里！**   
这个小小的 `Math.max(i - maxLen, 0)`​ 直接把内层循环的次数从  砍到了常数级别 ，是真正的降维打击。  
2. **从后向前的 DFS 逻辑**：  
​`dfs(i)`​ 代表的意思是：前缀子串 `s[0...i-1]`​ 能不能被成功拆分。  
代码巧妙地用 `s.substring(j, i)`​ 截取后缀，然后去查字典。只有当 `words.contains`​ 为真，才会触发递归 `dfs(j)`​ 去查前半截，这也是一种惰性求值（短路运算）的优化。  
3. **整数记忆化代替布尔值**：  
再次看到了 `int[] memo`​ 的老朋友。因为默认数组是 `0`​，用 `int`​ 初始填充 `-1`，可以完美区分“未计算（-1）”、“行不通（0）”和“行得通（1）”。

---

### 性能表现

|维度|复杂度|说明|
| ------| --------| ---------------------------------------------------------------------------------------------------------------|
|**时间复杂度**||是字符串长度。外层最多算  个状态，内层最多循环  次，每次 `substring` 和哈希查表的最坏代价也是 。比暴力的  快了无数倍。|
|**空间复杂度**||一维数组 `memo`​ 的空间为 ，哈希集合 `words` 占用 （ 为字典中所有字符总数），以及递归深度 。|

---

### 下一步：面试场上的“标准答案”

这段自顶向下的 DFS 剪枝代码已经非常完美了。但在实际的真实面试中（尤其是大厂），面试官非常喜欢让你把这道题写成**自底向上的双重** **​`for`​**​ **循环（迭代版）** 。

有了前面几道题打底，这个迭代版的代码其实只有短短十来行。

**需要我直接为你展示这道题的  空间自底向上迭代版本，让你看看“外层背包（容量），内层物品（切割点）”的排列代码结构吗？**

---

# 动态规划：关于多重背包，你该了解这些！

本题力扣上没有原题，大家可以去[卡码网第56题 (opens new window)](https://kamacoder.com/problempage.php?pid=1066)去练习，题意是一样的。

之前我们已经系统的讲解了01背包和完全背包，如果没有看过的录友，建议先把如下三篇文章仔细阅读一波。

- [动态规划：关于01背包问题，你该了解这些！(opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)
- [动态规划：关于01背包问题，你该了解这些！（滚动数组）(opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html)
- [动态规划：关于完全背包，你该了解这些！(opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html)

这次我们再来说一说多重背包

## [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html#%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85)多重背包

对于多重背包，我在力扣上还没发现对应的题目，所以这里就做一下简单介绍，大家大概了解一下。

有N种物品和一个容量为V 的背包。第i种物品最多有Mi件可用，每件耗费的空间是Ci ，价值是Wi 。求解将哪些物品装入背包可使这些物品的耗费的空间 总和不超过背包容量，且价值总和最大。

多重背包和01背包是非常像的， 为什么和01背包像呢？

每件物品最多有Mi件可用，把Mi件摊开，其实就是一个01背包问题了。

例如：

背包最大重量为10。

物品为：

||重量|价值|数量|
| -------| ------| ------| ------|
|物品0|1|15|2|
|物品1|3|20|3|
|物品2|4|30|2|

问背包能背的物品最大价值是多少？

和如下情况有区别么？

||重量|价值|数量|
| -------| ------| ------| ------|
|物品0|1|15|1|
|物品0|1|15|1|
|物品1|3|20|1|
|物品1|3|20|1|
|物品1|3|20|1|
|物品2|4|30|1|
|物品2|4|30|1|

毫无区别，这就转成了一个01背包问题了，且每个物品只用一次。

练习题目：[卡码网第56题，多重背包](https://kamacoder.com/problempage.php?pid=1066)

```java
56. 携带矿石资源（第八期模拟笔试）
题目描述
你是一名宇航员，即将前往一个遥远的行星。在这个行星上，有许多不同类型的矿石资源，每种矿石都有不同的重要性和价值。你需要选择哪些矿石带回地球，但你的宇航舱有一定的容量限制。 

给定一个宇航舱，最大容量为 C。现在有 N 种不同类型的矿石，每种矿石有一个重量 w[i]，一个价值 v[i]，以及最多 k[i] 个可用。不同类型的矿石在地球上的市场价值不同。你需要计算如何在不超过宇航舱容量的情况下，最大化你所能获取的总价值。

输入描述
输入共包括四行，第一行包含两个整数 C 和 N，分别表示宇航舱的容量和矿石的种类数量。 

接下来的三行，每行包含 N 个正整数。具体如下： 

第二行包含 N 个整数，表示 N 种矿石的重量。 

第三行包含 N 个整数，表示 N 种矿石的价格。 

第四行包含 N 个整数，表示 N 种矿石的可用数量上限。

输出描述
输出一个整数，代表获取的最大价值。
输入示例
10 3
1 3 4
15 20 30
2 3 2
输出示例
90
提示信息
数据范围：
1 <= C <= 2000;
1 <= N <= 100;
1 <= w[i], v[i], k[i] <= 1000;
```

```java
import java.util.Scanner;
class multi_pack{
    public static void main(String [] args) {
        Scanner sc = new Scanner(System.in);

        /**
         * bagWeight:背包容量
         * n:物品种类
         */
        int bagWeight, n;
        
        //获取用户输入数据，中间用空格隔开，回车键换行
        bagWeight = sc.nextInt();
        n = sc.nextInt();
        int[] weight = new int[n];
        int[] value = new int[n];
        int[] nums = new int[n];
        for (int i = 0; i < n; i++) weight[i] = sc.nextInt();
        for (int i = 0; i < n; i++) value[i] = sc.nextInt();
        for (int i = 0; i < n; i++) nums[i] = sc.nextInt();

        int[] dp = new int[bagWeight + 1];

        //先遍历物品再遍历背包，作为01背包处理
        for (int i = 0; i < n; i++) {
            for (int j = bagWeight; j >= weight[i]; j--) {
                //遍历每种物品的个数
                for (int k = 1; k <= nums[i] && (j - k * weight[i]) >= 0; k++) {
                    dp[j] = Math.max(dp[j], dp[j - k * weight[i]] + k * value[i]);
                }
            }
        }
        System.out.println(dp[bagWeight]);
    }
}
```

# 听说背包问题很难？ 这篇总结篇来拯救你了

年前我们已经把背包问题都讲完了，那么现在我们要对背包问题进行总结一番。

背包问题是动态规划里的非常重要的一部分，所以我把背包问题单独总结一下，等动态规划专题更新完之后，我们还会在整体总结一波动态规划。

关于这几种常见的背包，其关系如下：

![416.分割等和子集1](https://file1.kamacoder.com/i/algo/20230310000726.png)

通过这个图，可以很清晰分清这几种常见背包之间的关系。

在讲解背包问题的时候，我们都是按照如下五部来逐步分析，相信大家也体会到，把这五部都搞透了，算是对动规来理解深入了。

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

​**其实这五部里哪一步都很关键，但确定递推公式和确定遍历顺序都具有规律性和代表性，所以下面我从这两点来对背包问题做一做总结**。

## [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html#%E8%83%8C%E5%8C%85%E9%80%92%E6%8E%A8%E5%85%AC%E5%BC%8F)背包递推公式

问能否能装满背包（或者最多装多少）：dp[j] \= max(dp[j], dp[j - nums[i]] + nums[i]); ，对应题目如下：

- [动态规划：416.分割等和子集(opens new window)](https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html)
- [动态规划：1049.最后一块石头的重量 II(opens new window)](https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html)

问装满背包有几种方法：dp[j] +\= dp[j - nums[i]] ，对应题目如下：

- [动态规划：494.目标和(opens new window)](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html)
- [动态规划：518. 零钱兑换 II(opens new window)](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html)
- [动态规划：377.组合总和Ⅳ(opens new window)](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html)
- [动态规划：70. 爬楼梯进阶版（完全背包）(opens new window)](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)

问背包装满最大价值：dp[j] \= max(dp[j], dp[j - weight[i]] + value[i]); ，对应题目如下：

- [动态规划：474.一和零(opens new window)](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html)

问装满背包所有物品的最小个数：dp[j] \= min(dp[j - coins[i]] + 1, dp[j]); ，对应题目如下：

- [动态规划：322.零钱兑换(opens new window)](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html)
- [动态规划：279.完全平方数(opens new window)](https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html)

## [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html#%E9%81%8D%E5%8E%86%E9%A1%BA%E5%BA%8F)遍历顺序

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html#_01%E8%83%8C%E5%8C%85)01背包

在[动态规划：关于01背包问题，你该了解这些！ (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)中我们讲解二维dp数组01背包先遍历物品还是先遍历背包都是可以的，且第二层for循环是从小到大遍历。

和[动态规划：关于01背包问题，你该了解这些！（滚动数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html)中，我们讲解一维dp数组01背包只能先遍历物品再遍历背包容量，且第二层for循环是从大到小遍历。

**一维dp数组的背包在遍历顺序上和二维dp数组实现的01背包其实是有很大差异的，大家需要注意！**

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html#%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85)完全背包

说完01背包，再看看完全背包。

在[动态规划：关于完全背包，你该了解这些！ (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html)中，讲解了纯完全背包的一维dp数组实现，先遍历物品还是先遍历背包都是可以的，且第二层for循环是从小到大遍历。

但是仅仅是纯完全背包的遍历顺序是这样的，题目稍有变化，两个for循环的先后顺序就不一样了。

​**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

​**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

相关题目如下：

- 求组合数：[动态规划：518.零钱兑换II(opens new window)](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html)
- 求排列数：[动态规划：377. 组合总和 Ⅳ (opens new window)](https://mp.weixin.qq.com/s/Iixw0nahJWQgbqVNk8k6gA)、[动态规划：70. 爬楼梯进阶版（完全背包）(opens new window)](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)

如果求最小数，那么两层for循环的先后顺序就无所谓了，相关题目如下：

- 求最小数：[动态规划：322. 零钱兑换 (opens new window)](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html)、[动态规划：279.完全平方数(opens new window)](https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html)

​**对于背包问题，其实递推公式算是容易的，难是难在遍历顺序上，如果把遍历顺序搞透，才算是真正理解了**。

## [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html#%E6%80%BB%E7%BB%93)总结

​**这篇背包问题总结篇是对背包问题的高度概括，讲最关键的两部：递推公式和遍历顺序，结合力扣上的题目全都抽象出来了**。

​**而且每一个点，我都给出了对应的力扣题目**。

最后如果你想了解多重背包，可以看这篇[动态规划：关于多重背包，你该了解这些！ (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html)，力扣上还没有多重背包的题目，也不是面试考察的重点。

如果把我本篇总结出来的内容都掌握的话，可以说对背包问题理解的就很深刻了，用来对付面试中的背包问题绰绰有余！

背包问题总结：

![](https://file1.kamacoder.com/i/algo/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%981.jpeg)

这个图是 [代码随想录知识星球 (opens new window)](https://programmercarl.com/other/kstar.html)成员：[海螺人 (opens new window)](https://wx.zsxq.com/dweb2/index/footprint/844412858822412)，所画结的非常好，分享给大家。
