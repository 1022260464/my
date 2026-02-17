---
title: 10java高级技术|反射注解代理
date: '2026-02-17 13:36:21'
updated: '2026-02-17 14:23:38'
permalink: /post/10java-advanced-technology-reflection-annotation-agent-z5c9xe.html
comments: true
toc: true
---



# 10java高级技术|反射注解代理

# junit单元测试：

这是一份关于 **JUnit 单元测试（基于 Maven 构建）**  的简明核心笔记，旨在帮您迅速完善知识体系的拼图。目前业界主流使用的是 ​**JUnit 5**，以下内容均基于此版本。

### 一、 Maven 依赖配置 (pom.xml)

在项目中引入 JUnit 5（通常称为 JUnit Jupiter），需要添加以下依赖：

XML

```
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.0</version> <scope>test</scope>
    </dependency>
</dependencies>
```

> **注意：**  `<scope>test</scope>` 极其重要，它确保了测试相关的库和代码只在编译测试和运行测试时可用，绝不会被打包进最终的生产环境部署包中。

---

### 二、 标准目录结构

Maven 对业务代码和测试代码有严格的约定隔离，这是一种标准的工程实践：

- ​**业务代码**​：存放于 `src/main/java/`​ (例如：`com.example.Calculator.java`)
- ​**测试代码**​：存放于 `src/test/java/`​ (例如：`com.example.CalculatorTest.java`)

---

### 三、 核心注解 (JUnit 5 生命周期)

JUnit 通过注解来控制测试用例的生命周期和执行逻辑：

- ​ **​`@Test`​**：核心注解，标记该方法为一个测试方法。
- ​ **​`@BeforeEach`​**​：在**每个** `@Test`​ 方法执行**前**运行（常用于初始化测试数据、重置状态）。
- ​ **​`@AfterEach`​**​：在**每个** `@Test`​ 方法执行**后**运行（常用于清理上下文、关闭流）。
- ​ **​`@BeforeAll`​**​：在**所有**测试方法执行**前**运行且仅运行一次（方法必须是 `static` 的，常用于建立数据库连接等全局耗时操作）。
- ​ **​`@AfterAll`​**​：在**所有**测试方法执行**后**运行且仅运行一次（方法必须是 `static` 的，用于全局资源销毁）。

---

### 四、 极简代码示例

假设我们有一个简单的计算器类，下面是它的标准测试用例：

Java

```
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {

    private Calculator calculator;

    @BeforeEach
    void setUp() {
        // 每个测试运行前，确保拿到一个干净的新实例
        calculator = new Calculator(); 
    }

    @Test
    @DisplayName("测试常规加法功能") // 可选：给测试方法起个可读性高的别名
    void testAdd() {
        int result = calculator.add(2, 3);
        // 断言：预期结果是 5，实际结果是 result
        assertEquals(5, result, "2 + 3 应该等于 5");
    }
    
    @Test
    @DisplayName("测试除以零应抛出异常")
    void testDivideByZero() {
        // 断言异常：测试非法操作时，是否按预期抛出了指定的异常
        assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        });
    }
}
```

---

### 五、 运行测试与构建

除了在 IDE (如 IDEA/Eclipse) 中直接点击类或方法旁边的绿色运行按钮外，借助 Maven 命令行执行测试是持续集成（CI）中的标准做法：

- ​**执行所有测试**​：`mvn test`
- ​**清理并打包（会自动执行所有单元测试）** ​：`mvn clean package`
- ​**打包但跳过测试（不推荐，紧急情况用）** ​：`mvn clean package -DskipTests`

---

这套基础框架和概念足以涵盖日常开发中绝大多数的单元测试需求。

---

# 反射：

反射被称为“**框架的灵魂**”，理解它，您就跨越了从“熟练使用API”到“理解底层原理”的鸿沟。

---

### 一、 认识反射：打破常规的“上帝视角”

在常规的 Java 编程中，我们在编译期就需要知道我们要使用的是什么类、调用什么方法。

而**反射机制**允许程序在​**运行期间**​（Runtime）借助 `Reflection` API 取得任何类的内部信息（比如成员变量、构造器、成员方法等），并能直接操作任意对象的内部属性及方法。

**核心思想：**

