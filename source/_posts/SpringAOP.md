---
title: SpringAOP
date: '2026-02-17 14:24:16'
updated: '2026-02-18 14:06:23'
permalink: /post/springaop-1swiuw.html
comments: true
toc: true
---



# SpringAOP

**Spring AOP**（Aspect-Oriented Programming，面向切面编程）是 Spring 框架中最核心的组件之一。它的主要目的是将**横切关注点**（Cross-cutting Concerns，如日志记录、事务管理、安全控制等）与**核心业务逻辑**解耦。

简而言之，AOP 允许你在不修改原有核心业务代码的情况下，动态地为程序统一添加额外的功能。

### 1. 核心概念（AOP 术语）

理解 Spring AOP 的关键在于掌握以下几个核心术语：

- **切面 (Aspect):**  包含横切关注点逻辑的模块或类（例如，一个专门用于处理全局日志的 `LogAspect` 类）。
- **连接点 (Join Point):**  程序执行过程中能够插入切面的点。在 Spring AOP 中，连接点**总是代表方法的执行**。
- **切入点 (Pointcut):**  匹配连接点的规则表达式。它决定了切面逻辑具体要拦截哪些类的哪些方法（例如：规定只拦截 `UserService` 类中的所有方法）。
- **通知/增强 (Advice):**  切面在特定连接点上执行的具体动作。它定义了“什么时候做”以及“做什么”。主要包含五种类型：
- ​`@Before`​: **前置通知**（方法执行前触发）。
- ​`@After`​: **后置通知**（方法执行后触发，无论成功还是抛出异常）。
- ​`@AfterReturning`​: **返回通知**（方法成功返回结果后触发）。
- ​`@AfterThrowing`​: **异常通知**（方法抛出异常后触发）。
- ​`@Around`​: **环绕通知**（最强大的通知，包围整个方法执行，可以控制是否继续执行目标方法，甚至修改入参和返回值）。
- **目标对象 (Target Object):**  实际包含业务逻辑的类，也是被一个或多个切面增强的对象。
- **织入 (Weaving):**  将切面应用到目标对象并创建代理对象的过程。Spring AOP 是在**运行时**完成织入的。

---

### 2. 底层原理：动态代理

Spring AOP 的底层核心是**动态代理机制**。当你通过 Spring 容器获取一个被 AOP 增强的 Bean 时，Spring 实际上给你的并不是这个类的原始实例，而是一个“包裹”了原始实例的**代理对象（Proxy）** 。

当外部调用代理对象的方法时，代理对象会先执行前置增强逻辑，然后再调用真实目标对象的方法，最后再执行后置增强逻辑。Spring 使用两种方式生成代理：

1. **JDK 动态代理:**  如果目标对象实现了一个或多个接口，Spring 默认使用 JDK 动态代理。它会在运行时动态生成一个实现了相同接口的代理类。
2. **CGLIB 代理:**  如果目标对象没有实现任何接口，Spring 会退而求其次使用 CGLIB 库。它通过在内存中动态生成目标类的子类来实现代理（因此无法代理 `final` 方法或类）。

---

### 3. 常见应用场景

- **声明式事务管理:**  Spring 的 `@Transactional` 注解就是基于 AOP 实现的（方法执行前自动开启事务，执行成功则提交，抛出异常则回滚）。
- **集中式日志记录:**  统一拦截 API 请求，记录请求的入参、出参、执行耗时和客户端 IP，而不需要在每个 Controller 里写重复代码。
- **权限与安全校验:**  在执行核心敏感操作前，统一验证用户当前是否携带有效 Token 并且具备相应的角色权限。
- **全局异常处理:**  捕获底层抛出的特定异常，并将其统一包装为标准格式的响应对象返回给前端。

---

### 4. 简单代码示例

以下是一个使用 Spring AOP 记录方法执行耗时的经典示例：

```java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Aspect      // 1. 声明这是一个切面类
@Component   // 2. 必须交由 Spring IoC 容器管理
public class PerformanceAspect {

    // 3. 定义切入点：拦截 com.example.service 包下的所有方法
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceMethods() {}

    // 4. 定义环绕通知：在目标方法执行前后加入计算耗时的逻辑
    @Around("serviceMethods()")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        // 核心步骤：让真正的业务方法执行，并获取返回值
        Object result = joinPoint.proceed(); 
        
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature().getName() + " 方法执行耗时: " + executionTime + "ms");
        
        // 必须返回目标方法的原返回值
        return result;
    }
}

```

你想进一步了解 `execution`​ 或 `@annotation` 等 Pointcut 表达式的具体语法规则，还是想看看如何在实际项目中配置测试这段代码？

