---
title: day07
date: '2026-01-07 13:16:44'
updated: '2026-01-07 18:40:31'
permalink: /post/day07-z1fbznd.html
comments: true
toc: true
---



# day07

# 第三章 哈希表part02

## 今日任务

●  454.四数相加II

●  383. 赎金信

●  15. 三数之和

●  18. 四数之和

●  总结  

## 详细布置

###  454.四数相加II

建议：本题是 使用map 巧妙解决的问题，好好体会一下 哈希法 如何提高程序执行效率，降低时间复杂度，当然使用哈希法 会提高空间复杂度，但一般来说我们都是舍空间 换时间， 工业开发也是这样。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html)</u>

### 383. 赎金信  

建议：本题 和 242.有效的字母异位词 是一个思路 ，算是拓展题

[题目链接/文章讲解：]()​<u>[https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)</u>

### 15. 三数之和

建议：本题虽然和 两数之和 很像，也能用哈希法，但用哈希法会很麻烦，双指针法才是正解，可以先看视频理解一下 双指针法的思路，文章中讲解的，没问题 哈希法很麻烦。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)</u>

### 18. 四数之和

建议： 要比较一下，本题和 454.四数相加II 的区别，为什么 454.四数相加II 会简单很多，这个想明白了，对本题理解就深刻了。 本题 思路整体和 三数之和一样的，都是双指针，但写的时候 有很多小细节，需要注意，建议先看视频。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)</u>​[ ]()

‍

# 454.四数相加II