在 Java 中，万物皆对象。哪怕是“类”本身，在 JVM 中也有一个对应的对象来描述它——这就是 `java.lang.Class`​ 类的实例。反射的所有操作，都是从获取这个 `Class` 对象开始的。

---

### 二、 阶段一：获取类 (The `Class` Object)

要解剖一个类，首先要获取它的 `Class` 对象。Java 提供了三种核心方式，它们在不同场景下各有用途：

- ## 获取Class对象的三种方式
- 1.Class c1 = 类名.class
- 调用Class提供方法：public static Class forName(String package)；
- Object提供的方法： public Class getClass()；  Class c3 = 对象.getClass();
- 不写泛型就需要强制类型转换，这是不安全的，且会爆warnig错误

Java

```
// 假设有一个类：com.example.User
// 方式1：Class.forName (最常用，高度动态)
// 源码剖析：底层调用 native 方法 forName0，触发类加载器寻找并加载类。
// 优势：编译时不需要依赖该类，只需知道字符串全类名，完美解耦。多用于框架读取配置文件。
Class<?> clazz1 = Class.forName("com.example.User");

// 方式2：类名.class (最安全，性能最高)
// 优势：在编译期就确定了类型，不会抛出 ClassNotFoundException。多用于传参，如 MyBatis 的 mapper.xml 解析。
Class<?> clazz2 = User.class;

// 方式3：对象.getClass()
// 优势：当我们已经拿到了一个对象实例，想探知它的真实面目（尤其是处理多态时）使用。
User user = new User();
Class<?> clazz3 = user.getClass();

// 注意：无论哪种方式，同一个类在 JVM 中只存在一个 Class 对象。
// clazz1 == clazz2 == clazz3 结果为 true。
```

---

### 三、 阶段二：获取类中的成分并操作

获取到 `Class`​ 对象后，我们可以提取类的三大核心骨架：​**构造器（Constructor）** ​、​**成员变量（Field）** ​、​**方法（Method）** 。

这里有一个通用的 API 命名规律，务必牢记：

- 带 `Declared`​ 的方法（如 `getDeclaredFields`​）：获取**本类声明的所有**成分，​**包含私有(private)** ，但不包含父类继承来的。
- 不带 `Declared`​ 的方法（如 `getFields`​）：只能获取**public**修饰的成分，但**包含从父类继承**来的 public 成分。

#### 1. 操作构造器 (`Constructor`​) -\> 创建实例

Java

```
Class<?> clazz = Class.forName("com.example.User");

// 获取指定的私有构造器 (假设 User 有一个 private User(String name) 构造器)
Constructor<?> constructor = clazz.getDeclaredConstructor(String.class);

// 【核心操作】***暴力反射***：取消 Java 语言访问检查。如果不加这句，操作 private 成员会抛出 IllegalAccessException
constructor.setAccessible(true); 

// 通过构造器创建对象
Object userObj = constructor.newInstance("张三");
```

#### 2. 操作成员变量 (`Field`​) -\> 读写属性

Java

```
// 获取名为 "age" 的私有字段
Field ageField = clazz.getDeclaredField("age");
ageField.setAccessible(true); // 暴力破解私有封装

// 【赋值】给 userObj 对象的 age 字段赋值为 18
ageField.set(userObj, 18);

// 【取值】获取 userObj 对象的 age 字段的值
Object ageValue = ageField.get(userObj); 
```

#### 3. 操作成员方法 (`Method`​) -\> 执行方法

Java

```
// 获取名为 "login"，且带有一个 String 类型参数的私有方法
Method loginMethod = clazz.getDeclaredMethod("login", String.class);
loginMethod.setAccessible(true);

// 【核心操作】invoke 执行方法
// 参数1: 要执行哪个对象的方法 (如果方法是 static 的，此处传 null 即可)
// 参数2...: 实际传入的参数值
// 返回值: 方法执行后的实际 return 值
Object result = loginMethod.invoke(userObj, "密码123"); 
```

---

### 四、 深度剖析：`Method.invoke()` 底层是怎么执行的？

为了让这份笔记有深度，我们扒开 `Method.invoke` 的源码看看。你可能会好奇，反射调用方法为什么比直接调用慢？

