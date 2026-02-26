---
title: day46
date: '2026-02-26 21:02:24'
updated: '2026-02-26 21:12:34'
permalink: /post/day46-zikrxj.html
comments: true
toc: true
---



# day46

# 第九章 动态规划part13

今天 我们就要结束动态规划章节了，大家激不激动！！！

## ** 详细布置 **

### ** 647. 回文子串   **

动态规划解决的经典题目，如果没接触过的话，别硬想 直接看题解。

<u>[https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html](https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html)</u>

### ** 516.最长回文子序列 **

647. 回文子串，求的是回文子串，而本题要求的是回文子序列， 大家要搞清楚两者之间的区别。

<u>[https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html](https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html)</u>

### ** 动态规划总结篇 **

<u>[https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E6%80%BB%E7%BB%93%E7%AF%87.html](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E6%80%BB%E7%BB%93%E7%AF%87.html)</u>

‍

### 647. 回文子串

[647. 回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindromic-substrings/solutions/3038224/shi-pin-on-manacher-suan-fa-xiang-xi-zhu-3ky7/)

## 方法一：中心扩展法

```java
class Solution {
    pu** 647. 回文子串   **blic int countSubstrings(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < 2 * n - 1; i++) {
            int l = i / 2, r = (i + 1) / 2;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                ans++;
                l--;
                r++;
            }
        }
        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/palindromic-substrings/solutions/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 太妙了！在我们经历了长篇累牍的动态规划（DP）数组和状态转移之后，这段代码就像是一道极其清脆的开胃小菜。

  这是 LeetCode 647 题：​**回文子串 (Palindromic Substrings)** ​。虽然这道题完全可以用二维 DP 来做（定义 `dp[i][j]`​ 表示 `s[i...j]`​ 是否为回文），但灵茶山艾府在这里选用了一种​**空间复杂度碾压 DP 的绝妙解法——中心扩展法 (Expand Around Center)** 。

  这段代码最令人拍案叫绝的地方，在于它用一行极简的数学公式，把奇数长度和偶数长度的回文串**完美统一**了。

  让我们来拆解这段充满“数学之美”的逻辑：

  ### 核心破局点：回文串的“中心”到底在哪？

  回文串有一个极其对称的性质：从中心向两边展开，字符是一模一样的。

  但是，中心点有两种情况：

  1. ​**奇数长度**​（如 `aba`​）：中心是中间的那个字符 `b`。
  2. ​**偶数长度**​（如 `abba`​）：中心是中间两个字符 `b`​ 和 `b`​ 之间的​**缝隙**。

  如果按照常规的写法，我们需要写两个 `for` 循环：一个以单个字符为中心向外扩展，另一个以两个字符的间隙为中心向外扩展。代码会显得非常臃肿。

  ### 魔法公式：`2 * n - 1` 与整除的艺术

  灵茶山艾府用了一个极其聪明的技巧：**把字符和缝隙统统当成“中心点”！**

  对于一个长度为 `n`​ 的字符串，它有 `n`​ 个字符，以及 `n - 1`​ 个缝隙。加起来正好是 ​**​`2 * n - 1`​**​ **个潜在的中心点**！

  Java

  ```
  for (int i = 0; i < 2 * n - 1; i++) {
      int l = i / 2, r = (i + 1) / 2;
  ```

  这里的变量 `i`​ 就是这 `2 * n - 1` 个中心点的编号。接下来的这两行整除运算，简直是神来之笔（注意 Java 中整数除法会向下取整）：

  - ​**当** **​`i`​**​ **是偶数时（代表以字符为中心）** ​：  
    例如 `i = 2`​。`l = 2 / 2 = 1`​，`r = 3 / 2 = 1`​。左右指针 `l`​ 和 `r`​ 都指向索引 `1`​。这代表寻找**奇数长度**的回文串。
  - ​**当** **​`i`​**​ **是奇数时（代表以缝隙为中心）** ​：  
    例如 `i = 3`​。`l = 3 / 2 = 1`​，`r = 4 / 2 = 2`​。左指针指向 `1`​，右指针指向 `2`​。这代表寻找**偶数长度**的回文串。

  一个循环，一套公式，直接把奇偶情况全部通吃！

  ### 代码逐行拆解与向外扩展

  Java

  ```
      while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
  ```

  - ​**同步扩展**​：一旦确定了初始的 `l`​ 和 `r`（不管它们是指向同一个数，还是相邻的两个数），我们就开始向左右两边“同步扩张”。
  - ​**边界防御**​：`l >= 0 && r < n` 保证扩张不会越出字符串的边界。
  - ​**回文判定**​：`s.charAt(l) == s.charAt(r)` 保证两端新增的字符是相等的。

  Java

  ```
          ans++;
          l--;
          r++;
      }
  }
  return ans;
  ```

  - ​**发现目标**​：只要进到了 `while`​ 循环里，就说明找到了一个合法的回文子串，`ans` 计数器加一。
  - ​**继续扩张**​：左指针左移 `l--`​，右指针右移 `r++`​，继续去探索更长的回文可能。一旦两端字符不一样，或者碰到了边界，`while` 循环就会停止，进入下一个中心点的探测。

  ### 模拟运行 (Dry Run)

  假设字符串 `s = "aba"`​，长度 `n = 3`​。共有 `2 * 3 - 1 = 5` 个中心点。

  |**中心点 i**|**l**  **=**  **i/2**|**r**  **=**   **(i+1)/2**|**while 循环内的扩展与比较 (s[l]**   **==**  **s[r])**|**找到的回文串**|**ans 累加**|||||||
  | ---------------| ------| ------| --------------------------------------------------| ------------| ------| --| --| --| --| --| --|
  |​**​`0`​**(字符'a')|​`0`|​`0`|​`s[0]=='a' == s[0]=='a'`(匹配)<br /><br />下一次:`l=-1, r=1`(越界停止)|​`"a"`|​`1`|||||||
  |​**​`1`​**(缝隙)|​`0`|​`1`|​`s[0]=='a' != s[1]=='b'`(不匹配)|无|不变|||||||
  |​**​`2`​**(字符'b')|​`1`|​`1`|​`s[1]=='b' == s[1]=='b'`(匹配)<br /><br />下一次:`s[0]=='a' == s[2]=='a'`(匹配)<br /><br />下一次:`l=-1, r=3`(越界)|​`"b"`<br /><br />`"aba"`|​`+2`|||||||
  |​**​`3`​**(缝隙)|​`1`|​`2`|​`s[1]=='b' != s[2]=='a'`(不匹配)|无|不变|||||||
  |​**​`4`​**(字符'a')|​`2`|​`2`|​`s[2]=='a' == s[2]=='a'`(匹配)<br /><br />下一次:`l=1, r=3`(越界)|​`"a"`|​`+1`|||||||

  最终返回 `ans = 4`​（分别是 `"a"`​, `"b"`​, `"aba"`​, `"a"`）。

  ### 复杂度分析

  - ​**时间复杂度**：$O(N^2)$。有 $2N - 1$ 个中心点，每个中心点向外扩展最多 $N/2$ 次。整体依然是平方级别，但常数项极小，实际运行速度远快于二维 DP。
  - ​**空间复杂度**：$O(1)$！这是它完爆动态规划（通常需要 $O(N^2)$ 空间）的最大亮点。全场只用了几个整型变量。

  ---

  中心扩展法可以说是处理“回文串”问题的万能钥匙。

  既然你已经掌握了这把钥匙，如果在同样的思路下，题目从“求所有回文子串的数量”变成了\*\*“求最长的那个回文子串” (LeetCode 5: Longest Palindromic Substring)\*\*，你觉得在 `while` 循环里，我们还需要多维护哪两个变量就能搞定它呢？

## 方法二：Manacher 算法

```java
class Solution {
    public int countSubstrings(String s) {
        // Manacher 模板
        // 将 s 改造为 t，这样就不需要讨论 len(s) 的奇偶性，因为新串 t 的每个回文子串都是奇回文串（都有回文中心）
        // s 和 t 的下标转换关系：
        // (si+1)*2 = ti
        // ti/2-1 = si
        // ti 为偶数，对应奇回文串（从 2 开始）
        // ti 为奇数，对应偶回文串（从 3 开始）
        int n = s.length();
        char[] t = new char[n * 2 + 3];
        Arrays.fill(t, '#');
        t[0] = '^';
        for (int i = 0; i < n; i++) {
            t[i * 2 + 2] = s.charAt(i);
        }
        t[n * 2 + 2] = '$';

        // 定义一个奇回文串的回文半径=(长度+1)/2，即保留回文中心，去掉一侧后的剩余字符串的长度
        // halfLen[i] 表示在 t 上的以 t[i] 为回文中心的最长回文子串的回文半径
        // 即 [i-halfLen[i]+1,i+halfLen[i]-1] 是 t 上的一个回文子串
        int[] halfLen = new int[t.length - 2];
        halfLen[1] = 1;

        int ans = 0;
        // boxR 表示当前右边界下标最大的回文子串的右边界下标+1
        // boxM 为该回文子串的中心位置，二者的关系为 r=mid+halfLen[mid]
        int boxM = 0;
        int boxR = 0;
        for (int i = 2; i < halfLen.length; i++) {
            int hl = 1;
            if (i < boxR) {
                // 记 i 关于 boxM 的对称位置 i'=boxM*2-i
                // 若以 i' 为中心的最长回文子串范围超出了以 boxM 为中心的回文串的范围（即 i+halfLen[i'] >= boxR）
                // 则 halfLen[i] 应先初始化为已知的回文半径 boxR-i，然后再继续暴力匹配
                // 否则 halfLen[i] 与 halfLen[i'] 相等
                hl = Math.min(halfLen[boxM * 2 - i], boxR - i);
            }

            // 暴力扩展
            while (t[i - hl] == t[i + hl]) {
                hl++;
                boxM = i;
                boxR = i + hl;
            }

            halfLen[i] = hl;
            ans += hl / 2;
        }

        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/palindromic-substrings/solutions/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 太震撼了！恭喜你，终于触碰到了回文串领域的“圣杯”——​**马拉车算法 (Manacher's Algorithm)** ！

  如果你在面试中写出上一题的“中心扩展法”，面试官会给你发 Offer；但如果你能行云流水地写出这段 Manacher 算法，面试官可能会直接站起来跟你握手。

  这也是整个算法界中，把“利用已知历史信息来避免重复计算”这一动态规划哲学发挥到最为极致的算法之一。它硬生生地把中心扩展法 O(N²) 的时间复杂度，通过“照镜子”的魔法，降维打击到了恐怖的 ​**O(N)** 。

  Manacher 算法非常烧脑，但灵茶山艾府的这段模板代码极其工整。让我们深呼吸，一起来拆解这个精妙绝伦的魔法：

  ### 魔法第一步：字符串改造（统一奇偶，无痛边界）

  Java

  ```
  char[] t = new char[n * 2 + 3];
  Arrays.fill(t, '#');
  t[0] = '^';
  // ... 填充字符 ...
  t[n * 2 + 2] = '$';
  ```

  - ​**解决奇偶痛点**​：在字符之间、首尾都强行插入 `#`​。  
    比如 `aba`​ (奇数) 变成了 `^#a#b#a#$`​，`abba`​ (偶数) 变成了 `^#a#b#b#a#$`​。改造后，​**所有回文串的长度全变成了奇数**，也就是它们必定拥有一个实实在在的“中心字符”！我们再也不用去区分“以字符为中心”还是“以缝隙为中心”了。
  - ​**解决越界痛点**​：首尾分别放上不同的特殊字符 `^`​ 和 `$`​（哨兵）。这样在往两边扩展（`t[i - hl] == t[i + hl]`​）时，即使碰到了字符串边缘，由于 `^`​ 永远不等于 `$`​，`while`​ 循环会自动停止，彻底免去了写 `l >= 0 && r < t.length` 这种烦人的边界判断。

  ### 魔法第二步：回文半径数组 `halfLen`

  这个数组是记录历史经验的“备忘录”。

  - ​`halfLen[i]`​ 记录的是以 `t[i]`​ 为中心，向单侧（包含中心自身）能扩展的最大长度，也就是​**回文半径**。

  ### 魔法第三步：照镜子（O(N) 的核心机密）

  这是整段代码最核心、也是最烧脑的几行：

  Java

  ```
  int boxM = 0; // 最右回文“盒子”的中心
  int boxR = 0; // 最右回文“盒子”的右边界

  // ... 循环中 ...
  if (i < boxR) {
      hl = Math.min(halfLen[boxM * 2 - i], boxR - i);
  }
  ```

  想象我们有一个一直在往右移动的“最大回文盒子”（由 `boxM`​ 和 `boxR` 框定）。

  当我们来到了一个新的中心点 `i` 时：

  1. ​**如果在盒子外面 (**​**​`i >= boxR`​**​ **)** ​：没有任何历史经验可以借鉴，老老实实从 `hl = 1` 开始往外暴力扩展。
  2. ​**如果在盒子里面 (**​**​`i < boxR`​**​ **)** ​：魔法发生了！  
     因为盒子本身是个巨大的回文串，它是**左右对称**的！既然 `i`​ 在盒子的右半边，那么在盒子的左半边，一定存在一个和它完全对称的镜像点 `i' = boxM * 2 - i`。

     - 我们去查阅左边镜像点 `i'`​ 的历史记录 `halfLen[i']`。
     - 既然左右对称，那右边的 `i`​ **至少**也能扩展出和左边 `i'` 一样的回文半径！
     - ​**木桶效应限制**​：但是，如果左边 `i'`​ 的回文半径太大了，导致右边 `i`​ 的扩展范围超过了当前盒子 `boxR`​ 的保护罩，盒子外面的世界是未知的，不能直接照抄。所以我们只能抄到 `boxR - i` 为止。
     - 这就是 `Math.min(halfLen[boxM * 2 - i], boxR - i)`​ 的终极奥义：**在安全范围内，直接白嫖镜像点的计算结果！**

  ### 魔法第四步：继续扩展与更新盒子

  Java

  ```
  while (t[i - hl] == t[i + hl]) {
      hl++;
      boxM = i;
      boxR = i + hl;
  }
  ```

  - 白嫖完保底的 `hl`​ 后，继续尝试向两边扩展。由于有了极高的保底值，这里的 `while` 循环极大概率直接进不去，或者只执行一两次，从而将平摊时间复杂度降为 O(1)。
  - 如果当前的 `i`​ 扩展出了一个更靠右的边界，果断更新 `boxM`​ 和 `boxR`，把我们的“历史经验保护罩”推向更远的未来。

  ### 魔法第五步：数学映射与结果累加

  Java

  ```
  ans += hl / 2;
  ```

  - 每算出一个回文半径 `hl`​，它对应原字符串 `s` 中多少个回文串呢？
  - 因为我们插入了 `#`​，原串长度和新串半径之间存在一个绝妙的数学等式：以 `t[i]`​ 为中心，真实回文串的数量刚好等于 `hl / 2`（向下取整）。我们直接累加这个数值即可。

  ### 复杂度分析

  - ​**时间复杂度**​：O(N)。虽然 `for`​ 循环里套了个 `while`​ 循环，但因为 `boxR`​ 是一直向右单调递增的，绝不回头。`boxR`​ 最多走到 `2N`​，所以平摊下来每次 `for` 循环内部的执行次数是常数级别的，绝对的 O(N)！
  - ​**空间复杂度**​：O(N)。开辟了长度为 `2N + 3`​ 的新字符数组 `t`​ 和半径数组 `halfLen`。

  ---

  行云流水，酣畅淋漓！

  从暴力的 O(N³)，到中心扩展的 O(N²)，再到动态规划的空间压缩，最后用马拉车算法完成 O(N) 的最终绝杀。你已经完美通关了“字符串匹配与回文”这块极其难啃的硬骨头。

  现在，你的算法武器库里已经装满了大杀器。接下来，你是想继续挑战底层数据结构——​**去手撕几道关于链表 (Linked List) 或二叉树 (Binary Tree) 的经典题目**​，还是想换个维度，去看看大厂面试最爱考的**滑动窗口 (Sliding Window)**  问题？

# 516.最长回文子序列

[516. 最长回文子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)

## 一、记忆化搜索

```java
class Solution {
    public int longestPalindromeSubseq(String S) {
        char[] s = S.toCharArray();
        int n = s.length;
        int[][] memo = new int[n][n];
        for (int[] row : memo) {
            Arrays.fill(row, -1); // -1 表示还没有计算过
        }
        return dfs(0, n - 1, s, memo);
    }

    private int dfs(int i, int j, char[] s, int[][] memo) {
        if (i > j) {
            return 0; // 空串
        }
        if (i == j) {
            return 1; // 只有一个字母
        }
        if (memo[i][j] != -1) { // 之前计算过
            return memo[i][j];
        }
        if (s[i] == s[j]) {
            return memo[i][j] = dfs(i + 1, j - 1, s, memo) + 2; // 都选
        }
        return memo[i][j] = Math.max(dfs(i + 1, j, s, memo), dfs(i, j - 1, s, memo)); // 枚举哪个不选
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/longest-palindromic-subsequence/solutions/2203001/shi-pin-jiao-ni-yi-bu-bu-si-kao-dong-tai-kgkg/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 二、1:1 翻译成递推

```java
class Solution {
    public int longestPalindromeSubseq(String S) {
        char[] s = S.toCharArray();
        int n = s.length;
        int[][] f = new int[n][n];
        for (int i = n - 1; i >= 0; i--) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                f[i][j] = s[i] == s[j] ? f[i + 1][j - 1] + 2 :
                        Math.max(f[i + 1][j], f[i][j - 1]);
            }
        }
        return f[0][n - 1];
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/longest-palindromic-subsequence/solutions/2203001/shi-pin-jiao-ni-yi-bu-bu-si-kao-dong-tai-kgkg/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 三、空间优化

```java
class Solution {
    public int longestPalindromeSubseq(String S) {
        char[] s = S.toCharArray();
        int n = s.length;
        int[] f = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            f[i] = 1;
            int pre = 0; // 初始值为 f[i+1][i]
            for (int j = i + 1; j < n; j++) {
                int tmp = f[j];
                f[j] = s[i] == s[j] ? pre + 2 : Math.max(f[j], f[j - 1]);
                pre = tmp;
            }
        }
        return f[n - 1];
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/longest-palindromic-subsequence/solutions/2203001/shi-pin-jiao-ni-yi-bu-bu-si-kao-dong-tai-kgkg/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
