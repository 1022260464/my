---
title: day01
date: '2025-12-31 14:40:04'
updated: '2025-12-31 16:20:01'
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
                  
              }
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

2.如果更新指针时，将mid值包含进去会怎么样？

### 左闭右开区间

- 两个双指针，一个指向左边第一个有效数字，right指向有效数字后一位（类似于栈指针）。确定了搜索区域，再进行循环条件判断，我的两个双指针并不会相遇，因为右指针始终指向最后一位有效数字的后一位。在条件判断时，移动右指针，只需等于mid即可，此时已经不包含mid。移动左指针同理。

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

```java
class Solution {
    public int removeElement(int[] nums, int val) {//快慢指针法，我在一开始习惯性的用了对撞指针
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

‍

```java
class Solution {
    public int removeElement(int[] nums, int val) {//对撞指针
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            if (nums[left] != val) {
                left++;
            } else if (nums[right] == val) {
                right--;
            } else {
                nums[left] = nums[right];
                right--;
            }
        }
        return left;
    }
}
```

# 977.有序数组的平方[https://leetcode.cn/problems/squares-of-a-sorted-array/](https://leetcode.cn/problems/squares-of-a-sorted-array/)

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int i = 0;
        int j = n - 1;
        for (int p = n - 1; p >= 0; p--) {
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

‍
