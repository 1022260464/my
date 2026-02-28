---
title: 07-FILE、IO流
date: '2026-02-09 17:35:38'
updated: '2026-02-09 20:22:27'
permalink: /post/07file-ioliu-20tvin.html
comments: true
toc: true
---



# 07-FILE、IO流

# Java File 类详细知识点笔记

## 1. 概述

​`java.io.File`​ 类是文件和目录路径名的**抽象表示**。

- 它主要用于文件/文件夹的创建、删除、重命名、判断是否存在、获取属性等。
- **重要**：`File`​ 对象只能操作“文件本身”（属性、路径、元数据），**不能操作“文件内容”** （读写内容需要使用 IO 流）。

---

## 2. 核心重点：目录遍历 (`listFiles` 返回值规则)

这是 `File`​ 类中最复杂的细节，`public File[] listFiles()` 方法的返回情况如下：

|场景 (当主调对象是...)|返回值结果|备注|
| ------------------------| ------------| ---------------------------------------------|
|**文件**|​`null`|主调必须是文件夹，如果是文件则报错/返回null|
|**路径不存在**|​`null`|路径无效时返回 null|
|**空文件夹**|​`File[0]`|返回一个长度为 0 的空数组 (非 null)|
|**有内容的文件夹**|​`File[]`|包含文件夹内所有**一级**文件和文件夹的路径对象|
|**含隐藏文件的文件夹**|​`File[]`|包含所有文件，**包括隐藏文件**|
|**无访问权限的文件夹**|​`null`|即使存在，若无读权限也无法获取列表|

> **注意**：在使用 `listFiles()`​ 返回的数组前，**必须先判断是否为 `null**`​，否则极易抛出 `NullPointerException`。

```java
package com.itheima.demo1file;

import java.io.File;
import java.util.Arrays;

public class FileDemo2 {
    public static void main(String[] args) {
        // 目标：掌握File遍历一级文件对象的操作
        File dir = new File("E:/resource/dlei.txt");
        File dir2 = new File("E:\\resource\\eee777");

        /**
         * 当主调是文件，或者路径不存在时，返回null
         * 当主调是空文件夹时，返回一个长度为0的数组
         * 当主调是一个有内容的文件夹时，将里面所有一级文件和文件夹的路径放在File数组中返回
         * 当主调是一个文件夹，且里面有隐藏文件时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏文件
         * 当主调是一个文件夹，但是没有权限访问该文件夹时，返回null
         */
        File[] files = dir2.listFiles();
        System.out.println(Arrays.toString(files));
    }
}

```

---

## 3. 常用构造方法

​`File`​ 对象仅仅是一个路径的记录，创建对象时**不检查文件是否存在**。

1. ​`File(String pathname)`: 通过路径字符串创建。
2. ​`File(String parent, String child)`: 父路径字符串 + 子路径字符串。
3. ​`File(File parent, String child)`: 父 File 对象 + 子路径字符串。

---

## 4. 常用功能方法

### A. 创建功能

- ​`public boolean createNewFile()`: 当且仅当文件不存在时，创建一个新的空文件。
- ​`public boolean mkdir()`​: 创建目录。**仅能创建单级目录** (若父级不存在则失败)。
- ​`public boolean mkdirs()`​: 创建目录。**可以创建多级目录** (若父级不存在，会一并创建，推荐使用)。

### B. 删除功能

- ​`public boolean delete()`​: 删除文件或**空目录**。
- **注意**：Java 的删除不走回收站，直接从硬盘抹除。如果要删除非空目录，必须先递归删除其内容。

### C. 判断功能

- ​`public boolean exists()`: 判断文件或目录是否存在。
- ​`public boolean isDirectory()`: 判断是否为目录。
- ​`public boolean isFile()`: 判断是否为文件。
- ​`public boolean isHidden()`: 判断是否为隐藏文件。

### D. 获取功能

- ​`public String getAbsolutePath()`: 获取绝对路径。
- ​`public String getPath()`: 获取构造时的路径 (可能是相对，可能是绝对)。
- ​`public String getName()`: 获取文件或目录的名称。
- ​`public long length()`: 获取文件大小（字节数）。
- **注意**：若是文件夹，返回值是不确定的（取决于操作系统），不能用来统计文件夹大小。
- ​`public long lastModified()`: 获取最后修改时间 (毫秒值)。

