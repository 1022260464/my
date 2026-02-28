---
title: day03
date: '2026-01-02 15:51:38'
updated: '2026-01-29 17:39:12'
permalink: /post/day03-zivsbw.html
comments: true
toc: true
---



# day03

-  详细布置

   209.长度最小的子数组

  题目建议： 本题关键在于理解滑动窗口，这个滑动窗口看文字讲解 还挺难理解的，建议大家先看视频讲解。  拓展题目可以先不做。

  题目链接：https://leetcode.cn/problems/minimum-size-subarray-sum/  
  文章讲解：https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html  
  视频讲解：https://www.bilibili.com/video/BV1tZ4y1q7XE

   59.螺旋矩阵II

  题目建议：  本题关键还是在转圈的逻辑，在二分搜索中提到的区间定义，在这里又用上了。 

  题目链接：https://leetcode.cn/problems/spiral-matrix-ii/  
  文章讲解：https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html  
  视频讲解：https://www.bilibili.com/video/BV1SL4y1N7mV/

  # 区间和前缀和是一种思维巧妙很实用 而且 很有容易理解的一种算法思想，大家可以体会一下：

  文章讲解：https://www.programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html

  开发商购买土地  
  https://www.programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html  
  总结

  题目建议：希望大家 也做一个自己 对数组专题的总结

  文章链接：https://programmercarl.com/%E6%95%B0%E7%BB%84%E6%80%BB%E7%BB%93%E7%AF%87.html
- # 这里贴上灵神的题单

  本期视频介绍了三类典型的滑动窗口题目：最短/最长/方案数，代表题目分别为（包含其它语言代码）：  
  209. 长度最小的子数组 https://leetcode.cn/problems/minimum-size-subarray-sum/solution/biao-ti-xia-biao-zong-suan-cuo-qing-kan-k81nh/  
  3. 无重复字符的最长子串 https://leetcode.cn/problems/longest-substring-without-repeating-characters/solution/xia-biao-zong-suan-cuo-qing-kan-zhe-by-e-iaks/  
  713. 乘积小于 K 的子数组 https://leetcode.cn/problems/subarray-product-less-than-k/solution/xia-biao-zong-suan-cuo-qing-kan-zhe-by-e-jebq/

  课后作业：  
  3090. 每个字符最多出现两次的最长子字符串 https://leetcode.cn/problems/maximum-length-substring-with-two-occurrences/  
  2958. 最多 K 个重复元素的最长子数组 https://leetcode.cn/problems/length-of-longest-subarray-with-at-most-k-frequency/  
  2730. 找到最长的半重复子字符串 https://leetcode.cn/problems/find-the-longest-semi-repetitive-substring/  
  1004. 最大连续 1 的个数 III https://leetcode.cn/problems/max-consecutive-ones-iii/  
  2962. 统计最大元素出现至少 K 次的子数组 https://leetcode.cn/problems/count-subarrays-where-max-element-appears-at-least-k-times/  
  2302. 统计得分小于 K 的子数组数目 https://leetcode.cn/problems/count-subarrays-with-score-less-than-k/  
  1658. 将 x 减到 0 的最小操作数 https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/  
  76. 最小覆盖子串 https://leetcode.cn/problems/minimum-window-substring/

  题解在每道题目的题解区中。

  【题单】滑动窗口：  
  https://leetcode.cn/circle/discuss/0viNMK/

---