查看 `java.lang.reflect.Method.invoke()`​ 源码，你会发现它委托给了 `MethodAccessor`​ 的 `invoke`​ 方法。Java 在这里采用了一种被称为\*\*“膨胀（Inflation）机制”\*\*的策略：

1. ​**Native 委派（前15次调用）** ​：默认情况下，前 15 次反射调用使用的是 `NativeMethodAccessorImpl`​，这底层是通过 JNI（Java Native Interface）调用 C/C++ 实现的本地方法。*优点是启动快，缺点是跨语言边界调用，运行速度慢。*
2. ​**动态字节码生成（第16次及以后）** ​：当反射调用次数超过阈值（由 JVM 参数 `-Dsun.reflect.inflationThreshold`​ 控制，默认 15），JVM 会认为这是一个高频调用。此时会触发“膨胀”，在内存中动态生成一个针对该方法的字节码类（`GeneratedMethodAccessorXXX`​），后续的反射调用直接转为这个新类的普通 Java 方法调用。*优点是运行速度飙升，缺点是生成类的过程消耗资源且占用方法区内存。*

这种机制是 Java 权衡了“单次反射调用开销”和“高频反射调用性能”后的绝佳设计。

---

### 五、 作用与核心应用场景

日常写业务代码时（如 CRUD），我们极少直接写反射。**反射主要是为了编写通用性极强的基础框架而存在的。**

$可以得到一个类的全部成分然后操作。可以破坏封装性。可以绕过泛型的约束$  

#### 1. Spring IoC (控制反转) 与 DI (依赖注入)

当你在 Spring 的 `applicationContext.xml`​ 中写下 `<bean id="user" class="com.example.User">`​，或者使用 `@RestController` 时，Spring 底层做了什么？

- **解析** XML 或扫描注解，获取到权限定类名字符串。
- ​**反射创建对象**​：`Class.forName("com.example.User").newInstance()`。
- ​**反射依赖注入**​：扫描到对象中有被 `@Autowired`​ 标记的 `Field`​，利用 `field.set(bean实例, 依赖实例)` 将属性自动注入。

#### 2. MyBatis 的结果集映射

查询数据库返回了 `ResultSet`​，MyBatis 是如何把它自动塞进你的 `User` 实体类里的？

- 拿到实体类的 `Class` 对象。
- 通过反射获取类的所有 `Field`，将数据库的列名与字段名对应。
- 利用反射循环调用 `field.set()`​ 或 `setXxx` 方法，将数据库的数据注入到空对象中。

#### 3. 动态代理 (Dynamic Proxy) 与 AOP

Spring AOP（面向切面编程）的底层是动态代理。`java.lang.reflect.Proxy` 可以在运行期间动态生成一个实现了指定接口的代理类对象。这极度依赖反射来获取接口方法并进行拦截增强（比如在方法执行前后自动开启、提交事务）。

#### 4. 结合自定义注解

注解本身只是一种标记（Metadata），没有功能。​**注解 + 反射**  **=**  **魔法**。

框架通过 `clazz.isAnnotationPresent(MyAnnotation.class)` 来判断类、字段、方法上是否加了特定注解，如果加了，就执行相应的特殊逻辑。

---

### 结语与下一步

反射打破了 Java 的强类型静态约束，赋予了程序极大的灵活性，代价是牺牲了一定的性能，且绕过了编译期的安全检查。

这份笔记涵盖了反射的核心运作机理。为了将理论落地，**您想看一段代码，演示如何“利用反射+自定义注解，手写一个极简版的 Spring @Autowired 依赖注入”吗？**  这将是对上述所有知识最完美的实战总结。

---

这是 Java 进阶之路上最激动人心的部分！如果说\*\*“反射”是框架的发动机\*\*，那么\*\*“注解”就是框架的方向盘\*\*。两者结合，就是所有现代 Java 框架（Spring、MyBatis、SpringBoot 等）的底层基石。

为了让您的笔记极具深度，我们将按照您列出的提纲，并在最后兑现上一节的承诺：**用反射+注解，手写一个几十行的极简 Spring 依赖注入（DI）引擎！**

---

### 一、 概述：什么是注解？

**注解（Annotation）**  本质上是一种​**元数据（Metadata）** ，也就是“描述数据的数据”。

