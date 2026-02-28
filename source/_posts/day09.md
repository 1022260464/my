---
title: day09
date: '2026-01-09 18:25:29'
updated: '2026-01-29 17:38:21'
permalink: /post/day09-z2x54a5.html
comments: true
toc: true
---



# day09

# **第四章 字符串part02**

**KMP算法暂且放过，后续再学**

今日任务

- [ ] ● 151.翻转字符串里的单词
- [ ] ● 卡码网：55.右旋转字符串
- [ ] ● 28. 实现 strStr()
- [ ] ● 459.重复的子字符串
- [ ] ● 字符串总结
- [ ] ● 双指针回顾

##  详细布置

### 151.翻转字符串里的单词

建议：这道题目基本把 刚刚做过的字符串操作 都覆盖了，不过就算知道解题思路，本题代码并不容易写，要多练一练。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)</u>

### 卡码网：55.右旋转字符串

建议：题解中的解法如果没接触过的话，应该会想不到

题目链接/文章讲解：

[右旋转](https://programmercarl.com/kamacoder/0055.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

## **28. 实现 strStr()  （本题可以跳过）**

因为KMP算法很难，大家别奢求 一次就把kmp全理解了，大家刚学KMP一定会有各种各样的疑问，先留着，别期望立刻啃明白，第一遍了解大概思路，二刷的时候，再看KMP会 好懂很多。

或者说大家可以放弃一刷可以不看KMP，今天来回顾一下之前的算法题目就可以。

因为大家 算法能力还没到，细扣 很难的算法，会把自己绕进去，就算别人给解释，只会激发出更多的问题和疑惑。所以大家先了解大体过程，知道这么回事， 等自己有 算法基础和思维了，在看多看几遍视频，慢慢就理解了。

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)</u>​[ ]()

##  459.重复的子字符串  （本题可以跳过）

本题算是KMP算法的一个应用，不过 对KMP了解不够熟练的话，理解本题就难很多。

我的建议是 **KMP和本题，一刷的时候 ，可以适当放过，了解怎么回事就行，二刷的时候再来硬啃**

题目链接/文章讲解/视频讲解：<u>[https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html)</u>

### **字符串总结 **

比较简单，大家读一遍就行

题目链接/文章讲解：<u>[https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html](https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html)</u>

###  双指针回顾

此时我们已经做过10道双指针的题目了，来一起回顾一下，大家自己也总结一下双指针的心得

[文章讲解：]()​<u>[https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html](https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html)</u>

# 卡码网：55.右旋转字符串

```java
// 版本一
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String s = in.nextLine();

        int len = s.length();  //获取字符串长度
        char[] chars = s.toCharArray();
        reverseString(chars, 0, len - 1);  //反转整个字符串
        reverseString(chars, 0, n - 1);  //反转前一段字符串，此时的字符串首尾尾是0,n - 1
        reverseString(chars, n, len - 1);  //反转后一段字符串，此时的字符串首尾尾是n,len - 1
        
        System.out.println(chars);

    }

    public static void reverseString(char[] ch, int start, int end) {
        //异或法反转字符串，参照题目 344.反转字符串的解释
        while (start < end) {
            ch[start] ^= ch[end];
            ch[end] ^= ch[start];
            ch[start] ^= ch[end];
            start++;
            end--;
        }
    }
}
```

- 先整体翻转，再局部翻转

