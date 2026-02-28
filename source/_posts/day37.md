---
title: day37
date: '2026-02-20 20:29:31'
updated: '2026-02-20 20:36:25'
permalink: /post/day37-1g8etk.html
comments: true
toc: true
---



# day37

# 第九章 动态规划 part05

力扣上没有纯粹的完全背包的题目，我在卡码网上制作了题目，大家可以去做一做，题目链接在下面的文章链接里。

后面的两道题目，都是完全背包的应用，做做感受一下

### ** 完全背包 **

视频讲解：<u>[https://www.bilibili.com/video/BV1uK411o7c9](https://www.bilibili.com/video/BV1uK411o7c9)</u>

<u>[https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html]()</u>​<u>[ ](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html)</u>

### ** 518. 零钱兑换 II  **

视频讲解：<u>[https://www.bilibili.com/video/BV1KM411k75j](https://www.bilibili.com/video/BV1KM411k75j)</u>

<u>[https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html	](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html)</u>

### ** 377. 组合总和 Ⅳ  **

视频讲解：<u>[https://www.bilibili.com/video/BV1V14y1n7B6](https://www.bilibili.com/video/BV1V14y1n7B6)</u>

<u>[https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html)</u>

### **70. 爬楼梯 （进阶） **

这道题目 爬楼梯之前我们做过，这次再用完全背包的思路来分析一遍

<u>[https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)</u>

‍

### 完全背包

# 完全背包理论基础-二维DP数组

本题力扣上没有原题，大家可以去[卡码网第52题 (opens new window)](https://kamacoder.com/problempage.php?pid=1052)去练习，题意是一样的。

## [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85)完全背包

有N件物品和一个最多能背重量为W的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。​**每件物品都有无限个（也就是可以放入背包多次）** ，求解将哪些物品装入背包里物品价值总和最大。

​**完全背包和01背包问题唯一不同的地方就是，每种物品有无限件**。

同样leetcode上没有纯完全背包问题，都是需要完全背包的各种应用，需要转化成完全背包问题，所以我这里还是以纯完全背包问题进行讲解理论和原理。

在下面的讲解中，我拿下面数据举例子：

背包最大重量为4，物品为：

||重量|价值|
| -------| ------| ------|
|物品0|1|15|
|物品1|3|20|
|物品2|4|30|

**每件商品都有无限个！**

问背包能背的物品最大价值是多少？

​**如果没看到之前的01背包讲解，已经要先仔细看如下两篇，01背包是基础，本篇在讲解完全背包，之前的背包基础我将不会重复讲解**。

- [01背包理论基础（二维数组）(opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)
- [01背包理论基础（一维数组）(opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html)

动规五部曲分析完全背包，为了从原理上讲清楚，我们先从二维dp数组分析：

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#_1-%E7%A1%AE%E5%AE%9Adp%E6%95%B0%E7%BB%84%E4%BB%A5%E5%8F%8A%E4%B8%8B%E6%A0%87%E7%9A%84%E5%90%AB%E4%B9%89)1. 确定dp数组以及下标的含义

​**dp[i][j] 表示从下标为[0-i]的物品，每个物品可以取无限次，放进容量为j的背包，价值总和最大是多少**。

很多录友也会疑惑，凭什么上来就定义 dp数组，思考过程是什么样的， 这个思考过程我在 [01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)中的 “确定dp数组以及下标的含义” 有详细讲解。

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#_2-%E7%A1%AE%E5%AE%9A%E9%80%92%E6%8E%A8%E5%85%AC%E5%BC%8F)2. 确定递推公式

这里在把基本信息给出来：

||重量|价值|
| -------| ------| ------|
|物品0|1|15|
|物品1|3|20|
|物品2|4|30|

对于递推公式，首先我们要明确有哪些方向可以推导出 dp[i][j]。

这里依然拿dp[1][4]的状态来举例： （[01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)中也是这个例子，要注意下面的不同之处）

求取 dp[1][4] 有两种情况：

1. 放物品1
2. 还是不放物品1

如果不放物品1， 那么背包的价值应该是 dp[0][4] 即 容量为4的背包，只放物品0的情况。

推导方向如图：

![](https://file1.kamacoder.com/i/algo/20241126112952.png)

如果放物品1， ​**那么背包要先留出物品1的容量**，目前容量是4，物品1 的容量（就是物品1的重量）为3，此时背包剩下容量为1。

容量为1，只考虑放物品0 和物品1 的最大价值是 dp[1][1]， ​**注意 这里和** **[01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)**​**有所不同了**！

在 [01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)中，背包先空留出物品1的容量，此时容量为1，只考虑放物品0的最大价值是 dp[0][1]，​**因为01背包每个物品只有一个，既然空出物品1，那背包中也不会再有物品1**！

而在完全背包中，物品是可以放无限个，所以 即使空出物品1空间重量，那背包中也可能还有物品1，所以此时我们依然考虑放 物品0 和 物品1 的最大价值即： **dp[1][1]， 而不是 dp[0][1]**

所以 放物品1 的情况 \= dp[1][1] + 物品1 的价值，推导方向如图：

![](https://file1.kamacoder.com/i/algo/20241126113104.png)

（​**注意上图和** **[01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)**​**中的区别**，对于理解完全背包很重要）

两种情况，分别是放物品1 和 不放物品1，我们要取最大值（毕竟求的是最大价值）

​`dp[1][4] = max(dp[0][4], dp[1][1] + 物品1 的价值)`

以上过程，抽象化如下：

- ​**不放物品i**：背包容量为j，里面不放物品i的最大价值是dp[i - 1][j]。
- ​**放物品i**：背包空出物品i的容量后，背包容量为j - weight[i]，dp[i][j - weight[i]] 为背包容量为j - weight[i]且不放物品i的最大价值，那么dp[i][j - weight[i]] + value[i] （物品i的价值），就是背包放物品i得到的最大价值

递推公式： `dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);`

（注意，完全背包二维dp数组 和 01背包二维dp数组 递推公式的区别，01背包中是 `dp[i - 1][j - weight[i]] + value[i])`）

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#_3-dp%E6%95%B0%E7%BB%84%E5%A6%82%E4%BD%95%E5%88%9D%E5%A7%8B%E5%8C%96)3. dp数组如何初始化

​**关于初始化，一定要和dp数组的定义吻合，否则到递推公式的时候就会越来越乱**。

首先从dp[i][j]的定义出发，如果背包容量j为0的话，即dp[i][0]，无论是选取哪些物品，背包价值总和一定为0。如图：

![动态规划-背包问题2](https://file1.kamacoder.com/i/algo/2021011010304192.png)

在看其他情况。

状态转移方程 `dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);` 可以看出有一个方向 i 是由 i-1 推导出来，那么i为0的时候就一定要初始化。

dp[0][j]，即：存放编号0的物品的时候，各个容量的背包所能存放的最大价值。

那么很明显当 `j < weight[0]`的时候，dp[0][j] 应该是 0，因为背包容量比编号0的物品重量还小。

当`j >= weight[0]`​时，​**dp[0][j] 如果能放下weight[0]的话，就一直装，每一种物品有无限个**。

代码初始化如下：

```cpp
for (int i = 1; i < weight.size(); i++) {  // 当然这一步，如果把dp数组预先初始化为0了，这一步就可以省略，但很多同学应该没有想清楚这一点。
    dp[i][0] = 0;
}

// 正序遍历，如果能放下就一直装物品0
for (int j = weight[0]; j <= bagWeight; j++)
    dp[0][j] = dp[0][j - weight[0]] + value[0];
```

（注意上面初始化和 [01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)的区别在于物品有无限个）

此时dp数组初始化情况如图所示：

![](https://file1.kamacoder.com/i/algo/20241114161608.png)

dp[0][j] 和 dp[i][0] 都已经初始化了，那么其他下标应该初始化多少呢？

其实从递归公式： dp[i][j] \= max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]); 可以看出dp[i][j] 是由上方和左方数值推导出来了，那么 其他下标初始为什么数值都可以，因为都会被覆盖。

但只不过一开始就统一把dp数组统一初始为0，更方便一些。

最后初始化代码如下：

```cpp
// 初始化 dp
vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
for (int j = weight[0]; j <= bagWeight; j++) {
    dp[0][j] = dp[0][j - weight[0]] + value[0]; 
}
```

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#_4-%E7%A1%AE%E5%AE%9A%E9%81%8D%E5%8E%86%E9%A1%BA%E5%BA%8F)4. 确定遍历顺序

[01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)中我们讲过，01背包二维DP数组，先遍历物品还是先遍历背包都是可以的。

因为两种遍历顺序，对于二维dp数组来说，递推公式所需要的值，二维dp数组里对应的位置都有。

详细可以看 [01背包理论基础（二维数组） (opens new window)](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)中的 【遍历顺序】的讲解

所以既可以 先遍历物品再遍历背包：

```cpp
for (int i = 1; i < n; i++) { // 遍历物品
    for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);
    }
}
```

也可以 先遍历背包再遍历物品：

```cpp
for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
    for (int i = 1; i < n; i++) { // 遍历物品
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);
    }
}
```

### [#](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#_5-%E4%B8%BE%E4%BE%8B%E6%8E%A8%E5%AF%BCdp%E6%95%B0%E7%BB%84)5. 举例推导dp数组

以本篇举例数据为例，填满了dp二维数组如图：

![](https://file1.kamacoder.com/i/algo/20241126113752.png)

因为 物品0 的性价比是最高的，而且 在完全背包中，每一类物品都有无限个，所以有无限个物品0，既然物品0 性价比最高，当然是优先放物品0。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int bagWeight = scanner.nextInt();

        int[] weight = new int[n];
        int[] value = new int[n];

        for (int i = 0; i < n; i++) {
            weight[i] = scanner.nextInt();
            value[i] = scanner.nextInt();
        }

        int[][] dp = new int[n][bagWeight + 1];

        // 初始化
        for (int j = weight[0]; j <= bagWeight; j++) {
            dp[0][j] = dp[0][j - weight[0]] + value[0];
        }

        // 动态规划
        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= bagWeight; j++) {
                if (j < weight[i]) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);
                }
            }
        }

        System.out.println(dp[n - 1][bagWeight]);
        scanner.close();
    }
}

```

# 518.零钱兑换II

[518. 零钱兑换 II - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change-ii/description/)

```java
class Solution {
    public int change(int amount, int[] coins) {
        int n = coins.length;
        int[][] memo = new int[n][amount + 1];
        for (int[] row : memo) {
            Arrays.fill(row, -1); // -1 表示没有计算过
        }

        return dfs(n - 1, amount, coins, memo);
    }

    private int dfs(int i, int c, int[] coins, int[][] memo) {
        if (i < 0) {
            return c == 0 ? 1 : 0;
        }
        if (memo[i][c] != -1) { // 之前算过了
            return memo[i][c];
        }
        if (c < coins[i]) { // 只能不选
            return memo[i][c] = dfs(i - 1, c, coins, memo);
        }
        // 不选 + 继续选
        return memo[i][c] = dfs(i - 1, c, coins, memo) + dfs(i, c - coins[i], coins, memo);
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/coin-change-ii/solutions/2706227/shi-pin-wan-quan-bei-bao-cong-ji-yi-hua-o3ew0/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 恭喜你，成功跨越了 0-1 背包的各种变种，正式迎来了动态规划大山里的另一座高峰：​**完全背包问题 (Complete Knapsack Problem)** ！

  “零钱兑换 II”是最经典的完全背包问题。在这里，每种硬币（物品）的数量是**无限**的，你可以无限次地选择同一种硬币。

  你分享的这段记忆化搜索代码，看似只比 0-1 背包改动了一个极为微小的下标，但却在逻辑上实现了从“只能选一次”到“可以选无数次”的飞跃。

  下面我为你揭开这个“微小改动”背后蕴含的深刻逻辑。

  ---

  ### 核心拆解：一字之差的魔法

  在之前的 0-1 背包（如“目标和”、“分割子集”）中，如果我们决定把物品装进背包，状态转移方程是这样的：

  ​`dfs(i - 1, c - nums[i])`

  注意看，下标变成了 `i - 1`​，这意味着：当前物品处理完了，无论选没选，​**下一步必须去考虑前一个物品了**。

  但是，在这段完全背包的代码中，当你选择装入硬币时，状态转移方程变成了：

  Java

  ```
  dfs(i, c - coins[i], coins, memo)
  ```

  注意这里的下标​**依然是** **​`i`​**！这意味着：

  **即便我已经拿了一枚当前的硬币，我的额度变小了，但在下一步里，我依然可以继续考虑拿这枚硬币！**  直到我的剩余容量 `c`​ 再也装不下 `coins[i]`​ 为止，我才会通过另一条分支 `dfs(i - 1, ...)` 去考虑下一种硬币。

  这就完美契合了“物品数量无限”的题意。

  ### 代码亮点解析

  1. ​**彻底的组合计数**：  
     Java

     ```
     return memo[i][c] = dfs(i - 1, c, coins, memo) + dfs(i, c - coins[i], coins, memo);
     ```

     和“目标和”类似，由于这道题求的是“组合方案数”，所以是用加号 `+` 把两条分支的方案数加起来。

     - 左边：不要当前硬币，去看上一种硬币有多少种凑法。
     - 右边：要一枚当前硬币，然后在剩余容量里**继续**看当前硬币和其他硬币有多少种凑法。
  2. ​**严谨的边界处理**：  
     Java

     ```
     if (i < 0) {
         return c == 0 ? 1 : 0;
     }
     ```

     老规矩，必须把所有的硬币种类（哪怕是不选）都遍历完（即 `i < 0`​），再去判断最终剩余的金额 `c` 是否刚好减到了 0。

  ---

  ### 性能表现

  |**维度**|**复杂度**|**说明**||||
  | --| --| --| --| --| --|
  |**时间复杂度**||||||

  $$
  O(n \times amount)
  $$

  |$n$是硬币种类数。虽然每种硬币可以选多次，但在记忆化的加持下，总的状态数依然是$n \times amount$种组合，每个状态只计算一次。|  
  |**空间复杂度**|

  $$
  O(n \times amount)
  $$

  |二维记忆化数组`memo`所占用的空间，以及递归调用栈的最深深度（最坏情况为$amount / \min(coins)$）。|

  ---

  ### 下一步：回旋镖！还记得 0-1 背包里的“陷阱”吗？

  我们在讨论第一道 0-1 背包题（研究材料）的空间优化时，我曾特别强调过一个​**陷阱**：

  > “使用一维数组时，内层遍历背包容量 `j`​ 的循环必须​**倒序遍历**，否则会导致同一个物品被重复放入（那就变成完全背包问题了）。”
  >

  现在，命运的齿轮转动了。既然这道题恰好就是​**完全背包问题**​，那是不是意味着，如果我们把这道题重构为一维 `dp`​ 数组，内层循环​**正序遍历**，就能完美解决这道题？

  **想不想看看这道题如何化繁为简，用极其优雅的短短几行“一维正序遍历”代码，实现** **$O(amount)$** **的空间复杂度？**

---

# 377. 组合总和 Ⅳ

[377. 组合总和 Ⅳ - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iv/description/)

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] memo = new int[target + 1];
        Arrays.fill(memo, -1); // -1 表示没有计算过
        return dfs(target, nums, memo);
    }

    private int dfs(int i, int[] nums, int[] memo) {
        if (i == 0) { // 爬完了
            return 1;
        }
        if (memo[i] != -1) { // 之前计算过
            return memo[i];
        }
        int res = 0;
        for (int x : nums) { // 枚举所有可以爬的台阶数
            if (x <= i) {
                res += dfs(i - x, nums, memo);
            }
        }
        return memo[i] = res; // 记忆化
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/combination-sum-iv/solutions/2706336/ben-zhi-shi-pa-lou-ti-cong-ji-yi-hua-sou-y52j/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 哇，你这套连招打得太漂亮了！我们刚聊完“零钱兑换 II”（完全背包），你立刻就抛出了这道名字极具欺骗性的“组合总和 IV”。

  这道题是动态规划中一个极其经典的易错点。题目名字叫“组合”，但实际上它要求算的却是**排列**（Permutation）！因为题目明确说明：`顺序不同的序列被视作不同的组合`。

  你分享的代码依然是自顶向下的记忆化搜索。原作者那句“**本质是爬楼梯**”的注释，直接一语道破天机。

  下面我为你详细拆解，这道题是如何在“完全背包”的基础上，发生奇妙反转的。

  ---

  ### 核心拆解：组合 vs 排列，一维之差

  让我们对比一下上一题（零钱兑换 II）和这题的核心区别：

  - **上一题（求组合）** ：状态是 `dfs(i, c)`​。我们是按物品种类 `i`​ 顺序往下拿的。先决定拿多少个硬币 A，再决定拿多少个硬币 B。这种策略天然排除了“先拿 B 再拿 A”的可能性，所以求出来的是**组合数**。
  - **这一题（求排列）** ：状态变成了只有一个维度 `dfs(i)`​。这里没有物品种类的限制 `i`​ 了！你在每一次递归（每站在一个台阶上）时，都会用一个 `for`​ 循环**把所有的数字重新看一遍**。  
    这意味着，你先选 `1`​ 再选 `2`​，和先选 `2`​ 再选 `1`​，会作为两条完全不同的递归分支走下去，最终都被统计进来。这就是求**排列数**的核心机制。

  ### 代码亮点解析

  1. **完美契合“爬楼梯”模型**：  
     想象你要爬到一个高度为 `target`​ 的楼层。`nums` 数组就是你每次可以跨越的台阶数（比如一步跨 1 阶、2 阶或 3 阶）。  
     你想知道爬到顶楼有多少种不同的踩法？

  ```java
  for (int x : nums) { 
      if (x <= i) {
          res += dfs(i - x, nums, memo);
      }
  }

  ```

  当前在第 `i`​ 阶，你只要把“退后 `x`​ 阶的走法总数”全部加起来，就是当前的走法总数。这简直就是斐波那契数列（普通爬楼梯）的超级进阶版！  
  2. **极致精简的一维记忆化**：  
  因为我们在每次选择时都可以挑选所有物品，状态只与“当前剩余的容量（目标和）”有关，所以 `memo`​ 数组只需要一维 `new int[target + 1]` 即可。这让空间复杂度变得非常小。

  ---

  ### 性能表现

  |维度|复杂度|说明|
  | ------| --------| ------------------------------------------------------------------------------------|
  |**时间复杂度**||是 `nums`​ 数组的长度。我们需要计算  个状态，每个状态内部都需要一个长度为  的 `for` 循环。|
  |**空间复杂度**||一维的记忆化数组 `memo` 所占用的空间，以及递归调用栈的最大深度 。|

  ---

  ### 下一步：自底向上 DP 的“世纪大坑”

  我们一路推演过来，如果你要把这段自顶向下的递归代码改成**自底向上的双重** **​`for`​**​ **循环（迭代）** ，你会发现一个极其有趣的现象：

  在上一题“零钱兑换 II”（完全背包）中，外层循环是**物品**，内层循环是**容量**。  
  而在这道题（排列问题）中，为了在每个容量下都能考虑所有物品，必须**反过来**：外层循环是**容量**，内层循环是**物品**！

  这就诞生了 DP 界最著名的一句口诀：**组合问题外物品内背包，排列问题外背包内物品。**

  **需要我直接写出这道题的  空间迭代版本，让你直观感受一下这句口诀在代码上是如何体现的吗？**

---

‍

# 70. 爬楼梯（进阶版）

[卡码网：57. 爬楼梯(opens new window)](https://kamacoder.com/problempage.php?pid=1067)

```java
 爬楼梯
题目描述
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。 
每次你可以爬至多m (1 <= m < n)个台阶。你有多少种不同的方法可以爬到楼顶呢？ 
注意：给定 n 是一个正整数。

输入描述
	输入共一行，包含两个正整数，分别表示n, m
	输出描述
	输出一个整数，表示爬到楼顶的方法数。
输入示例
	3 2
输出示例
	3
提示信息
数据范围：
	1 <= m < n <= 32;
	当 m = 2，n = 3 时，n = 3 这表示一共有三个台阶，m = 2 代表你每次可以爬一个台阶或者两个台阶。

此时你有三种方法可以爬到楼顶。
1 阶 + 1 阶 + 1 阶段
1 阶 + 2 阶
2 阶 + 1 阶
```

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬至多m (1 \<\= m \< n)个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

输入描述：输入共一行，包含两个正整数，分别表示n, m

输出描述：输出一个整数，表示爬到楼顶的方法数。

输入示例：3 2

输出示例：3

提示：

当 m \= 2，n \= 3 时，n \= 3 这表示一共有三个台阶，m \= 2 代表你每次可以爬一个台阶或者两个台阶。

此时你有三种方法可以爬到楼顶。

- 1 阶 + 1 阶 + 1 阶段
- 1 阶 + 2 阶
- 2 阶 + 1 阶

## [#](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html#%E6%80%9D%E8%B7%AF)思路

之前讲这道题目的时候，因为还没有讲背包问题，所以就只是讲了一下爬楼梯最直接的动规方法（斐波那契）。

**这次终于讲到了背包问题，我选择带录友们再爬一次楼梯！**

这道题目 我们在[动态规划：爬楼梯 (opens new window)](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF.html)中已经讲过一次了，这次我又给本题加点料，力扣上没有原题，所以可以在卡码网[57. 爬楼梯 (opens new window)](https://kamacoder.com/problempage.php?pid=1067)上来刷这道题目。

我们之前做的 爬楼梯 是只能至多爬两个台阶。

这次**改为：一步一个台阶，两个台阶，三个台阶，.......，直到 m个台阶。问有多少种不同的方法可以爬到楼顶呢？**

这又有难度了，这其实是一个完全背包问题。

1阶，2阶，.... m阶就是物品，楼顶就是背包。

每一阶可以重复使用，例如跳了1阶，还可以继续跳1阶。

问跳到楼顶有几种方法其实就是问装满背包有几种方法。

**此时大家应该发现这就是一个完全背包问题了！**

和昨天的题目[动态规划：377. 组合总和 Ⅳ (opens new window)](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html)基本就是一道题了。

动规五部曲分析如下：

1. 确定dp数组以及下标的含义

​**dp[i]：爬到有i个台阶的楼顶，有dp[i]种方法**。

2. 确定递推公式

在[动态规划：494.目标和 (opens new window)](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html)、 [动态规划：518.零钱兑换II (opens new window)](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html)、[动态规划：377. 组合总和 Ⅳ (opens new window)](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html)中我们都讲过了，求装满背包有几种方法，递推公式一般都是dp[i] +\= dp[i - nums[j]];

本题呢，dp[i]有几种来源，dp[i - 1]，dp[i - 2]，dp[i - 3] 等等，即：dp[i - j]

那么递推公式为：dp[i] +\= dp[i - j]

3. dp数组如何初始化

既然递归公式是 dp[i] +\= dp[i - j]，那么dp[0] 一定为1，dp[0]是递归中一切数值的基础所在，如果dp[0]是0的话，其他数值都是0了。

下标非0的dp[i]初始化为0，因为dp[i]是靠dp[i-j]累计上来的，dp[i]本身为0这样才不会影响结果

4. 确定遍历顺序

这是背包里求排列问题，即：**1、2 步 和 2、1 步都是上三个台阶，但是这两种方法不一样！**

所以需将target放在外循环，将nums放在内循环。

每一步可以走多次，这是完全背包，内循环需要从前向后遍历。

5. 举例来推导dp数组

介于本题和[动态规划：377. 组合总和 Ⅳ (opens new window)](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html)几乎是一样的，这里我就不再重复举例了。

```cpp
import java.util.Scanner;
class climbStairs{
    public static void main(String [] args){
        Scanner sc = new Scanner(System.in);
        int m, n;
        while (sc.hasNextInt()) {
            // 从键盘输入参数，中间用空格隔开
            n = sc.nextInt();
            m = sc.nextInt();

            // 求排列问题，先遍历背包再遍历物品
            int[] dp = new int[n + 1];
            dp[0] = 1;
            for (int j = 1; j <= n; j++) {
                for (int i = 1; i <= m; i++) {
                    if (j - i >= 0) dp[j] += dp[j - i];
                }
            }
            System.out.println(dp[n]);
        }
    }
}
```

- 时间复杂度: O(n \* m)
- 空间复杂度: O(n)

代码中m表示最多可以爬m个台阶，代码中把m改成2就是 力扣：70.爬楼梯的解题思路。

**当然注意 力扣是 核心代码模式，卡码网是ACM模式**

## [#](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html#%E6%80%BB%E7%BB%93)总结

**本题看起来是一道简单题目，稍稍进阶一下其实就是一个完全背包！**

如果我来面试的话，我就会先给候选人出一个 本题原题，看其表现，如果顺利写出来，进而在要求每次可以爬[1 - m]个台阶应该怎么写。

顺便再考察一下两个for循环的嵌套顺序，为什么target放外面，nums放里面。

这就能考察对背包问题本质的掌握程度，候选人是不是刷题背公式，一眼就看出来了。

这么一连套下来，如果候选人都能答出来，相信任何一位面试官都是非常满意的。

**本题代码不长，题目也很普通，但稍稍一进阶就可以考察完全背包，而且题目进阶的内容在leetcode上并没有原题，一定程度上就可以排除掉刷题党了，简直是面试题目的绝佳选择！**

## 

‍