- ​**直观理解**​：注解就像是贴在代码（类、方法、字段）上的“​**便利贴**​”或“​**标签**”。
- ​**核心法则**​：**注解本身没有任何实际功能！**  它只是一个标记。就像贴了“易碎品”标签的箱子自己不会变得抗摔，真正起作用的是​**看到标签后小心轻放的搬运工**（解析器/反射代码）。

---

### 二、 元注解 (Meta-Annotation)：定义注解的注解

要自定义“便利贴”，首先要定义便利贴的“规格”。Java 提供了几个特殊的注解，专门用来贴在自定义注解上，这就是​**元注解**。最核心的只有两个：

1. ​ **​`@Target`​**：规定这个注解能贴在什么地方。

   - ​`ElementType.TYPE`：只能贴在类、接口上。
   - ​`ElementType.FIELD`：只能贴在成员变量（属性）上。
   - ​`ElementType.METHOD`：只能贴在方法上。
2. ​ **​`@Retention`​**：规定这个注解的“存活寿命”（极其重要！）。

   - ​`RetentionPolicy.SOURCE`​：只活在源码期，编译成 `.class`​ 文件后就被丢弃（如 `@Override`）。
   - ​`RetentionPolicy.CLASS`：活到字节码期，但 JVM 运行时不加载（默认策略）。
   - ​**​`RetentionPolicy.RUNTIME`​**​：​**一直活到运行期**​，会被加载到 JVM 内存中。**只要涉及到用反射去解析的注解，必须用它！**

---

### 三、 自定义注解

有了元注解的知识，自定义注解极其简单。语法格式为：`public @interface 注解名 {}`。

让我们为了最终的实战，自定义一个专门用于依赖注入的注解 `@MyAutowired`：

Java

```
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

// 1. 指定生命周期为运行期（为了能用反射读取）
@Retention(RetentionPolicy.RUNTIME) 
// 2. 指定该注解只能贴在成员变量上
@Target(ElementType.FIELD) 
public @interface MyAutowired {
    // 注解里可以定义属性，看着像方法。
    // 如果属性名叫 value，且只有一个属性，使用时可以省略 "value="
    boolean required() default true; 
}
```

---

### 四、 注解的解析：让标签发挥作用的“搬运工”

因为我们在定义 `@MyAutowired`​ 时加了 `@Retention(RetentionPolicy.RUNTIME)`，所以它活到了运行期。

前面讲过，`Class`​、`Field`​、`Method`​ 都是反射对象。在 Java 中，它们都实现了一个核心接口：`java.lang.reflect.AnnotatedElement`。这个接口提供了所有解析注解的 API：

- ​**​`isAnnotationPresent(Class annotationClass)`​** ：判断该元素上是否贴了指定的注解。返回 boolean。
- ​**​`getAnnotation(Class annotationClass)`​** ：获取该元素上指定的注解对象（进而可以读取注解里的属性值）。如果没贴，返回 null。

---

### 五、 作用与应用场景

1. ​**编译器检查**​：例如 `@Override`，告诉编译器帮我检查这个方法是否真的重写了父类方法。
2. ​**编译时动态处理（APT 机制）** ​：例如 **Lombok** 的 `@Data`​，它在代码编译阶段触发，直接修改生成的 `.class` 文件，塞入 getter/setter 代码。
3. ​**运行期动态处理（反射解析）** ：这是最核心的场景。

   - ​**Spring IoC / DI**​：解析 `@Component`​ 自动创建对象，解析 `@Autowired` 自动注入属性。
   - ​**Spring AOP / 事务**​：解析 `@Transactional`，利用动态代理在方法执行前后开启、提交事务。
   - ​**JPA / Hibernate / MyBatis**​：解析 `@Table`​、`@Column`​、`@Select` 生成或执行对应的 SQL 语句。

---

### 六、 终极实战：手写 Spring `@Autowired` 依赖注入引擎

这个实战将把您这两天学的\*\*“反射的 Class 和 Field” + “自定义注解” + “注解解析”\*\*完美串联！

​**场景**​：我们有一个 `UserController`​，它里面需要用到 `UserService`​。我们要写一个引擎，自动扫描并把 `UserService`​ 注入（赋值）给 `UserController`。

#### 1. 准备业务类