- 异或运算（XOR，符号 `^`​）实现交换变量是一种很“极客”的写法，它的核心原理利用了异或运算的​**数学性质**。

  我们可以把它拆解为三个步骤来看，本质上它是一场\*\*“数据的消消乐”\*\*。

  ### 1. 核心口诀：两个相同的数异或，结果为 0

  这就像正负抵消一样，是理解这个算法的唯一钥匙：

  1. ​**归零律**​：`N ^ N = 0` （任何数和自己异或，都变成0）
  2. ​**恒等律**​：`N ^ 0 = N` （任何数和0异或，保持不变）
  3. ​**结合律**​：`a ^ b ^ c`​ \= `a ^ c ^ b` （顺序无所谓）

  ---

  ### 2. 代码逐行解析（代数推导）

  假设此时 `ch[start]`​ 的值是 ​**A**​，`ch[end]`​ 的值是 ​**B**。

  #### 第一行：混合

  Java

  ```
  ch[start] ^= ch[end];
  ```

  - 此时 `ch[start]`​ 变成了 `A ^ B`。
  - ​`ch[end]`​ 还是 `B`。
  - ​**理解**​：现在的 `ch[start]` 保存了 A 和 B 的“混合特征”（或者叫差异值）。

  #### 第二行：取出 A 给 B

  Java

  ```
  ch[end] ^= ch[start];
  ```

  - 代入刚才的值：`ch[end] = B ^ (A ^ B)`
  - 根据结合律：`ch[end] = (B ^ B) ^ A`
  - 根据归零律：`ch[end] = 0 ^ A`
  - 根据恒等律：`ch[end] = A`
  - ​**结果**​：`ch[end]`​ 成功变成了 ​**A**！

  #### 第三行：取出 B 给 A

  Java

  ```
  ch[start] ^= ch[end];
  ```

  - 此时 `ch[start]`​ 还是混合体 `(A ^ B)`​，但 `ch[end]`​ 已经是 `A` 了。
  - 代入计算：`ch[start] = (A ^ B) ^ A`
  - 根据结合律：`ch[start] = (A ^ A) ^ B`
  - 根据归零律：`ch[start] = 0 ^ B`
  - ​**结果**​：`ch[start]`​ 成功变成了 ​**B**！

  ---

  ### 3. 二进制实战演练（图解）

  我们用具体的数字来演示。

  假设 a \= 5 (二进制 0101), b \= 3 (二进制 0011)。

  |**步骤**|**代码**|**运算过程 (二进制)**|**结果解释**|||||
  | --| ------| --------------------| -----------------------| --| --| --| --|
  |**初始**||​`a = 0101`​,`b = 0011`||||||
  |**1**|​`a ^= b`|​`0101`​\^`0011`​\=**​`0110`​**|​`a`变成了 6 (混合值)|||||
  |**2**|​`b ^= a`|​`0011`​\^**​`0110`​**​\=**​`0101`​**|​`b`​变成了 5 (​**原a的值**)|||||
  |**3**|​`a ^= b`|​**​`0110`​**​\^**​`0101`​**​\=**​`0011`​**|​`a`​变成了 3 (​**原b的值**)|||||

  **交换完成！**  `a`​ 变成了 3，`b` 变成了 5。

  ### 4. 致命陷阱（为什么必须 start \< end）

  你在代码中看到 `while (start < end)`​ 而不是 `start <= end`，这非常关键。

  如果 `start`​ 和 `end`​ 指向数组的​**同一个位置**（也就是引用了同一块内存），会发生可怕的事情：

  1. 执行第一句 `ch[start] ^= ch[end]`：

     - 这就等于 `x ^= x`。
     - ​**结果**​：该位置的值瞬间变成了 ​**0**。
  2. 后面的代码都在 `0 ^ 0` 之间打转。

  结论：原来的数据直接丢失了，变成了一团空字符。

  所以，异或交换法不适用于“自我交换”，必须保证两个变量是内存中不同的两个地址。普通的 tmp 临时变量交换法就没有这个问题。

  ‍

- # [分享｜从集合论到位运算，常见位运算技巧分类总结！ - 讨论 - 力扣（LeetCode）.pdf](assets/分享｜从集合论到位运算，常见位运算技巧分类总结！%20-%20讨论%20-%20力扣（LeetCode）-20260109185051-7dgx1ux.pdf)

