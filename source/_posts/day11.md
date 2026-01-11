---
title: day11
date: '2026-01-11 18:10:14'
updated: '2026-01-11 19:13:04'
permalink: /post/day11-z1j0ucx.html
comments: true
toc: true
---



# day11

# **第五章 栈与队列part02**

### 

### 150. 逆波兰表达式求值

本题不难，但第一次做的话，会很难想到，所以先看视频，了解思路再去做题

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html](https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html)</u>

###  239. 滑动窗口最大值 （有点难度，可能代码写不出来，但一刷至少需要理解思路）

之前讲的都是栈的应用，这次该是队列的应用了。

本题算比较有难度的，需要自己去构造单调队列，建议先看视频来理解。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html)</u>

### 347.前 K 个高频元素  （有点难度，可能代码写不出来，一刷至少需要理解思路）

大/小顶堆的应用， 在C++中就是优先级队列

本题是 大数据中取前k值 的经典思路，了解想法之后，不算难。

[题目链接/文章讲解/视频讲解：]()​<u>[https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html](https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html)</u>

###  总结

栈与队列做一个总结吧，加油

<u>[https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E6%80%BB%E7%BB%93.html](https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E6%80%BB%E7%BB%93.html)</u>

‍

# 150. 逆波兰表达式求值

[150. 逆波兰表达式求值 - 力扣（LeetCode）](https://leetcode.cn/problems/evaluate-reverse-polish-notation/solutions/3830661/li-jie-ti-yi-yong-zhan-mo-ni-pythonjavac-tr9x/)

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> st = new ArrayDeque<>(); // 更快的写法见【Java 数组】
        for (String token : tokens) {
            char c = token.charAt(0);
            if (token.length() > 1 || Character.isDigit(c)) { // token 是数字
                st.push(Integer.parseInt(token));
                continue;
            }

            int x = st.pop();
            int y = st.pop(); // 无法直接修改栈顶
            if (c == '+') {
                st.push(y + x);
            } else if (c == '-') {
                st.push(y - x);
            } else if (c == '*') {
                st.push(y * x);
            } else {
                st.push(y / x);
            }
        }
        return st.peek();
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/evaluate-reverse-polish-notation/solutions/3830661/li-jie-ti-yi-yong-zhan-mo-ni-pythonjavac-tr9x/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 数组写法：

```java
class Solution {
    public int evalRPN(String[] tokens) {
        int[] st = new int[(tokens.length + 1) / 2]; // 用数组模拟栈
        int top = -1; // 栈顶下标
        for (String token : tokens) {
            char c = token.charAt(0);
            if (token.length() > 1 || Character.isDigit(c)) { // token 是数字
                st[++top] = Integer.parseInt(token); // 入栈
                continue;
            }

            int x = st[top--]; // 出栈
            if (c == '+') {
                st[top] += x;
            } else if (c == '-') {
                st[top] -= x;
            } else if (c == '*') {
                st[top] *= x;
            } else {
                st[top] /= x;
            }
        }
        return st[top];
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/evaluate-reverse-polish-notation/solutions/3830661/li-jie-ti-yi-yong-zhan-mo-ni-pythonjavac-tr9x/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 好的，我们继续按照\*\*“先看工具（语法），再看图纸（思路）”\*\*的模式，来拆解这道 LeetCode 150 题：​**逆波兰表达式求值**。

  这道题其实就是计算机视角的“做算术题”。

  ---

  ### 第一部分：语法总结 (你手里的工具)

  这段代码里大部分工具你都见过了，但有一个**判定数字的逻辑**写得非常老练（骚操作），值得细看。

  #### 1. `Integer.parseInt(String s)`

  - ​**作用**：把“字符串的样子”变成“真正的数字”。
  - ​*例子*：

    - ​`"123"`​ (String) -\> `123` (int)。
    - ​`"-5"`​ (String) -\> `-5` (int)。
  - ​**注意**：如果字符串里不是合法的整数（比如 "abc"），它会报错。但这道题保证输入合法。

  #### 2. 判断是不是数字 (核心难点)

  代码里有一行非常精简的判断：

  Java

  ```
  if (token.length() > 1 || Character.isDigit(c))
  ```

  为什么要这样写？ 为什么不直接用 Character.isDigit？

  这是为了处理 负数。

  - **情况 A：正数，如**  **​`"5"`​** 

    - ​`length` 是 1，不满足第一个条件。
    - ​`c`​ 是 `'5'`​，`isDigit`​ 为真。-\> ​**判定为数字**。
  - **情况 B：负数，如**  **​`"-3"`​** 

    - ​`c`​ 是 `'-'`​ (负号)，`isDigit` 是假（负号不是数字字符）。
    - ​**补救措施**​：`token.length() > 1`​ 为真（因为 "-3" 长度是 2）。-\> ​**判定为数字**。
  - **情况 C：运算符，如**  **​`"+"`​** ​ **或**  **​`"-"`​** 

    - ​`length` 是 1。
    - ​`c`​ 是 `'+'`，不是数字。
    - 两个都不满足。-\> ​**判定为运算符**。

  ​**结论**​：这行代码完美区分了 `"3"`​、`"-3"`​ 和 `"-"` (减号)。

  #### 3. 栈的操作

  依旧是 `push`​ (进) 和 `pop` (出)。

  - 注意：这里栈存的是 `Integer`​ (数字)，不是 `Character`，因为我们要算出结果，而不仅仅是匹配符号。

  ---

  ### 第二部分：思路总结 (你的解题图纸)

  **逆波兰表达式 (RPN)**  看起来很吓人，其实它就是\*\*“后缀表达式”\*\*。

  - 平常我们写：`1 + 2` （运算符在中间）
  - 逆波兰写法：`1 2 +` （运算符在后面）

  **它的最大好处是：不需要括号，也不需要考虑“先乘除后加减”，遇到符号直接算就行。**

  #### 1. 核心策略：数字进栈，符号开火

  想象栈是一个 ​ **“备菜区”** 。

  - ​**遇到数字**​：还没到处理的时候，先切好放在备菜区（​**入栈**）。
  - ​**遇到符号**：马上开始炒菜！从备菜区拿两个最近的原料出来算，算完把新菜放回去。

  #### 2. 关键动作：出栈顺序 (初学者最大的坑)

  Java

  ```
  int x = st.pop(); // 第一个出来的是“右操作数”
  int y = st.pop(); // 第二个出来的是“左操作数”
  // ...
  st.push(y - x);   // 注意是 y - x，不是 x - y
  ```

  **千万注意顺序！**

  - 栈是后进先出。
  - 对于表达式 `3 - 1`​，逆波兰式是 `3 1 -`。
  - ​**进栈顺序**​：先进 `3`​ (在底)，后进 `1` (在顶)。
  - ​**出栈顺序**：

    - 第一次 `pop`​ 拿到 `1`​ (赋值给 `x`)。
    - 第二次 `pop`​ 拿到 `3`​ (赋值给 `y`)。
  - ​**计算**​：应该是 `3 - 1`​，也就是 `y - x`。
  - ​*口诀*​：​**后出的 减/除 先出的**。

  #### 3. 模拟演示 (手把手)

  输入：["2", "1", "+", "3", "\*"]

  (对应普通算式：(2 + 1) \* 3)

  1. ​**读到 "2"** ：是数字。

     - 栈：`[2]`
  2. ​**读到 "1"** ：是数字。

     - 栈：`[2, 1]` (1 在顶)
  3. ​**读到 "+"** ​：是符号！**开火！**

     - ​`pop`​ 出 `1` (x)
     - ​`pop`​ 出 `2` (y)
     - 计算 `2 + 1 = 3`
     - 把结果 `3` 扔回栈里。
     - 栈：`[3]`
  4. ​**读到 "3"** ：是数字。

     - 栈：`[3, 3]`
  5. ​**读到 "*"** ​：是符号！**开火！**

     - ​`pop`​ 出 `3` (x)
     - ​`pop`​ 出 `3` (y)
     - 计算 `3 * 3 = 9`
     - 把结果 `9` 扔回栈里。
     - 栈：`[9]`
  6. ​**结束**​：返回栈顶的 `9`。

  ### 总结给初学者

  1. ​**工具上**​：学会用 `Integer.parseInt`​ 转数字，学会用 `length > 1` 这种小技巧辅助判断负数。
  2. ​**逻辑上**：

     - ​**看到数字就存**。
     - ​**看到符号就拿两个出来算**，算完把结果存回去。
     - ​**特别小心减法和除法**​：`pop`​ 出来的第一个数是放在减号/除号 **后面** 的！

#  239. 滑动窗口最大值 （有点难度，可能代码写不出来，但一刷至少需要理解思路）

‍

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1]; // 窗口个数
        Deque<Integer> q = new ArrayDeque<>(); // 更快的写法见【Java 数组】

        for (int i = 0; i < n; i++) {
            // 1. 右边入
            while (!q.isEmpty() && nums[q.getLast()] <= nums[i]) {
                q.removeLast(); // 维护 q 的单调性
            }
            q.addLast(i); // 注意保存的是下标，这样下面可以判断队首是否离开窗口

            // 2. 左边出
            int left = i - k + 1; // 窗口左端点
            if (q.getFirst() < left) { // 队首离开窗口
                q.removeFirst();
            }

            // 3. 在窗口左端点处记录答案
            if (left >= 0) {
                // 由于队首到队尾单调递减，所以窗口最大值就在队首
                ans[left] = nums[q.getFirst()];
            }
        }

        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/sliding-window-maximum/solutions/2499715/shi-pin-yi-ge-shi-pin-miao-dong-dan-diao-ezj6/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 数组写法:

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1]; // 窗口个数
        int[] q = new int[n];
        int head = 0, tail = -1;

        for (int i = 0; i < n; i++) {
            // 1. 右边入
            while (head <= tail && nums[q[tail]] <= nums[i]) {
                tail--; // 右边出队
            }
            q[++tail] = i; // 右边入队

            // 2. 左边出
            int left = i - k + 1; // 窗口左端点
            if (q[head] < left) { // 队首离开窗口
                head++;
            }

            // 3. 在窗口左端点处记录答案
            if (left >= 0) {
                // 由于队首到队尾单调递减，所以窗口最大值就在队首
                ans[left] = nums[q[head]];
            }
        }

        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/sliding-window-maximum/solutions/2499715/shi-pin-yi-ge-shi-pin-miao-dong-dan-diao-ezj6/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这道题是 LeetCode 239 ​**滑动窗口最大值**​。它是面试中非常高频的题目，也是  **“单调队列”**  这个数据结构的入门经典。

  还是老规矩：先看你手里的​**工具（语法）** ​，再看​**图纸（思路）** 。

  ---

  ### 第一部分：语法总结 (你手里的工具)

  这段代码的核心工具还是 `Deque`（双端队列），但用法比之前更灵活，两头都在操作。

  #### 1. 双端操作 API 速查

  - ​**队尾操作（处理新来的）** ：

    - ​`getLast()`：看一眼排在最后的那个人。
    - ​`removeLast()`：把排在最后的那个人踢走（出队）。
    - ​`addLast(i)`：让新人排到队尾。
  - ​**队首操作（处理过期的）** ：

    - ​`getFirst()`：看一眼排在最前面的老大。
    - ​`removeFirst()`：把老大送走（出队）。

  #### 2. 关键点：为什么存下标 `i`​ 而不是数值 `nums[i]`？

  这是新手最容易卡住的地方。

  Java

  ```
  q.addLast(i); // 注意保存的是下标
  ```

  - ​**原因**​：我们需要判断\*\*“这个元素是不是滑出窗口了”\*\*。
  - ​**逻辑**​：只有通过下标 `i`​，我们才能算出它离当前位置有多远。如果只存数值 `5`​，你就不知道这个 `5` 是刚才进来的，还是十轮之前进来的。
  - ​**取值时**​：虽然存的是下标，但在比较大小时，要用 `nums[q.getLast()]` 去拿真正的数值来比。

  #### 3. 数组大小的数学题

  Java

  ```
  int[] ans = new int[n - k + 1];
  ```

  - 如果你有 5 个数字，窗口大小是 3。
  - 窗口能滑几次？`[1,2,3], 4, 5`​ -\> `1, [2,3,4], 5`​ -\> `1, 2, [3,4,5]`。一共 3 次。
  - ​**公式**​：`n - k + 1` 就是窗口滑动的总次数。

  ---

  ### 第二部分：思路总结 (你的解题图纸)

  这道题的暴力解法是：每移动一步，就重新遍历一遍窗口找最大值，太慢了。

  我们要用的神器叫 “单调队列”。

  #### 1. 核心思想：残酷的职场淘汰制

  想象这个队列是一个\*\*“精英团队”，规则极其残酷，只有“有用”的人才能留在里面。

  所谓“单调队列”，就是队列里的元素必须严格按照从大到小排序\*\*。

  - ​**队首**​：永远是当前窗口里的​**最大值**（老大）。
  - ​**队尾**：是刚进来的新人（潜力股）。

  #### 2. 详细流程：两头都在忙

  我们遍历数组，每遇到一个新元素 `nums[i]`，就做三件事：

  **第一步：去尾（淘汰弱者）—— 这是最精彩的一步**

  Java

  ```
  // 如果新来的(nums[i]) 比 队尾的那个人 还要强（或一样强）
  while (!q.isEmpty() && nums[q.getLast()] <= nums[i]) {
      q.removeLast(); // 队尾那个弱者直接滚蛋
  }
  ```

  - ​**逻辑**​：如果新来的员工比老员工​**能力强**​（数值大），而且新员工还​**更年轻**（下标大，能留得更久），那老员工就彻底没用了。
  - ​**结果**：经过这一波清洗，队列里留下的都是比新人大的（或者队列空了）。
  - ​*金句*：“既生瑜，何生亮”。既然我（新来的）比你大，你又比我先过期，那你毫无存在的意义，删掉！

  **第二步：去头（退休离职）**

  Java

  ```
  // left 是当前窗口最左边的边界
  int left = i - k + 1;
  // 如果队首的老大(q.getFirst()) 的下标小于 left，说明他滑出窗口了
  if (q.getFirst() < left) {
      q.removeFirst();
  }
  ```

  - ​**逻辑**：老大虽然最大，但如果他的位置已经不在当前窗口里了（过期了），那也得走人。

  **第三步：记录答案**

  Java

  ```
  // 只要窗口完全形成了（i >= k-1）
  if (left >= 0) {
      ans[left] = nums[q.getFirst()]; // 队首永远是当前最大的
  }
  ```

  - ​**逻辑**​：因为我们在第一步保证了队列是“单调递减”的，所以​**队首一定就是当前窗口的最大值**。直接拿来用！

  #### 3. 模拟演示 (以 `[1, 3, -1, -3, 5]`​, k\=3 为例)

  |**步骤**|**当前元素**|**队列操作 (存下标)**|**队列状态 (对应数值)**|**窗口最大值 (ans)**||||||
  | --| ------| ----------------------------------------------------------| ------| ------------| --| --| --| --| --|
  |**i=0**|​`1`|队列空，直接进|​`[1]`|(窗口未满)||||||
  |**i=1**|​`3`|3比1大，1被踢走，3进|​`[3]`|(窗口未满)||||||
  |**i=2**|​`-1`|-1比3小，乖乖排后面|​`[3, -1]`|**3**(队首)||||||
  |**i=3**|​`-3`|-3比-1小，乖乖排后面|​`[3, -1, -3]`|**3**(队首)||||||
  |**i=4**|​`5`|**大清洗！** <br /><br />5比-3大，踢-3<br /><br />5比-1大，踢-1<br /><br />5比3大，踢3<br /><br />5进|​`[5]`|**5**(队首)||||||

   *(注意：i=3时，如果窗口继续滑动，原本的 3 就会因为下标过期而在“去头”步骤被移走)*

  ### 总结给初学者

  1. ​**工具**​：用 `Deque`​，存​**下标**。
  2. ​**核心口诀**：

     - ​**进队前**：比我小的全踢走（保持队首最大）。
     - ​**进队后**：检查队首有没有过期（保持在窗口内）。
     - ​**取结果**：队首就是大哥。

# 347.前 K 个高频元素  （有点难度，可能代码写不出来，一刷至少需要理解思路）

[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/description/)

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // 第一步：统计每个元素的出现次数
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum); // cnt[x]++
        }
        int maxCnt = Collections.max(cnt.values());

        // 第二步：把出现次数相同的元素，放到同一个桶中
        List<Integer>[] buckets = new ArrayList[maxCnt + 1];
        Arrays.setAll(buckets, _ -> new ArrayList<>());
        for (Map.Entry<Integer, Integer> e : cnt.entrySet()) {
            buckets[e.getValue()].add(e.getKey());
        }

        // 第三步：倒序遍历 buckets，把出现次数前 k 大的元素加入答案
        int[] ans = new int[k];
        int j = 0;
        for (int i = maxCnt; i >= 0 && j < k; i--) {
            // 注意题目保证答案唯一，一定会出现某次循环结束后 j 恰好等于 k 的情况
            for (int x : buckets[i]) {
                ans[j++] = x;
            }
        }
        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/top-k-frequent-elements/solutions/3655287/tong-pai-xu-on-xian-xing-zuo-fa-pythonja-oqq2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# 数组写法:

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        int mn = nums[0];
        int mx = mn;
        for (int x : nums) {
            mn = Math.min(mn, x);
            mx = Math.max(mx, x);
        }

        // 第一步：统计每个元素的出现次数
        int[] cnt = new int[mx - mn + 1];
        int maxCnt = 0;
        for (int x : nums) {
            cnt[x - mn]++; // 避免负数下标
            maxCnt = Math.max(maxCnt, cnt[x - mn]);
        }

        // 第二步：把出现次数相同的元素，放到同一个桶中
        List<Integer>[] buckets = new ArrayList[maxCnt + 1];
        Arrays.setAll(buckets, _ -> new ArrayList<>());
        for (int x = mn; x <= mx; x++) {
            buckets[cnt[x - mn]].add(x);
        }

        // 第三步：倒序遍历 buckets，把出现次数前 k 大的元素加入答案
        int[] ans = new int[k];
        int j = 0;
        for (int i = maxCnt; i >= 0 && j < k; i--) {
            // 注意题目保证答案唯一，一定会出现某次循环结束后 j 恰好等于 k 的情况
            for (int x : buckets[i]) {
                ans[j++] = x;
            }
        }
        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/top-k-frequent-elements/solutions/3655287/tong-pai-xu-on-xian-xing-zuo-fa-pythonja-oqq2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 这道题是 LeetCode 347 ​**前 K 个高频元素**。

  这也是一道非常经典的题目。通常有两种解法：一种是用 ​**堆 (Heap/PriorityQueue)** ​，另一种就是你贴的这段代码使用的 ​**桶排序 (Bucket Sort)** 。

  桶排序的方法更巧妙，它是典型的 ​ **“空间换时间”** ，速度非常快（$O(N)$）。

  还是老规矩：先看​**工具（语法）** ​，再看​**图纸（思路）** 。

  ---

  ### 第一部分：语法总结 (你手里的工具)

  这段代码用了很多 Java 的\*\*“现代写法”\*\*（Java 8 及以上的新特性），写起来很简洁，但初学者可能看着眼晕。我们一个个拆解。

  #### 1. `cnt.merge(x, 1, Integer::sum)` —— 计数神器

  这是 `Map` 里的一个高级方法，用来统计次数极其方便。

  - ​**传统写法**：
  - ​**merge 写法**：

    - ​`x`：Key（要统计的数字）。
    - ​`1`：如果这个 Key 以前不存在，就给它赋值 1。
    - ​`Integer::sum`​：如果这个 Key 已经存在了，就把**旧值**和\*\*新值(1)\*\*加起来（sum）。
  - ​**总结**：这一句话就完成了“统计出现次数”的所有逻辑。

  #### 2. `List<Integer>[] buckets` —— 数组里装列表

  Java

  ```
  List<Integer>[] buckets = new ArrayList[maxCnt + 1];
  ```
  - 这叫 ​ **“列表数组”** 。
  - ​**形象理解**​：这是一个数组（像一排柜子），但是柜子的每一个格子里，放的不是一个数字，而是一个 ​**List（可以伸缩的袋子）** 。
  - ​**用途**：因为可能有很多数字出现的次数是一样的（比如 A 出现了 3 次，B 也出现了 3 次），所以同一个格子里要能装下多个数字。

  #### 3. `Arrays.setAll` —— 批量初始化

  Java

  ```
  Arrays.setAll(buckets, _ -> new ArrayList<>());
  ```
  - ​**问题**​：刚创建的数组 `buckets`​ 里面全是 `null`​。如果你直接用 `buckets[i].add(...)` 会报空指针错误。
  - ​**传统写法**​：写个 `for`​ 循环，给每个位置 `new ArrayList()`。
  - ​**setAll 写法**：这是 Arrays 工具类的方法，意思是“给数组的每一个位置都执行后面的操作”。
  - ​**关于**  **​`_`​** ​：这是 Java 的 Lambda 表达式写法。通常这里写 `i`​ 代表索引，但因为我们在创建 List 时并不关心索引是多少，所以用下划线 `_`​ 表示 ​ **“我不关心这个参数叫什么”** （这是较新版本 Java 的一种习惯写法，表示匿名变量）。

  ---

  ### 第二部分：思路总结 (你的解题图纸)

  这道题的核心思路是 ​ **“反客为主”** 。

  - ​**普通思维**​：`数字 -> 出现次数`。

    - 比如：1 出现了 3 次。
  - ​**桶排序思维**​：`出现次数 -> 数字有哪些`。

    - 比如：出现 3 次的数字有 [1, 5, ...]。

  #### 1. 核心步骤拆解

  第一步：统计选票

  遍历数组，算出每个数字的“得票数”。

  - 输入：`[1, 1, 1, 2, 2, 3]`
  - Map 结果：

    - ​`1`​ -\> 3 票
    - ​`2`​ -\> 2 票
    - ​`3`​ -\> 1 票
  - 最牛的得票数 (`maxCnt`) 是 3。

  第二步：建立“频率桶” (最关键的一步)

  我们要建一个数组，数组的下标代表“得票数”。

  - 创建一个大小为 4 (0\~3) 的数组 `buckets`。
  - 遍历 Map，把数字扔进对应的“得票桶”里：

    - 数字 `1`​ 有 3 票 -\> 扔进 `buckets[3]`
    - 数字 `2`​ 有 2 票 -\> 扔进 `buckets[2]`
    - 数字 `3`​ 有 1 票 -\> 扔进 `buckets[1]`
  - ​**桶的状态**：

    - 下标 3: `[1]`​  \<-- (出现最频繁的)
    - 下标 2: `[2]`
    - 下标 1: `[3]`
    - 下标 0: `[]`

  第三步：倒着收割

  题目要 前 K 个 高频元素。

  - 因为桶的​**下标越大**​，代表​**出现频率越高**。
  - 所以我们要**从后往前**遍历桶。
  - 假设 `k=2`：

    1. 先看 `buckets[3]`​：拿到了 `1`。 (现在有了 1 个答案)
    2. 再看 `buckets[2]`​：拿到了 `2`。 (现在有了 2 个答案)
    3. 够了，收工！返回 `[1, 2]`。

  #### 2. 为什么要用桶排序？

  如果用普通的排序算法（快排），时间复杂度是 \$O(N \\log N)\$。

  而桶排序的时间复杂度是 \$O(N)\$。

  - 因为我们不需要对所有元素进行复杂的比较，我们只需要“数数”，然后把它们扔进对应的坑位里就行了。

  #### 3. 模拟演示

  输入：`nums = [1,1,1,2,2,3]`​, `k = 2`

  1. ​**Map 统计**​：`{1:3, 2:2, 3:1}`​。最大频率 `maxCnt = 3`。
  2. ​**建桶**：

     - ​`buckets` 数组长度为 4 (索引 0-3)。
     - ​`1`​ 放入 `buckets[3]`。
     - ​`2`​ 放入 `buckets[2]`。
     - ​`3`​ 放入 `buckets[1]`。
  3. ​**取结果**：

     - 倒序循环 `i` 从 3 开始。
     - ​`buckets[3]`​ 里有 `1`​。`ans = [1, 0]`​，`j` 变成 1。
     - ​`i` 变成 2。
     - ​`buckets[2]`​ 里有 `2`​。`ans = [1, 2]`​，`j` 变成 2。
     - ​`j < k`​ 不再满足 (2 \< 2 False)，循环结束。
  4. ​**返回**​：`[1, 2]`。

  ### 总结给初学者

  1. ​**语法上**​：学会 `merge`​ 这个偷懒写法；理解 `List[]` 是“装着列表的数组”。
  2. ​**逻辑上**：

     - ​**第一次映射**​：`数字 -> 次数` (用 Map)。
     - ​**第二次映射**​：`次数 -> 数字` (用数组下标)。
     - ​**最后**：从大下标（高频）往下拿，拿到 k 个就停。