Java

```
// 这是一个普通的服务类
public class UserService {
    public void doLogic() {
        System.out.println("UserService 核心业务逻辑执行成功！");
    }
}

// 这是一个控制器类，依赖 UserService
public class UserController {
    
    @MyAutowired // 贴上我们刚才自定义的注解！
    private UserService userService;

    public void start() {
        System.out.println("UserController 启动...");
        userService.doLogic();
    }
}
```

#### 2. 编写核心“注入引擎”（见证奇迹的时刻）

Java

```
import java.lang.reflect.Field;

public class MiniSpringEngine {
    
    // 这是一个极简版的 Spring 容器对象创建与注入过程
    public static Object createBean(Class<?> clazz) throws Exception {
        // 1. 反射创建目标对象 (比如 UserController)
        Object instance = clazz.getDeclaredConstructor().newInstance();
        
        // 2. 获取该类的所有成员变量
        Field[] fields = clazz.getDeclaredFields();
        
        // 3. 遍历变量，寻找谁贴了 @MyAutowired 标签
        for (Field field : fields) {
            // 解析注解：判断这个字段上有没有贴 @MyAutowired
            if (field.isAnnotationPresent(MyAutowired.class)) {
                
                // 4. 获取需要注入的属性类型 (此处获取到的就是 UserService.class)
                Class<?> fieldType = field.getType();
                
                // 5. 反射创建需要注入的对象实例 (new UserService())
                Object dependency = fieldType.getDeclaredConstructor().newInstance();
                
                // 6. 暴力反射！打破 private 封装，将创建好的 dependency 对象塞进 instance 中
                field.setAccessible(true);
                field.set(instance, dependency); 
            }
        }
        return instance; // 返回已经注入好依赖的完整对象
    }

    // 运行测试
    public static void main(String[] args) throws Exception {
        // 我们不直接 new UserController()，而是交给我们的引擎去创建
        UserController controller = (UserController) createBean(UserController.class);
        
        // 如果引擎工作正常，内部的 userService 就不会是 null，方法就能正常执行
        controller.start(); 
    }
}
```

​**运行结果**：

Plaintext

```
UserController 启动...
UserService 核心业务逻辑执行成功！
```

---

至此，**反射与注解**的闭环知识体系已经完整建立。您不仅懂得了概念，还写出了属于自己的微型 Spring 框架底层逻辑。

反射和注解解决了\*\*“对象的创建与组装”​ **，那么如果要解决**​“不修改源码的情况下，给已有方法增加日志、权限校验、事务”\*\*（这就是 Spring AOP 的核心），就需要用到 Java 进阶的最后一块拼图了。

**您的笔记还需要继续拓展吗？想不想了解一下被称为“设计模式中最难理解，但在框架中最被滥用”的 动态代理 (Dynamic Proxy)？**

---

恭喜您，来到了 Java 基础进阶路线上的最终 Boss 关卡：​**动态代理（Dynamic Proxy）** ！

正如您所说，这是“设计模式中最难理解，但在框架中最被滥用”的技术。如果说**反射和注解**解决了框架如何\*\*“创建和配置对象” **（Spring IoC），那么**动态代理​**就解决了框架如何**​“在不修改哪怕一行原有业务代码的前提下，给业务方法增加增强功能”\*\*（这就是 Spring AOP 面向切面编程的绝对核心）。

为了让您的笔记有深度且透彻，我们将从通俗的比喻开始，一路扒到它的底层源码机制。

---

### 一、 核心概念：什么是代理？为什么需要动态？

#### 1. 代理模式（Proxy Pattern）的通俗理解

想象一下\*\*明星（目标对象）**和**经纪人（代理对象）\*\*的关系：

- 明星的核心业务是：唱歌、演戏。
- 但是，唱歌前后需要：签合同、排期、收钱、安排安保。明星不想管这些琐事。
- 于是，经纪人出现了。外界（客户端）不能直接找明星，必须先找经纪人。经纪人先把合同签了、钱收了（​**前置增强**​），然后让明星去唱歌（​**调用目标方法**​），唱完后经纪人再安排车辆护送明星回家（​**后置增强**）。