- [ ] [454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/solutions/3038338/on2-jian-ji-xie-fa-pythonjavacgojsrust-b-paps/)

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        HashMap<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums1) {
            for (int y : nums2) {
                cnt.merge(x + y, 1, Integer::sum); // cnt[x+y]++
            }
        }

        int ans = 0;
        for (int x : nums3) {
            for (int y : nums4) {
                ans += cnt.getOrDefault(-x - y, 0);
            }
        }
        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/4sum-ii/solutions/3038338/on2-jian-ji-xie-fa-pythonjavacgojsrust-b-paps/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 你好！欢迎继续挑战。你现在接触到的这道题叫 ​ **“四数之和 II”** 。

  这道题的代码虽然短，但它包含了一个​**算法思想的飞跃**​（从暴力法到分治法），以及一个 Java 8 引入的高级 `HashMap`​ 语法：​**​`merge`​**。

  对于零基础的你来说，这段代码非常值得细读。我们还是分**思路**和**语法**两部分来拆解。

  ---

  ## 1. 核心解题思路：分组作战（分治思想）

  ### 问题是啥？

  给你四个数组（A, B, C, D），让你从每个数组里各取一个数，要求这四个数的和等于 0。

  即：\$A[i] + B[j] + C[k] + D[l] \= 0\$。

  ### 为什么不能直接暴力做？

  如果你写 4 个 `for` 循环嵌套（一层套一层），计算机会运行 $N \times N \times N \times N$ 次。如果数组只有 100 个数，就要算 1 亿次，太慢了！

  ### 聪明的解法：2+2 分组

  我们可以把这 4 个数组分成“前半组”和“后半组”：

  1. ​**前半场 (nums1, nums2)** ：

     - 算出这两个数组里所有可能的两数之和 $(a+b)$。
     - 把这些**和**以及它们**出现的次数**存进哈希表（Map）。
     - *比如：和为 5 出现了 3 次，那就记下来。*
  2. ​**后半场 (nums3, nums4)** ：

     - 算出这两个数组里所有可能的两数之和 $(c+d)$。
     - 我们要找的总和是 0。如果现在的和是 $(c+d)$，那我们需要去哈希表里找它的**相反数** $-(c+d)$。
     - *比如：如果你算出的和是 -5，那你就去查查“前半场”里有没有 5。如果有 3 个 5，那就能凑出 3 个等于 0 的组合。*

  这样，原本 4 层循环就被拆成了两个 2 层循环，速度快了非常多（从 $N^4$ 降到了 $N^2$）。

  ---

  ## 2. 重点语法补充：`merge` 方法

  这段代码里最让初学者困惑的应该是这一行：

  Java

  ```
  cnt.merge(x + y, 1, Integer::sum);
  ```

  这是一个非常高级且优雅的写法。回顾一下上一题（异位词）里我们是怎么计数的：

  **老写法 (你已经学会的)：**

  Java

  ```
  // 先查出来，加 1，再放回去
  cnt.put(key, cnt.getOrDefault(key, 0) + 1);
  ```

  新写法 (这里用的 merge)：

  merge 方法的意思是：“合并”。它一次性搞定了“如果不存在则新增，如果存在则修改”的逻辑。

  它的三个参数分别代表：

  1. ​**​`x + y`​**​  **(Key)** ：我们要存的“键”（两数之和）。
  2. ​**​`1`​**​  **(Value)** ​：​**如果这个 Key 之前不存在**，就直接存入这个值（相当于初始化为 1）。
  3. ​**​`Integer::sum`​**​  **(重映射函数)** ​：​**如果这个 Key 之前已经存在了**，该怎么办？

     - ​`Integer::sum` 是一个方法的引用，意思是“求和”。
     - 它会把**旧的值**和\*\*你提供的参数2（即那个 1）\*\*加起来，作为新的值存进去。

  **通俗翻译：**

  > “哈希表，麻烦你看一下 x+y 这个数字有没有。
  >
  > 如果没有，就把它存进去，数量记为 1。
  >
  > 如果有，就把旧的数量和 1 加起来（也就是数量+1）。”
  >

  所以，这一行代码完美替代了之前那一长串 `put`​ + `getOrDefault`。

  ---

  ## 3. 代码逐行解析

  ### 第一部分：前半场记录 (nums1, nums2)

  Java

  ```
  HashMap<Integer, Integer> cnt = new HashMap<>(); // 创建记账本
  for (int x : nums1) {           // 遍历第一个数组
      for (int y : nums2) {       // 遍历第二个数组
          // 计算和，并更新哈希表
          // cnt[和] = cnt[和] + 1
          cnt.merge(x + y, 1, Integer::sum); 
      }
  }
  ```

  - ​**逻辑**​：把 `nums1`​ 和 `nums2`​ 能组合出的所有“和”，以及这些“和”出现的次数，都记录在 `cnt` 里。

  ### 第二部分：后半场查找 (nums3, nums4)

  Java

  ```
  int ans = 0;                    // 记录最终找到的组合总数
  for (int x : nums3) {           // 遍历第三个数组
      for (int y : nums4) {       // 遍历第四个数组
          // 核心公式： (a+b) + (c+d) = 0  =>  (a+b) = -(c+d)
          // 我们现在的和是 x+y，我们要去 Map 里找的是 -(x+y)
          // 也就是 -x - y
          ans += cnt.getOrDefault(-x - y, 0);
      }
  }
  return ans;
  ```

  - ​ **​`-x - y`​**​：这就是 `-(x + y)`，也就是我们需要找的“另一半”。
  - ​**​`cnt.getOrDefault(..., 0)`​** ：去哈希表里查。

    - 如果有：比如找到了 3 次，说明能组成 3 个 0，把 `3` 加到答案里。
    - 如果没有：返回 `0`，答案不变。

  ---

  ## 4. 总结

  1. ​**算法套路**​：遇到这种“多个数组凑数”的问题，如果直接遍历太慢，就试着\*\*“中间劈开，两两分组”\*\*，用空间（哈希表）换时间。
  2. ​**Java 技能点**：

     - ​**​`map.merge(key, 1, Integer::sum)`​**  是一个用来计数的超级简便写法。
     - 它的作用等同于：`map.put(key, map.getOrDefault(key, 0) + 1)`。

  建议你可以试着把上一题（字母异位词）的代码，用这个新的 `merge` 方法改写一下，看看能不能写出来？这会让你对这个新语法记忆非常深刻！

# 383. 赎金信  

- [ ] [383. 赎金信 - 力扣（LeetCode）](https://leetcode.cn/problems/ransom-note/submissions/689746315/)

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote.length() > magazine.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (char c : magazine.toCharArray()) {
            cnt[c - 'a']++;
        }
        for (char c : ransomNote.toCharArray()) {
            if (--cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/ransom-note/solutions/3643383/jian-dan-ti-jian-dan-zuo-pythonjavaccgoj-nh3q/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