---

## 5. 路径分隔符 (跨平台)

Windows 和 Linux/Mac 的路径分隔符不同，为了代码跨平台，应使用 `File` 类提供的常量：

- ​`File.separator`​: 路径分隔符 (Windows 是 `\`​, Linux/Mac 是 `/`)。
- ​`File.pathSeparator`​: 路径列表分隔符 (Windows 是 `;`​, Linux/Mac 是 `:`)。

**示例写法**：

```java
File file = new File("d:" + File.separator + "develop" + File.separator + "a.txt");

```

---

## 6. 绝对路径 vs 相对路径

- **绝对路径**：以盘符 (Windows) 或 `/` (Linux) 开始的完整路径。
- **相对路径**：不以盘符开始。
- 在 Eclipse/IDEA 项目中，相对路径通常是相对于**项目根目录** (Project Root)。

---

### 代码示例

```java
package com.itheima.demo1file;

import java.io.File;

public class FileDemo1 {
    public static void main(String[] args) throws Exception {
        // 目标：创建File创建对象代表文件（文件/目录），搞清楚其提供的对文件进行操作的方法。
        // 1、创建File对象，去获取某个文件的信息
//        File f1 = new File("E:\\resource\\dlei.jpg");
        File f1 = new File("E:/resource/dlei.jpg");

        System.out.println(f1.length()); // 字节个数
        System.out.println(f1.getName());
        System.out.println(f1.isFile()); // true
        System.out.println(f1.isDirectory()); // false

        // 2、可以使用相对路径定位文件对象
        // 只要带盘符的都称之为：绝对路径 E:/resource/dlei.jpg
        // 相对路径：不带盘符，默认是到你的idea工程下直接寻找文件的。一般用来找工程下的项目文件的。
        File f2 = new File("day03-file-io\\src\\dlei01.txt");
        System.out.println(f2.length());
        System.out.println(f2.getAbsoluteFile()); // 获取绝对路径

        // 3、创建对象代表不存在的文件路径。
        File f3 = new File("E:\\resource\\dlei01.txt");
        System.out.println(f3.exists()); // 判断是否存在
        System.out.println(f3.createNewFile()); // 把这个文件创建出来

        // 4、创建对象代表不存在的文件夹路径。
        File f4 = new File("E:\\resource\\aaa");
        System.out.println(f4.mkdir()); // mkdir只能创建一级文件夹

        File f5 = new File("E:\\resource\\kkk\\jjj\\mmm");
        System.out.println(f5.mkdirs()); // mkdir可以创建多级文件夹，很重要！

        // 5、创建File对象代表存在的文件，然后删除它
        File f6 = new File("E:\\resource\\dlei01.txt");
        System.out.println(f6.delete()); // 删除文件

        // 6、创建File对象代表存在的文件夹，然后删除它
        File f7 = new File("E:\\resource\\aaa");
        System.out.println(f7.delete());  // 只能删除空文件，不能删除非空文件夹

        File f8 = new File("E:\\resource");
        System.out.println(f8.delete());  // 只能删除空文件，不能删除非空文件夹

        // 8、可以获取某个目录下的全部一级文件名称
        File f9 = new File("E:\\磊哥面授\\AI+Java基础入门课程\\day08-面向对象高级、Lambda、GUI编程\\视频");
        String[] names = f9.list();
        for (String name : names) {
            System.out.println(name);
        }

        File[] files = f9.listFiles();
        for (File file : files) {
            System.out.println(file.getAbsoluteFile()); // 获取绝对路径
        }
    }
}

```

# 递归搜索：

```java
package com.itheima.demo2recursion;

import java.io.File;
import java.io.IOException;

public class FileSearchOptimized {
    public static void main(String[] args) {
        File dir = new File("D:/");
        // 调用搜索，找到目标后通过返回值判断是否停止
        searchFile(dir, "QQ.exe");
    }

    /**
     * @return boolean 如果找到了目标文件，返回 true，通知上一层递归停止
     */
    public static boolean searchFile(File dir, String fileName) {
        // 1. 基础校验 (空或者是文件则不处理)
        if (dir == null || !dir.exists() || dir.isFile()) {
            return false;
        }

        // 2. 获取子文件
        File[] files = dir.listFiles();

        // 3. 关键判空：防止无权限目录导致空指针异常
        if (files != null && files.length > 0) {
            for (File file : files) {
                if (file.isFile()) {
                    // 4. 使用 equalsIgnoreCase 精确匹配
                    if (file.getName().equalsIgnoreCase(fileName)) {
                        System.out.println("找到目标文件：" + file.getAbsolutePath());
                        try {
                            // 启动程序
                            Runtime.getRuntime().exec(file.getAbsolutePath());
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                        return true; // 【关键】找到后，返回 true
                    }
                } else {
                    // 5. 递归调用，并接收返回值
                    // 如果子文件夹里找到了，这里也返回 true，向上层传递“已找到”的信号
                    boolean hasFound = searchFile(file, fileName);
                    if (hasFound) {
                        return true; // 剪枝：一旦找到，不再遍历剩余文件，直接层层返回
                    }
                }
            }
        }
        return false; // 当前目录下没找到
    }
}
```

- - **防御** **​`null`​**​：当扫描到系统受保护的隐藏文件夹（如 `D:/System Volume Information`​）时，没有权限访问，`listFiles()`​ 会返回 **​`null`​**​。如果没有 `files != null`​ 的判断，程序运行到这里会直接报 `NullPointerException` 崩溃。（会爆空指针异常 的条件可以先放在前面）
  - **防御空数组**：虽然 `files.length > 0` 不是必须的（因为增强 for 循环可以处理空数组），但写上也无妨，逻辑更显性。
- ### 这里贴上FILe的返回值规则：

  |场景 (当主调对象是...)|返回值结果|备注|
  | ------------------------| ------------| ---------------------------------------------|
  |**文件**|​`null`|主调必须是文件夹，如果是文件则报错/返回null|
  |**路径不存在**|​`null`|路径无效时返回 null|
  |**空文件夹**|​`File[0]`|返回一个长度为 0 的空数组 (非 null)|
  |**有内容的文件夹**|​`File[]`|包含文件夹内所有**一级**文件和文件夹的路径对象|
  |**含隐藏文件的文件夹**|​`File[]`|包含所有文件，**包括隐藏文件**|
  |**无访问权限的文件夹**|​`null`|即使存在，若无读权限也无法获取列表|

  ‍

---

# 常见字符集与编解码操作笔记

## 1. 常见字符集 (Character Sets)

字符集（Charset）不仅决定了字符如何存储（二进制），还决定了占用多少字节。

### A. ASCII 字符集

- ​**地位**：最基础的字符集，美国标准。
- ​**包含**：英文大小写、数字、标点符号。
- ​**特点**​：使用 **1个字节** 存储一个字符。首位是 0。
- ​**局限**：无法存储中文。

### B. GBK 字符集 (中国人最熟悉)

- ​**地位**：Windows 系统默认的中文环境编码（cmd 命令行默认就是 GBK）。
- ​**包含**：完全兼容 ASCII，同时包含几万个汉字（简体、繁体）。
- ​**特点**：

  - ​**英文/数字**​：使用 ​**1个字节**。
  - ​**中文**​：使用 ​**2个字节**。
  - ​**识别规则**：如果由两个字节组成，且第一个字节是负数（高位为1），则强制将这两个字节拼在一起解析为一个汉字。

### C. Unicode (万国码) 与 UTF-8

- ​**地位**：国际标准，互联网通用标准（Java 源码、网页、Linux 默认都是 UTF-8）。
- ​**UTF-8 编码方案**​：是 Unicode 的一种实现方式，它是**可变长**的。
- ​**特点**：

  - ​**英文/数字**​：使用 ​**1个字节**（兼容 ASCII）。
  - ​**中文**​：通常使用 ​**3个字节**。
  - ​**生僻字/表情**：可能使用 4个字节。

### 总结对比表 (重点)

|**字符集**|**英文/数字占用**|**中文占用**|**备注**|||||
| --| --------| ----------| ------------------------| --| --| --| --|
|**ASCII**|1 byte|(不支持)|基础|||||
|**GBK**|1 byte|**2 bytes**|Windows 默认，中国特供|||||
|**UTF-8**|1 byte|**3 bytes**|**国际标准，开发推荐**|||||

---

## 2. 编码与解码 (Encoding & Decoding)

这是 IO流操作文本的核心机制。

- ​**编码 (Encode)** ：字符 (String) $\rightarrow$ 字节 (byte[])。也就是：​**把人看的懂的变成计算机看的懂的**。
- ​**解码 (Decode)** ：字节 (byte[]) $\rightarrow$ 字符 (String)。也就是：​**把计算机存的变成人看的懂的**。

> **核心原则：**  **编码和解码必须使用同一个字符集，否则必然乱码！**

### A. Java 中的编码 (String $\to$ byte[])

主要使用 `String`​ 类的 `getBytes()` 方法。

Java

```
String str = "a我b";

// 1. 使用默认字符集编码 (依赖 IDE 设置，通常是 UTF-8)
byte[] bytes1 = str.getBytes(); 

// 2. 指定字符集编码 (推荐)
// "a" (1) + "我" (3) + "b" (1) = 5 字节
byte[] bytes2 = str.getBytes("UTF-8"); 

// "a" (1) + "我" (2) + "b" (1) = 4 字节
byte[] bytes3 = str.getBytes("GBK"); 
```

### B. Java 中的解码 (byte[] $\to$ String)

主要使用 `String`​ 类的构造器 `new String(...)`。

Java

```
// 假设 bytes3 是上面用 GBK 编码得到的字节数组

// 1. 使用默认字符集解码 (如果是 UTF-8)
// 结果：乱码！因为 UTF-8 试图把 GBK 的 2个字节当成 3个字节解析。
String s1 = new String(bytes3); 

// 2. 指定正确的字符集解码
// 结果："a我b" (正确)
String s2 = new String(bytes3, "GBK"); 
```

---

## 3. 代码演示 (包含乱码场景)

Java

```
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class EncodeDecodeTest {
    public static void main(String[] args) throws UnsupportedEncodingException {
        String data = "我爱你中国";

        // -------------------------------
        // 1. 编码 (Encode)
        // -------------------------------
        // 使用 UTF-8 编码 (中文占3字节)
        byte[] bytesUTF8 = data.getBytes("UTF-8");
        System.out.println("UTF-8 字节长度: " + bytesUTF8.length); // 15 (5个字 * 3)
        System.out.println("UTF-8 字节内容: " + Arrays.toString(bytesUTF8));

        // 使用 GBK 编码 (中文占2字节)
        byte[] bytesGBK = data.getBytes("GBK");
        System.out.println("GBK 字节长度: " + bytesGBK.length);   // 10 (5个字 * 2)
        System.out.println("GBK 字节内容: " + Arrays.toString(bytesGBK));


        // -------------------------------
        // 2. 解码 (Decode)
        // -------------------------------
        // Case A: 正常解码 (编码和解码一致)
        String s1 = new String(bytesUTF8, "UTF-8");
        System.out.println("UTF-8 解码: " + s1); // 我爱你中国

        String s2 = new String(bytesGBK, "GBK");
        System.out.println("GBK 解码: " + s2);   // 我爱你中国

        // Case B: 乱码演示 (用 UTF-8 解码 GBK 的数据)
        // 原因：GBK 的"我"是 2个字节，UTF-8 把它当成三分之二个汉字去读，导致错位
        String s3 = new String(bytesGBK, "UTF-8");
        System.out.println("乱码演示: " + s3);   // й (典型乱码)
    }
}
```

## 4. 重点记忆

1. **英文**在 ASCII、GBK、UTF-8 中都只占 ​**1个字节**，所以纯英文通常不会乱码。
2. **中文**在 **GBK** 中占 ​**2个字节**​，在 **UTF-8** 中占 ​**3个字节**。
3. 如果不指定字符集，Java 会采用默认字符集（JDK 18 之前依赖环境，JDK 18+ 默认为 UTF-8）。​**建议始终显式指定字符集**。

---

# Java IO 流体系详细笔记

## 1. 认识 IO 流 (Overview)

### 1.1 什么是 IO？

- ​**I (Input)** : 输入。数据从硬盘/网络 $\rightarrow$ 内存 (读)。
- ​**O (Output)** : 输出。数据从内存 $\rightarrow$ 硬盘/网络 (写)。

### 1.2 流的分类 (重要)

|**分类标准**|**类型**|**说明**|**顶级父类 (抽象类)**|||||
| --| --| ---------------------------------------| ---------| --| --| --| --|
|**按流向**|**输入流**|读数据|​`InputStream`​,`Reader`|||||
||**输出流**|写数据|​`OutputStream`​,`Writer`|||||
|**按单位**|**字节流**|操作 8bit (1字节)，**万能**(图片、视频、文本)|​`InputStream`​,`OutputStream`|||||
||**字符流**|操作 16bit (字符)，**仅限文本**(自动处理编码)|​`Reader`​,`Writer`|||||
|**按角色**|**节点流**|直接连接数据源 (如文件)|​`FileInputStream`​,`FileReader`|||||
||**处理流**|包装其他流，增加功能 (如缓冲)|​`BufferedInputStream`|||||

---

## 2. 字节流 (Byte Streams)

​**核心用途**：文件复制、非文本文件 (图片/视频) 处理。

### 2.1 字节输入流 (`InputStream`​ -\> `FileInputStream`)

- ​**作用**：以字节为单位读取文件。
- ​**常用方法**：

  - ​`int read()`: 读取一个字节，返回字节的 int 值 (0-255)，读到末尾返回 -1。
  - ​`int read(byte[] b)`​: 读取多个字节存入数组 `b`​，​**返回实际读取的字节数**。 (推荐使用)
  - ​`void close()`: 释放资源。

### 2.2 字节输出流 (`OutputStream`​ -\> `FileOutputStream`)

- ​**作用**：以字节为单位写入文件。
- ​**构造器**：

  - ​`new FileOutputStream("path")`: 覆盖模式 (清空原文件)。
  - ​`new FileOutputStream("path", true)`​: **追加模式** (在原文件末尾写入)。
- ​**常用方法**：

  - ​`void write(int b)`: 写一个字节。
  - ​`void write(byte[] b)`: 写一个字节数组。
  - ​`void write(byte[] b, int off, int len)`: 写数组的一部分。

> ​**注意**​：换行符在 Windows 是 `\r\n`​，Linux 是 `\n`。

---

## 3. 字符流 (Character Streams)

​**核心用途**：读取/写入纯文本文件，解决中文乱码问题。

### 3.1 字符输入流 (`Reader`​ -\> `FileReader`)

- ​**原理**：底层还是字节流，但在读取时会自动根据字符集 (UTF-8/GBK) 将字节解码为字符。
- ​**常用方法**：

  - ​`int read()`: 读取一个字符。
  - ​`int read(char[] cbuf)`: 读取字符到数组。

### 3.2 字符输出流 (`Writer`​ -\> `FileWriter`)

- ​**原理**：将字符编码为字节写入。
- ​**缓冲机制 (坑点)** ：

  - ​`FileWriter` 自带一个小缓冲区。
  - ​**必须调用** **​`flush()`​** ​ **或** **​`close()`​** ，数据才会真正写到硬盘，否则数据会丢失在内存中。
- ​**常用方法**：

  - ​`write(String str)`: 直接写字符串 (最常用)。
  - ​`write(char[] cbuf)`: 写字符数组。

---

## 4. 缓冲流 (Buffered Streams)

​**核心用途**​：​**提高性能**。

- ​**原理**​：在内存中创建一个 **8KB (8192字节)**  的数组缓冲区。减少与硬盘 (OS) 的交互次数。
- ​**构造**​：`new BufferedInputStream(new FileInputStream(...))` (包装模式)。

### 4.1 字节缓冲流

- ​`BufferedInputStream`​ / `BufferedOutputStream`
- ​**用法**：与普通字节流完全一致，只是效率更高。

### 4.2 字符缓冲流 (有特有方法)

- ​**​`BufferedReader`​**:

  - ​**特有方法**​: `String readLine()`​ —— 一次读一行文本。读完返回 `null`。
- ​**​`BufferedWriter`​**:

  - ​**特有方法**​: `void newLine()` —— 写入一个跨平台的换行符。

---

## 5. 性能分析 (Benchmark)

在进行文件复制 (如复制一个视频) 时，不同方式的性能差异巨大：

|**方式**|**描述**|**速度评估**||||
| --| ---------| ------------------------------| --| --| --|
|**低级字节流 (单字节)**|​`read()`​/`write()`|**极慢**(禁止在生产环境使用)||||
|**低级字节流 (数组)**|​`read(byte[])`|**较快**(常用，推荐 1024\*8 大小)||||
|**缓冲字节流 (单字节)**|​`Buffered`​+`read()`|**快**(因为有内存缓冲)||||
|**缓冲字节流 (数组)**|​`Buffered`​+`read(byte[])`|**最快**(内存缓冲 + 数组传输)||||

> ​**结论**​：开发中首选 **字节缓冲流 + 数组** 或 ​**直接使用 commons-io**。

---

## 6. 其他流 (高级应用)

### 6.1 转换流 (Convert Streams)

- ​**作用**​：在字节流和字符流之间转换，​**并指定字符集 (编码/解码)** 。
- ​**类**：

  - ​`InputStreamReader`: 字节输入流 $\rightarrow$ 字符输入流 (解码)。
  - ​`OutputStreamWriter`: 字符输出流 $\rightarrow$ 字节输出流 (编码)。
- ​**场景**：读取 GBK 编码的文件 (否则会乱码)，或者将字符串以特定编码写入文件。

### 6.2 打印流 (Print Streams)

- ​**类**​：`PrintStream`​ (字节), `PrintWriter` (字符)。
- ​**特点**：

  1. ​**只操作输出**，不操作输入。
  2. ​**不会抛出 IOException**。
  3. 提供 `print()`​ 和 `println()` 方法，非常方便。
- ​**场景**​：`System.out`​ 就是一个 `PrintStream`。可以用来做日志输出重定向。

### 6.3 对象流 (序列化流)

- ​**作用**：将 Java 对象 (内存) 存入文件 (序列化)，或从文件恢复对象 (反序列化)。
- ​**类**​：`ObjectInputStream`​, `ObjectOutputStream`。
- ​**要求**​：操作的对象类必须实现 **​`Serializable`​** 接口 (标记接口)。
- ​**注意**​：`transient` 关键字修饰的成员变量不会被序列化。

---

## 7. IO 框架 (Commons IO)

实际开发中，我们很少手写原生的 IO 流代码 (太繁琐)，而是使用 Apache Commons IO 库。

### 7.1 引入依赖

XML

```
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.11.0</version>
</dependency>
```

### 7.2 `FileUtils` 常用方法 (神器)

一行代码解决复杂操作：

- **文件复制：**

  ```java
  FileUtils.copyFile(new File("a.jpg"), new File("b.jpg"));
  ```

- **文件夹复制**:

  ```java
  FileUtils.copyDirectory(new File("D:/src"), new File("D:/dest"));	
  ```
- **删除文件夹 (含内容)** :

  ```java
  FileUtils.deleteDirectory(new File("D:/temp"));
  ```
- **读取文件为字符串**:

  ```java
  String content = FileUtils.readFileToString(new File("a.txt"), "UTF-8");
  ```
- **写入字符串到文件**:

  ```java
  FileUtils.writeStringToFile(new File("a.txt"), "Hello Java", "UTF-8");
  ```

---

## 8. 最佳实践总结

1. ​**文件拷贝**​：使用 `BufferedInputStream`​ + `BufferedOutputStream`​ + `byte[]` 数组。
2. ​**文本读写**​：使用 `BufferedReader`​ / `BufferedWriter`。
3. **资源释放**：必须使用 JDK 7 引入的 **​`try-with-resources`​** 语法，自动关闭流。

   ```java
   try (FileInputStream fis = new FileInputStream("a.txt")) {
       // 读取操作
   } catch (IOException e) {
       e.printStackTrace();
   } // 自动调用 close()	
   ```
4. **能用框架就用框架** (Commons IO / Hutool)。

![image](/source/images/image-20260209201709-wx3v2b4.png)

‍