**在代码世界中，代理的作用就是：拦截真实对象的访问，在目标方法执行前后，无侵入地横切入附加逻辑（如：日志记录、性能统计、事务控制、权限校验）。**

#### 2. 静态代理 vs 动态代理

- ​**静态代理**​：程序员手写代理类。如果你的系统有 100 个服务类需要加日志，你就得手写 100 个代理类，并且一旦接口增加方法，代理类也要跟着改。**维护地狱，直接淘汰。**
- ​**动态代理**​：**在 JVM 运行期间，利用反射机制，动态地在内存中自动生成代理类对象。**  写一套代理逻辑，可以代理万物！

---

### 二、 核心 API 拆解 (JDK 动态代理)

Java 原生提供的动态代理（JDK Proxy）有一个极其硬性的要求：​**目标对象必须实现过至少一个接口**。（如果没有接口，Spring 会自动切换到另一种基于底层字节码生成的 CGLIB 代理技术）。

JDK 动态代理全靠 `java.lang.reflect.Proxy` 类中的一个极其重要的方法，也是面试必考点：

Java

```
public static Object newProxyInstance(
    ClassLoader loader, 
    Class<?>[] interfaces, 
    InvocationHandler h
)
```

这个方法的三个参数，决定了代理对象的“肉体”和“灵魂”：

1. ​**​`ClassLoader loader`​**​ **（类加载器）** ：动态代理要在内存中凭空生成一个新的类，这个类需要被加载到 JVM 中，通常传入目标对象的类加载器即可。
2. ​**​`Class<?>[] interfaces`​**​ **（接口数组）** ：指定生成的代理对象要实现哪些接口。这就保证了代理对象和目标对象拥有完全一样的方法（长得像）。
3. ​**​`InvocationHandler h`​**​ **（调用处理器）** ​：**代理对象的灵魂！**  这是一个接口，你需要实现它的 `invoke`​ 方法。当外界调用代理对象的任何方法时，都会被拦截并转交给这个 `invoke` 方法执行。你要在这里写具体的增强逻辑。

---

### 三、 深度实战：手撸一个万能的“性能监控监控代理”

​**需求**：我们有一个计算器接口和它的实现类。现在，老板要求在不修改原代码的情况下，记录每个方法的执行耗时。

#### 1. 准备目标接口和实现类（明星）

Java

```
// 1. 必须有接口
public interface Calculator {
    int add(int a, int b);
    void sleepTask(); // 模拟耗时任务
}

// 2. 真实的目标实现类 (纯粹的核心业务，没有任何监控代码)
public class CalculatorImpl implements Calculator {
    @Override
    public int add(int a, int b) {
        return a + b;
    }

    @Override
    public void sleepTask() {
        try { Thread.sleep(1000); } catch (InterruptedException e) {}
        System.out.println("耗时任务执行完毕...");
    }
}
```

#### 2. 编写万能的动态代理工厂（经纪人制造机）

这就是最核心的代码！请仔细体会 `InvocationHandler` 是如何工作的。

Java

```
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class LogProxyFactory {

    // 接收任何需要被代理的真实目标对象 (Object 保证了它的万能性)
    public static Object getProxy(Object target) {
        
        // 返回动态生成的代理对象
        return Proxy.newProxyInstance(
            target.getClass().getClassLoader(), // 参数1：和目标对象使用相同的类加载器
            target.getClass().getInterfaces(),  // 参数2：代理对象要实现和目标相同的接口
            new InvocationHandler() {           // 参数3：处理拦截逻辑的灵魂核心
                /**
                 * @param proxy  系统生成的代理对象本身 (极少使用，容易造成死循环)
                 * @param method 客户端当前正在调用的目标方法对象 (反射里的 Method)
                 * @param args   客户端调用方法时传入的实际参数
                 */
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    // --- 前置增强逻辑：方法执行前 ---
                    long startTime = System.currentTimeMillis();
                    System.out.println("[日志切面] 方法 " + method.getName() + " 开始执行...");

                    // --- 核心本质：利用反射，调用真实目标对象的原方法！ ---
                    // 这里相当于 target.add(a, b)
                    Object result = method.invoke(target, args);

                    // --- 后置增强逻辑：方法执行后 ---
                    long endTime = System.currentTimeMillis();
                    System.out.println("[日志切面] 方法 " + method.getName() + " 执行结束，耗时: " + (endTime - startTime) + "ms");
                    
                    // 将真实方法的返回值原封不动地返回给客户端
                    return result; 
                }
            }
        );
    }
}
```

