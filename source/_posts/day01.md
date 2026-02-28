---
title: day01
date: '2025-12-31 14:40:04'
updated: '2026-01-29 17:39:31'
permalink: /post/day01-z2whn1w.html
comments: true
toc: true
---



# day01

### 第一章  数组part01

#### 今日任务

#### 详细布置

‍

##### 数组理论基础

文章链接：[https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

题目建议： 了解一下数组基础，以及数组的内存空间地址，数组也没那么简单。

##### 704. 二分查找

题目建议： 大家今天能把 704.二分查找 彻底掌握就可以，至于 35.搜索插入位置 和 34. 在排序数组中查找元素的第一个和最后一个位置 ，如果有时间就去看一下，没时间可以先不看，二刷的时候在看。

先把 704写熟练，要熟悉 根据 左闭右开，左闭右闭 两种区间规则 写出来的二分法。

题目链接：[https://leetcode.cn/problems/binary-search/](https://leetcode.cn/problems/binary-search/)

文章讲解：[https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)

视频讲解：[https://www.bilibili.com/video/BV1fA4y1o715](https://www.bilibili.com/video/BV1fA4y1o715)

##### 27. 移除元素

题目建议：  暴力的解法，可以锻炼一下我们的代码实现能力，建议先把暴力写法写一遍。 双指针法 是本题的精髓，今日需要掌握，至于拓展题目可以先不看。 

题目链接：[https://leetcode.cn/problems/remove-element/](https://leetcode.cn/problems/remove-element/)

文章讲解：[https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)

视频讲解：[https://www.bilibili.com/video/BV12A4y1Z7LP](https://www.bilibili.com/video/BV12A4y1Z7LP)

##### 

##### 977.有序数组的平方

题目建议： 本题关键在于理解双指针思想 

题目链接：[https://leetcode.cn/problems/squares-of-a-sorted-array/](https://leetcode.cn/problems/squares-of-a-sorted-array/)

文章讲解：[https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html](https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html)

视频讲解： [https://www.bilibili.com/video/BV1QB4y1D7ep](https://www.bilibili.com/video/BV1QB4y1D7ep)

# 基本知识点：

- 左闭右开，左闭右闭区间

##### 704. 二分查找[https://leetcode.cn/problems/binary-search/](https://leetcode.cn/problems/binary-search/)

---

- ```java
    class Solution {
      public int search(int[] nums, int target) {
          //左闭右闭区间
          int left =0;
          int n = nums.length;
          int right = n-1;
          int ans =0;
          while (left<=right){
              int numsmid =(left+right)/2;//int相加有可能会溢出，此处如果是奇数呢？3/2?会向下取整，符合数学计算逻辑
              if(nums[right]<target||nums[left]>target){//错误点：此处仅判断了target左溢出的情况，忽略了右溢出的情况
                  return -1;
                  
              }//此处应该放在外面进行提前剪枝，同时可以省去因为二分查找可以进行越界检查。
              if(nums[numsmid]>target)
              {//中值大于target，更新右边界,numsmid不包含
                  right= numsmid-1;
              }else if(nums[numsmid]<target)
                  {//中值小于target，更新左边界，不包含
                      left = numsmid+1;
                  }
              else
                  {
                      return ans =numsmid;//错误点：直接返回了ans没有赋值
                  }
          }
          return ans;
      }
  }
  ```

## 解题思路：

- 二分查找法本质上是在双指针法上的一个延伸，同样是通过缩小搜索区域来实现。在二分查找法当中，首先要明确的一个问题就是，区间的确定。普遍采用的方式有两种，左闭右开，左闭右闭区间。通过明确区间的定义，在全局当中确定一个边界的处理规则，有助于理解并理清思路。
- #### 左闭右闭区间：

	可以理解为我的两个双指针都是指向的真实的值，left指针指向左边第一个数据，right指针指向右边最后一个数据（初始化为nums.length -1）。可以用画数轴的方式去理解。此时已经初始化了一个我的基本搜索区域，那么我该如何进行循环条件的定义？当我是左闭右闭的区间，我需要思考我的循环初始条件和结束条件，以及特殊值。这有点类似于函数的定义域与值域，以及特殊值的研究。我的二分查找法的循环，基本逻辑是两个指针从左右两边相互逼近，最终会在找到值的时候停下，当我的两个指针相遇时，左右指针是否能够同时指向一个值？当左作闭右闭区间的时候 ，我的两个指针是可以指向同一个值的，因为两个指针指向的都是有效值域。也就对应于while(left<=target)  。当我的中间值小于target值时代表,我的搜索区域应当在mid与right之间，此时更新左指针，因为我的mid指针此时已不在搜索区域类，所以left+1。

小于则同理。

##### 思考：

1.如果不写while ()等于号会怎么样？

- 在 `[left, right]`​ 写法中，当 `left == right` 时，区间是有效的（包含一个元素），必须进入循环检查它。

2.如果更新指针时，将mid值包含进去会怎么样？用特殊值去理解

- #### 死循环场景模拟 (Right \= mid)

  假设数组 `[2, 5]`​，查找 `1` (比所有数都小)。

  - ​`left = 0`​, `right = 1`。
  - ​`mid = 0`​ (因为 `(0+1)/2 = 0`)。
  - ​`nums[0] (2) > target (1)`，说明要在左边找。
  - ​**错误写法**​：`right = mid`​ -\> `right`​ 变成了 `0`。
  - ​**下一轮**​：`left = 0`​, `right = 0`。

    - ​`mid = 0`。
    - ​`nums[0] > target`。
    - ​`right = mid`​ -\> `right`​ 还是 `0`。
    - **死循环！**  指针卡住不动了。

‍

### 左闭右开区间

- 两个双指针，一个指向左边第一个有效数字，right指向有效数字后一位（类似于栈指针）。确定了搜索区域，再进行循环条件判断，我的两个双指针并不会相遇，因为右指针始终指向最后一位有效数字的后一位。在条件判断时，移动右指针，只需等于mid即可，此时已经不包含mid。移动左指针同理。
- ‍

  > [!CAUTION]
  > 模版有一个优化,这是一种工程化的写法，有助于解决整数溢出的问题，导致运算结果溢出变为负数。直观上的理解就是：
  >
  > 两条15.1的绳子相加再去进行>>2,绳子总长30，相加的结果发生了溢出，此时符号位改变变为负数，结果出错。
  >
  > 但是我现在先算出两端之间的距离的一半（15.1），然后再让左端向前走一半的距离，这就有效避免了整数的溢出。
  >
  > - ```java
  >   int mid = left + (right - left) / 2;
  >   //或者用位运算右移一位
  >   int mid = left + ((right - left) >> 1);
  >   ```
  >

### 模版：

```java
class Solution {
    public int search(int[] nums, int target) {
        int i = lowerBound(nums, target); // 选择其中一种写法即可
        return i < nums.length && nums[i] == target ? i : -1;
    }

    // 【下面列了三种写法，选一种自己喜欢的就行】

    // lowerBound 返回最小的满足 nums[i] >= target 的 i
    // 如果数组为空，或者所有数都 < target，则返回 nums.length
    // 要求 nums 是非递减的，即 nums[i] <= nums[i + 1]

    // 闭区间写法
    private int lowerBound(int[] nums, int target) {
        int left = 0, right = nums.length - 1; // 闭区间 [left, right]
        while (left <= right) { // 区间不为空
            // 循环不变量：
            // nums[left-1] < target
            // nums[right+1] >= target
            int mid = left + (right - left) / 2;
            if (nums[mid] < target)
                left = mid + 1; // 范围缩小到 [mid+1, right]
            else
                right = mid - 1; // 范围缩小到 [left, mid-1]
        }
        return left; // 或者 right+1
    }

    // 左闭右开区间写法
    private int lowerBound2(int[] nums, int target) {
        int left = 0, right = nums.length; // 左闭右开区间 [left, right)
        while (left < right) { // 区间不为空
            // 循环不变量：
            // nums[left-1] < target
            // nums[right] >= target
            int mid = left + (right - left) / 2;
            if (nums[mid] < target)
                left = mid + 1; // 范围缩小到 [mid+1, right)
            else
                right = mid; // 范围缩小到 [left, mid)
        }
        return left; // 或者 right
    }

    // 开区间写法
    private int lowerBound3(int[] nums, int target) {
        int left = -1, right = nums.length; // 开区间 (left, right)
        while (left + 1 < right) { // 区间不为空
            // 循环不变量：
            // nums[left] < target
            // nums[right] >= target
            int mid = left + (right - left) / 2;
            if (nums[mid] < target)
                left = mid; // 范围缩小到 (mid, right)
            else
                right = mid; // 范围缩小到 (left, mid)
        }
        return right; // 或者 left+1
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/binary-search/solutions/2023397/er-fen-cha-zhao-zong-shi-xie-bu-dui-yi-g-eplk/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 时间复杂度：O(logn)，其中 n 为 nums 的长度。
- 空间复杂度：O(1)。

# 27. 移除元素[https://leetcode.cn/problems/remove-element/](https://leetcode.cn/problems/remove-element/)

## 基本知识点：

- 数组在内存中是连续存储的，当调用erase函数其本质是O(N)的，我需要找到这个数字并删除，然后将后面的每一个值向前赋值。也就是说在内存的实际存储当中

  我的最后一个数字是仍然存在的。但是例如c++和java都会对数组进行一个封装，当返回的时候我们看到的size实际上就是-1后的数字。

#### 暴力解法：

- 暴力解法的时间复杂度是O（N^2）,并不推荐

```java
//暴力解法
class Solution {
    public int removeElement(int[] nums, int val) {
	// 暴力法
        int n = nums.length;//两个for循环嵌套
        for (int i = 0; i < n; i++) {
            if (nums[i] == val) {//寻找数字，找到则将后面的数字向前赋值。此种方法的时间复杂度其实是O（n^2)的
                for (int j = i + 1; j < n; j++) {
                    nums[j - 1] = nums[j];
                }
                i--;//if循环当中，找到数字i需要减一，比如当我的数组长度为5，找到位于第2位的数字，此时n-1，i指向第2位。
//当把后面的元素往前移位覆盖时，新替补上来的那个数字占据了当前坐标 i。此时我的nums【2】已经在循环开始检查过了，
//所以此时后面移上来的数字（如果存在相同情况则会漏检）不会被检查。所以需要将i后退一位。
                n--;
            }
        }
        return n;
    }
}


```

### 快慢指针法：

- 利用双指针快速的缩小我的搜索空间（勘误，快速进行数据搬移），解决了一层for循环所带来的n数量级的时间复杂度增长。

```java
class Solution {
    public int removeElement(int[] nums, int val) {//快慢指针法，我在一开始习惯性的用了对撞指针，但是没想出来
        int n = nums.length;
        int left = 0;
        for (int right = 0; right < n; right++) {
            if (nums[right] != val) {//循环条件，nums不等于val值，此时right不等于，向前保存
                nums[left] = nums[right];
                left++;
            }
        }
        return left;
    }
}
```

#### 解题思路：

- 实际上定义了一个快慢指针，用于简化for循环操作，将两层for循环简化为一层for循环。我的快指针指向的是我当前数组中，也就是旧数组当中的元素，慢指针指向的是新数组当中的下 标索引。当我找到旧数组中的元素等于target值的时候，left保持原地不动（说明此时无新值传入）right向前移动，else如果不等于，则将新值赋值给慢指针对应的新数组。
- 综上，我感觉对于双指针的办法就是通过将重复的更新数组的操作提取出来。 此时双指针不再扮演缩小搜索空间的角色，而是在进行数据搬移

  ###### 以下为gemini给出的答案：
- 你现在的理解非常到位！你的直觉很敏锐， **“将重复更新数组的操作提取出来”**  这句话其实抓住了优化的核心。

  ### 1. 双指针到底起到了什么作用？

  你可以把双指针法想象成\*\*“在一个数组里同时操作两个逻辑数组”\*\*。

  - **原来的暴力法：**  是在“修补”数组。一旦发现坏掉的（`val`），就把后面所有的砖块都搬过来填坑。搬一次是 $O(N)$，最坏情况要搬 $N$ 次，所以是 $O(N^2)$。
  - **现在的双指针法：**  是在“重建”数组。我们不再填坑，而是只挑好的砖块重排。

  #### 指针的具体分工（角色扮演）：

  - **快指针 (**​**​`right`​**​ **) —— “侦察兵 / 搬运工”**

    - ​**职责**：它负责遍历整个旧数组（搜索空间）。
    - ​**作用**​：它的任务是**寻找**那些“不等于 `val`​”的​**有效元素**。它不管我们要删除什么，它只关心我们要保留什么。
  - **慢指针 (**​**​`left`​**​ **) —— “地基 / 接收者”**

    - ​**职责**：它永远指向“新数组”当前等待填入的位置。
    - ​**作用**​：它维护的是**结果集**的边界。`0`​ 到 `left-1` 都是处理好的、干净的数据。

  总结作用：

  双指针的作用不是为了“减少搜索空间”（因为 right 还是把数组从头到尾扫了一遍，搜索空间依然是 \$N\$），而是将“多次大规模移动”优化为了“一次精准赋值”。

  （me：原来的暴力枚举法实际是通过大规模的数据迁移赋值，达到更新数组的目的，这种方法过于低效，而双指针则是精确的进行复制，相当于用O（1）的操作操作了O（n）的数据）

  ### 2. 为什么说它不是减少搜索空间，而是减少“数据搬运”？

  让我们对比一下操作成本：

  #### 场景：数组 `[1, 2, 2, 2, 3]`​，我们要删掉 `2`。

  **暴力解法（痛苦的搬运）：**

  1. 找到第一个 `2`​。把后面的 `2, 2, 3` 全部往前挪一步。
  2. 找到第二个 `2`​（原第三个）。把后面的 `2, 3` 全部往前挪一步。
  3. 找到第三个 `2`​。把后面的 `3` 往前挪一步。
  4. **核心痛点：**  后面的元素（比如 `3`）被无辜地搬运了多次。

  **快慢指针法（精准的覆盖）：**

  1. ​`right`​ 找到 `1`​（好元素） -\> 赋值给 `nums[left]`​（其实没变），`left` 进 1。
  2. ​`right`​ 遇到 `2`​ -\> 无视，继续走。
  3. ​`right`​ 遇到 `2`​ -\> 无视，继续走。
  4. ​`right`​ 遇到 `2`​ -\> 无视，继续走。
  5. ​`right`​ 找到 `3`​（好元素） -\> 直接把 `3`​ 赋值给 `nums[left]`​（覆盖掉了原来的第一个 `2`）。
  6. **核心优势：**  每个保留下来的元素，​**最多只被搬运一次**。

  ### 3. 关于你提到的“对撞指针”

  你在注释里提到一开始习惯用“对撞指针”（左右指针向中间靠拢）。

  其实这道题​**完全可以用对撞指针**！而且在某些情况下，对撞指针甚至更快（因为它原本在末尾的元素不需要移动那么远的距离，直接填到前面就行）。

  - **快慢指针（你现在的写法）：**  优点是​**保留了元素的相对顺序**​（比如 `[1, 3, 5]`​ 删掉 `3`​ 还是 `1, 5`）。
  - **对撞指针（左边找** **​`val`​**​ **，右边找非** **​`val`​**​ **填坑）：**  优点是交换次数可能更少，但​**会打乱顺序**。因为题目说“元素的顺序可以改变”，所以对撞指针也是标准解法之一。

  ### 一句话总结

  你的双指针法，核心在于​**用** **​`O(N)`​** ​ **的读取（快指针）和** **​`O(N)`​** ​ **的写入（慢指针），替代了暴力法中** **​`O(N^2)`​** ​ **的数据搬移**。

### 对撞指针法：

```java
class Solution {
    public int removeElement(int[] nums, int val) {//对撞指针
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {//左闭右闭区间
            if (nums[left] != val) {//左边开始寻找，遇到等于的停下
                left++;
            } else if (nums[right] == val) {//如果此时的right等于，则丢掉，向左继续走
                right--;
            } else {
                nums[left] = nums[right];//如果此时右指针指向的是好的值，则赋值给左指针，右继续向前寻找，left不动，等待下一轮检查
                right--;
            }
        }
        return left;
    }
}
```

## 解题思路：

- 左指针负责指向需要检查的数，并负责存储新数组中的新元素。右指针负责供给，提供备用的数字。

有点像甲乙两队同时施工，左指针和右指针同时检查，并丢弃相对应的垃圾值，同时右指针以左指针为主，在遇到左指针丢弃垃圾值的时候，右指针提供对应值，对应于elseif 和else。

- 相较于快慢指针法，对撞指针并不保证最后的新数组是有序的，仅适用于题目未要求有序的情况

- ### 2. 快慢指针 vs 对撞指针：怎么选？

  这是面试中的高频考点，决定你使用哪种指针的核心在于：**元素的相对顺序是否重要？**

  #### 比较表格

  |**特性**|**快慢指针 (Fast-Slow)**|**对撞指针 (Left-Right)**||||
  | --| ----------------------------| ------------------------------| --| --| --|
  |**核心动作**|**覆盖 (Overwrite)**|**交换/填充 (Swap/Fill)**||||
  |**元素顺序**|**保序**(相对位置不变)|**不保序**(位置会被打乱)||||
  |**写操作次数**|较多 (好元素都要移动一遍)|**最少**(只移动必要的元素)||||
  |**典型场景**|这里的`removeElement`、移动零、链表找环|两数之和、反转数组、二分查找||||

  #### 什么时候用谁？

  1. **必须要用“快慢指针”的情况：**

     - 题目要求 ​ **“保持原有元素的相对顺序”** 。
     - 例如：[移动零](https://leetcode.cn/problems/move-zeroes/)。题目明确要求不能改变非零元素的顺序。如果用对撞指针，原本在最后的非零元素会被填到前面，顺序就乱了。
  2. **可以用（甚至推荐）“对撞指针”的情况：**

     - 题目说 ​ **“元素的顺序可以改变”** 。
     - 此时对撞指针往往​**效率更高**。
     - **为什么更优？**  举个极端的例子：

       - 数组：`[1, 2, 2, 2, ..., 2, 5]`​，目标是删除 `2`。
       - ​**快慢指针**​：会把最后的 `5` 一步步往前挪，直到挪到索引 1 的位置，写入次数非常多。
       - ​**对撞指针**​：`left`​ 停在第一个 `2`​，`right`​ 停在 `5`​。直接把 `5`​ 填到第一个 `2` 的位置，一步搞定！

  ---

  ### 3. 双指针的本质是什么？

  无论是快慢还是对撞，双指针的本质都是：**通过两个变量定义“不变量区间”，将多余的计算（如暴力法的O(N²)）转化为一次遍历。**

  它们的本质就是​**数组分区 (Partitioning)** 。

  - **对于快慢指针：**

    - 它把数组分成了三个区域：
    - ​`[0, slow)`​：​**处理好的、保留的数据**。
    - ​`[slow, fast)`​：​**垃圾区/被逻辑删除的数据**。
    - ​`[fast, n)`​：​**待检查的数据**。
  - **对于对撞指针：**

    - 它把数组分成了三个区域：
    - ​`[0, left)`​：​**左边处理好的数据**。
    - ​`(right, n-1]`​：​**右边处理好的（或者被废弃的）数据**。
    - ​`[left, right]`​：​**待检查的未知区域**。

  **总结：**

  - 想要​**稳**（保序），用快慢。
  - 想要​**快**（减少写入），用对撞。
  - 这道题 `removeElement`​ 允许乱序，所以你的​**对撞指针解法在理论上是写入操作更优的解法**。

‍

‍

‍

# 977.有序数组的平方[https://leetcode.cn/problems/squares-of-a-sorted-array/](https://leetcode.cn/problems/squares-of-a-sorted-array/)

```java
class Solution {//对撞指针，因为最大值在两端，快慢指针是一个递进的顺序操作
//有序数组的变换/查找（如平方、两数之和） -> 利用有序性，使用 对撞指针 缩小范围。
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];//定义等长的一个数组
        int i = 0;//左指针
        int j = n - 1;//右指针
        for (int p = n - 1; p >= 0; p--) {//从右到左填入，因为平方和最大的在右边
            int x = nums[i] * nums[i];
            int y = nums[j] * nums[j];
            if (x > y) {
                ans[p] = x;
                i++;
            } else {
                ans[p] = y;
                j--;
            }
        }
        return ans;
    }
}
```

# 解题思路:

- 这题跟两数之和类似，都是通过对撞指针对有序数组。此题较简单。

# 优化:

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int i = 0;
        int j = n - 1;
        for (int p = n - 1; p >= 0; p--) {
            int x = nums[i];
            int y = nums[j];
            if (-x > y) {
                ans[p] = x * x;
                i++;
            } else {
                ans[p] = y * y;
                j--;
            }
        }
        return ans;
    }
}

作者：灵茶山艾府
链接：https://leetcode.cn/problems/squares-of-a-sorted-array/solutions/2806253/xiang-xiang-shuang-zhi-zhen-cong-da-dao-blda6/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 对于方法一，我们是先进行了乘法的运算之后再进行了比价，这种比较中的乘法运算开销过大，在大规模的数据运算下，乘法的运算开销要比加减取反等操作的运算开销要大，同时也有可能造成整数的溢出。那么我们是不是可以先进行比较再去进行乘法的运算呢？
- - **如果是“一正一负”的情况（这是主要冲突点）：**

    - ​`nums[i]`​ (左指针 `x`) 是负数。
    - ​`nums[j]`​ (右指针 `y`) 是正数。
    - 我们要比的是绝对值：`|x|`​ vs `y`。
    - 因为 `x`​ 是负数，`|x|`​ 就是 `-x`。
    - 所以直接比 `-x > y` 即可。
  - **如果是“全是负数”的情况：**

    - ​`x`​ 是 `-10`​，`y`​ 是 `-3`。
    - ​`-x`​ 变成 `10`。
    - ​`10 > -3` 成立。
    - 取 `x`​（即 `-10`），它的平方确实更大。逻辑没问题。
  - **如果是“全是正数”的情况：**

    - ​`x`​ 是 `3`​，`y`​ 是 `10`。
    - ​`-x`​ 变成 `-3`。
    - ​`-3 > 10` 不成立。
    - 走 `else`​ 分支，取 `y`​（即 `10`）。逻辑也没问题。

通过数学逻辑，优化了比较