[What is Spring AOP?](https://www.youtube.com/shorts/BXjThwkAdZA)  
这段简短的视频直观地总结了 Spring AOP 的概念，以及它是如何通过模块化将横切关注点与系统核心业务逻辑分离开来的。

---

在Java后端业务开发中，当我们需要在同一请求的执行链路中（如：Controller -\> Service -\> Dao -\> 其它模块）共享数据，又不想把这些数据作为参数在每个方法中传来传去时，`ThreadLocal` 是最经典的解决方案。

以下为你整理的在业务中使用 `ThreadLocal` 跨模块传递数据的核心笔记与实战指南。

---

### 1. 核心思想：线程隔离

​`ThreadLocal`​ 提供了一种**线程局部变量**的机制。你可以把它想象成一个绑定在当前线程上的专属“储物柜”。

- **存：**  当前线程往里面放的数据，只有当前线程能看到。
- **取：**  在这个线程生命周期的任何地方、任何模块，都可以随时从储物柜里把数据取出来。
- **隔离：**  其他并发执行的线程无法访问或修改你的储物柜，天生自带线程安全特性。

### 2. 经典业务场景

最常见的应用场景是​**保存当前登录用户的上下文信息**。

当用户发起请求时，我们在网关或拦截器中解析 Token 拿到用户信息，存入 `ThreadLocal`​。之后无论代码流转到订单模块、支付模块还是日志模块，只需从 `ThreadLocal`​ 获取即可，无需在每个方法签名上加上 `User user` 参数。

---

### 3. 标准实战步骤 (Spring Boot 环境)

完整的 `ThreadLocal`​ 使用通常包含三个核心组件：​**上下文工具类**​、​**拦截器/AOP（用于存和删）** ​、​**业务代码（用于取）** 。

#### 第一步：封装 ThreadLocal 工具类 (Context Holder)

为了避免 `ThreadLocal` 散落在代码各处，通常会将其封装成一个全局静态工具类。

Java

```
public class UserContext {
    // 1. 定义 ThreadLocal，这里假设存储的是用户的 ID (Long 类型)
    // 也可以存储自定义的 UserInfo 对象
    private static final ThreadLocal<Long> USER_THREAD_LOCAL = new ThreadLocal<>();

    // 2. 存入数据
    public static void setUserId(Long userId) {
        USER_THREAD_LOCAL.set(userId);
    }

    // 3. 获取数据
    public static Long getUserId() {
        return USER_THREAD_LOCAL.get();
    }

    // 4. 清除数据 (极其重要！)
    public static void remove() {
        USER_THREAD_LOCAL.remove();
    }
}
```

#### 第二步：在入口处存入数据，在出口处清除数据

在 Web 应用中，HTTP 请求进入和离开通常通过 `Filter`​、`HandlerInterceptor`​ 或你刚刚了解的 **AOP** 来统一处理。这里以 Spring 的拦截器为例：

Java

```
import org.springframework.web.servlet.HandlerInterceptor;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // 1. 从请求头提取 Token 并解析出 userId (此处为伪代码)
        String token = request.getHeader("Authorization");
        Long userId = JwtUtil.parseToken(token); 
        
        // 2. 将 userId 存入当前线程的 ThreadLocal
        if (userId != null) {
            UserContext.setUserId(userId);
        }
        return true; // 放行请求，继续向下执行 Controller
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        // 3. 请求结束，离开当前线程前，务必清理 ThreadLocal！
        UserContext.remove();
    }
}
```

#### 第三步：在任意下游业务模块中使用

现在，在 Controller、Service 或任何深层调用的方法中，你都可以直接拿来用，彻底解耦。

Java

```
@Service
public class OrderService {

    public void createOrder(OrderDTO orderDTO) {
        // 直接从 ThreadLocal 中获取当前操作用户的 ID，不需要前端传，也不需要 Controller 传
        Long currentUserId = UserContext.getUserId();
        
        // 执行业务逻辑...
        System.out.println("用户 " + currentUserId + " 正在创建订单...");
        
        // 调用库存模块等其他代码...
    }
}
```

---

### 4. ⚠️ 避坑指南：为什么必须调用 `remove()`？

在业务代码中使用 `ThreadLocal`​，​**最容易引发严重的生产事故就是后端内存泄露（Memory Leak）和数据串标**。

1. **底层原理：**  Tomcat 等 Web 服务器为了处理高并发，底层使用的是​**线程池**​。这意味着处理完当前请求的线程​**不会被销毁**，而是被放回线程池等待下一个请求复用。
2. **数据污染（串标）：**  如果你在请求结束时不调用 `remove()`​，下一个分配到这个线程的 HTTP 请求，就会读到上一个用户残留的 `ThreadLocal` 数据。这会导致极为严重的安全问题（比如用户 A 登录后，看到了用户 B 的信息）。
3. **内存泄露：**  `ThreadLocal`​ 底层依靠 `ThreadLocalMap`​ 存储，它的 Key 是弱引用，但 Value 是强引用。如果不手动 `remove()`，哪怕外层没有对这块数据的引用了，只要线程不死（线程池里的核心线程通常一直存活），Value 对象就永远无法被垃圾回收器（GC）回收，久而久之就会撑爆 JVM 内存。

**总结铁律：**  使用 `ThreadLocal`​ 必须是 `set()`​ 和 `remove()`​ 成对出现。配合 `try-finally`​ 块或者在 Spring 的 `afterCompletion`​ / AOP 的 `@After` 通知中执行清理操作，是标准的工程规范。

‍