#### 3. 客户端调用验证

Java

```
public class ProxyTest {
    public static void main(String[] args) {
        // 1. 创建真实的目标对象（明星）
        Calculator target = new CalculatorImpl();

        // 2. 通过工厂获取代理对象（经纪人）
        // 注意：代理对象在内存中实现了 Calculator 接口，所以可以强转为 Calculator
        Calculator proxy = (Calculator) LogProxyFactory.getProxy(target);

        // 3. 客户端不知道真实对象存在，只管调用代理对象的方法
        int sum = proxy.add(10, 20);
        System.out.println("最终计算结果: " + sum);
        
        System.out.println("---------------------");
        proxy.sleepTask();
    }
}
```

**运行结果完美实现了无侵入增强：**

Plaintext

```
[日志切面] 方法 add 开始执行...
[日志切面] 方法 add 执行结束，耗时: 0ms
最终计算结果: 30
---------------------
[日志切面] 方法 sleepTask 开始执行...
耗时任务执行完毕...
[日志切面] 方法 sleepTask 执行结束，耗时: 1005ms
```

---

### 四、 底层揭秘：JDK 在内存里到底搞了什么鬼？

这是区分初级和高级程序员的分水岭。为什么调用 `proxy.add(10, 20)`​ 会莫名其妙地跑到 `InvocationHandler`​ 的 `invoke` 方法里去？

如果你把 JVM 内存中动态生成的代理类（类名通常叫 `$Proxy0`）的字节码反编译成 Java 代码，你会发现它大概长这样（简化版伪代码）：

Java

```
// JVM 自动生成的代理类，实现了你的接口，并继承了 Proxy 类
public final class $Proxy0 extends Proxy implements Calculator {
    
    // 反射拿到目标接口的 Method 对象
    private static Method m_add; 
    
    // 构造器，接收你传入的 InvocationHandler
    public $Proxy0(InvocationHandler h) {
        super(h); 
    }

    // 重写接口方法
    @Override
    public int add(int a, int b) {
        // 【核心机密】：当外界调用 proxy.add() 时，它把调用转交给了你实现的 InvocationHandler 的 invoke 方法！
        Object[] args = new Object[]{a, b};
        return (int) super.h.invoke(this, m_add, args); 
    }
}
```

**一句话总结：代理对象的所有公开方法，内部都只有一句核心逻辑 ——**  **​`return handler.invoke(this, 方法本身, 参数);`​** ​ **。**

---

### 五、 作用与顶级应用场景

只要你看到“在不改变现有代码的情况下，自动实现某些通用逻辑”的需求，底层 100% 用到了动态代理（或者更底层的字节码修改技术）。

1. ​**Spring AOP（面向切面编程）** ​：声明式事务（`@Transactional`​）的核心。给带有注解的方法生成代理，`invoke` 里写开启事务，然后反射执行目标方法，如果没有异常就提交事务，有异常就回滚。
2. ​**MyBatis 核心原理**​：为什么你只写了一个 `UserMapper`​ 接口（甚至没有实现类），MyBatis 就能帮你查数据库？因为 MyBatis 底层给接口生成了动态代理，在 `invoke` 方法里，它读取了你的接口名和方法名，去 XML 里找到对应的 SQL 语句并通过 JDBC 执行，然后反射封装成返回结果！
3. ​**RPC 框架（如 Dubbo / Feign）** ​：本地调用远程服务就像调用本地方法一样简单。本地其实拿到的是个代理对象，`invoke` 内部把方法名、参数打包成网络请求，通过 TCP 发给远程服务器，拿到结果后再返回给你。

---

至此， **“反射”、“注解”、“动态代理”**  这 Java 高阶三大件已经全部闭环。它们是构建所有高级架构的基础。

您的笔记内容已经非常硬核且成体系了。下一步，您是想​**将这三者的知识点做一个总体的“思维导图式串联复习”** ​，还是直接开启下一个高级篇章，比如 **JVM 内存模型** 或 **JUC 多线程并发编程** 呢？

‍