# 209.长度最小的子数组

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int n = nums.length;
        int right = 0;
        int sum =0;
        int ans = n+1;
        for(right=0;right<=n-1;right++){
           sum +=nums[right];//在循环中不断累加和
           while(sum-nums[left]>=target){
            //如果左端点可以减去
            //控制左端点缩小
            sum -=nums[left];
            left++;
           }
           if(sum>=target){
            ans= Math.min(ans,right-left+1);
            //返回最小数组长度
           }
          
        }
         return ans<=n? ans:0;
}
}
//第二种写法在while循环中更新答案
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int ans = n + 1;
        int sum = 0; // 子数组元素和
        int left = 0; // 子数组左端点
        for (int right = 0; right < n; right++) { // 枚举子数组右端点
            sum += nums[right];
            while (sum >= target) { // 满足要求
//疑惑点，这样不是只有当我的sum符合要求时才会进行左指针缩短操作吗？其实只要我的sum是大于target的，我的循环就会一直执行下去，只有当我的sum<target的时候才会跳出循环，此时写
//入ans的操作并不会执行，还是保持在上次的数值。
                ans = Math.min(ans, right - left + 1);
                sum -= nums[left];
                left++; // 左端点右移
            }
        }
        return ans <= n ? ans : 0;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/minimum-size-subarray-sum/solutions/1959532/biao-ti-xia-biao-zong-suan-cuo-qing-kan-k81nh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```

## 解题思路：

- 题目要求返回最小长度的子数组，因此定义一个滑动窗口（有点像快慢指针），通过右指针去控制我窗口的右边界，也就是终点，左指针去控制的左边界，也就是滑动窗口的起点。当我的sum和，在右指针不断右移的过程当中，如果左指针可以删除，则更新，保持是最小的滑动窗口。如果大于等于target则更新答案。
- 注意：代码的while循环，即收缩环节，必须要写在if判断前。当我进窗操作执行完成，必须要先执行收缩环节才能够去更新我的答案。否则会导致实际答案过大。

## 一些进阶知识点

- 为什么第二种（标准写法）的速度要更快？

  这是一个非常敏锐的观察！

  虽然这两种写法的时间复杂度在理论上都是 $O(N)$，但在实际运行中（尤其是在像 LeetCode 这样的大规模测试用例下），​ **“标准写法”（在 while 内部更新）往往比“预判写法”（你的这段代码）略快**。

  主要原因在于**CPU 指令执行的开销**和​**判断条件的复杂度**。

  我们可以把这两者做一个微观对比：

  ### 1. 循环判断条件的开销 (这是主要原因)

  我们来看看 `while` 这一行代码在底层发生了什么。

  - **你的写法（预判法）：**   
    Java

    ```
    while (sum - nums[left] >= target)
    ```

    每一次循环判断，CPU 都需要做以下动作：

    1. ​**内存读取**​：去数组里把 `nums[left]` 的值取出来。
    2. ​**算术运算**​：执行 `sum - nums[left]` 的减法。
    3. ​**逻辑比较**​：把减法结果和 `target` 比较。

    ​**关键点**：哪怕循环即将结束（即条件不满足时），这最后一次“失败的判断”也必须完整执行一遍“读取+减法+比较”，这是一种计算资源的浪费。
  - **标准写法（直接法）：**   
    Java

    ```
    while (sum >= target)
    ```

    每一次循环判断，CPU 只需要做：

    1. ​**逻辑比较**​：直接比较寄存器里的 `sum`​ 和 `target`。

    ​**对比**：标准写法的判断极其廉价，没有内存读取，没有减法运算。

  ### 2. `Math.min` 的开销 vs 数组访问开销

  你可能会想：“但是标准写法在循环里多次调用了 `Math.min`，难道不耗时吗？”

  - ​**你的写法**​：`Math.min`​ 只执行 1 次（在循环外），但 `sum - nums[left]` 这种“带数组访问的减法”执行了 N 次。
  - ​**标准写法**​：`Math.min` 执行了 N 次，但循环判断非常快。

  在计算机底层中：

  - **数组访问 (Array Access)**  是相对较慢的操作（可能涉及缓存未命中）。
  - **Math.min** 是极快的操作（现代 CPU 有专门的指令，或者简单的条件跳转）。

  ​**结论**​：用 N 次“昂贵的数组读取+减法”来换取减少 N 次“极廉价的 Math.min”，这笔交易通常是**亏本**的。

  ### 3. 一个生动的类比

  想象你在过高速收费站（Target 是收费金额）：

  - 你的写法（预判法）：  
    你每往前挪一步，都要先掏出计算器，算一下：“如果我现在把车上那个胖子赶下去，剩下的钱还够不够交过路费？”

    - 你需要​**先算**，再决定要不要赶人。算这个过程很累。
  - 标准写法（直接法）：  
    你不管三七二十一，先看现在的钱够不够。只要够，就记录一下（Math.min），然后把胖子赶下去。再看钱够不够……

    - 你只看​**现状**。看一眼是很轻松的。虽然你多记录了几次，但比起每次都要“掏出计算器做减法”，还是看一眼更快。

  ### 总结

  虽然两者都是 $O(N)$，但因为你的写法在**高频执行**的 `while`​ 条件判断中加入了**数组访问**和​**减法运算**，增加了常数级的开销，所以通常会比标准写法稍慢一点点。

  更重要的是，​**标准写法逻辑更简单，不容易写错边界条件**​（不需要在最后补一个 `if` 判断），所以更推荐标准写法。

# 59.螺旋矩阵II

```java
 class Solution {
    public int[][] generateMatrix(int n) {
        int l = 0, r = n - 1, t = 0, b = n - 1;
        int[][] mat = new int[n][n];//初始化一个n*n数组
        int num = 1, tar = n * n;
        while(num <= tar){
            for(int i = l; i <= r; i++) mat[t][i] = num++; // left to right.
            t++;//上边界加一
            for(int i = t; i <= b; i++) mat[i][r] = num++; // top to bottom.
            r--;//右边界减一
            for(int i = r; i >= l; i--) mat[b][i] = num++; // right to left.
            b--;//下边界减一
            for(int i = b; i >= t; i--) mat[i][l] = num++; // bottom to top.
            l++;//左边界加一
        }
        return mat;
    }
}


```

![ccff416fa39887c938d36fec8e490e1861813d3bba7836eda941426f13420759-Picture1](/source/images/ccff416fa39887c938d36fec8e490e1861813d3bba7836eda941426f13420759-Picture1-20260102172151-3gmhkot.png)

```java
class Solution {
    private static final int[][] DIRS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}}; // 右下左上
//

    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int i = 0;
        int j = 0;
        int di = 0;//di代表选择的档位，对应于第几行
        for (int val = 1; val <= n * n; val++) { // 要填入的数
            ans[i][j] = val;//先进行填入
            int x = i + DIRS[di][0];//初始化，选中第一列
            int y = j + DIRS[di][1]; // 下一步的位置，选中第二列
            // 如果 (x, y) 出界或者已经填入数字
            if (x < 0 || x >= n || y < 0 || y >= n || ans[x][y] != 0) {
                di = (di + 1) % 4; // 右转 90°，取模运算，总共4行，确保3加一后不会越界而是返回到0
//这表格设计太厉害了！
            }
            i += DIRS[di][0];//检测后判断再走
            j += DIRS[di][1]; // 走一步
        }
        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/spiral-matrix-ii/solutions/3059650/jian-ji-xie-fa-pythonjavaccgojsrust-by-e-fhmr/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

|**di (行号/方向)**| **[0] (第1列：行变化量)**| **[1] (第2列：列变化量)**|**含义**|
| --| --| --| ---------------------|
|**0**|**0**|**1**|向右 (行不变，列+1)|
|**1**|**1**|**0**|向下 (行+1，列不变)|
|**2**|**0**| **-1**|向左 (行不变，列-1)|
|**3**| **-1**|**0**|向上 (行-1，列不变)|

# 灵神就是神！！！！！

#### 复杂度分析

- 时间复杂度：O(n2)。
- 空间复杂度：O(1)。返回值不计入。

‍