- 结合刚才涉及的两道算法题（LeetCode 541 和 右旋字符串），这里为你整理一份\*\*“刷字符串算法题必备的 Java 语法补给包”\*\*。

  在 Java 中做字符串题目，最痛苦的点在于 ​**Java 的 String 是不可变的（Immutable）** ​。这意味着你不能像 C++ 或是 Python 那样直接修改字符串里的某个字符（比如 `s[i] = 'a'` 在 Java 里是非法的）。

  以下是破解这些障碍的关键语法：

  ### 1. 核心转换：String 与 char[]

  这是处理“原地修改”类题目的标准起手式。

  - **String 转 数组（为了修改）：**
  - **数组 转 String（为了返回结果）：**

  ### 2. 输入输出陷阱：Scanner 的坑

  在第二段代码中，你看到了这样的写法：

  Java

  ```
  int n = Integer.parseInt(in.nextLine());
  String s = in.nextLine();
  ```

  **为什么要这样写？**  为什么不直接用 `in.nextInt()`？

  - ​**陷阱场景**：
  - ​**后果**​：`s` 会读到一个空字符串。
  - ​**原因**​：`nextInt()`​ 只读取了数字 `3`​，但​**没有读取你按下的“回车符” (**​ **​`\n`​**​ **)** ​。这个回车符留在了缓冲区里。紧接着 `nextLine()` 一执行，它看到缓冲区里有个回车，以为这行结束了，直接收工。
  - 最佳实践（防坑）：  
    永远使用 nextLine() 读取整行，然后自己转换类型。

  ### 3. 边界神器：Math.min

  在处理“分块”或“分页”逻辑时（如 LeetCode 541），这个函数能省去繁琐的 `if-else`。

  - ​**场景**​：你要截取从 `i`​ 开始的 `k`​ 个字符，但不知道剩余长度够不够 `k` 个。
  - ​**传统写法**：
  - ​**优雅写法**：

  ### 4. 异或运算 (`^`) 的性质速查

  除了刚才的交换变量，异或在算法题中还有很多妙用。

  - ​**性质表**：

    1. ​`0 ^ 0 = 0`
    2. ​`1 ^ 0 = 1`
    3. ​`0 ^ 1 = 1`
    4. ​`1 ^ 1 = 0` (同性相斥为0，异性相吸为1)
  - ​**核心定律**：

    - ​`x ^ 0 = x`
    - ​`x ^ x = 0`
  - ​**常见算法题应用**：

    - ​**只出现一次的数字**：数组中除了一个数字出现一次，其他都出现两次。把所有数字异或起来，结果就是那个唯一的数字。（因为相同的数字两两抵消变成了0）。

  ### 5. 高级替补：StringBuilder

  如果题目不仅仅是“修改”字符，而是涉及大量的​**插入**​、​**删除**​、**拼接**操作，用 `char[]`​ 会很麻烦（因为数组长度固定）。这时候要请出 `StringBuilder`。

  - ​**常用 API**：
  - ​**注意**​：`StringBuilder`​ 是可变的，效率比用 String `+` 拼接高得多。

  ### 总结

  1. ​**改内容**​：转 `char[]`。
  2. ​**拼内容**​：用 `StringBuilder`。
  3. ​**读输入**​：尽量全用 `nextLine()`。
  4. **算边界**：善用 `Math.min`。

# 151.翻转字符串里的单词

