---
title: day08
date: '2026-01-08 19:57:21'
updated: '2026-01-29 17:38:30'
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

- # 卡码网：54.替换数字
- 给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。

  例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。

  对于输入字符串 "a5b"，函数应该将其转换为 "anumberb"

  输入：一个字符串 s,s 仅包含小写字母和数字字符。

  输出：打印一个新的字符串，其中每个数字字符都被替换为了number

  样例输入：a1b2c3

  样例输出：anumberbnumbercnumber

  数据范围：1 \<\= s.length \< 10000。

# 解法一：

```java
import java.util.Scanner;

public class Main {
    
    public static String replaceNumber(String s) {
        int count = 0; // 统计数字的个数
        int sOldSize = s.length();
        for (int i = 0; i < s.length(); i++) {
            if(Character.isDigit(s.charAt(i))){
                count++;
            }
        }
        // 扩充字符串s的大小，也就是每个空格替换成"number"之后的大小
        char[] newS = new char[s.length() + count * 5];
        int sNewSize = newS.length;
        // 将旧字符串的内容填入新数组
        System.arraycopy(s.toCharArray(), 0, newS, 0, sOldSize);
        // 从后先前将空格替换为"number"
        for (int i = sNewSize - 1, j = sOldSize - 1; j < i; j--, i--) {
            if (!Character.isDigit(newS[j])) {
                newS[i] = newS[j];
            } else {
                newS[i] = 'r';
                newS[i - 1] = 'e';
                newS[i - 2] = 'b';
                newS[i - 3] = 'm';
                newS[i - 4] = 'u';
                newS[i - 5] = 'n';
                i -= 5;
            }
        }
        return new String(newS);
    };
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String s = scanner.next();
        System.out.println(replaceNumber(s));
        scanner.close();
    }
}
```

# 解法二：

```java
// 为了还原题目本意，先把原数组复制到扩展长度后的新数组，然后不再使用原数组、原地对新数组进行操作。
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        int len = s.length();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= 0 && s.charAt(i) <= '9') {
                len += 5;
            }
        }
        
        char[] ret = new char[len];
        for (int i = 0; i < s.length(); i++) {
            ret[i] = s.charAt(i);
        }
        for (int i = s.length() - 1, j = len - 1; i >= 0; i--) {
            if ('0' <= ret[i] && ret[i] <= '9') {
                ret[j--] = 'r';
                ret[j--] = 'e';
                ret[j--] = 'b';
                ret[j--] = 'm';
                ret[j--] = 'u';
                ret[j--] = 'n';
            } else {
                ret[j--] = ret[i];
            }
        }
        System.out.println(ret);
    }
}
```

