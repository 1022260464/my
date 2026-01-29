---
title: day08
date: '2026-01-08 19:57:21'
updated: '2026-01-29 14:04:10'
permalink: /post/day08-1kk3kj.html
comments: true
toc: true
---



# day08

# **第四章 字符串part01**

## 今日任务

●  344.反转字符串

●  541. 反转字符串II

●  卡码网：54.替换数字

##  详细布置

###  344.反转字符串

建议： 本题是字符串基础题目，就是考察 reverse 函数的实现，同时也明确一下 平时刷题什么时候用 库函数，什么时候 不用库函数

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html)</u>

### 541. 反转字符串II

建议：本题又进阶了，自己先去独立做一做，然后在看题解，对代码技巧会有很深的体会。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html](https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html)</u>

# 卡码网：54.替换数字

建议：对于线性数据结构，填充或者删除，后序处理会高效的多。好好体会一下。

[题目链接/文章讲解：]()​<u>[替换数字](https://programmercarl.com/kamacoder/0054.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html)</u>

# 344.反转字符串

[344. 反转字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string/submissions/690003858/)

```java
class Solution {
    public void reverseString(char[] s) {
        int n = s.length;
        for (int left = 0, right = n - 1; left < right; left++, right--) {
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/reverse-string/solutions/2376290/ji-chong-bu-tong-de-xie-fa-pythonjavacgo-9trb/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 541. 反转字符串II

[541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/description/)

```java
class Solution {
    public String reverseStr(String S, int k) {
        char[] s = S.toCharArray();
        int n = s.length;
        for (int i = 0; i < n; i += k * 2) {
            reverse(s, i, Math.min(i + k, n) - 1);
        }
        return new String(s);
    }

    private void reverse(char[] s, int left, int right) {
        while (left < right) {
            char tmp = s[left];
            s[left++] = s[right];
            s[right--] = tmp;
        }
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/reverse-string-ii/solutions/3056217/jian-dan-ti-jian-dan-zuo-pythonjavaccgoj-ig15/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

这段代码是 LeetCode 第 541 题  **“反转字符串 II”**  的标准优秀解法。

它非常高效地处理了字符串的​**分块操作**。为了让你彻底理解，我把它拆解成三个核心逻辑模块来讲解。

---

### 1. 核心逻辑图解

这道题的要求是：每隔 `2k`​ 个字符，反转前 `k` 个。

也就是说，字符串被看作是一个个大小为 `2k` 的“块”：

- **前 k 个：**  要反转。
- **后 k 个：**  保持原样。

代码最聪明的地方在于 `for` 循环的步长设计。

### 2. 代码深度解析

#### 第一步：转化为数组 (为什么要这么做？)

Java

```
char[] s = S.toCharArray();
```

- **原因：**  在 Java 中，`String`​ 是**不可变**的（Immutable）。你不能直接修改字符串里的某个字符（比如不能写 `S[0] = 'a'`）。
- **解决：**  所以必须先把它转换成 `char[]` 字符数组，这样就可以随意交换位置了。

#### 第二步：聪明的循环步长 (步长是 2k)

Java

```
// i 每次增加 k * 2，相当于直接跳到下一个“处理单元”的开头
for (int i = 0; i < n; i += k * 2) {
    // ...
}
```

- 普通循环通常是 `i++`​，但这里是 `i += k * 2`。
- 这意味 `i`​ 指向的永远是每个 `2k`​ 块的​**起始位置**​。你只需要关心从 `i` 开始的那一段怎么反转就行，后面的不用管。

#### 第三步：计算反转边界 (最难理解的一句)

Java

```
reverse(s, i, Math.min(i + k, n) - 1);
```

这句话是全篇的精华，它处理了​**两种情况**：

1. **正常情况（剩余字符够 k 个）：**

   - 我们要反转从 `i`​ 开始的 `k` 个字符。
   - 结束索引应该是 `i + k - 1`。
2. **尾部情况（剩余字符不够 k 个）：**

   - 如果字符串剩下不到 `k`​ 个字符了，题目要求是将​**剩下的全部反转**。
   - 这时 `i + k`​ 会超过字符串总长度 `n`。
   - 所以我们要用 `Math.min(i + k, n)` 来限制边界，不能越界。
   - ​ **​`- 1`​**​ **的原因：**  数组下标是从 0 开始的。如果长度是 `n`​，最后一个元素的下标是 `n-1`。

#### 第四步：双指针反转 (Helper 方法)

Java

```
private void reverse(char[] s, int left, int right) {
    while (left < right) {
        char tmp = s[left];
        s[left++] = s[right];
        s[right--] = tmp;
    }
}
```

- 这是最经典的​**双指针交换法**。
- ​`left`​ 指针往右走，`right` 指针往左走，如果不相遇，就互换手中的字符。

---

### 3. 模拟执行 (Dry Run)

假设字符串 `S = "abcdefg"`​, `k = 2`。

总长度 `n = 7`。

**第一次循环 (**​**​`i = 0`​**​ **)：**

- **目标：**  反转前 `k=2` 个。
- **计算边界：**  `Math.min(0 + 2, 7) - 1`​ \= `1`。
- **操作：**  反转索引 `0`​ 到 `1`​ ("ab") -\> 变成 "ba"。
- **当前结果：**  `"bacdefg"`。
- **下一步：**  `i`​ 增加 `2*k` (也就是 4)。

**第二次循环 (**​**​`i = 4`​**​ **)：**

- **位置：**  跳过了中间的 "cd"，现在 `i` 指向 'e'。
- **目标：**  反转接下来的 `k=2` 个。
- **计算边界：**  `Math.min(4 + 2, 7) - 1`​ \= `5`。
- **操作：**  反转索引 `4`​ 到 `5`​ ("ef") -\> 变成 "fe"。
- **当前结果：**  `"bacdfe g"`。
- **下一步：**  `i` 增加 4，变成 8。

**第三次判断：**

- ​`i = 8`​，`n = 7`​。`i < n` 不成立，循环结束。

---