```java
class Solution {
   /**
     * 不使用Java内置方法实现
     * <p>
     * 1.去除首尾以及中间多余空格
     * 2.反转整个字符串
     * 3.反转各个单词
     */
    public String reverseWords(String s) {
        // System.out.println("ReverseWords.reverseWords2() called with: s = [" + s + "]");
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        // System.out.println("ReverseWords.removeSpace() called with: s = [" + s + "]");
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        // System.out.println("ReverseWords.removeSpace returned: sb = [" + sb + "]");
        return sb;
    }

    /**
     * 反转字符串指定区间[start, end]的字符
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        // System.out.println("ReverseWords.reverseString() called with: sb = [" + sb + "], start = [" + start + "], end = [" + end + "]");
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        // System.out.println("ReverseWords.reverseString returned: sb = [" + sb + "]");
    }

    private void reverseEachWord(StringBuilder sb) {
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseString(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
//解法二：创建新字符数组填充。时间复杂度O(n)
class Solution {
    public String reverseWords(String s) {
        //源字符数组
        char[] initialArr = s.toCharArray();
        //新字符数组
        char[] newArr = new char[initialArr.length+1];//下面循环添加"单词 "，最终末尾的空格不会返回
        int newArrPos = 0;
        //i来进行整体对源字符数组从后往前遍历
        int i = initialArr.length-1;
        while(i>=0){
            while(i>=0 && initialArr[i] == ' '){i--;}  //跳过空格
            //此时i位置是边界或!=空格，先记录当前索引，之后的while用来确定单词的首字母的位置
            int right = i;
            while(i>=0 && initialArr[i] != ' '){i--;} 
            //指定区间单词取出(由于i为首字母的前一位，所以这里+1,)，取出的每组末尾都带有一个空格
            for (int j = i+1; j <= right; j++) {
                newArr[newArrPos++] = initialArr[j];
                if(j == right){
                    newArr[newArrPos++] = ' ';//空格
                }
            }
        }
        //若是原始字符串没有单词，直接返回空字符串；若是有单词，返回0-末尾空格索引前范围的字符数组(转成String返回)
        if(newArrPos == 0){
            return "";
        }else{
            return new String(newArr,0,newArrPos-1);
        }
    }
}
//解法三：双反转+移位，String 的 toCharArray() 方法底层会 new 一个和原字符串相同大小的 char 数组，空间复杂度：O(n)
class Solution {
    /**
     * 思路：
     *	①反转字符串  "the sky is blue " => " eulb si yks eht"
     *	②遍历 " eulb si yks eht"，每次先对某个单词进行反转再移位
     *	   这里以第一个单词进行为演示：" eulb si yks eht" ==反转=> " blue si yks eht" ==移位=> "blue si yks eht"
     */
    public String reverseWords(String s) {
        //步骤1：字符串整体反转（此时其中的单词也都反转了）
        char[] initialArr = s.toCharArray();
        reverse(initialArr, 0, s.length() - 1);
        int k = 0;
        for (int i = 0; i < initialArr.length; i++) {
            if (initialArr[i] == ' ') {
                continue;
            }
            int tempCur = i;
            while (i < initialArr.length && initialArr[i] != ' ') {
                i++;
            }
            for (int j = tempCur; j < i; j++) {
                if (j == tempCur) { //步骤二：二次反转
                    reverse(initialArr, tempCur, i - 1);//对指定范围字符串进行反转，不反转从后往前遍历一个个填充有问题
                }
                //步骤三：移动操作
                initialArr[k++] = initialArr[j];
                if (j == i - 1) { //遍历结束
                    //避免越界情况，例如=> "asdasd df f"，不加判断最后就会数组越界
                    if (k < initialArr.length) {
                        initialArr[k++] = ' ';
                    }
                }
            }
        }
        if (k == 0) {
            return "";
        } else {
            //参数三：以防出现如"asdasd df f"=>"f df asdasd"正好凑满不需要省略空格情况
            return new String(initialArr, 0, (k == initialArr.length) && (initialArr[k - 1] != ' ') ? k : k - 1);
        }
    }

    public void reverse(char[] chars, int begin, int end) {
        for (int i = begin, j = end; i < j; i++, j--) {
            chars[i] ^= chars[j];
            chars[j] ^= chars[i];
            chars[i] ^= chars[j];
        }
    }
}
/*
 * 解法四：时间复杂度 O(n)
 * 参考卡哥 c++ 代码的三步骤：先移除多余空格，再将整个字符串反转，最后把单词逐个反转
 * 有别于解法一 ：没有用 StringBuilder  实现，而是对 String 的 char[] 数组操作来实现以上三个步骤
 */
class Solution {
    //用 char[] 来实现 String 的 removeExtraSpaces，reverse 操作
    public String reverseWords(String s) {
        char[] chars = s.toCharArray();
        //1.去除首尾以及中间多余空格
        chars = removeExtraSpaces(chars);
        //2.整个字符串反转
        reverse(chars, 0, chars.length - 1);
        //3.单词反转
        reverseEachWord(chars);
        return new String(chars);
    }

    //1.用 快慢指针 去除首尾以及中间多余空格，可参考数组元素移除的题解
    public char[] removeExtraSpaces(char[] chars) {
        int slow = 0;
        for (int fast = 0; fast < chars.length; fast++) {
            //先用 fast 移除所有空格
            if (chars[fast] != ' ') {
                //在用 slow 加空格。 除第一个单词外，单词末尾要加空格
                if (slow != 0)
                    chars[slow++] = ' ';
                //fast 遇到空格或遍历到字符串末尾，就证明遍历完一个单词了
                while (fast < chars.length && chars[fast] != ' ')
                    chars[slow++] = chars[fast++];
            }
        }
        //相当于 c++ 里的 resize()
        char[] newChars = new char[slow];
        System.arraycopy(chars, 0, newChars, 0, slow); 
        return newChars;
    }

    //双指针实现指定范围内字符串反转，可参考字符串反转题解
    public void reverse(char[] chars, int left, int right) {
        if (right >= chars.length) {
            System.out.println("set a wrong right");
            return;
        }
        while (left < right) {
            chars[left] ^= chars[right];
            chars[right] ^= chars[left];
            chars[left] ^= chars[right];
            left++;
            right--;
        }
    }

    //3.单词反转
    public void reverseEachWord(char[] chars) {
        int start = 0;
        //end <= s.length() 这里的 = ，是为了让 end 永远指向单词末尾后一个位置，这样 reverse 的实参更好设置
        for (int end = 0; end <= chars.length; end++) {
            // end 每次到单词末尾后的空格或串尾,开始反转单词
            if (end == chars.length || chars[end] == ' ') {
                reverse(chars, start, end - 1);
                start = end + 1;
            }
        }
    }
}
```

# 用java库的解法：

```java
class Solution {
    public String reverseWords(String s) {
       s = s.trim(); // 删除首尾空格
       int j = s.length() - 1, i = j;
       StringBuilder res = new StringBuilder();

       while (i >= 0) {
           // 1. 搜索首个空格（确定单词左边界）
           while (i >= 0 && s.charAt(i) != ' ') i--; 
           
           // 2. 将单词提取并添加到 res
           res.append(s.substring(i + 1, j + 1)).append(" "); 
           
           // 3. 跳过单词之间的多余空格（关键一步！）
           while (i >= 0 && s.charAt(i) == ' ') i--;
           
           // 4. 让 j 指向下一个单词的末尾
           j = i;
       }    
       return res.toString().trim();   
    }
}
```