- 这段代码是解决\*\*“替换空格”​**或**​“替换数字”\*\*类算法题的经典解法（通常出现在剑指Offer或代码随想录中）。

  你的核心逻辑使用了​**双指针法（从后向前填充）** ，这是处理数组扩容且避免 $O(N^2)$ 元素移动的最佳方案。

  不过，你的代码中有一个​**隐蔽的逻辑 Bug**，通过编译没问题，但运行结果可能会出错。

  以下是详细的代码点评和逻辑解析：

  ### 1. 致命小 Bug：字符与数字的比较

  请看这一行代码：

  Java

  ```
  // 你的写法：0 是数字零（整型），ASCII码为0
  if (s.charAt(i) >= 0 && s.charAt(i) <= '9') 
  ```
  - **问题：**  你写的是数字 `0`​，而不是字符 `'0'`。
  - **后果：**  字符 `'0'`​ 的 ASCII 码是 48。而整数 `0` 的 ASCII 码是 null。

    - 任何字符的 ASCII 码都大于等于 `0`。
    - 这意味着，`!`​、`#`​、`$`​、`%`​ 等符号（ASCII码都在 33-47 之间），虽然不是数字，但满足 `>= 0`​ 且 `<= '9'`。
    - **结果：**  输入包含这些符号时，程序会错误地计算长度，并在后续步骤中把符号也替换成 "number"，导致逻辑混乱。

  **修正方法：**  加上单引号。

  Java

  ```
  // 修正后：'0' 是字符零
  if (s.charAt(i) >= '0' && s.charAt(i) <= '9')
  ```
  ---

  ### 2. 核心算法图解：为什么要从后向前？

  你的代码核心在于最后那个 `for` 循环。

  Java

  ```
  for (int i = s.length() - 1, j = len - 1; i >= 0; i--)
  ```
  - ​**指针** **​`i`​**​：指向**原始字符串**的末尾（读取数据）。
  - ​**指针** **​`j`​**​：指向**扩容后数组**的末尾（写入数据）。

  #### 为什么不从前向后填充？

  如果从前向后（`i=0` 开始）：

  1. 遇到数字，你需要插入 "number"（6个字符）。
  2. 因为数组是连续内存，插入位置后面的所有元素都必须**向后移动** 5 位来腾出空间。
  3. 这会导致时间复杂度变成 $O(N^2)$，效率极低。

  #### 从后向前的妙处

  - 因为 `j`​（新长度）一定大于等于 `i`（旧长度）。
  - **永远是在空地上操作：**  `j`​ 指向的位置要么是新扩容出来的空白区，要么是 `i` 已经扫描过不需要的数据区。
  - **不需要移动元素：**  直接填入，一步到位。时间复杂度为 $O(N)$。

  ---

  ### 3. 代码细节优化

  虽然逻辑是对的，但那个倒着写 "rebmun" 的部分稍微有点容易眼花。可以稍微封装一下，或者保持现状（这种写法在算法题中很常见，为了追求极致性能）。

  此外，你的代码先将字符串复制到了 `ret` 数组的前半部分：

  Java

  ```
  // 先复制原字符串
  for (int i = 0; i < s.length(); i++) {
      ret[i] = s.charAt(i);
  }
  ```
  然后在第二个循环中，`ret[i]`​ 读取的是刚刚复制进去的数据。​**这步操作是安全的**​，因为 `j`​ 始终比 `i`​ 跑得慢或者持平（`j >= i`​），所以 `j`​ 写入数据时，绝对不会覆盖 `i` 还没读取的数据。

  ### 4. 修正后的完整代码

  针对那个 `'0'` 的 Bug，修正后的版本如下：

  Java

  ```
  import java.util.*;

  public class Main {
      public static void main(String[] args) {
          Scanner sc = new Scanner(System.in);
          if (sc.hasNext()) { // 建议加上输入检查
              String s = sc.next();
              int len = s.length();
              
              // 1. 计算扩容后的长度
              for (int i = 0; i < s.length(); i++) {
                  // Bug 修复：这里必须是 '0'
                  if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                      len += 5; // 原本占1位，现在占6位("number")，增加5位
                  }
              }
              
              // 2. 定义新数组
              char[] ret = new char[len];
              
              // 3. 将原字符串复制到新数组头部 (这一步是为了符合你"在原数组操作"的模拟思路)
              for (int i = 0; i < s.length(); i++) {
                  ret[i] = s.charAt(i);
              }
              
              // 4. 双指针从后向前遍历
              // i 指向原字符串末尾，j 指向新数组末尾
              for (int i = s.length() - 1, j = len - 1; i >= 0; i--) {
                  // Bug 修复：这里必须是 '0'
                  if (ret[i] >= '0' && ret[i] <= '9') {
                      // 倒着填入 "number" -> r, e, b, m, u, n
                      ret[j--] = 'r';
                      ret[j--] = 'e';
                      ret[j--] = 'b';
                      ret[j--] = 'm';
                      ret[j--] = 'u';
                      ret[j--] = 'n';
                  } else {
                      // 不是数字，直接复制
                      ret[j--] = ret[i];
                  }
              }
              System.out.println(ret);
          }
      }
  }
  ```
  ### 总结

  你对**双指针**的理解非常到位，特别是**从后向前**的处理思路，这是解决此类数组原地操作问题的核心技巧。唯一需要注意的就是字符比较时的单引号问题。
