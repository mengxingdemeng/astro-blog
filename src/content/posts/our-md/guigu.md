硅谷银行业务管理系统 — 教学式完整教程

本教程是教学式的：每一节先讲要做什么、为什么这么做，再给参考代码。
建议你读完每一节的"设计"部分后，自己动手写，卡住了再看代码。

📚 Java 预备知识大节

如果你觉得某个小节已经知道了，可以跳过直接看后面。但建议至少扫一遍标题，确认没有不知道的东西。因为整个项目的代码依赖这些基础，缺了任何一个写起来都会卡壳。

预备-1：Java 是什么？为什么学 Java？

Java 是什么？

Java 是一门编程语言，由 Sun 公司（现在是 Oracle）在 1995 年推出。它的设计理念是"一次编写，到处运行"——你在 Windows 上写的 Java 代码，放到 Mac 或 Linux 上不用改就能跑。

Java 应用在世界的每一个角落：

安卓手机：App 主要用 Java 写的

企业后端：银行、电商、物流系统大量用 Java（你现在学的这个银行系统就是典型的企业后端场景）

大数据：Hadoop、Spark 都是 Java 写的

物联网：很多嵌入式设备也跑 Java

为什么选 Java 来学？

语法严谨——不像 Python 那样随便缩进，Java 强制你规范写代码，初学者不容易学歪

生态成熟——25 年了，教程、问题、库应有尽有，遇到问题 Google 基本能找到答案

岗位多——全球范围内 Java 开发者需求量很大

这门课的目标——学完这个银行项目，你就掌握了面向对象编程的核心技能，换任何语言（Python/C#/Go）都能快速上手

它不是完美的

Java 也有缺点：启动慢、语法啰嗦（写个简单的事要打很多字）。但对于入门来说，这些"啰嗦"反而是好事——让你明确知道自己在写什么。

预备-2：JDK、JRE、JVM 三位一体

这三个东西是学 Java 的第一道坎，必须搞清楚。

图解关系

你写的 .java 文件
    │
    │  javac（编译器）
    ▼
.class 字节码文件
    │
    │  java（JVM 启动器）
    ▼
JVM（Java 虚拟机，真正执行你的代码的东西）
    ▲
    │ 里面有
    │
JRE（Java 运行环境，= JVM + 基础类库）
    ▲
    │ 再加
    │
JDK（Java 开发工具包，= JRE + 编译器 javac + 各种工具）

一句话总结

缩写 全称 一句话说明 JVM Java Virtual Machine 真正跑 .class 的虚拟机 JRE Java Runtime Environment 跑 Java 程序需要的所有东西 = JVM + 基础类库 JDK Java Development Kit 开发 Java 程序需要的所有东西 = JRE + javac 编译器 + 各种工具

为什么要分这三层？

你如果只是运行别人写好的 .class 文件，装 JRE 就够了

你如果要自己开发 Java 程序，必须装 JDK

我们这个项目是开发项目，所以你需要的是 JDK

安装后验证

装完 JDK 后打开终端，输入这两条：

javac -version
java -version

应该能看到版本号，比如：

javac 11.0.22
openjdk version "11.0.22" 2023-07-18

看到版本号就装好了。没看到的话说明环境变量没配好。

javac → java 完整流程

这是每次运行 Java 程序都在做的事，必须理解：

你的电脑
├── HelloWorld.java     ← 你用编辑器写的源码文件（文本文件）
│
│ 第一步：javac 编译
│ javac HelloWorld.java
│
├── HelloWorld.class    ← 编译器生成的字节码文件（二进制，机器可读）
│
│ 第二步：java 运行
│ java HelloWorld        ← 注意：这里不加 .class 后缀！
│
│
JVM 启动 → 加载 HelloWorld.class → 找到 main 方法 → 执行代码 → 程序结束

为什么要编译？ .java 是人类写的文本，CPU 看不懂。javac 把它翻译成 JVM 能懂的 .class 字节码。JVM 是所有平台都有的（Windows/Mac/Linux 各有自己的 JVM），所以同一份 .class 到处都能跑——这就是"一次编写，到处运行"。

预备-3：IntelliJ IDEA：你的开发神器

为什么用 IDEA？

纯文本编辑器 + 命令行 javac 也能写 Java，但 IDEA 能帮你：

自动编译：写好代码按 Ctrl+F9 就编译了，不用敲 javac

智能提示：写 String. 时自动弹出 .length()、.format() 等方法

一键运行：绿色三角按钮 ▶️，点一下就运行

报错红线：写错了立刻用波浪线标红，鼠标悬停告诉你哪里错了

下载和安装

去 www.jetbrains.com/idea/download 下载 Community 版本（免费）

安装时一路点 Next 就行

第一次启动会让你选主题（Darcula 深色或 Light 浅色，随便选）

然后就可以开始建项目了

新建第一个 Java 项目

File → New → Project
    ├─ 左侧选 "Java"
    ├─ Project SDK 选你装的 JDK
    ├─ Project name: 随便起（比如 bank-project01）
    ├─ 位置自己选（比如桌面的某个文件夹）
    └─ 点 "Create"

建好后 IDEA 会生成一个标准的 Java 项目结构：

bank-project01/
├── src/              ← 所有 .java 文件都放这里
└── out/              ← IDEA 自动编译的 .class 输出到这里（不用你管）

IDEA 界面速览

┌─────────────────────────────────────────────────────────┐
│ 菜单栏（File, Edit, View, Code, ...）                     │
├────────┬────────────────────────────────────────────────┤
│        │                                                │
│ 项目   │              代码编辑区（最大的区域）            │
│ 结构   │                                                │
│ 树     │                                                │
│        │  你写 HelloWorld.java 的地方                     │
│ src/   │                                                │
│ └── 包 │                                                │
│        │                                                │
├────────┴────────────────────────────────────────────────┤
│ 底部：Terminal、Run、Debug、Problems 等                  │
└─────────────────────────────────────────────────────────┘

左侧的项目树里，src 文件夹就是你的代码根目录。右键 src → New → Package 就可以新建包了。

预备-4：HelloWorld + main 方法

写第一个程序

右键 src → New → Class，类名填 HelloWorld，回车。IDEA 自动生成：

public class HelloWorld {
}

在花括号中间写：

public static void main(String[] args) {
    System.out.println("Hello, Java！");
}

main 方法是什么？

main 方法是程序的入口——JVM 运行你的程序时，会找这个方法然后从这里开始执行。

public static void main(String[] args) {

这个签名是固定的，一个字母都不能改（大小写也不行）：

public：公共的，JVM 能访问到

static：静态的，JVM 不用 new 对象就能直接调

void：不返回值

main：方法名，JVM 认这个名字

String[] args：命令行参数数组，暂时不用管

System.out.println 是什么？

这是 Java 里向控制台打印一行文字的标准写法：

System：JDK 自带的一个类，不用 import

out：System 里的一个静态字段，代表"标准输出"（就是你的控制台）

println：out 的一个方法，"print line"——打印一行后自动换行

运行它

写好代码后：

找到 main 方法左边的绿色小三角 ▶️

点一下 → 选 Run 'HelloWorld.main()'

底部 Run 窗口出现 Hello, Java！

✅ 恭喜你，Java 入门第一步成功！

预备-5：package 和 import 全解

这两个概念贯穿整个项目，必须彻底搞懂。

package 是什么？

package 就是文件夹。 仅此而已。

你的源码放在 src/com/bank/domain/Account.java，那它的 package 就是 com.bank.domain——对应文件系统的路径 com/bank/domain/。

文件的第一行必须写 package 声明：

package com.bank.domain;  // ← 必须在所有 import 和 class 之前

为什么要用包？

避免重名——可能有两个都叫 Account 的类，一个在 com.bank.domain 包，一个在 com.stock.domain 包，不冲突

组织代码——按功能分文件夹，找代码方便

域名倒写——业界惯例用 com.公司名.项目名，比如阿里是 com.alibaba，谷歌是 com.google

import 是什么？

import 就是"我要用这个类，先把它引进来"。

Java 里的类互相独立。如果你要在 Account 类里用 OverdraftException 类，必须：

// 方式 1：显式导入（推荐）
import com.bank.exception.OverdraftException;

// 方式 2：通配符导入（导入整个包）
import com.bank.exception.*;

// 方式 3：不写 import，每次用全限定名（最啰嗦）
throw new com.bank.exception.OverdraftException("余额不足", 50.0);

怎么知道什么时候要 import？

看 IDEA 报不报错。 如果你在代码里写了 OverdraftException 但没 import，IDEA 会用红色波浪线标出来，按 Alt + Enter（或点击红色灯泡 💡）→ Import class，IDEA 自动帮你加上。

永远不用 import 的类

以下类在 java.lang 包里，Java 自动导入，你不用写：

String、System、Math

Exception、IllegalArgumentException、RuntimeException

Integer、Double、Boolean

Object（所有类的父类）

预备-6：变量、字段、常量、final

这几个东西教程里到处在用，但没系统讲过。

四种东西的区别

名称 在哪定义 作用范围 举例 局部变量 方法内部 只在那个方法里活 String name = scanner.nextLine(); 字段（成员变量） 类内部、方法外部 整个对象都能访问 private double balance; 静态字段 类内部、加 static 整个类共享一份 private static final Bank INSTANCE; 常量（final） 加 final 的字段 赋值后不能改 private final double interestRate;

final 到底是什么？

final 是 Java 里的"锁死"关键字，意思是"这个东西只能赋值一次"：

final int x = 10;   // 基本类型：值锁死了
// x = 20;          // ❌ 编译报错！不能再改

Customer jane = new Customer("Jane", "Simms");
Customer maria = new Customer("Maria", "Soley");

final Customer person = jane;   // 引用类型：引用锁死了，不能再指别的对象
// person = maria;              // ❌ 编译报错！不能换指向
person.addAccount(...);         // ✅ 但对象内部的内容可以改！final 只锁引用，不锁内容

关键理解：final 锁的是"引用"，不是"对象"。 就像你写死了"这个人永远是 Jane"，但 Jane 自己可以改发型、加账户。

final 用在哪？

修饰对象 效果 什么时候用 基本类型变量 值不能改 不希望被误改的数值，比如利率 引用类型变量 引用不能改 比如 BankService 里 private final Bank bank = Bank.getInstance();——不希望有人把 bank 换成另一个实例 类 不能被继承 CurrencyFormatter——工具类不希望有人继承改行为 方法 不能被重写 一般少用，除非你确定子类不应该改这个方法

为什么 final 字段不能不赋值？

普通字段（比如 private double balance;）如果不赋值，Java 会给默认值（double 是 0，String 是 null）。但 final 字段必须显式赋值——要么在声明时给，要么在构造器里给：

// 方式 1：声明时赋值（最简单）
private final String firstName;
private final List<Account></account> accounts = new ArrayList<>();

// 方式 2：构造器里赋值（也可以）
public Customer(String firstName, String lastName) {
    this.firstName = firstName;  // ✅ 构造器里赋值也行
}

预备-7：访问修饰符（四种）

教程里到处有 public、private、protected，搞不清它们会写不出代码。

四种修饰符一览

修饰符 意思 同一个类 同一个包 子类 不同包 public 公开的 ✅ ✅ ✅ ✅ protected 受保护的 ✅ ✅ ✅ ❌ 默认（不写） 包级私有 ✅ ✅ ❌ ❌ private 私有的 ✅ ❌ ❌ ❌

用一句话记：

public    → 全世界都能看
protected → 只有自己家和亲戚家能看
默认      → 只有自己小区能看
private   → 只有自己家能看

项目里的实际例子

为什么 Account.balance 用 protected 不用 private？

// Account.java
protected double balance;   // 用 protected

// 如果写成 private：
// 子类 CheckingAccount 就碰不到 balance 了
// 但 CheckingAccount.withdraw 里需要写 balance -= amount;
// 写成 protected，子类就能访问了

为什么 Bank 构造器用 private？

// Bank.java
private Bank() {    // 私有构造器
}

// 如果写成 public：
// 任何人都能 new Bank()，就破坏单例了
// 写成 private，只有 Bank 自己的代码里能调用这个构造器

为什么 CurrencyFormatter 用 final + private 构造器？

// CurrencyFormatter.java
public final class CurrencyFormatter {   // final：不能继承
    private CurrencyFormatter() {         // 私有：不能实例化
    }
    public static String format(...) {    // 只有 static 方法，类名直接调
    }
}

这是工具类的三件套设计规范：final 类 + 私有构造器 + 全 static 方法。

什么时候用哪个？

字段：默认都用 private（最高安全性），除非子类需要用才改成 protected

方法：给外部调用的用 public，只在类内部用的用 private

构造器：普通类 public，单例 private，工具类 private

预备-8：static 关键字

static 是什么？

简单说：static 是"属于类，不属于对象"的东西。

public class Bank {

    // 非 static：每个 Bank 对象各自一份（实例字段）
    private List<Customer></customer> customers = new ArrayList<>();

    // static：所有 Bank 对象共享一份（静态字段）
    private static final Bank INSTANCE = new Bank();

    // 非 static 方法：必须 new 出 Bank 对象才能调
    public void addCustomer(...) { }

    // static 方法：不用 new，类名直接调
    public static Bank getInstance() { return INSTANCE; }
}

用内存图来理解

非 static 字段：
Bank 单例对象（堆内存里）
├── customers: ArrayList    ← 只有这一份
├── INSTANCE: Bank 实例     ← static，放在方法区/元空间
│
static 字段/方法：
class Bank 的定义
├── static INSTANCE: Bank   ← 跟类绑在一起，跟对象数量无关
├── static getInstance()

static 方法的限制

static 方法不能直接访问非 static 字段/方法！

public class Bank {
    private List<Customer></customer> customers = new ArrayList<>();

    public static Bank getInstance() {
        // customers.add(...)     // ❌ 编译报错！static 方法碰不到实例字段
        // 为什么？因为 static 方法调的时候还没对象呢，customers 存在哪个对象上都不知道
        return INSTANCE;
    }
}

项目里的 static 用法

位置 static 用法 为什么这么设计 Bank.INSTANCE private static final 字段 单例的唯一实例，跟类绑定，全局共享 Bank.getInstance() public static 方法 类名直接调，不用 new Bank CurrencyFormatter.format() public static 方法 工具类的方法，类名直接调 CurrencyFormatter.CNY private static final 字段 NumberFormat 只创建一次，缓存起来 main() public static void JVM 不用 new 对象就能调

预备-9：类、对象、new、构造器

类和对象的关系

这是面向对象编程的核心概念：

类 = 设计图/模板。比如说"手机"这个概念就是一个类。

对象 = 具体实例。你手里的那台 iPhone 15 就是"手机"类的一个对象。

// 这是一个类（设计图）
public class Customer {
    private String firstName;
    private String lastName;
}

// 这是对象（具体实例）
Customer jane = new Customer("Jane", "Simms");  // jane 是一个 Customer 对象
Customer maria = new Customer("Maria", "Soley"); // maria 是另一个 Customer 对象
// jane 和 maria 各自有独立的 firstName/lastName，互不干扰

new 到底做了什么？

new Customer(...) 执行时，Java 干了三件事：

Customer jane = new Customer("Jane", "Simms");

1. 在堆内存里开一块新空间       ← 给新对象分配内存
2. 调 Customer 的构造器           ← 执行构造器代码（比如赋值 firstName）
3. 返回对象的引用（内存地址）       ← 赋值给 jane 变量

构造器是什么？

构造器是专门用来初始化新对象的特殊方法：

public class Customer {
    private String firstName;
    private String lastName;

    // 这就是构造器
    // 名字和类名完全一样！没有返回类型（连 void 都不能写！）
    public Customer(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}

this 关键字

this = "当前这个对象自己"。

public Customer(String firstName, String lastName) {
    // this.firstName 是"当前对象的 firstName 字段"
    // = 右边的 firstName 是"方法参数传进来的 firstName"
    this.firstName = firstName;

    // 如果不重名可以省：
    // this.xxx = xxx   ← 参数名和字段名相同时用 this.区分
}

默认构造器

如果你不写任何构造器，Java 会偷偷给你生成一个无参构造器：

public class Customer {
    // 你没写任何构造器
    // Java 自动生成：
    // public Customer() {}    ← 无参构造器，啥也不干
}

但一旦你自己写了任何一个构造器，Java 就不再自动生成无参构造器了。 这时候如果别人写 new Customer()（无参）就会编译报错。

预备-10：方法

方法签名五要素

public static int calculateMonthlyInterest(double balance, double rate)
│    │       │     │                  │            │
│    │       │     │                  │            └─ 参数列表
│    │       │     │                  └─ 方法名
│    │       │     └─ 返回类型（void 表示不返回）
│    │       └─ 修饰符（可选：static/final/synchronized...）
│    └─ 返回值类型
└─ 访问修饰符（public/private/protected）

参数传递：值传递 vs 引用传递

Java 只有值传递，没有引用传递。 初学者最容易搞混的点：

// 情况 1：基本类型（int/double/boolean...）
void tryChange(int x) {
    x = 999;   // 改的是副本
}

int a = 10;
tryChange(a);
System.out.println(a);  // 还是 10！方法里的修改不影响外面

// 情况 2：引用类型（String/Customer/List...）
void changeCustomerName(Customer c) {
    c.firstName = "NewName";   // 改的是对象内部的内容，外面能看到！
    c = new Customer("Other", "Person");  // 但如果让 c 指向新对象，外面看不到
}

核心区别：

基本类型传的是值的副本——方法里怎么改副本，外面的原值不变

引用类型传的是内存地址的副本——方法里通过地址改对象内容，外面能看到；但如果让副本指向新对象，外面看不到

方法重载（overload）

同一个方法名，不同的参数列表——Java 能根据你传什么参数来调对应的版本：

// CheckingAccount 里有两个 openCheckingAccount
public CheckingAccount openCheckingAccount(Customer c, double balance) {
    return openCheckingAccount(c, balance, 0);  // 调下面那个
}

public CheckingAccount openCheckingAccount(Customer c, double balance, double limit) {
    CheckingAccount a = new CheckingAccount(balance, limit);
    c.addAccount(a);
    return a;
}

// 调的时候：
service.openCheckingAccount(customer, 200);           // 自动调两参数版本
service.openCheckingAccount(customer, 200, 500);       // 自动调三参数版本

重载的规则：

方法名必须相同

参数列表必须不同（类型不同、数量不同、顺序不同都行）

返回类型可以相同也可以不同（但返回类型不同不能单独构成重载——参数必须也不同）

方法重写（override） vs 重载（overload）

这俩太容易搞混了，用表格区分：

对比项 重写 override 重载 overload 发生在哪 子类重写父类的方法 同一个类里（或父子类）多个方法同名 方法名 必须相同 必须相同 参数列表 必须相同 必须不同 返回类型 必须相同（或子类） 可以不同 修饰符 访问权限不能更小 可以不同 抛出异常 不能更大 可以不同 绑定时机 运行时（多态的核心） 编译时（根据参数选） 加 @Override ✅ 建议加（帮你检查是不是重写） ❌ 不加 项目里的例子 CheckingAccount.withdraw 重写 Account.withdraw openCheckingAccount 两个版本

预备-11：异常完整图解

异常是什么？

异常就是"程序运行时出了问题"。 比如：

你要取 200 块，但余额只有 100 → 余额不足

你要打开一个文件，但文件不存在 → 文件找不到

你要把 "abc" 转成整数 → 数字格式不对

Java 用异常对象来表示这些"出问题了"的情况。异常对象里包含了：

出了什么错（message）

错在哪（堆栈 trace）

额外信息（比如我们 OverdraftException 里的 deficit）

完整异常体系图

Throwable（所有可抛出对象的根类）
│
├── Error（严重错误，程序不该处理）
│   ├── OutOfMemoryError   内存溢出
│   ├── StackOverflowError 栈溢出
│   └── ...
│
└── Exception（所有异常，应该处理）
    │
    ├── RuntimeException（运行时异常，可选处理）
    │   ├── NullPointerException       空指针（最常见的 Bug）
    │   ├── IndexOutOfBoundsException  索引越界
    │   ├── IllegalArgumentException   参数不合法
    │   ├── ClassCastException         类型转换错误
    │   ├── NumberFormatException      "abc" 转整数失败
    │   └── ...
    │
    └── 其他 Exception（checked exception，必须处理）
        ├── IOException
        ├── SQLException
        ├── FileNotFoundException
        └── OverdraftException（我们自己写的！）

Error vs Exception

对比项 Error Exception 含义 JVM 或系统级别的严重错误 程序逻辑上的问题 例子 内存溢出、栈溢出 余额不足、文件找不到 要不要处理？ 不处理，说明你的程序有大问题 要处理，catch 住或 throws 出去 继承自 Throwable Throwable

RuntimeException vs Checked Exception

对比项 RuntimeException（unchecked） 其他 Exception（checked） 编译器强制处理吗？ 不强制 强制！不处理编译报错 什么时候检查？ 运行时才检查 编译时就检查 常见场景 编程错误（空指针、参数错） 可预期但无法避免的情况（网络断开、余额不足） 例子 NullPointerException IOException、OverdraftException

throw 和 throws

throw = "我现在就要抛出一个异常！"（动词，在方法体里用）
throws = "我这个方法可能会抛异常，调用者要处理"（名词，在方法签名上用）

// throws 声明：告诉调用方"我可能会抛这个异常"
public void withdraw(double amount) throws OverdraftException {
    if (balance < amount) {
        // throw 执行：真的抛出一个异常对象
        throw new OverdraftException("余额不足", amount - balance);
    }
    balance -= amount;
}

try-catch-finally 完整语法

try {
    // 正常的业务代码
    // 如果这里 throw 了异常，就跳到 catch 块
} catch (OverdraftException e) {
    // 捕获异常，e 就是那个异常对象
    System.out.println("出错了：" + e.getMessage());
    System.out.println("堆栈：");
    e.printStackTrace();  // 打印完整的错误堆栈
} catch (Exception e) {
    // 可以有多个 catch，按从上到下的顺序匹配
    // 先 catch 子类，再 catch 父类
} finally {
    // 无论是否异常，finally 都会执行（只有 System.exit(0) 能跳过）
    // 用来释放资源（关闭文件、关闭数据库连接等）
}

异常堆栈怎么读？

当你的程序炸了，控制台会输出一堆 at ...，这就是堆栈追踪。教你怎么读：

Exception in thread "main" com.bank.exception.OverdraftException: 余额不足
                                                        │            │
                                                        │            └── 这行是你给的 message
                                                        └── 异常类型
    at com.bank.domain.Account.withdraw(Account.java:30)     ← 异常在这里被 throw 的
    at com.bank.domain.CheckingAccount.withdraw(CheckingAccount.java:40) ← 上一层
    at com.bank.service.BankService.withdraw(BankService.java:75) ← 上一层
    at com.bank.ui.BankUI.demo(BankUI.java:120)              ← 上一层
    at com.bank.ui.Main.main(Main.java:5)                    ← 最顶层（入口）

读堆栈的方法：

先看第一行——什么异常、message 是什么

再看最上面的 at ...——异常在哪一行被 throw 的（Account.java:30）

往下看调用链，理解异常是怎么一路传到 main 的

最上面的那个文件通常就是你要改的地方

为什么我们的 OverdraftException 选 checked 而不是 unchecked？

教程第 1 节已经提了，但再强调一下：

余额不足不是"程序 Bug"（比如空指针那是编程错误），而是业务上可预期的情况。调用方必须处理——要么 catch 住给用户友好提示，要么继续 throws 往上抛。用 checked exception（继承 Exception）就能强制编译器帮你检查：你忘了处理就编译不过。

预备-12：继承完整讲解

什么是继承？

继承就是"子类自动得到父类的东西"。 好比儿子会继承父亲的姓氏、血型，还可以有自己的新特征。

// 父类：Account
public abstract class Account {
    protected double balance;
    public void deposit(double amount) { ... }
    public void withdraw(double amount) throws OverdraftException { ... }
}

// 子类：SavingAccount
public class SavingAccount extends Account {
    private double interestRate;   // 子类自己的新字段
    // 自动继承了 balance、deposit()、withdraw()

    public double calculateMonthlyInterest() { ... }  // 子类自己的新方法
}

继承得到什么、继承不到什么

父类成员 子类能继承吗？ 说明 public 字段/方法 ✅ 能 公开的都能拿到 protected 字段/方法 ✅ 能 受保护的给子类留了门 默认（包级）字段/方法 ✅ 能（同包时） 只有同一个包下的子类能拿到 private 字段/方法 ❌ 不能 子类完全看不到 构造器 ❌ 不能 但子类构造器必须调父类构造器

super 关键字

super = "父类"。和 this（当前对象）对比：

// 子类构造器里
public SavingAccount(double initialBalance, double interestRate) {
    super(initialBalance);   // ← 必须在第一行！调父类 Account 的构造器
    // 如果不写 super(...)，Java 自动调 super()（无参的）
    // 但 Account 没有无参构造器，所以必须显式写 super(initialBalance)

    this.interestRate = interestRate;
}

重写父类方法

子类可以重写父类已有的方法：

// 父类 Account 的 withdraw：只允许余额够的时候取
public void withdraw(double amount) throws OverdraftException {
    if (balance >= amount) {
        balance -= amount;
    } else {
        throw new OverdraftException("余额不足", amount - balance);
    }
}

// 子类 CheckingAccount 重写了 withdraw：允许透支
@Override
public void withdraw(double amount) throws OverdraftException {
    double available = balance + getAvailableCredit();   // 不同的判断逻辑
    if (available >= amount) {
        balance -= amount;
    } else {
        throw new OverdraftException(...);
    }
}

@Override 注解

@Override 是一个编译器检查——它告诉 IDEA 和 javac："我确定下面这个方法是重写父类的，如果不是就报错！"

@Override   // ✅ 加这个是好习惯，帮你检查拼写、参数、返回类型是否正确
public String getAccountType() {
    return "储蓄账户";
}

如果你把方法名拼错了（比如写成 getAccounttype 小写 t），加了 @Override 就会立刻标红报错，提醒你"你没重写任何方法"。如果不加，编译器以为你在子类里新写了一个方法，悄悄放过这个 Bug。

预备-13：多态完整讲解

多态是面向对象编程的最高级特性，也是初学者最晕的。但它就一句话：同一个引用类型，指向不同子类对象，调用同一个方法，行为不同。

用项目里的例子

Account a = new SavingAccount(500, 0.03);   // 父类引用 → 子类对象
Account b = new CheckingAccount(200, 500);  // 另一个子类对象

a.withdraw(100);   // 调的是 SavingAccount 继承来的 withdraw（不支持透支）
b.withdraw(100);   // 调的是 CheckingAccount 重写的 withdraw（支持透支）

// 同一个 .withdraw(100)，不同对象，行为不同 → 这就是多态！

多态的前提条件

继承：子类必须 extends 父类

重写：子类重写了父类的方法

向上转型：父类引用指向子类对象

向上转型和向下转型

// 向上转型（自动的，不用写什么）
Account a = new SavingAccount(500, 0.03);
// 就像 "这是一辆车（Account），具体是一辆宝马（SavingAccount）"
// 父类类型的引用可以指向子类类型的对象

// 向下转型（需要显式强转 + instanceof 检查）
Account a = ...;  // a 可能是 SavingAccount，也可能是 CheckingAccount
if (a instanceof SavingAccount) {         // 先检查"a 真的是 SavingAccount 吗？"
    SavingAccount sa = (SavingAccount) a;  // 安全地强转
    double rate = sa.getInterestRate();    // 现在可以调 SavingAccount 独有的方法了
}

为什么我们需要 instanceof + 强转？

父类 Account 上没有 getInterestRate() 方法（因为支票账户没有利率）。所以：

Account a = new SavingAccount(500, 0.03);

// a.getInterestRate();   ❌ 编译报错！Account 上没这个方法

// 必须这样：
if (a instanceof SavingAccount) {
    SavingAccount sa = (SavingAccount) a;
    sa.getInterestRate();   // ✅ 先强转再调
}

多态的好处

看 BankService.applyMonthlyInterest() 就清楚了：

public void applyMonthlyInterest() {
    for (Customer customer : bank.getCustomers()) {
        for (Account account : customer.getAccounts()) {  // ← 这里用多态！
            if (account instanceof SavingAccount) {
                ((SavingAccount) account).applyMonthlyInterest();
            }
            // CheckingAccount 什么都不用做，自动跳过
        }
    }
}

如果没有多态（用数组存，每个账户类型分开写），就得：

// 没有多态的写法（很笨）
for (Account acc : allAccounts) {
    if (acc instanceof SavingAccount) { ... }
    else if (acc instanceof CheckingAccount) { ... }
    else if (acc instanceof CreditCardAccount) { ... }
    // 每加一种新账户类型，都要来改这里
}

多态让你的代码对扩展开放，对修改关闭——加新的账户类型时，只需写新的子类，不用改遍历逻辑。

预备-14：抽象类完整讲解

什么是抽象类？

抽象类就是"还没完全想好的类"——它定义了子类必须有的骨架，但具体实现留给子类。

public abstract class Account {   // 加 abstract 关键字
    protected double balance;

    // 有实现的方法：所有账户都通用
    public double getBalance() { return balance; }

    // 抽象方法：只有签名，没有实现。子类必须重写！
    public abstract String getAccountType();

    // 也可以有非抽象方法
    public void deposit(double amount) { balance += amount; }
}

核心特点

不能 new：Account a = new Account(100); ❌ 编译报错

可以有抽象方法（没有方法体，只有签名）

也可以有普通方法（有完整实现）

子类必须重写所有抽象方法（除非子类也是 abstract）

Account 为什么是抽象类？

因为现实中不存在"没有类型"的账户——你去银行开户，要么开储蓄要么开支票。单独的 Account 没有任何实际意义，它只是一个"所有账户的父类模板"。

抽象类 vs 接口（对比表）

对比项 抽象类 接口 关键字 abstract class interface 能有构造器吗？ ✅ 能 ❌ 不能 能有字段吗？ ✅ 任意字段 ❌ 只能是 public static final 常量 能有实现方法吗？ ✅ 能（普通方法和抽象方法都可以） Java 8+ 能用 default 方法 子类能继承几个？ 只能 1 个（Java 单继承） 能实现多个 用途 "是一个"关系（SavingAccount 是 Account） "能做"关系（Runnable 能跑） 我们项目里 Account 无（这个项目没用到接口）

预备-15：集合框架入门

为什么不用数组？

Java 有数组，但数组有很多局限：

问题 数组 List（比如 ArrayList） 长度 固定！创建时就定死了，想加超过容量就麻烦 自动扩容，想加多少加多少 方法 只有 length 属性，没有 add/remove 等方法 有完整的 add/get/remove/size/iterator 等方法 安全 arr[-1] 运行时才报错 同样有 IndexOutOfBoundsException，但有更好的校验 泛型 数组不能很好地表达"里面装什么类型" List<Account></account> 明确告诉编译器"这个列表只装 Account"

List 和 ArrayList

// List 是接口（"能装东西的列表"）
// ArrayList 是实现类（"用数组实现的列表"）

List<Account></account> accounts = new ArrayList<>();  // ← 标准写法

accounts.add(savingAccount);     // 加一个
accounts.add(checkingAccount);   // 再加一个
int size = accounts.size();      // 长度：2
Account a = accounts.get(0);     // 取第一个：savingAccount

泛型 List

<Account></account> 叫泛型参数——它告诉编译器："这个 List 里只能放 Account 类型的对象。放别的编译报错。"

List<Account></account> accounts = new ArrayList<>();
accounts.add(new SavingAccount(500, 0.03));   // ✅ Account 的子类可以放
accounts.add(new CheckingAccount(200, 500));  // ✅ 另一个子类也可以
accounts.add("hello");                         // ❌ 编译报错！String 不是 Account
accounts.add(123);                             // ❌ 编译报错！

泛型让代码更安全——编译器帮你检查类型，错了立刻标红。

Collections.unmodifiableList 是什么？

在 Customer.getAccounts() 里：

public List<Account></account> getAccounts() {
    return Collections.unmodifiableList(accounts);
}

为什么不直接 return accounts;？因为外部拿到这个 List 后可以直接修改：

// 如果直接 return accounts：
Customer jane = ...;
jane.getAccounts().clear();  // 💀 把 Jane 所有账户都清空了！
// 或者：
jane.getAccounts().add(null);  // 💀 加进去一个 null，遍历的时候 NPE

Collections.unmodifiableList(accounts) 返回一个只读视图——能看、能遍历、能 get，但 add/remove/clear 都会抛 UnsupportedOperationException。

for-each 循环

这是 Java 5 引入的"增强 for 循环"，用来遍历集合最方便：

// 遍历 accounts 里的每个 Account
for (Account account : accounts) {
    System.out.println(account.getBalance());
}

// 等价于：
for (int i = 0; i < accounts.size(); i++) {
    Account account = accounts.get(i);
    System.out.println(account.getBalance());
}

for-each 更简洁，但有个限制：遍历过程中不能用 List 的 add/remove（会 ConcurrentModificationException）。如果要边遍历边删除，用 Iterator。

预备-16：单例模式详解

什么是单例？

单例 = 整个应用里只有一个实例。

我们的 Bank 类就是单例——整个程序只有一家银行。如果可以 new Bank() 出来两家银行，客户就被分到两家去了，报告也只打印其中一家的客户，就乱套了。

单例的三要素（饿汉式）

public class Bank {

    // 要素 1：私有构造器——外面不能 new Bank()
    private Bank() {
    }

    // 要素 2：私有 static final 实例——唯一的一份，类加载时就创建
    private static final Bank INSTANCE = new Bank();

    // 要素 3：公有 static 方法——外面通过这个方法拿实例
    public static Bank getInstance() {
        return INSTANCE;
    }
}

为什么叫"饿汉式"？

因为它很饿——类加载的时候就急着创建好了实例。不管你用不用，它都先 new 出来。

好处：线程安全（JVM 保证 static final 只初始化一次）。
坏处：类加载就创建，如果这个类很大、创建很耗时，会拖慢启动速度。

验证单例正确

Bank a = Bank.getInstance();
Bank b = Bank.getInstance();
System.out.println(a == b);     // true！a 和 b 是同一个对象
// Bank c = new Bank();         // 编译报错！构造器是 private

== 在这里比较的是引用（是不是同一个对象），不是值。true 说明 a 和 b 指向堆内存里的同一块地址。

预备-17：调试方法（必学！）

写代码不会一遍就对，调试是程序员每天做的事。学会调试比背 API 重要 10 倍。

IDEA 里打断点

在你怀疑有问题的那行代码左边，点一下：

public void withdraw(double amount) throws OverdraftException {
    if (balance >= amount) {
        balance -= amount;   // 在这里左边点一下，出现红点 ●
    }
}

Debug 运行

点右上角的红色小虫图标 🐛（或 Shift + F9），程序会在断点处暂停，等你发指令。

Debug 窗口的按钮

┌──────────────────────────────────────────┐
│ ▶ Resume    继续运行到下一个断点             │
│ ⏭ Step Over  执行当前行，不进入方法内部       │
│ 🎯 Step Into  进入方法内部（跟进调用链）      │
│ ⏮ Step Out   跳出当前方法                    │
│ 🖱 Drop Frame 回到上一层调用                 │
│ ⏹ Stop      停止调试                        │
└──────────────────────────────────────────┘

看变量值

程序暂停时：

底部 Variables 窗口：自动显示当前方法里所有变量的值

鼠标悬停：把鼠标移到变量名上，弹出小气泡显示当前值

Evaluate Expression（Alt+F8）：可以临时写表达式求值，比如 balance + getAvailableCredit()，看看是不是你预期的结果

用我们的项目举个例子

假设你想调试"取 147.62 会不会成功"：

在 BankUI.demonstrateTransactions() 里 service.withdraw(janeChecking, 147.62); 这行打断点

Debug 运行

程序停在断点，底部看到：janeChecking = CheckingAccount(balance=72.5, limit=500)、amount = 147.62

Step Into（F7）跟进 service.withdraw

继续 Step Into 跟进 account.withdraw

这时已经到 CheckingAccount.withdraw 里了，可以看 available = balance + getAvailableCredit() 到底算出来是多少

Evaluate Expression（Alt+F8）输入 147.62 - available，看看 deficit 是多少

一步步追踪，完全理解代码怎么跑的

学会调试，比看 100 行解释文字都管用。

好，Java 预备知识大节到此结束。现在让我继续扩充第 1-12 节，给每节加代码逐行解释、常见错误排查表。由于篇幅很长，我会分批来。先看下现在教程有多大了：

0.0 环境准备（必须先确认）

在开始之前，确保你的电脑上有：

1. JDK 8 或更高版本

打开终端（Windows 用 cmd/PowerShell，Mac/Linux 用 Terminal），输入：

java -version
javac -version

两行都能输出版本号（比如 javac 11.0.22）就说明 JDK 装好了。如果提示"找不到命令"，需要先去 Oracle 或 Adoptium 下载安装 JDK。

2. 一个编辑器/IDE

推荐用 IntelliJ IDEA（社区版免费），本教程默认你用 IDEA。当然用 VS Code 加 Java 插件也行，甚至纯文本编辑器 + 命令行 javac 都可以。

0.1 业务有什么？

一个银行系统，最少要有这些东西：

银行：全局只有一家，管理所有客户

客户：一个客户可以开多个账户

账户：分两种——储蓄账户（有利率）、支票账户（可透支）

异常：取钱不够了怎么办？抛个异常，告诉调用方"差多少钱"

报告：能打印出所有客户和他们的账户余额

0.2 代码分几层？

把代码按职责分成 5 层，每层只管自己的事：

ui        ← 入口和交互，比如 main 方法、打印报告
service   ← 业务流程，比如"开户流程"是 openCustomer 再 openAccount
domain    ← 数据模型，比如 Account 有 balance，Customer 有 firstName
exception ← 自定义异常
util      ← 工具类，比如把数字格式化成人民币 ￥500.00

为什么要分层？ 举个例子：domain 层只知道"Account 有 balance 属性，withdraw 方法做减法"，它不该知道报告怎么打印、界面怎么交互。分层就是为了不让代码搅成一团。

0.3 包名和目录

用域名倒写的方式定义包名（业界惯例），我们用 com.bank：

src/com/bank/
├── domain/
├── exception/
├── service/
├── util/
└── ui/

有两种方式建这些目录：

方式 A：IDEA 里右键新建（推荐）

打开 IDEA，File → New → Project，选 Java，名称随便起（比如 bank-project01），JDK 选你电脑上的版本

右键 src → New → Package，输入 com.bank.domain 回车

同样的方法依次建 com.bank.exception、com.bank.service、com.bank.util、com.bank.ui

IDEA 会自动帮你创建好 src/com/bank/... 的目录结构

方式 B：命令行 mkdir

⚠️ 下面的命令是 bash 语法（Mac/Linux/Git Bash）。Windows 原生 cmd 不支持花括号展开，需要手动写 5 行 mkdir。

# Mac/Linux 或 Windows Git Bash

mkdir -p src/com/bank/{domain,exception,service,util,ui}

# Windows cmd（每行单独执行）

mkdir src\com\bank\domain
mkdir src\com\bank\exception
mkdir src\com\bank\service
mkdir src\com\bank\util
mkdir src\com\bank\ui

0.4 章节顺序说明

章节顺序是按依赖关系排的——写后面的类时，前面的类必须已经存在：

第1节 OverdraftException  ← 最底层，谁都不依赖
第2节 Account             ← 依赖 OverdraftException
第3节 SavingAccount       ← 依赖 Account
第4节 CheckingAccount     ← 依赖 Account + OverdraftException
第5节 Customer            ← 依赖 Account
第6节 Bank                ← 依赖 Customer
第7节 CurrencyFormatter   ← 谁都不依赖，纯工具
第8节 BankService         ← 依赖 domain 全部 + util
第9节 BankUI + Main       ← 依赖全部

好，开始动手。

第 1 节：OverdraftException — 自定义透支异常

1.1 为什么先写异常？

因为后面 Account 类的 withdraw 方法要用到它。异常是基础设施，好比盖楼先打地基。

1.2 要写什么？

创建一个类 OverdraftException，放在 com.bank.exception 包下。

1.3 设计思考

问题 1：继承谁？

继承 java.lang.Exception。这样它就是一个 checked exception——编译器强制调用方必须 try-catch 或继续 throws，不会有人忘记处理"取款失败"的情况。

如果继承 RuntimeException，调用方可以不处理，容易出 bug。

问题 2：异常里要携带什么信息？

光说"余额不足"不够。调用方还想知道"还差多少钱"。所以除了 Exception 自带的 message，我们再加一个 deficit 字段。

问题 3：构造器怎么写？

两个参数：(String message, double deficit)。message 传给父类，deficit 存到自己的字段里。

问题 4：deficit 要不要 setter？

不要。异常对象创建后不该再改内容。字段设为 final，构造器里赋值，只给 getter。

1.4 动手写

文件位置：src/com/bank/exception/OverdraftException.java

先自己写，卡住了看下面的参考代码。要点回顾：

继承 Exception

一个 final double 字段 deficit

两参数构造器，super(message) 后赋值 deficit

一个 getter getDeficit()

1.5 参考代码

package com.bank.exception;

public class OverdraftException extends Exception {

    private final double deficit;

    public OverdraftException(String message, double deficit) {
        super(message);
        this.deficit = deficit;
    }

    public double getDeficit() {
        return deficit;
    }
}

1.6 写完怎么验证？

写个 main 方法测试一下：

public static void main(String[] args) {
    OverdraftException e = new OverdraftException("余额不足", 50.0);
    System.out.println(e.getMessage());   // 余额不足
    System.out.println(e.getDeficit());    // 50.0
}

打印正确就说明这个类没问题。

1.7 关键决策：为什么选 checked exception？（vs RuntimeException）

这是整个项目最重要的设计决策之一——我们的项目里同时存在两种异常体系，理解它们的分工是异常处理的核心：

OverdraftException（本节写的） IllegalArgumentException（JDK 自带） 继承谁 Exception（checked） RuntimeException（unchecked） 编译器强制处理 是——调用 withdraw 必须 try-catch 或 throws 否——编译器不管 在哪个类里 throw Account.withdraw、CheckingAccount.withdraw Account 构造器、deposit、withdraw 参数校验 谁来 catch BankService.withdraw 内部 catch，转 boolean 返回 BankUI.deposit/withdraw 里 catch，打印友好提示 设计意图 "取款不够是业务逻辑的正常分支，不是 bug，调用方必须处理" "参数非法是调用方的 bug，应该尽早暴露"

选 checked 的理由：OverdraftException 是"余额不足"——这不是程序写挂了，而是"用户取钱超出了余额"的正常业务场景。如果 unchecked，调用方（比如某个新程序员写的新 BankService 方法）可能忘了处理，用户就会看到程序崩溃。checked 强制"谁调 withdraw 谁必须考虑不够的情况"。

选 unchecked 的理由：IllegalArgumentException 是"调用方传了负数"——这就是 bug！不该发生。checked 会污染每个调用方，unchecked 让坏代码"早炸早暴露"。

项目里的完整分工表：

throw 语句的三种出现位置：

① 构造器里参数校验 → unchecked（IllegalArgumentException）
   Account(double initialBalance)    → if (initialBalance < 0) throw new IllegalArgumentException(...)
   CheckingAccount 构造器            → if (overdraftLimit < 0) throw new IllegalArgumentException(...)
   Customer 构造器                   → if (firstName.isEmpty()) throw new IllegalArgumentException(...)
   Bank.getCustomer(int index)       → if (index越界) throw new IndexOutOfBoundsException(...)

② 业务方法里参数校验 → unchecked
   Account.deposit(double amount)    → if (amount <= 0) throw new IllegalArgumentException(...)
   Account.withdraw(double amount)   → if (amount <= 0) throw new IllegalArgumentException(...)
   CheckingAccount.withdraw 重写版    → if (amount <= 0) throw new IllegalArgumentException(...)

③ 业务方法里业务逻辑不满足 → checked（OverdraftException）
   Account.withdraw                  → if (余额不够) throw new OverdraftException(...)
   CheckingAccount.withdraw          → if (可用额度不够) throw new OverdraftException(...)

记住这个原则：

参数校验一律 unchecked（早失败、早暴露）

业务逻辑分支（余额不足）一律 checked（强制调用方处理）

1.8 throw 和 throws：两种抛出方式

很多初学者搞混这两个关键字，它们不是二选一，是配合关系：

// throws —— 方法签名上声明"我可能抛这个异常"
public void withdraw(double amount) throws OverdraftException {

    // throw —— 方法体内真的创建并抛出异常对象
    if (balance < amount) {
        throw new OverdraftException("余额不足", amount - balance);
    }
}

项目里哪些地方有 throws？

方法 声明了 throws 什么 为什么 Account.withdraw throws OverdraftException checked exception 必须声明 CheckingAccount.withdraw throws OverdraftException 重写父类方法，throws 声明可以一样（不能更严格）

项目里哪些地方只有 throw 没有 throws？

所有 throw IllegalArgumentException 的地方——因为它是 unchecked，编译器不强制声明。你可以写，但写了也没用：

// ❌ 没必要写
public void deposit(double amount) throws IllegalArgumentException {
    if (amount <= 0) throw new IllegalArgumentException("...");
}
// ✅ 直接 throw 就行，编译器不要求 throws 声明

1.9 自定义异常最佳实践 Checklist

写一个异常类时，按这个清单检查：

继承 Exception 还是 RuntimeException？（看 1.7 的决策表）

需要额外字段吗？（我们加了 deficit）

字段用 final 吗？（必须！异常不可变）

构造器调 super(message) 吗？（必须！message 存在父类 Throwable 里）

要不要传 cause？（可选，链式异常时有用，后面讲）

只给 getter 不给 setter？（必须！异常对象不该被修改）

拓展变体：如果我们想让 OverdraftException 也 unchecked，只需改一行：

// 改前（本项目用的）
public class OverdraftException extends Exception {

// 改后（unchecked）
public class OverdraftException extends RuntimeException {

改了之后，Account.withdraw 就可以删掉 throws OverdraftException 声明，BankService.withdraw 里的 try-catch 也可以删掉。但代价是：以后有人写代码调 account.withdraw() 忘了处理，编译器不会提醒，用户可能看到程序崩溃。本项目选 checked 是为了教学目的——让你看到完整的异常处理链路。

1.10 拓展：还能加什么？

拓展 1：加 cause（链式异常）

假设我们的 OverdraftException 是"因为底层数据库连接断了"间接导致的，可以这样写：

// 构造器重载，支持 cause
public OverdraftException(String message, double deficit, Throwable cause) {
    super(message, cause);    // Throwable 有 (message, cause) 构造器
    this.deficit = deficit;
}

这样异常链里既有我们的业务信息（差多少），又有底层原因。

拓展 2：加错误码

真实项目里异常通常带错误码：

public class OverdraftException extends Exception {
    private final String errorCode;   // 比如 "BANK_OVERDRAFT_001"
    private final double deficit;
    // ...
}

调用方可以根据错误码做不同处理，而不是靠 message 字符串匹配。

拓展 3：重写 toString 而非只靠 getMessage

@Override
public String toString() {
    return "OverdraftException[message=" + getMessage() + ", deficit=" + deficit + "]";
}

打印堆栈时信息更完整。

第 2 节：Account — 账户抽象基类

2.1 设计思考

问题 1：Account 应该是抽象类吗？

应该。现实中不存在"没有类型"的账户——你去银行开户，要么开储蓄要么开支票。Account 只负责定义"所有账户都有的共性"（余额、存钱、取钱），具体行为由子类实现。

问题 2：balance 属性用什么访问修饰符？

用 protected。因为子类（CheckingAccount）在透支时需要直接修改 balance。如果用 private，子类碰不到，只能调用父类的 withdraw，但父类的 withdraw 不支持透支逻辑。

问题 3：构造器要做参数校验吗？

要。如果有人 new Account(-100)，不应该让它悄悄通过。防御性编程，坏数据越早发现越好。构造器里如果 initialBalance < 0，直接 throw new IllegalArgumentException。

什么是 IllegalArgumentException？ 这是 JDK 自带的一个异常类（在 java.lang 包里，不用 import），意思就是"调用这个方法时传的参数不合法"。它是 RuntimeException 的子类，属于 unchecked exception——编译器不强制你 try-catch，运行时如果参数错了才会炸。用来做参数校验非常方便，Java 标准库里到处都在用它。

问题 4：deposit 和 withdraw 都要参数校验吗？

要。金额 <= 0 没有意义。

问题 5：deposit 要不要抛 OverdraftException？

不要。存款只会让余额变多，不可能"透支"。所以 deposit 不声明 throws。

问题 6：withdraw 抛什么异常？

抛 OverdraftException。余额够就减钱，不够就 throw，带上 message 和 deficit（差多少）。

问题 7：要给子类留什么抽象方法？

加一个 getAccountType() 抽象方法。不同的账户返回不同的类型名，多态调用时就能知道"这是个储蓄还是支票"。

2.2 动手写

文件位置：src/com/bank/domain/Account.java

要点总结：

public abstract class Account

protected double balance（注意是 protected 不是 private）

构造器做负数检查

getBalance() 返回 balance

deposit(double) 检查金额 > 0，然后 balance += amount

withdraw(double) throws OverdraftException：检查金额，余额够就减，不够就 throw new OverdraftException("余额不足", amount - balance)

public abstract String getAccountType()

可选：重写 toString()

2.3 参考代码

package com.bank.domain;

import com.bank.exception.OverdraftException;

public abstract class Account {

    protected double balance;

    public Account(double initialBalance) {
        if (initialBalance < 0) {
            throw new IllegalArgumentException("初始余额不能为负数");
        }
        this.balance = initialBalance;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("存款金额必须大于 0");
        }
        balance += amount;
    }

    public void withdraw(double amount) throws OverdraftException {
        if (amount <= 0) {
            throw new IllegalArgumentException("取款金额必须大于 0");
        }
        if (balance >= amount) {
            balance -= amount;
        } else {
            throw new OverdraftException("余额不足", amount - balance);
        }
    }

    public abstract String getAccountType();

    @Override
    public String toString() {
        return getAccountType() + "{balance=" + balance + "}";
    }
}

2.4 注意事项

别忘了 import com.bank.exception.OverdraftException

protected double balance——如果你写成 private，后面 CheckingAccount 会访问不到

withdraw 方法签名上的 throws OverdraftException 不要漏

2.5 关键设计：throws 和 throw 在这里共存

看 Account.withdraw 方法，同时出现了两种异常方式：

public void withdraw(double amount) throws OverdraftException {
    if (amount <= 0) {
        throw new IllegalArgumentException("取款金额必须大于 0");   // ① unchecked：参数校验
    }
    if (balance >= amount) {
        balance -= amount;
    } else {
        throw new OverdraftException("余额不足", amount - balance);  // ② checked：业务分支
    }
}

为什么两种不同处理？ 回到 1.7 的决策原则：

① IllegalArgumentException 是"调用方传了负数"——bug，unchecked，早炸早暴露，不用声明 throws

② OverdraftException 是"余额不够"——正常业务场景，checked，强制调用方处理，必须声明 throws

调用方怎么同时处理这两种？ 两种选择：

// 选择 A：只 catch checked 那个，unchecked 让它冒泡（本项目 BankService 用的）
try {
    account.withdraw(500);   // 500 是合法正数，参数校验不会触发
} catch (OverdraftException e) {
    return false;
}
// 如果 amount 是 -500，IllegalArgumentException 会直接穿透 try-catch，冒泡到 UI 层

// 选择 B：两个都 catch（比如 UI 层）
try {
    account.withdraw(amount);
} catch (OverdraftException e) {
    System.out.println("余额不足");
} catch (IllegalArgumentException e) {
    System.out.println("输入错误: " + e.getMessage());
}

好消息：你不需要在每个调用点都显式处理 IllegalArgumentException——它会自动冒泡到最顶层（main 或 UI）。只有 checked exception 才强制你在每个层级都声明或处理。

2.6 double 精度坑！（初学者绝对想不到的）

问题：用 double 表示金额有精度问题！

double x = 0.1 + 0.2;
System.out.println(x);   // 你以为是 0.3？实际是 0.30000000000000004！

在我们的项目里这会导致什么？

Account a = new Account(0.1);
a.deposit(0.2);
System.out.println(a.getBalance());   // 0.30000000000000004
// 看起来没差？但如果做判断：
System.out.println(a.getBalance() == 0.3);  // false！

为什么会这样？ double 是二进制浮点数，0.1 用二进制表示是无限循环小数，只能近似存储。

本项目怎么处理的？ 用 CurrencyFormatter.format() 打印时 NumberFormat 会自动四舍五入到两位小数，所以打印出来是 ￥0.30，看起来没问题。但内部存储的 balance 仍然是不精确的。

真实项目应该用什么？ java.math.BigDecimal：

// 正确做法（真实银行系统）
private BigDecimal balance;
public Account(BigDecimal initialBalance) {
    this.balance = initialBalance;
}
public void deposit(BigDecimal amount) {
    balance = balance.add(amount);    // 不是 +=，是调方法
}
// 使用时：
Account a = new Account(new BigDecimal("0.1"));  // 用字符串构造！
a.deposit(new BigDecimal("0.2"));
System.out.println(a.getBalance());   // 精确的 0.3

注意 new BigDecimal(0.1) 也会丢精度——必须用字符串构造 new BigDecimal("0.1")！

为什么本项目用 double？ 教学简化。初学者还没学 BigDecimal，double 足够演示逻辑。但你要知道：真实金融系统绝对不用 double 存金额。

2.7 拓展：balance 为什么不用 private？（protected 的代价）

我们用了 protected double balance，因为 CheckingAccount.withdraw 里要直接写 balance -= amount。但 protected 有代价——所有子类都能随意修改它，没有封装。

替代方案 1：加一个 protected 的修改方法

// Account 里加个方法
protected void setBalance(double newBalance) {
    this.balance = newBalance;
}
// CheckingAccount.withdraw 里用
setBalance(balance - amount);

比直接访问 balance 好，因为可以在 setBalance 里加校验（比如不让设成负数，或者记日志）。

替代方案 2：CheckingAccount 不重写 withdraw，而是重写一个辅助方法

// Account 里把判断逻辑提取出来
protected boolean canWithdraw(double amount) {
    return balance >= amount;
}
// CheckingAccount 重写 canWithdraw
@Override
protected boolean canWithdraw(double amount) {
    return balance + getAvailableCredit() >= amount;
}
// withdraw 里用 canWithdraw 替代原来的判断
public void withdraw(double amount) throws OverdraftException {
    if (amount <= 0) throw new IllegalArgumentException(...);
    if (canWithdraw(amount)) {
        balance -= amount;
    } else {
        throw new OverdraftException(...);
    }
}

这样 CheckingAccount 根本不需要访问 balance！但代码改动大，为了教学简洁本项目没这么做。

2.8 拓展：withdraw 里的参数校验为什么和 deposit 分开？

你可能会问：能不能把 if (amount <= 0) 提到一个公共方法里？

protected void validateAmount(double amount) {
    if (amount <= 0) throw new IllegalArgumentException("金额必须大于 0");
}
// deposit 和 withdraw 都调 validateAmount(amount)

可以！但好处是统一校验逻辑（比如以后要加"金额不能超过 100 万"只需改一处），坏处是多了一层方法调用。教学项目保持简单直接更好。

第 3 节：SavingAccount — 储蓄账户

3.1 设计思考

问题 1：储蓄账户比 Account 多了什么？

一个利率 interestRate，因为储蓄账户按利率算利息。

问题 2：要不要自己实现 withdraw？

不用。储蓄账户没有透支功能，父类 Account 的 withdraw 逻辑完全够用。

问题 3：要加什么新方法？

calculateMonthlyInterest()：算一个月的利息 = balance × rate / 12

applyMonthlyInterest()：把利息真的加到 balance 上

为什么拆成两个？调用方可能只想知道利息多少，不想立刻入账。职责分离。

问题 4：interestRate 用 final 吗？

建议用 final。开户时利率定好就不变了。如果业务上允许后期调整利率，那再改成普通字段加 setter。

3.2 动手写

文件位置：src/com/bank/domain/SavingAccount.java

要点总结：

public class SavingAccount extends Account

private final double interestRate

构造器 (double initialBalance, double interestRate)，super(initialBalance) 后赋值 interestRate，也要校验利率 >= 0

getInterestRate()

calculateMonthlyInterest() 返回 balance * interestRate / 12

applyMonthlyInterest() 调用 calculate 后加到 balance

getAccountType() 返回 "储蓄账户"

3.3 参考代码

package com.bank.domain;

public class SavingAccount extends Account {

    private final double interestRate;

    public SavingAccount(double initialBalance, double interestRate) {
        super(initialBalance);
        if (interestRate < 0) {
            throw new IllegalArgumentException("利率不能为负数");
        }
        this.interestRate = interestRate;
    }

    public double getInterestRate() {
        return interestRate;
    }

    public double calculateMonthlyInterest() {
        return balance * interestRate / 12;
    }

    public void applyMonthlyInterest() {
        balance += calculateMonthlyInterest();
    }

    @Override
    public String getAccountType() {
        return "储蓄账户";
    }
}

3.4 注意事项

super(initialBalance) 必须在构造器第一行——Java 规定子类构造器必须显式或隐式调用父类构造器，而且必须在第一行

写完可以测试：SavingAccount a = new SavingAccount(1000, 0.03); System.out.println(a.calculateMonthlyInterest()); 应该输出 2.5（1000 × 0.03 / 12）

3.5 利率存 0.03 还是 3？（设计决策）

我们存的是 0.03（小数形式），意思是 3%。两种表示法各有优缺点：

小数形式 0.03（本项目用的） 百分比整数 3 计算利息 balance * rate / 12 balance * rate / 1200 用户友好性 不直观——用户说"3%"你得先除 100 直观——直接存 3 就是 3% 容易搞混 有人可能以为 0.03 是 0.03% 有人可能以为 3 是 300%

本项目选小数的理由：数学计算上直接用 balance × rate，不需要额外除 100。但 UI 层展示给用户时应该转成百分比：

// 如果存的是 0.03
System.out.println("年利率: " + (rate * 100) + "%");   // 显示 "年利率: 3.0%"

3.6 为什么拆成 calculate 和 apply？（单一职责设计）

// 拆成两个
public double calculateMonthlyInterest() { ... }   // 只算，不改余额
public void applyMonthlyInterest() { ... }         // 算 + 改余额

// 如果合并成一个
public double applyAndReturnInterest() {
    double interest = balance * rate / 12;
    balance += interest;
    return interest;
}

拆分的好处：

展示利息但不入账——比如月末给用户发"您本月将获得 ￥2.50 利息"，但还没到入账时间

单元测试更简单——测试 calculate 不需要准备余额状态，只给 balance 和 rate 就行

幂等性——calculate 调一万次结果一样，apply 调两次会加两次利息（这可能是 bug）

什么时候合并？ 如果"计算利息后一定入账"是铁律，那合并更安全（防止有人只调 calculate 忘了调 apply）。但银行业务里经常有"预结算"和"正式入账"的区别，所以拆开设想更合理。

3.7 SavingAccount 要不要重写 deposit/withdraw？

不用。储蓄账户和父类 Account 的规则完全一样——存钱只加不减，取钱不能超过余额。但如果未来要加"储蓄账户每月限取 3 次"这种规则，就需要重写 withdraw：

public class SavingAccount extends Account {
    private int withdrawalsThisMonth;

    @Override
    public void withdraw(double amount) throws OverdraftException {
        if (withdrawalsThisMonth >= 3) {
            throw new IllegalArgumentException("本月取款次数已用完");
        }
        super.withdraw(amount);    // 调父类的 withdraw，保持原有逻辑
        withdrawalsThisMonth++;
    }
}

注意重写时用 super.withdraw(amount) 调父类逻辑——这叫重写 + 扩展，而不是完全替换父类行为。这是继承的精髓：先复用父类，再加自己的规则。

3.8 拓展：synchronized 真的需要吗？

如果是多线程环境（比如真实银行系统），applyMonthlyInterest() 里 balance += calculateMonthlyInterest() 不是原子操作——可能两个线程同时读到旧 balance，各自加利息，然后写回，导致少加了一次利息。

本项目是单线程控制台程序，所以不用考虑。但真实项目应该加 synchronized：

public synchronized void applyMonthlyInterest() {
    balance += calculateMonthlyInterest();
}

或者整个方法用同步块。多线程是高级话题，这里知道就行。

第 4 节：CheckingAccount — 支票账户（带透支保护）

4.1 设计思考

这是整个项目逻辑最复杂的类，仔细想。

问题 1：支票账户比 Account 多了什么？

一个透支额度 overdraftLimit。比如额度 500 表示余额可以透支到 -500。

问题 2：存总额度还是剩余额度？

只存总额度。剩余额度可以动态计算出来：

已用透支 = max(0, -balance)    // balance 为负表示用了透支
剩余额度 = overdraftLimit - 已用透支

为什么不直接存剩余额度？ 因为存了就容易数据不一致。比如取钱后忘了同时改剩余额度和 balance，数据就错了。存一个、算一个最安全。

问题 3：为什么 overdraftLimit 是 final？

和 SavingAccount 的 interestRate 同理——开户时定好就不变。如果允许调整再改。

问题 4：重写 withdraw 要改什么逻辑？

父类的判断是：balance >= amount？

支票账户的判断是：balance + 剩余额度 >= amount？

可用总额 = balance + getAvailableCredit()
如果 可用总额 >= amount → 正常扣钱
否则 → 抛 OverdraftException

问题 5：没开透支保护的支票账户怎么办？

提供一个单参数构造器，默认透支额度为 0：

public CheckingAccount(double initialBalance) {
    this(initialBalance, 0);  // 调用双参数构造器
}

4.2 动手写

文件位置：src/com/bank/domain/CheckingAccount.java

要点总结：

public class CheckingAccount extends Account

private final double overdraftLimit

双参数构造器 (initialBalance, overdraftLimit)，校验额度 >= 0

单参数构造器 (initialBalance) → this(initialBalance, 0)

getOverdraftLimit()

getAvailableCredit()：用 Math.max(0, -balance) 算已用，再用总额度减

重写 withdraw：判断条件改为 balance + getAvailableCredit() >= amount，不够就抛异常

getAccountType() 返回 "支票账户"

4.3 参考代码

package com.bank.domain;

import com.bank.exception.OverdraftException;

public class CheckingAccount extends Account {

    private final double overdraftLimit;

    public CheckingAccount(double initialBalance, double overdraftLimit) {
        super(initialBalance);
        if (overdraftLimit < 0) {
            throw new IllegalArgumentException("透支额度不能为负数");
        }
        this.overdraftLimit = overdraftLimit;
    }

    public CheckingAccount(double initialBalance) {
        this(initialBalance, 0);
    }

    public double getOverdraftLimit() {
        return overdraftLimit;
    }

    public double getAvailableCredit() {
        double used = Math.max(0, -balance);
        return overdraftLimit - used;
    }

    @Override
    public void withdraw(double amount) throws OverdraftException {
        if (amount <= 0) {
            throw new IllegalArgumentException("取款金额必须大于 0");
        }

        double available = balance + getAvailableCredit();

        if (available >= amount) {
            balance -= amount;
        } else {
            throw new OverdraftException(
                "超出可用额度（含透支）",
                amount - available
            );
        }
    }

    @Override
    public String getAccountType() {
        return "支票账户";
    }
}

4.4 透支计算举例

验证一下逻辑：

overdraftLimit = 500, balance = 100
available = 100 + (500 - 0) = 600

取 600 → balance = -500, available = -500 + (500 - 500) = -500 → 取不了了 ✓
取 550 → balance = -450, available = -450 + (500 - 450) = -400 → 取不了了 ✓
取 150 → balance = -50,  available = -50 + (500 - 50) = 400  → 还能取 400 ✓
取 50  → balance = 50,   available = 50 + (500 - 0) = 550   → 还能取 550 ✓

available 为什么会是负数？ 当余额已经透支到了 -500（把 500 额度全用完了），available = -500 + (500 - 500) = -500。负数说明"已经一分钱都没有了，而且还欠着银行"。available >= amount 这个判断条件自动覆盖了所有情况：正数表示还能取、0 表示刚好取完、负数表示取不了。

4.5 注意事项

getAvailableCredit() 里的 Math.max(0, -balance) 很关键。当 balance 是正数时 -balance 是负数，max 一下就变成 0 了，已用透支不会出现负数

别忘了 @Override 注解——它能帮你检查是不是真的重写了父类方法（比如参数写错了就会报错）

4.6 重复参数校验：子类和父类都写了 amount <= 0 检查，是不是多余？

看代码你会发现：Account.withdraw 和 CheckingAccount.withdraw 重写版，都有 if (amount <= 0) throw new IllegalArgumentException(...)。

// 父类 Account.withdraw
public void withdraw(double amount) throws OverdraftException {
    if (amount <= 0) throw new IllegalArgumentException("...");   // 检查 ①
    ...
}

// 子类 CheckingAccount.withdraw 重写版
@Override
public void withdraw(double amount) throws OverdraftException {
    if (amount <= 0) throw new IllegalArgumentException("...");   // 检查 ② —— 重复了？
    ...
}

答案：不多余！而且必须写。 原因很简单——重写后子类的方法是独立的，不会自动调父类的逻辑。如果你不在子类里重复写检查 ②，有人直接调 checkingAccount.withdraw(-100) 就会绕过检查，进入后面的透支逻辑，产生意外行为。

那能不能用 super 调父类来复用？ 比如：

@Override
public void withdraw(double amount) throws OverdraftException {
    super.withdraw(amount);    // 调父类，父类里有参数校验
    // 但 super.withdraw 里还有 if (balance >= amount) 的判断...
    // 我需要的是"透支判断"不是"余额判断"，所以 super.withdraw 逻辑不对
}

问题来了——父类 withdraw 里的 if (balance >= amount) 我们是要替换的（改成可用额度判断），所以不能直接调 super.withdraw。要么把参数校验提取出来变成 protected 方法，要么就老实重复写一遍。本项目选择了重复写——简单直接，初学者一眼能看懂。

如果你真想消除重复，可以这样重构：

// Account 里提取参数校验
protected void validateWithdrawAmount(double amount) {
    if (amount <= 0) throw new IllegalArgumentException("取款金额必须大于 0");
}

// Account.withdraw 里用
public void withdraw(double amount) throws OverdraftException {
    validateWithdrawAmount(amount);   // 复用
    if (balance >= amount) ...
}

// CheckingAccount.withdraw 重写版里也用
@Override
public void withdraw(double amount) throws OverdraftException {
    validateWithdrawAmount(amount);   // 复用同一个校验方法
    if (balance + getAvailableCredit() >= amount) ...
}

更干净，但增加了一个 protected 方法。教学项目保持直接重复更清晰。

4.7 getAvailableCredit 算法能不能改得更直白？

现在的写法：

public double getAvailableCredit() {
    double used = Math.max(0, -balance);   // 已用透支
    return overdraftLimit - used;           // 剩余额度
}

为什么是 Math.max(0, -balance)？ 因为 balance 有两种情况：

balance = 100（没透支）→ -balance = -100 → max(0, -100) = 0 → 已用透支 0 ✓

balance = -50（透支了 50）→ -balance = 50 → max(0, 50) = 50 → 已用透支 50 ✓

直白版写法：用 if-else 替代 Math.max

public double getAvailableCredit() {
    double used;
    if (balance >= 0) {
        used = 0;              // 没透支
    } else {
        used = -balance;       // 透支了，balance 是负数，取反
    }
    return overdraftLimit - used;
}

功能完全一样，只是写法不同。Math.max 一行搞定但需要动点脑子，if-else 啰嗦但一眼看懂。你自己选哪种都行。

4.8 透支后的 balance 精度会怎样？

举个例子验证：

overdraftLimit = 500, balance = 100
available = 100 + (500 - 0) = 600
取 550 → balance = 100 - 550 = -450

简单减法，没问题。但如果涉及到 0.1 这种精度陷阱的数字：

balance = 0.1, 取 0.2
available = 0.1 + 500 = 500.1
500.1 >= 0.2 → 可以取
balance = 0.1 - 0.2 = -0.10000000000000002   ← 精度出来了！

不过这在显示时被 CurrencyFormatter.format() 隐藏了（显示 ￥-0.10）。真实项目用 BigDecimal 就不会有这问题。

4.9 拓展：透支功能的其他设计

拓展 1：透支要收利息

真实银行的透支不是免费的——超额后开始按日息收利息：

public class CheckingAccount extends Account {
    private double overdraftInterestRate;  // 比如 0.0005（日息万分之五）

    public double calculateOverdraftInterest() {
        if (balance < 0) {
            return -balance * overdraftInterestRate;   // 透支金额 × 日息
        }
        return 0;
    }
}

拓展 2：透支额度可以调整

真实银行会根据客户信用调整额度：

public class CheckingAccount extends Account {
    private double overdraftLimit;   // 去掉 final

    public void setOverdraftLimit(double newLimit) {
        if (newLimit < 0) throw new IllegalArgumentException(...);
        this.overdraftLimit = newLimit;
    }
}

去掉 final 就能改了，但需要 setter + 参数校验。注意：如果调低额度导致"已用透支 > 新额度"怎么办？这也是要处理的边界情况。

第 5 节：Customer — 客户

5.1 设计思考

问题 1：客户有哪些属性？

firstName、lastName。然后客户有多个账户，所以需要一个 Account 列表。

问题 2：用数组还是 ArrayList？

ArrayList。Java 集合框架比数组好用太多：

自动扩容，不用管最大容量

自带 size() 方法，不用自己维护 numberOfAccounts 变量

有 add()、get()、iterator() 等便捷方法

问题 3：accounts 什么时候初始化？

在声明时直接初始化：private final List<Account></account> accounts = new ArrayList<>()。这样保证 accounts 永远不是 null，addAccount 不会 NPE。构造器里忘写了也没关系。

问题 4：firstName 和 lastName 要不要 final？

要。姓名创建后不该变。设为 final 后编译器帮你检查不会被误改。

问题 5：getAccounts() 返回什么？

不能直接返回内部的 accounts 引用——外部拿到后可以 customer.getAccounts().clear()，把客户的所有账户都清掉。应该返回 Collections.unmodifiableList(accounts)，外部只读不能改。

问题 6：还要什么方法？

addAccount(Account)：开户时调用

getAccount(int index)：按索引取账户

getAccountCount()：账户数量

accountIterator()：返回 Iterator，方便遍历

5.2 动手写

文件位置：src/com/bank/domain/Customer.java

要点总结：

private final String firstName; private final String lastName;

private final List<Account></account> accounts = new ArrayList<>();（声明时初始化，final）

构造器校验姓名不为空

getFirstName()、getLastName()、getFullName()（拼接 "f l"）

addAccount(Account) 校验非 null

getAccount(int) 校验索引范围

getAccountCount() → accounts.size()

getAccounts() → Collections.unmodifiableList(accounts)

accountIterator() → accounts.iterator()

需要导入：java.util.ArrayList、java.util.Collections、java.util.Iterator、java.util.List

5.3 参考代码

package com.bank.domain;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

public class Customer {

    private final String firstName;
    private final String lastName;
    private final List<Account></account> accounts = new ArrayList<>();

    public Customer(String firstName, String lastName) {
        if (firstName == null || firstName.isEmpty()) {
            throw new IllegalArgumentException("名不能为空");
        }
        if (lastName == null || lastName.isEmpty()) {
            throw new IllegalArgumentException("姓不能为空");
        }
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public String getFullName() {
        return firstName + " " + lastName;
    }

    public void addAccount(Account account) {
        if (account == null) {
            throw new IllegalArgumentException("账户不能为空");
        }
        accounts.add(account);
    }

    public int getAccountCount() {
        return accounts.size();
    }

    public Account getAccount(int index) {
        if (index < 0 || index >= accounts.size()) {
            throw new IndexOutOfBoundsException("账户索引越界: " + index);
        }
        return accounts.get(index);
    }

    public List<Account></account> getAccounts() {
        return Collections.unmodifiableList(accounts);
    }

    public Iterator<Account></account> accountIterator() {
        return accounts.iterator();
    }

    @Override
    public String toString() {
        return "Customer{" + getFullName() + ", accounts=" + accounts.size() + "}";
    }
}

5.4 注意事项

import 四个 java.util 的类，一个不能少

索引校验：index < 0 || index >= accounts.size() 要用 >= 不是 >（索引从 0 开始）

addAccount 方法要校验参数非 null，否则外部传 null 进来，后面遍历 accounts 时会 NPE

5.5 Collections.unmodifiableList 到底做了什么？

我们写了 getAccounts() { return Collections.unmodifiableList(accounts); }——它返回的不是副本，是视图。

Customer c = ...;
List<Account></account> view = c.getAccounts();

// ① 外部调 view.add(...) → 抛 UnsupportedOperationException
//    但 accounts 内部列表不受影响 ✓

// ② 但如果有人持有 accounts 的引用：
//    Customer 内部是 private final accounts，外部拿不到这个引用
//    所以 accounts 内部列表是安全的 ✓

// ③ 如果 Customer 内部自己改了 accounts（比如 addAccount），
//    view 里立刻能看到变化（因为是同一个底层列表的视图）

为什么不是返回副本？ new ArrayList<>(accounts) 创建的是真正的副本，外部改副本不影响内部，但：

每次都拷贝一份列表，浪费内存

外部看到的是旧数据快照，不是实时状态

unmodifiableList 是"只读视图"的标准做法——性能好，外部也改不了。

注意：它只保证列表不能 add/remove，但列表里的对象引用不变。如果 Account 类有 balance 的 setter，外部还是能改 view.get(0).setBalance(99999)！我们的 Account 类没有 setter，所以安全。如果 Account 有 setter，需要额外处理——或者让 Account 也不可变，或者返回副本。

5.6 null 返回值 vs Optional

Bank.findCustomerByName 和 Customer.getAccount(int) 的设计：

// Bank.findCustomerByName 没找到 → 返回 null
public Customer findCustomerByName(String fullName) {
    for (Customer c : customers) {
        if (c.getFullName().equals(fullName)) return c;
    }
    return null;
}

// Customer.getAccount(int) 越界 → 抛异常（IndexOutOfBoundsException）
public Account getAccount(int index) {
    if (index < 0 || index >= accounts.size()) throw new IndexOutOfBoundsException(...);
    return accounts.get(index);
}

同样是"找不到/越界"，为什么两种处理方式？

findCustomerByName getAccount(int) "找不到"是正常情况吗？ 是——搜一个不存在的客户很正常 否——索引越界说明调用方代码有 bug 处理方式 返回 null（让调用方决定怎么办） 抛异常（早失败） 调用方要做什么 必须 null 检查 if (customer != null) 不需要——如果越界，是 bug，应该修代码

Java 8 之后推荐用 Optional 替代 null 返回：

public Optional<Customer></customer> findCustomerByName(String fullName) {
    for (Customer c : customers) {
        if (c.getFullName().equals(fullName)) return Optional.of(c);
    }
    return Optional.empty();
}
// 调用方：
bank.findCustomerByName("John Smith").ifPresent(c -> System.out.println(c));
// 或者：
Customer c = bank.findCustomerByName("John").orElseThrow(() -> new IllegalArgumentException("没找到"));

Optional 让"可能为空"这个事实在类型层面就显式了，编译器和 IDE 都会提醒你处理。本项目为了教学简单用了 null，但你在现代项目里应该用 Optional。

5.7 for-each 还是 Iterator？

我们在 BankService.applyMonthlyInterest 里用的是 for-each：

for (Customer customer : bank.getCustomers()) {
    for (Account account : customer.getAccounts()) { ... }
}

for-each 和 Iterator 是同一个东西——for-each 底层就是用 Iterator。区别只是语法：

// 显式 Iterator（本项目 Customer/Bank 里也提供了 accountIterator() 方法）
Iterator<Account></account> it = customer.getAccounts().iterator();
while (it.hasNext()) {
    Account a = it.next();
    // 这里可以 it.remove() —— for-each 做不到
}

// for-each（语法糖）
for (Account a : customer.getAccounts()) {
    // 简洁，但是不能在这里 add/remove
}

什么时候用 Iterator？需要在遍历中删除元素时。比如"删除所有余额为 0 的账户"，只能用 Iterator 的 remove()，for-each 里调 accounts.remove(a) 会抛 ConcurrentModificationException。

5.8 拓展：final 字段 vs 可变引用

private final List<Account></account> accounts = new ArrayList<>();

很多人以为 final List 意味着列表内容不可变——错！ final 只保证 accounts 这个引用不能指向别的列表，但列表里的元素照样可以 add/remove/clear。

如果你想要真正不可变的 accounts：

// 方案 1：构造器里就创建好，之后完全不 add
private final List<Account></account> accounts = List.of(newAccount1, newAccount2);

// 方案 2：用 Collections.unmodifiableList + 不暴露 add 方法
private final List<Account></account> accounts = new ArrayList<>();
public List<Account></account> getAccounts() { return Collections.unmodifiableList(accounts); }
// 但内部还有 addAccount() 方法... 如果真要不可变，连 addAccount 都不该有

本项目需要开户功能（addAccount），所以 accounts 是可变的，只对外暴露只读视图。

第 6 节：Bank — 银行（单例）

6.1 设计思考

问题 1：Bank 为什么要单例？

全局只有一家银行。所有客户都属于同一家银行。如果可以随便 new Bank() 出来多家银行，客户就被分散到不同 Bank 对象里了，报告也打印不全。

问题 2：单例怎么实现？三要素：

1. 私有构造器         private Bank() { ... }
2. 私有静态 final 实例 private static final Bank INSTANCE = new Bank();
3. 公有静态获取方法    public static Bank getInstance() { return INSTANCE; }

为什么用"饿汉式"（类加载就创建）而不是"懒汉式"（第一次用时才创建）？

饿汉式写法简单、线程安全（JVM 类加载机制保证 static final 只初始化一次）。对于银行这种"启动就要用、全局只有一个"的对象，饿汉式最合适。

问题 3：Bank 和 Customer 是什么关系？

和 Customer 里的 accounts 一样——Bank 有一个 List<Customer></customer>。所有设计和 Customer 里的 accounts 同理。

问题 4：Bank 比 Customer 多什么方法？

findCustomerByName(String fullName)——按全名查找客户。遍历 List，用 equals 比较字符串。

6.2 动手写

文件位置：src/com/bank/domain/Bank.java

要点总结：

单例三要素（私有构造器 + 静态 final 实例 + getInstance）

private final List<Customer></customer> customers = new ArrayList<>()

addCustomer(firstName, lastName)：new Customer 加入 List，返回它方便后续开户

getCustomerCount() → customers.size()

getCustomer(int index)：校验索引

getCustomers() → Collections.unmodifiableList(customers)

customerIterator() → customers.iterator()

findCustomerByName(String)：遍历 List 逐个 equals 比较

需要导入：同 Customer，四个 java.util

6.3 参考代码

package com.bank.domain;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

public class Bank {

    private final List<Customer></customer> customers = new ArrayList<>();

    private Bank() {
    }

    private static final Bank INSTANCE = new Bank();

    public static Bank getInstance() {
        return INSTANCE;
    }

    public Customer addCustomer(String firstName, String lastName) {
        Customer customer = new Customer(firstName, lastName);
        customers.add(customer);
        return customer;
    }

    public int getCustomerCount() {
        return customers.size();
    }

    public Customer getCustomer(int index) {
        if (index < 0 || index >= customers.size()) {
            throw new IndexOutOfBoundsException("客户索引越界: " + index);
        }
        return customers.get(index);
    }

    public List<Customer></customer> getCustomers() {
        return Collections.unmodifiableList(customers);
    }

    public Iterator<Customer></customer> customerIterator() {
        return customers.iterator();
    }

    public Customer findCustomerByName(String fullName) {
        for (Customer c : customers) {
            if (c.getFullName().equals(fullName)) {
                return c;
            }
        }
        return null;
    }
}

6.4 单例验证

你可以写个快速测试：

public static void main(String[] args) {
    Bank a = Bank.getInstance();
    Bank b = Bank.getInstance();
    System.out.println(a == b);  // true
    // Bank c = new Bank();      // 这行应该编译报错
}

如果 a == b 是 true 且 new Bank() 报错，说明单例实现正确。

6.5 注意事项

单例的静态实例 INSTANCE 必须是 private static final，少任何一个修饰符都不对

构造器必须是 private，否则外部还能 new

findCustomerByName 没找到返回 null，调用方需要做 null 检查

addCustomer 返回新创建的 Customer，这样调用方可以立刻给客户开户：Customer jane = Bank.getInstance().addCustomer("Jane", "Simms"); jane.addAccount(...)

6.6 单例的四种实现方式对比

我们用了饿汉式（类加载就创建）。Java 单例有四种经典写法，各有适用场景：

写法 线程安全 延迟加载 优点 缺点 饿汉式（本项目用的） ✅ JVM 类加载保证 ❌ 启动就创建 最简单、无锁 Bank 类加载时就占内存 懒汉式 + synchronized ✅ ✅ 第一次用才创建 延迟加载 每次调 getInstance 都加锁，性能差 双重检查锁（DCL） ✅ ✅ 锁只加一次，性能好 写法复杂，JVM 内存模型坑多 枚举单例 ✅ ❌ 最简洁、天然防反射 不能延迟加载

本项目为什么选饿汉式？

Bank 对象很小，占不了多少内存，"延迟加载"收益为 0

启动就要用 Bank，不会"加载了但没用"

代码最简单，适合教学

没有线程安全坑

如果将来 Bank 变重了（比如启动时要连数据库、读配置），可以改成懒汉式或枚举单例。

6.7 拓展：如果有多家银行怎么办？

现在是单例——全局只有一家硅谷银行。如果有"中国工商银行"、"中国建设银行"，单例就废了。

方案：工厂 + Map

public class BankRegistry {
    private static final Map<String, Bank> banks = new HashMap<>();

    public static Bank getBank(String bankName) {
        return banks.computeIfAbsent(bankName, name -> new Bank(name));
    }
    // 注意：现在 Bank 构造器不能是 private 了！
    // 要改成包级可见，或者 BankRegistry 和 Bank 在同一个包
}

// 使用：
Bank icbc = BankRegistry.getBank("中国工商银行");
Bank ccb = BankRegistry.getBank("中国建设银行");

本质上是"多例"——不是只有一个实例，而是"每个 name 对应一个实例"。Spring 的 Bean 管理就是这个思路。

6.8 BankService 和 Bank 是什么关系？——为什么 BankService 还要持有 Bank？

看 BankService：

public class BankService {
    private final Bank bank = Bank.getInstance();   // 又拿了一遍单例
    // ...
}

然后 BankUI 里又有：

public class BankUI {
    private final Bank bank = Bank.getInstance();     // UI 层也拿了
    private final BankService service = new BankService();
    // ...
}

为什么不直接在方法里调 Bank.getInstance()，而是存到字段里？

// 每次都调：
public void printReport() {
    for (Customer c : Bank.getInstance().getCustomers()) { ... }
}

// 存到字段里：
private final Bank bank = Bank.getInstance();
public void printReport() {
    for (Customer c : bank.getCustomers()) { ... }
}

两种写法功能一样（因为是单例，getInstance() 每次返回同一个对象）。存到字段的好处：

少写代码——不用每次都写 Bank.getInstance()

方便测试——单元测试时可以注入一个 mock Bank，而 Bank.getInstance() 是硬编码的

语义清晰——"这个类依赖 Bank"一眼就能看出来

第 7 节：CurrencyFormatter — 货币格式化工具

7.1 设计思考

问题 1：为什么不直接在打印时写 System.out.println("￥" + amount)？

有三个原因：

要自动加千分位：1500 → ￥1,500.00

要统一小数位：500 → ￥500.00（两位小数）

全局统一格式，改一处就行

问题 2：工具类怎么设计？

final 类（不让继承）

私有构造器（不让实例化）

全是 static 方法（直接用类名调用）

问题 3：NumberFormat 和 Locale 是什么？

这是 JDK 自带的两个类：

NumberFormat（在 java.text 包）：专门把数字格式化成各种样子。比如 getCurrencyInstance() 就是"按货币格式"，getPercentInstance() 就是"按百分比格式"。

Locale（在 java.util 包）：表示"你是哪个国家/地区"。Locale.CHINA 表示中国，Locale.US 表示美国。不同的 Locale 格式不一样——同样 1500，中国输出 ￥1,500.00，美国输出 $1,500.00，法国输出 1 500,00 €。

所以 NumberFormat.getCurrencyInstance(Locale.CHINA) 的意思是："给我一个人民币格式器"。每次创建它有开销，所以缓存到 static final 字段里，整个程序只建一次。

7.2 动手写

文件位置：src/com/bank/util/CurrencyFormatter.java

要点总结：

public final class CurrencyFormatter

private CurrencyFormatter() {} 空的私有构造器

private static final NumberFormat CNY = NumberFormat.getCurrencyInstance(Locale.CHINA)

public static String format(double amount) { return CNY.format(amount); }

需要导入：java.text.NumberFormat、java.util.Locale

7.3 参考代码

package com.bank.util;

import java.text.NumberFormat;
import java.util.Locale;

public final class CurrencyFormatter {

    private static final NumberFormat CNY = NumberFormat.getCurrencyInstance(Locale.CHINA);

    private CurrencyFormatter() {
    }

    public static String format(double amount) {
        return CNY.format(amount);
    }
}

7.4 注意事项

Locale.CHINA 确保输出人民币符号 ￥。如果不指定 Locale，可能根据系统输出 $ 或 €

测试一下：CurrencyFormatter.format(1500) 应该输出 ￥1,500.00

7.5 工具类设计 Checklist

工具类（又叫 Utility Class）的设计有一套固定规范——按这个清单检查：

public final class？不让继承，没意义

私有构造器 private CurrencyFormatter() {}？不让实例化，没意义

所有方法和静态字段都是 static？用 CurrencyFormatter.format(...) 而不是 new CurrencyFormatter().format(...)

没有可变字段？工具类应该是无状态的——format(1500) 什么时候调都输出 ￥1,500.00

import 正确？NumberFormat 在 java.text，Locale 在 java.util

反面教材：不该这么写

// ❌ 错误 1：不是 final，可以被继承出子类工具类
public class BadUtil { ... }

// ❌ 错误 2：构造器不是 private，有人 new 出实例来调
public BadUtil() {}

// ❌ 错误 3：有实例字段（不是 static），实例状态不统一
private double rate = 0.03;   // 有人改了 rate，其他实例看到的不一样

// ❌ 错误 4：有可变静态字段
public static String prefix = "$";   // 有人改了 prefix，全局全变了

7.6 double 格式化后的精度会怎样？

还记得 2.6 讲的 double 精度坑吗？NumberFormat.format 怎么处理它？

double x = 0.1 + 0.2;   // 0.30000000000000004
System.out.println(CurrencyFormatter.format(x));   // ￥0.30 ✓ 正常了！

double big = 1000000000000.1;   // 超过 16 位有效数字的数
System.out.println(CurrencyFormatter.format(big));  // ￥1,000,000,000,000.10
// 但如果是更大的：
double huge = 1234567890123456.7;   // 16 位以上
System.out.println(CurrencyFormatter.format(huge));  // ￥1,234,567,890,123,456.73 ← 精度丢了！

结论：NumberFormat 会四舍五入到两位小数，隐藏了小数部分的精度问题。但对于超大数字（超过 double 精度范围的），格式化也救不了——这时候应该用 BigDecimal。

7.7 拓展：NumberFormat 还能做什么？

NumberFormat 不止能格式化货币——它是数字格式化的瑞士军刀：

// 百分比格式化
NumberFormat percent = NumberFormat.getPercentInstance();
percent.format(0.085);   // "8%"（注意：0.085 会变成 8.5%，自动乘 100）

// 千分位整数
NumberFormat integer = NumberFormat.getIntegerInstance();
integer.format(1234567);   // "1,234,567"

// 不同 Locale 不同格式
NumberFormat us = NumberFormat.getCurrencyInstance(Locale.US);
us.format(1500);    // "$1,500.00"

NumberFormat france = NumberFormat.getCurrencyInstance(Locale.FRANCE);
france.format(1500);   // "1 500,00 €"（法国用空格和逗号）

NumberFormat japan = NumberFormat.getCurrencyInstance(Locale.JAPAN);
japan.format(1500);   // "￥1,500"（日元不用小数）

7.8 拓展：为什么不直接用 String.format？

// String.format 也能格式化
String.format("￥%.2f", 1500);   // "￥1500.00"

// 但千分位呢？
String.format("￥%,.2f", 1500);  // "￥1,500.00"

// Locale 呢？
String.format(Locale.US, "$%,.2f", 1500);  // "$1,500.00"

看起来 String.format 也能做。那为什么我们用 NumberFormat？

CurrencyFormatter（NumberFormat） String.format 千分位、两位小数 ✅ 自动处理 要写 %,.2f 格式字符串 Locale 自动适配 ✅ Locale.CHINA 就输出人民币格式 要手动加 Locale 参数 缓存复用 ✅ 只创建一次 NumberFormat 每次都新建内部 formatter 可扩展 ✅ 可以换 Locale、换格式 格式字符串改起来绕

CurrencyFormatter 封装了这些细节，调用方只用写 format(amount) 就行。

第 8 节：BankService — 业务服务层

8.1 设计思考

问题 1：为什么需要 service 层？domain 层不够吗？

domain 层只管"数据长什么样、单个操作怎么干"。service 层管"业务流程怎么走"。

举个例子：

Account.withdraw 只管"钱够不够，够就减，不够就抛异常"

BankService 要管"开户流程：先 openCustomer，再 openSavingAccount，再把账户加到客户上"

问题 2：BankService 和 Bank 的关系？

BankService 内部持有 Bank 单例的引用（private final Bank bank = Bank.getInstance()）。service 对外提供方法，内部调用 Bank 和 Account 的方法。

问题 3：withdraw 方法在 service 层怎么设计？

domain 层的 withdraw 抛 OverdraftException。service 层选择捕获异常，打印友好提示，返回 boolean。

原因：控制台程序不想层层 try-catch。UI 层只需 service.withdraw(account, 500) 拿到 true/false 就行。

问题 4：要不要提供 applyMonthlyInterest？

要。遍历所有客户、所有账户，给储蓄账户算利息。这个流程放在 service 层合适。

8.2 动手写

文件位置：src/com/bank/service/BankService.java

要点总结：

private final Bank bank = Bank.getInstance()

openCustomer(firstName, lastName) → 调 bank.addCustomer，返回 Customer

openSavingAccount(customer, balance, rate) → new SavingAccount，customer.addAccount，返回

openCheckingAccount(customer, balance) 和三参数重载版

deposit(account, amount) → 调 account.deposit

withdraw(account, amount) → try { account.withdraw; return true; } catch { 打印提示 return false; }

applyMonthlyInterest() → 双层 for 循环遍历，instanceof 判断 SavingAccount，调用 applyMonthlyInterest

8.3 参考代码

package com.bank.service;

import com.bank.domain.Account;
import com.bank.domain.Bank;
import com.bank.domain.CheckingAccount;
import com.bank.domain.Customer;
import com.bank.domain.SavingAccount;
import com.bank.exception.OverdraftException;
import com.bank.util.CurrencyFormatter;

public class BankService {

    private final Bank bank = Bank.getInstance();

    public Customer openCustomer(String firstName, String lastName) {
        return bank.addCustomer(firstName, lastName);
    }

    public SavingAccount openSavingAccount(Customer customer, double balance, double rate) {
        SavingAccount account = new SavingAccount(balance, rate);
        customer.addAccount(account);
        return account;
    }

    public CheckingAccount openCheckingAccount(Customer customer, double balance) {
        return openCheckingAccount(customer, balance, 0);
    }

    public CheckingAccount openCheckingAccount(Customer customer, double balance, double overdraftLimit) {
        CheckingAccount account = new CheckingAccount(balance, overdraftLimit);
        customer.addAccount(account);
        return account;
    }

    public void deposit(Account account, double amount) {
        account.deposit(amount);
    }

    public boolean withdraw(Account account, double amount) {
        try {
            account.withdraw(amount);
            return true;
        } catch (OverdraftException e) {
            System.out.println("取款失败: " + e.getMessage()
                + " (差额 " + CurrencyFormatter.format(e.getDeficit()) + ")");
            return false;
        }
    }

    public void applyMonthlyInterest() {
        for (Customer customer : bank.getCustomers()) {
            for (Account account : customer.getAccounts()) {
                if (account instanceof SavingAccount) {
                    ((SavingAccount) account).applyMonthlyInterest();
                }
            }
        }
    }
}

8.4 注意事项

所有 domain 类都在 com.bank.domain 下，所以 import 要写全路径

instanceof 后强转：(SavingAccount) account。强转是必要的，因为 Account 上没有 applyMonthlyInterest 方法

8.5 关键决策：service 层的两种异常处理策略

BankService 有两个方法，对异常的处理方式完全不同——这是整个项目最值得你仔细品味的设计决策：

// 方法 1：deposit —— 完全不 catch
public void deposit(Account account, double amount) {
    account.deposit(amount);   // IllegalArgumentException 会冒泡出去
}

// 方法 2：withdraw —— catch 一个，让另一个冒泡
public boolean withdraw(Account account, double amount) {
    try {
        account.withdraw(amount);
        return true;
    } catch (OverdraftException e) {         // catch checked 的
        System.out.println("取款失败: " + e.getMessage());
        return false;
    }
    // IllegalArgumentException 不 catch，让它冒泡
}

为什么两个方法策略不同？ 根源是"方法的契约"：

deposit(Account, double) withdraw(Account, double) 返回值 void（成功就完事，没什么要返回的） boolean（告诉调用方成没成功） check 异常（OverdraftException） Account.deposit 根本不抛 OverdraftException Account.withdraw 可能抛 unchecked 异常（IllegalArgumentException） account 为 null 或 amount <= 0 时抛 同样场景 service 层策略 完全不处理，让它冒泡到 UI catch OverdraftException 转 boolean，IllegalArgumentException 让它冒泡

deposit 为什么不 catch？

Account.deposit 只可能抛 IllegalArgumentException（参数非法）

参数非法是调用方 bug——UI 层不该让用户输入负数然后传给 service

service 层 catch 了也没更好的处理方式，不如"让它炸，调用人赶紧修"

withdraw 为什么要 catch OverdraftException？

OverdraftException 是正常业务场景——用户余额不足不是 bug

把异常转成 boolean 让 UI 层好处理：if (service.withdraw(...)) { 打印成功 } else { 打印失败 }

如果不 catch，UI 层就要写 try-catch，看起来更复杂

IllegalArgumentException 在 withdraw 里为什么也不 catch？

同样是"参数非法是 bug"的理由

amount <= 0 这种事应该在 UI 层就拦住，不应该让它到 service 和 domain

8.6 完整异常传播链路图（现在你能看懂了吧！）

用户输入 -500 取钱
    ↓
BankUI.withdraw() → readDoubleSafe() 读到 -500
    ↓
BankUI.withdraw() → service.withdraw(account, -500)
    ↓
BankService.withdraw() → try { account.withdraw(-500) }
    ↓
Account.withdraw(-500) → if (amount <= 0) throw new IllegalArgumentException("取款金额必须大于 0")
    ↓
Exception 对象冒泡回 BankService.withdraw()
    ↓
BankService.withdraw() 的 try-catch 只 catch OverdraftException
    ↓
IllegalArgumentException 穿透 try-catch，继续冒泡回 BankUI.withdraw()
    ↓
BankUI.withdraw() 的 try-catch(IllegalArgumentException e) 兜住！
    ↓
打印 "✗ 取钱失败: 取款金额必须大于 0"
    ↓
while(true) 循环回到菜单，用户可以重试

如果用户输入合法的正数（比如 500），但余额不够呢？

用户输入 500 取钱，但余额 100
    ↓
BankService.withdraw() → try { account.withdraw(500) }
    ↓
Account.withdraw(500) → amount > 0 校验通过
    ↓
Account.withdraw(500) → balance(100) < 500 → throw new OverdraftException("余额不足", 400)
    ↓
BankService.withdraw() → catch(OverdraftException e) ✓ 兜住了
    ↓
打印 "取款失败: 余额不足 (差额 ￥400.00)"
    ↓
return false
    ↓
BankUI.withdraw() → if (false) { ... } → 什么都不打印（因为失败消息已经在 service 层打印了）
    ↓
循环回到菜单

8.7 BankService 为什么不做参数校验？——让 domain 层说了算

BankService 里 openSavingAccount 方法：

public SavingAccount openSavingAccount(Customer customer, double balance, double rate) {
    SavingAccount account = new SavingAccount(balance, rate);   // balance 校验在构造器里
    customer.addAccount(account);                               // customer 校验非 null
    return account;
}

BankService 自己没做 if (balance < 0)、if (rate < 0)、if (customer == null) 的检查——它直接调下层，让下层（SavingAccount 构造器、Customer.addAccount）自己校验。

这叫"让领域对象保持自治"：

Account 构造器自己检查余额——它最懂"余额不能为负"

Customer.addAccount 自己检查 account 非 null——它最懂"客户的账户不能是 null"

BankService 只负责"协调流程"，不重复校验逻辑

这样做的好处：如果以后有人绕过 BankService 直接 new SavingAccount(-500, 0.03) 也会立刻报错，不会悄悄创建出坏数据。校验逻辑只在一处（domain 层），不会出现"Service 层改了校验条件但 Account 构造器忘了改"的不同步问题。

8.8 BankService 应该是单例吗？

现在 BankUI 里 new BankService()——每次 BankUI 创建一个新的 BankService。但 BankService 内部只是持有 Bank 单例的引用，没有自己的状态。

两种选择：

现在的做法：每次 new 改成单例或静态方法 优点 简单，new 就行 更省内存，但 BankService 本身不占多少 缺点 多个 BankService 实例浪费（但实际只 new 一次） 单例增加复杂度，静态方法难测试 适用场景 service 有状态（比如"当前操作人"、"事务上下文"） service 完全无状态

本项目 BankService 是无状态的，两种都可以。保留 new BankService() 更符合"面向对象"的写法，也方便以后扩展（比如 BankService 里加一个"当前数据库连接"字段）。

8.9 拓展：如果 openCustomer 也要处理异常？

现在 openCustomer(firstName, lastName) 直接调 bank.addCustomer()，校验在 Customer 构造器里。如果 firstName 是 null，会抛 IllegalArgumentException，冒泡到 UI 层——UI 层现在没 catch 这个！

可以让 UI 层的 openCustomer 方法加一个 try-catch：

private void openCustomer() {
    System.out.println("--- 新开客户 ---");
    System.out.print("名 (first name): ");
    String firstName = scanner.nextLine().trim();
    System.out.print("姓 (last name): ");
    String lastName = scanner.nextLine().trim();
    try {
        Customer c = service.openCustomer(firstName, lastName);
        System.out.println("✓ 客户创建成功: " + c.getFullName());
    } catch (IllegalArgumentException e) {
        System.out.println("✗ 创建失败: " + e.getMessage());
    }
}

虽然 Scanner.nextLine() 读到空字符串的概率很低，但加上更安全。本项目现在可能漏了这个——如果你想加可以自己补上。

8.10 拓展：service 层 vs domain 层的边界在哪里？

区分标准很简单：

domain 层：单个对象的数据不变量、单个对象的操作逻辑。Account 管"钱能不能取、余额怎么改"，Customer 管"账户列表不能有 null"

service 层：跨多个对象的流程编排。"开户流程"涉及 Bank（存客户）、Customer（存账户）、Account（创建账户）三个 domain 对象，所以在 service 层

如果将来有以下需求，应该放在哪层？

"创建客户时同时开一个默认储蓄账户" → service 层（跨 Customer + SavingAccount）

"给所有储蓄账户加利息并打印通知" → service 层（遍历 + 协调）

"Account 加一个 equals 方法比较余额" → domain 层（单个对象内部逻辑）

"转账：A 账户减钱，B 账户加钱" → service 层（跨两个 Account 对象）

第 9 节：BankUI + Main — 演示程序

9.1 设计思考

问题 1：为什么分 BankUI 和 Main 两个类？

Main 只做一件事——调用 BankUI。BankUI 负责所有交互逻辑。入口和业务分离，以后要改 UI（比如改成扫描器菜单）只需改 BankUI。

问题 2：BankUI 要演示什么？

按教程顺序，分四步：

setupDemoData() —— 创建 4 个客户，每人开 1~2 个账户

printCustomerReport() —— 打印报告

demonstrateTransactions() —— 演示存钱、取钱、透支

printFinalReport() —— 交易后再打印一次（可以看到利息入账）

问题 3：报告怎么打印？

双层循环：外层遍历 Bank 的 customers，内层遍历 Customer 的 accounts。instanceof 判断账户类型后用不同格式输出。

9.2 动手写

Main 很简单，三行搞定。重点是 BankUI。

文件位置：src/com/bank/ui/Main.java 和 src/com/bank/ui/BankUI.java

9.3 参考代码 — Main.java

package com.bank.ui;

public class Main {
    public static void main(String[] args) {
        BankUI demo = new BankUI();
        demo.run();
    }
}

9.4 参考代码 — BankUI.java

package com.bank.ui;

import com.bank.domain.*;
import com.bank.service.BankService;
import com.bank.util.CurrencyFormatter;
import com.bank.exception.OverdraftException;

public class BankUI {

    private final BankService service = new BankService();
    private final Bank bank = Bank.getInstance();

    public void run() {
        setupDemoData();
        printCustomerReport();
        demonstrateTransactions();
        printFinalReport();
    }

    private void setupDemoData() {
        System.out.println("========== 初始化演示数据 ==========");

        Customer jane = service.openCustomer("Jane", "Simms");
        service.openSavingAccount(jane, 500, 0.03);
        service.openCheckingAccount(jane, 200, 500);

        Customer owen = service.openCustomer("Owen", "Bryant");
        service.openCheckingAccount(owen, 200);

        Customer tim = service.openCustomer("Tim", "Soley");
        service.openSavingAccount(tim, 1500, 0.025);
        service.openCheckingAccount(tim, 200);

        Customer maria = service.openCustomer("Maria", "Soley");
        service.openCheckingAccount(maria, 200);
        service.openSavingAccount(maria, 150, 0.03);

        System.out.println("已创建 " + bank.getCustomerCount() + " 个客户\n");
    }

    private void printCustomerReport() {
        System.out.println("========== 客户报告 ==========");

        for (Customer customer : bank.getCustomers()) {
            System.out.println();
            System.out.println("客户: " + customer.getLastName() + ", " + customer.getFirstName());

            for (Account account : customer.getAccounts()) {
                if (account instanceof SavingAccount) {
                    SavingAccount sa = (SavingAccount) account;
                    System.out.println("  ├─ 储蓄账户 (利率 "
                        + String.format("%.1f%%", sa.getInterestRate() * 100)
                        + "): " + CurrencyFormatter.format(account.getBalance()));
                } else if (account instanceof CheckingAccount) {
                    CheckingAccount ca = (CheckingAccount) account;
                    System.out.println("  ├─ 支票账户 (透支额度 "
                        + CurrencyFormatter.format(ca.getOverdraftLimit())
                        + "): " + CurrencyFormatter.format(account.getBalance()));
                }
            }
        }
        System.out.println();
    }

    private void demonstrateTransactions() {
        System.out.println("========== 交易演示 ==========\n");

        Customer jane = bank.findCustomerByName("Jane Simms");
        Account janeChecking = jane.getAccount(1);

        System.out.println("【Jane 的支票账户】余额 " + CurrencyFormatter.format(janeChecking.getBalance()));
        System.out.print("  取 150 → ");
        service.withdraw(janeChecking, 150);
        System.out.print("  存 22.50 → ");
        service.deposit(janeChecking, 22.50);
        System.out.print("  取 147.62 → ");
        service.withdraw(janeChecking, 147.62);
        System.out.println("  当前余额: " + CurrencyFormatter.format(janeChecking.getBalance()));

        System.out.print("  取 470 (超出可用额度) → ");
        try {
            janeChecking.withdraw(470);
        } catch (OverdraftException e) {
            System.out.println("失败: " + e.getMessage() + "，还差 " + CurrencyFormatter.format(e.getDeficit()));
        }

        System.out.println();

        Customer owen = bank.findCustomerByName("Owen Bryant");
        Account owenChecking = owen.getAccount(0);

        System.out.println("【Owen 的支票账户】余额 " + CurrencyFormatter.format(owenChecking.getBalance()) + "，无透支保护");
        System.out.print("  取 100 → ");
        service.withdraw(owenChecking, 100);
        System.out.print("  存 25 → ");
        service.deposit(owenChecking, 25);
        System.out.print("  取 175 (余额只剩 125，无透支保护 → 必然失败) → ");
        service.withdraw(owenChecking, 175);
        System.out.println("  当前余额: " + CurrencyFormatter.format(owenChecking.getBalance()));

        System.out.println();
        System.out.println("【月末结算】给所有储蓄账户计算月利息");
        service.applyMonthlyInterest();
    }

    private void printFinalReport() {
        System.out.println();
        System.out.println("========== 最终报告 ==========");
        printCustomerReport();
    }
}

9.5 注意事项

import 用了 com.bank.domain.*，这是通配符导入，一次导入整个 domain 包

findCustomerByName 返回可能是 null，这里我们知道演示数据里肯定有，所以没做 null 检查。实际生产代码应该加上

关于 String.format("%.1f%%", ...)

这是 JDK 自带的字符串格式化方法，和 C 语言的 printf 类似：

%.1f：格式化浮点数，保留 1 位小数。比如 String.format("%.1f", 3.0) → "3.0"

%%：因为 % 在格式串里是特殊字符，想输出一个真正的 % 就得写 %%

所以 String.format("%.1f%%", 3.0) → "3.0%"，把 0.03 这种小数利率转成好读的百分比

关于 jane.getAccount(1) 的索引问题

jane.getAccount(1) 取的是 Jane 的支票账户，为什么索引是 1？因为 setupDemoData 里 Jane 先开储蓄（index 0）再开支票（index 1）。索引顺序完全取决于 addAccount 的调用顺序。如果你把 Maria 的两行开户顺序调一下（先开支票再开储蓄），她的 index 0 就是支票了。

关于 service.withdraw() 和 account.withdraw() 两种调用

demonstrateTransactions 里混用了两种取钱方式：

绝大多数情况用的是 service.withdraw(account, amount)——service 层帮你 catch OverdraftException，返回 true/false，打印友好提示

Jane 最后那笔取 470 用的是 janeChecking.withdraw(470)——直接调 domain 层的 withdraw，自己 try-catch。这是故意演示一下 OverdraftException 的直接使用方式，让你看到"异常对象里有 message 和 deficit 两个字段"。实际开发推荐统一用 service 层

第 10 节：编译运行 + 预期输出

10.1 编译

把 .java 源码变成 JVM 能执行的 .class 字节码。

方式 A：IDEA 里直接运行（推荐）

不用手动编译。写完代码后：

右键 Main.java（或文件里的 main 方法旁边会有绿色三角 ▶️）

点 Run 'Main.main()'

IDEA 自动帮你编译 + 运行，结果出现在底部控制台

方式 B：命令行 javac

⚠️ 命令行适合你想了解底层原理的时候。日常开发直接用 IDEA 即可。

# 第一步：创建输出目录（存放编译后的 .class 文件）

mkdir -p out

# 第二步：把所有 .java 编译到 out 目录

find src -name "*.java" | xargs javac -d out

逐条解释：

find src -name "*.java"：递归找出 src 目录下所有 .java 结尾的文件

|：管道符，把左边的输出作为右边的输入

xargs：把 find 找到的文件列表"展开"成命令行参数传给 javac

javac：Java 编译器，把源码翻译成字节码

-d out：指定编译后的 .class 文件输出到 out 目录（保持和源码一样的包结构）

如果是 Windows cmd，find ... | xargs 这一行不行。可以用 IDEA 编译，或者手动逐个 javac。

10.2 运行

方式 A：IDEA

上一步已经 Run 过了，结果就在底部控制台。

方式 B：命令行 java

java -cp out com.bank.ui.Main

逐条解释：

java：Java 虚拟机，执行 .class 文件

-cp out：-cp 是 classpath（类路径）的缩写，告诉 JVM "去 out 目录里找 .class 文件"

com.bank.ui.Main：要运行的主类。注意这里写的是全限定类名（包名 + 类名），不是文件名。JVM 会去 out/com/bank/ui/Main.class 找这个文件执行

10.3 预期输出

========== 初始化演示数据 ==========
已创建 4 个客户

========== 客户报告 ==========

客户: Simms, Jane
  ├─ 储蓄账户 (利率 3.0%): ¥500.00
  ├─ 支票账户 (透支额度 ¥500.00): ¥200.00

客户: Bryant, Owen
  ├─ 支票账户 (透支额度 ¥0.00): ¥200.00

客户: Soley, Tim
  ├─ 储蓄账户 (利率 2.5%): ¥1,500.00
  ├─ 支票账户 (透支额度 ¥0.00): ¥200.00

客户: Soley, Maria
  ├─ 支票账户 (透支额度 ¥0.00): ¥200.00
  ├─ 储蓄账户 (利率 3.0%): ¥150.00

========== 交易演示 ==========
...（省略中间）
========== 最终报告 ==========
（这里储蓄账户余额有小幅上涨，因为 applyMonthlyInterest 生效了）

如果能看到类似输出，恭喜你，自己写出来了整个系统！

第 11 节：完整文件清单

src/com/bank/
├── domain/
│   ├── Account.java              抽象基类
│   ├── SavingAccount.java        储蓄账户
│   ├── CheckingAccount.java      支票账户
│   ├── Customer.java             客户
│   └── Bank.java                 银行（单例）
├── exception/
│   └── OverdraftException.java   透支异常
├── service/
│   └── BankService.java          业务服务
├── util/
│   └── CurrencyFormatter.java    货币格式化
└── ui/
    ├── BankUI.java               交互式控制台菜单 + 报告
    └── Main.java                 入口

第 12 节：扩展 — 交互式控制台菜单

12.1 为什么要加交互？

到第十一节为止，项目已经能跑了，但它是个"一次性"程序——启动、自动跑完演示、打印报告、结束。用户没法手动操作。

加一个控制台菜单，用户启动后可以：

自己新开客户

自己给客户开户

手动存钱、取钱（包括体验透支失败）

打印报告随时看

查客户、月末结算

想玩多久玩多久，输入 0 才退出

12.2 设计思路

原教程里的 BankUI 是这样的结构：

public void run() {
    setupDemoData();          // 1. 建演示数据
    printCustomerReport();    // 2. 打报告
    demonstrateTransactions();// 3. 演示交易
    printFinalReport();       // 4. 再打报告
}

改成交互式的之后，run() 变成一个 while 循环，不停显示菜单、等用户选择、执行对应操作，直到用户输入 0 退出：

public void run() {
    askLoadDemoData();        // 可选：要不要加载演示数据

    while (true) {            // ← 死循环，不停显示菜单
        showMainMenu();
        int choice = readInt();

        switch (choice) {
            case 1: openCustomer(); break;
            case 2: openAccount();  break;
            case 3: deposit();      break;
            case 4: withdraw();     break;
            case 5: printReport();  break;
            case 6: findCustomer(); break;
            case 7: applyInterest();break;
            case 0: return;         // ← 唯一的退出点
            default: 提示错误;
        }
    }
}

12.3 要用到的新东西：Scanner

Java 内置的 Scanner 类用来从控制台读用户输入。需要导入 java.util.Scanner。

// 声明（放在类里作为字段，整个 BankUI 共用一个）
private final Scanner scanner = new Scanner(System.in);

// 读一行字符串
String name = scanner.nextLine();

// 读一个整数（需要自己处理非数字输入）
String line = scanner.nextLine();
int num = Integer.parseInt(line);

// 读一个小数
double amount = Double.parseDouble(line);

12.4 输入辅助方法：防崩

新手最容易犯的错：用户输入了字母，你用 Integer.parseInt("abc") 就会崩溃。所以我们写两个"安全"的输入方法：

// 安全读整数：如果用户输了字母，提示重新输，直到输对为止
private int readIntSafe() {
    while (true) {
        String line = scanner.nextLine().trim();
        try {
            return Integer.parseInt(line);
        } catch (NumberFormatException e) {
            System.out.print("请输入整数: ");
        }
    }
}

// 安全读小数：同上，而且要求 > 0
private double readDoubleSafe(String prompt) {
    System.out.print(prompt);
    while (true) {
        String line = scanner.nextLine().trim();
        try {
            double d = Double.parseDouble(line);
            if (d > 0) return d;
            System.out.print("金额必须大于 0，请重新输入: ");
        } catch (NumberFormatException e) {
            System.out.print("请输入有效数字: ");
        }
    }
}

12.5 菜单需要哪些方法？

每个菜单选项对应一个 private 方法：

菜单项 方法 做什么 1 openCustomer() 让用户输入姓名，调用 service 开户 2 openAccount() 先选客户，再选储蓄/支票，输余额和利率 3 deposit() 选客户 → 选账户 → 输金额 4 withdraw() 选客户 → 选账户 → 输金额（自动处理透支） 5 printCustomerReport() 原教程已有的方法，保留 6 findCustomer() 让用户输全名，调 Bank.findCustomerByName 7 applyMonthlyInterest() 原教程已有的方法，保留 辅助 selectCustomer() 列出所有客户，让用户选编号 辅助 selectAccount(Customer) 列出某个客户的所有账户

12.6 完整的 BankUI.java

把原教程里的 BankUI.java 整个替换成下面这个。其他所有文件（domain、exception、service、util、Main）完全不需要改——这就是分层架构的好处：UI 层怎么变，底下都不受影响。

package com.bank.ui;

import com.bank.domain.*;
import com.bank.service.BankService;
import com.bank.util.CurrencyFormatter;

import java.util.Scanner;

public class BankUI {

    private final BankService service = new BankService();
    private final Bank bank = Bank.getInstance();
    private final Scanner scanner = new Scanner(System.in);

    public void run() {
        System.out.println("========== 硅谷银行业务管理系统 ==========");
        System.out.println();
        askLoadDemoData();

        while (true) {
            showMainMenu();
            int choice = readIntSafe();

            switch (choice) {
                case 0:
                    System.out.println("感谢使用硅谷银行，再见！");
                    scanner.close();
                    return;
                case 1:
                    openCustomer();
                    break;
                case 2:
                    openAccount();
                    break;
                case 3:
                    deposit();
                    break;
                case 4:
                    withdraw();
                    break;
                case 5:
                    printCustomerReport();
                    break;
                case 6:
                    findCustomer();
                    break;
                case 7:
                    applyMonthlyInterest();
                    break;
                default:
                    System.out.println("无效选择，请输入 0-7 之间的数字");
            }
        }
    }

    private void askLoadDemoData() {
        System.out.print("是否加载演示数据 (y/n): ");
        String input = scanner.nextLine().trim().toLowerCase();
        if ("y".equals(input)) {
            setupDemoData();
        }
        System.out.println();
    }

    private void setupDemoData() {
        Customer jane = service.openCustomer("Jane", "Simms");
        service.openSavingAccount(jane, 500, 0.03);
        service.openCheckingAccount(jane, 200, 500);

        Customer owen = service.openCustomer("Owen", "Bryant");
        service.openCheckingAccount(owen, 200);

        Customer tim = service.openCustomer("Tim", "Soley");
        service.openSavingAccount(tim, 1500, 0.025);
        service.openCheckingAccount(tim, 200);

        Customer maria = service.openCustomer("Maria", "Soley");
        service.openCheckingAccount(maria, 200);
        service.openSavingAccount(maria, 150, 0.03);

        System.out.println("✓ 已加载 4 个客户的演示数据");
    }

    private void showMainMenu() {
        System.out.println();
        System.out.println("==========================================");
        System.out.println("  硅谷银行 —— 主菜单");
        System.out.println("  当前客户数: " + bank.getCustomerCount());
        System.out.println("------------------------------------------");
        System.out.println("  1. 新开客户");
        System.out.println("  2. 给客户开户 (储蓄/支票)");
        System.out.println("  3. 存钱");
        System.out.println("  4. 取钱");
        System.out.println("  5. 打印全部客户报告");
        System.out.println("  6. 按姓名查找客户");
        System.out.println("  7. 月末结算 (计算储蓄账户月利息)");
        System.out.println("  0. 退出");
        System.out.println("==========================================");
        System.out.print("请选择: ");
    }

    private int readIntSafe() {
        while (true) {
            String line = scanner.nextLine().trim();
            try {
                return Integer.parseInt(line);
            } catch (NumberFormatException e) {
                System.out.print("请输入整数: ");
            }
        }
    }

    private double readDoubleSafe(String prompt) {
        System.out.print(prompt);
        while (true) {
            String line = scanner.nextLine().trim();
            try {
                double d = Double.parseDouble(line);
                if (d > 0) return d;
                System.out.print("金额必须大于 0，请重新输入: ");
            } catch (NumberFormatException e) {
                System.out.print("请输入有效数字: ");
            }
        }
    }

    private Customer selectCustomer() {
        if (bank.getCustomerCount() == 0) {
            System.out.println("当前没有任何客户，请先新开客户");
            return null;
        }
        System.out.println();
        System.out.println("请选择客户:");
        for (int i = 0; i < bank.getCustomerCount(); i++) {
            Customer c = bank.getCustomer(i);
            System.out.println("  " + (i + 1) + ". " + c.getFullName() + " (" + c.getAccountCount() + " 个账户)");
        }
        System.out.print("输入客户编号: ");
        int idx = readIntSafe() - 1;
        if (idx < 0 || idx >= bank.getCustomerCount()) {
            System.out.println("✗ 无效的客户编号");
            return null;
        }
        return bank.getCustomer(idx);
    }

    private Account selectAccount(Customer customer) {
        if (customer.getAccountCount() == 0) {
            System.out.println(customer.getFullName() + " 还没有任何账户");
            return null;
        }
        System.out.println();
        System.out.println(customer.getFullName() + " 的账户列表:");
        for (int i = 0; i < customer.getAccountCount(); i++) {
            Account a = customer.getAccount(i);
            String type = a instanceof SavingAccount ? "储蓄账户" : "支票账户";
            System.out.println("  " + (i + 1) + ". " + type + "  余额: " + CurrencyFormatter.format(a.getBalance()));
        }
        System.out.print("输入账户编号: ");
        int idx = readIntSafe() - 1;
        if (idx < 0 || idx >= customer.getAccountCount()) {
            System.out.println("✗ 无效的账户编号");
            return null;
        }
        return customer.getAccount(idx);
    }

    private void openCustomer() {
        System.out.println();
        System.out.println("--- 新开客户 ---");
        System.out.print("名 (first name): ");
        String firstName = scanner.nextLine().trim();
        System.out.print("姓 (last name): ");
        String lastName = scanner.nextLine().trim();

        try {
            Customer customer = service.openCustomer(firstName, lastName);
            System.out.println("✓ 客户创建成功: " + customer.getFullName());
        } catch (IllegalArgumentException e) {
            System.out.println("✗ 创建失败: " + e.getMessage());
        }
    }

    private void openAccount() {
        System.out.println();
        System.out.println("--- 给客户开户 ---");
        Customer customer = selectCustomer();
        if (customer == null) return;

        System.out.println();
        System.out.println("选择账户类型:");
        System.out.println("  1. 储蓄账户 (按月计息)");
        System.out.println("  2. 支票账户 (可设置透支额度)");
        System.out.print("请选择: ");
        int type = readIntSafe();

        if (type != 1 && type != 2) {
            System.out.println("✗ 无效的账户类型");
            return;
        }

        double balance = readDoubleSafe("初始余额: ");

        if (type == 1) {
            double rate = readDoubleSafe("年利率 (例如 0.03 表示 3%): ");
            try {
                SavingAccount sa = service.openSavingAccount(customer, balance, rate);
                System.out.println("✓ 储蓄账户开户成功");
                System.out.println("  客户: " + customer.getFullName());
                System.out.println("  初始余额: " + CurrencyFormatter.format(balance));
                System.out.println("  年利率: " + String.format("%.1f%%", rate * 100));
            } catch (IllegalArgumentException e) {
                System.out.println("✗ 开户失败: " + e.getMessage());
            }
        } else {
            System.out.print("是否设置透支额度? (y/n): ");
            String hasOverdraft = scanner.nextLine().trim().toLowerCase();
            if ("y".equals(hasOverdraft)) {
                double limit = readDoubleSafe("透支额度: ");
                try {
                    CheckingAccount ca = service.openCheckingAccount(customer, balance, limit);
                    System.out.println("✓ 支票账户开户成功 (透支额度 " + CurrencyFormatter.format(limit) + ")");
                } catch (IllegalArgumentException e) {
                    System.out.println("✗ 开户失败: " + e.getMessage());
                }
            } else {
                CheckingAccount ca = service.openCheckingAccount(customer, balance);
                System.out.println("✓ 支票账户开户成功 (无透支保护)");
            }
        }
    }

    private void deposit() {
        System.out.println();
        System.out.println("--- 存钱 ---");
        Customer customer = selectCustomer();
        if (customer == null) return;
        Account account = selectAccount(customer);
        if (account == null) return;

        System.out.println("当前余额: " + CurrencyFormatter.format(account.getBalance()));
        double amount = readDoubleSafe("存入金额: ");
        try {
            service.deposit(account, amount);
            System.out.println("✓ 存钱成功");
            System.out.println("  存入: " + CurrencyFormatter.format(amount));
            System.out.println("  当前余额: " + CurrencyFormatter.format(account.getBalance()));
        } catch (IllegalArgumentException e) {
            System.out.println("✗ 存钱失败: " + e.getMessage());
        }
    }

    private void withdraw() {
        System.out.println();
        System.out.println("--- 取钱 ---");
        Customer customer = selectCustomer();
        if (customer == null) return;
        Account account = selectAccount(customer);
        if (account == null) return;

        System.out.println("当前余额: " + CurrencyFormatter.format(account.getBalance()));
        if (account instanceof CheckingAccount) {
            CheckingAccount ca = (CheckingAccount) account;
            double available = account.getBalance() + ca.getAvailableCredit();
            System.out.println("可用总额 (含透支): " + CurrencyFormatter.format(available));
        }
        double amount = readDoubleSafe("取出金额: ");
        try {
            boolean success = service.withdraw(account, amount);
            if (success) {
                System.out.println("✓ 取钱成功");
                System.out.println("  取出: " + CurrencyFormatter.format(amount));
                System.out.println("  当前余额: " + CurrencyFormatter.format(account.getBalance()));
            }
        } catch (IllegalArgumentException e) {
            System.out.println("✗ 取钱失败: " + e.getMessage());
        }
    }

    private void printCustomerReport() {
        System.out.println();
        System.out.println("========== 全部客户报告 ==========");
        System.out.println("客户总数: " + bank.getCustomerCount());

        if (bank.getCustomerCount() == 0) {
            System.out.println("  暂无客户数据");
        }

        for (Customer customer : bank.getCustomers()) {
            System.out.println();
            System.out.println("客户: " + customer.getLastName() + ", " + customer.getFirstName()
                + " (" + customer.getAccountCount() + " 个账户)");

            for (Account account : customer.getAccounts()) {
                if (account instanceof SavingAccount) {
                    SavingAccount sa = (SavingAccount) account;
                    System.out.println("  ├─ 储蓄账户 (利率 "
                        + String.format("%.1f%%", sa.getInterestRate() * 100)
                        + ")  余额: " + CurrencyFormatter.format(account.getBalance()));
                } else if (account instanceof CheckingAccount) {
                    CheckingAccount ca = (CheckingAccount) account;
                    System.out.println("  ├─ 支票账户 (透支额度 "
                        + CurrencyFormatter.format(ca.getOverdraftLimit())
                        + ")  余额: " + CurrencyFormatter.format(account.getBalance())
                        + "  可用透支: " + CurrencyFormatter.format(ca.getAvailableCredit()));
                }
            }
        }

        System.out.println();
        System.out.println("==================================");
    }

    private void findCustomer() {
        System.out.println();
        System.out.println("--- 按姓名查找客户 ---");
        System.out.print("请输入客户全名 (例如 Jane Simms): ");
        String fullName = scanner.nextLine().trim();
        Customer customer = bank.findCustomerByName(fullName);
        if (customer == null) {
            System.out.println("未找到客户: " + fullName);
        } else {
            System.out.println("✓ 找到客户: " + customer.getFullName());
            System.out.println("  账户数量: " + customer.getAccountCount());
            for (Account account : customer.getAccounts()) {
                String type = account instanceof SavingAccount ? "储蓄" : "支票";
                System.out.println("  - " + type + "账户: " + CurrencyFormatter.format(account.getBalance()));
            }
        }
    }

    private void applyMonthlyInterest() {
        System.out.println();
        System.out.println("--- 月末结算 ---");
        service.applyMonthlyInterest();
        System.out.println("✓ 已为所有储蓄账户计算月利息");
        System.out.println("提示: 选择菜单 5 查看结算后的完整报告");
    }
}

12.7 新增 import

BankUI.java 比原来多了一个 import：

import java.util.Scanner;

12.8 预期运行效果

========== 硅谷银行业务管理系统 ==========

是否加载演示数据 (y/n): y
✓ 已加载 4 个客户的演示数据

==========================================
  硅谷银行 —— 主菜单
  当前客户数: 4
---------------

1. 新开客户
2. 给客户开户 (储蓄/支票)
3. 存钱
4. 取钱
5. 打印全部客户报告
6. 按姓名查找客户
7. 月末结算 (计算储蓄账户月利息)
8. 退出
   ==========================================
   请选择: 3

--- 存钱 ---

请选择客户:

1. Jane Simms (2 个账户)
2. Owen Bryant (1 个账户)
3. Tim Soley (2 个账户)
4. Maria Soley (2 个账户)
   输入客户编号: 1

Jane Simms 的账户列表:

1. 储蓄账户  余额: ¥500.00
2. 支票账户  余额: ¥200.00
   输入账户编号: 2
   当前余额: ¥200.00
   存入金额: 800
   ✓ 存钱成功
   存入: ¥800.00
   当前余额: ¥1,000.00

==========================================
  ...菜单继续显示...
====================

请选择: 0
感谢使用硅谷银行，再见！

12.9 本节总结

知识点 一句话 Scanner Java 内置类，读控制台输入 while (true) + switch 做菜单最经典的模式——死循环里 switch 分发 安全输入方法 try-catch + 死循环，输错就重输，不崩 分层架构的好处 只改 UI 层，底下 4 层一个字没动 方法拆分 selectCustomer() / selectAccount() 让每个功能方法只有几行，好读好维护

第 13 节：架构图解大节

学完代码回头看架构，就像站在山顶看全貌。这一节把整个项目用图的方式重新呈现一遍，帮你把零散的知识点串成一张网。

13.1 项目整体架构图（分层架构）

┌─────────────────────────────────────────────────────────────────────┐
│                          用户 (User)                                │
│                     你，通过控制台输入                                 │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ 输入数字、姓名、金额
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    UI 层 (presentation)                              │
│                                                                     │
│  BankUI.java         ←  交互式菜单、Scanner 输入、System.out 输出    │
│  Main.java           ←  程序入口，三行代码                            │
│                                                                     │
│  职责：和用户打交道，不包含任何业务逻辑                                │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ 调用 service 的方法
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                   Service 层 (业务逻辑)                              │
│                                                                     │
│  BankService.java    ←  openCustomer / deposit / withdraw / ...     │
│                                                                     │
│  职责：把 domain 对象组合起来完成一件业务事                              │
│        catch OverdraftException 并打印友好提示                        │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ 操作 domain 对象
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    Domain 层 (核心数据)                               │
│                                                                     │
│  Bank.java            ←  单例银行，持有所有客户列表                    │
│  Customer.java        ←  客户，持有多个账户                            │
│  Account.java         ←  抽象账户基类                                │
│    ├─ SavingAccount.java   ←  储蓄账户 + 利率                         │
│    └─ CheckingAccount.java ←  支票账户 + 透支额度                     │
│                                                                     │
│  职责：纯粹的数据 + 数据上的操作方法，不知道 UI 也不知道 Service        │
└──────────────────────────┬──────────────────────────────────────────┘
                           │ 可能抛异常
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  Exception 层 (异常定义)                             │
│                                                                     │
│  OverdraftException.java   ←  取款超过余额（含透支）时抛出              │
│                                                                     │
│  职责：定义项目专属的异常类型                                          │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                    Util 层 (工具辅助)                                 │
│                                                                     │
│  CurrencyFormatter.java  ←  把数字格式化成 "¥1,234.56" 这样的字符串     │
│                                                                     │
│  职责：可复用的小工具，任何层都能调用                                   │
└─────────────────────────────────────────────────────────────────────┘

分层的核心好处：

改 UI 不改底层：我们把自动演示改成交互式菜单，只改了 BankUI.java，其他 8 个文件一个字没动

改底层不改 UI：如果以后要加一个"定期存款账户"，只要继承 Account 写一个新类，UI 层几乎不用改

每层可独立测试：domain 层的 withdraw 逻辑可以单独写测试，不需要启动整个程序

13.2 类关系图（UML 风格）

                          ┌──────────────┐
                          │  Overdraft   │
                          │  Exception   │
                          └──────────────┘
                                 ▲
                                 │ extends Exception
                                 │

                 ┌───────────────────────────────────┐
                 │           Account                 │  ← abstract
                 │  ───────────────────────          │
                 │  # balance: double                │  # = protected
                 │  ───────────────────────          │
                 │  + getBalance(): double           │  + = public
                 │  + deposit(double): void          │
                 │  + withdraw(double): void         │
                 │  + getAccountType(): String {ab}  │  {ab} = abstract
                 │  + toString(): String             │
                 └───────┬───────────────┬──────────┘
                         │               │
              extends    │               │    extends
                         ▼               ▼
            ┌─────────────────┐   ┌─────────────────────┐
            │  SavingAccount  │   │   CheckingAccount   │
            │  ─────────────  │   │  ────────────────   │
            │  - interestRate │   │  - overdraftLimit    │  - = private
            │  ─────────────  │   │  ────────────────   │
            │  + calculate    │   │  + getAvailable     │
            │    MonthlyInt() │   │    Credit(): double │
            │  + apply        │   │  + withdraw(double) │  ← override!
            │    MonthlyInt() │   │  + getAccountType() │
            │  + getAccount   │   └─────────────────────┘
            │    Type()       │
            └─────────────────┘

    ┌───────────────────────────┐         ┌─────────────────────┐
    │        Customer           │         │       Bank           │
    │  ──────────────────────   │         │  ──────────────────  │
    │  - firstName: String      │         │  - customers: List   │
    │  - lastName: String       │         │    <Customer></customer>        │
    │  - accounts: List         │         │  - INSTANCE: Bank    │
    │    <Account></account>              │         │  ──────────────────  │
    │  ──────────────────────   │         │  + getInstance(): Bank│
    │  + addAccount(Account)    │         │  + addCustomer(...)  │
    │  + getAccount(int)       │         │  + getCustomer(int)  │
    │  + getAccounts()          │         │  + findCustomerBy()  │
    │  + getFullName()          │         └──────────┬──────────┘
    └───────────┬───────────────┘                    │
                │                                    │
     1 customer 关系                                  │
     有多个账户                                       │ 1 Bank 持有所有 Customer
                │                                    │
                ▼                                    ▼
         List<Account></account>                        List<Customer></customer>

    ┌───────────────────────────┐
    │     BankService           │
    │  ──────────────────────   │
    │  - bank: Bank             │  ← 持有 Bank 单例
    │  ──────────────────────   │
    │  + openCustomer(...)      │
    │  + openSavingAccount(...) │
    │  + openCheckingAccount()  │
    │  + deposit(Account, dbl)  │
    │  + withdraw(Account, dbl) │  ← 内部 try-catch
    │  + applyMonthlyInterest() │
    └───────────────────────────┘

    ┌───────────────────────────┐
    │  CurrencyFormatter        │  ← final 类，不能继承
    │  ──────────────────────   │
    │  - CNY: NumberFormat      │
    │  - CurrencyFormatter()    │  ← private 构造器
    │  ──────────────────────   │
    │  + format(double): String │  ← static 方法
    └───────────────────────────┘

核心关系：

Account → SavingAccount、CheckingAccount：继承（is-a），储蓄账户"是一种"账户

Bank → Customer：聚合（has-a），银行"包含"多个客户

Customer → Account：聚合（has-a），客户"拥有"多个账户

BankService → Bank：组合，Service 内部持有 Bank 单例来操作

13.3 对象创建时序图（以"给 Jane 开储蓄账户"为例）

用户         BankUI        BankService       Bank         Customer      SavingAccount
 │              │              │              │              │               │
 │ "开户"       │              │              │              │               │
 │─────────────>│              │              │              │               │
 │              │ selectCust() │              │              │               │
 │              │─────────────>│              │              │               │
 │              │              │  已经有 Jane │              │               │
 │              │              │<───────────────────────────│               │
 │              │              │              │              │               │
 │              │ openSaving   │              │              │               │
 │              │─────────────>│              │              │               │
 │              │              │ new Saving   │              │               │
 │              │              │────────────────────────────────────────────>│
 │              │              │              │              │               │
 │              │              │<────────────────────────────────────────────│
 │              │              │  account    │              │               │
 │              │              │              │              │               │
 │              │              │ cust.addAcct │              │               │
 │              │              │─────────────>│              │  .add()       │
 │              │              │              │              │──────────────>│
 │              │              │              │              │               │
 │              │<─────────────│ account 返回 │              │               │
 │              │              │              │              │               │
 │<─────────────│ "开户成功"   │              │              │               │
 │              │              │              │              │               │

关键点：Service 层是"中间人"——它创建 SavingAccount 对象，然后让 Customer 把这个账户加进自己的列表。Service 不自己持有 Account，Account 由 Customer 持有。

13.4 内存结构图（运行时对象长什么样）

当 setupDemoData() 执行完毕后，JVM 堆内存里的对象关系：

JVM 堆内存
│
├─ Bank (单例，只有一个)
│  │
│  └─ customers ──> ArrayList<Customer></customer>
│                    │
│                    ├─ [0] Customer "Jane Simms"
│                    │     ├─ firstName = "Jane"
│                    │     ├─ lastName  = "Simms"
│                    │     └─ accounts ──> ArrayList<Account></account>
│                    │                       ├─ [0] SavingAccount
│                    │                       │     ├─ balance = 500
│                    │                       │     ├─ interestRate = 0.03
│                    │                       │     └─ (继承自 Account)
│                    │                       │
│                    │                       └─ [1] CheckingAccount
│                    │                             ├─ balance = 200
│                    │                             ├─ overdraftLimit = 500
│                    │                             └─ (继承自 Account)
│                    │
│                    ├─ [1] Customer "Owen Bryant"
│                    │     └─ accounts ──> [CheckingAccount(balance=200)]
│                    │
│                    ├─ [2] Customer "Tim Soley"
│                    │     └─ accounts ──> [SavingAccount, CheckingAccount]
│                    │
│                    └─ [3] Customer "Maria Soley"
│                          └─ accounts ──> [CheckingAccount, SavingAccount]
│
└─ BankService (BankUI 里 new 出来的，持有上面那个 Bank 的引用)

关键理解：

Bank 是单例，但 BankService 不是单例。BankService 每次 new BankService() 都会创建新对象，但它内部的 bank 字段始终指向同一个 Bank 实例

所有对象之间通过引用（内存地址）互相关联，不是复制。比如 BankService.bank 和 BankUI.bank 里存的是同一个 Bank 对象的地址

ArrayList 里存的也是引用，不是对象本身

13.5 异常传播路径图（以 CheckingAccount 透支失败为例）

用户取钱 800
    │
    ▼
BankUI.withdraw()
    │  service.withdraw(account, 800)
    ▼
BankService.withdraw()
    │  try {
    │    account.withdraw(800);
    │  } catch (OverdraftException e) { ... }
    │
    ▼  ← 进入 Account.withdraw()？不！
    │  多态调用，实际是 CheckingAccount.withdraw()
    │
    ▼
CheckingAccount.withdraw(800)
    │  available = balance(200) + getAvailableCredit(300) = 500
    │  500 >= 800？不满足！
    │
    │  throw new OverdraftException("超出可用额度", 800 - 500 = 300)
    │
    ▼  ← 异常从这里抛出
    │
BankService.withdraw()  ← 这里 catch 到了！
    │  e.getMessage() → "超出可用额度"
    │  e.getDeficit() → 300.0
    │  print → "取款失败: 超出可用额度（含透支） (差额 ¥300.00)"
    │  return false
    │
    ▼
BankUI.withdraw()  ← 收到 false，什么都不打印（失败提示已经在 service 层打印了）

关键：异常像接力棒一样往上抛，直到某个 try-catch 接住它。这个项目里 OverdraftException 的接力路径是：

CheckingAccount.withdraw()  (抛出)
    → BankService.withdraw()  (接住，处理)
    → BankUI 不需要管

如果 BankService 也不 catch，异常会继续往上抛到 BankUI，再往上就是 JVM，程序崩溃。

13.6 数据流图（存钱的完整路径）

用户输入 "1000" (存入金额)
    │
    ▼
BankUI.readDoubleSafe("存入金额: ")
    │  Scanner.nextLine() → "1000" → Double.parseDouble("1000") → 1000.0
    │  返回 1000.0 这个 double 值
    │
    ▼
BankUI.deposit()
    │  service.deposit(account, 1000.0)
    │
    ▼
BankService.deposit(account, 1000.0)
    │  account.deposit(1000.0)
    │  (注意：这里没有 try-catch，因为 deposit 只抛 IllegalArgumentException，
    │   这是 unchecked exception，而且 BankService 觉得 IllegalArgumentException
    │   应该往上抛，让 UI 层处理)
    │
    ▼
Account.deposit(1000.0)
    │  if (1000.0 <= 0) → false，跳过
    │  balance += 1000.0 → balance 从 200 变成 1200
    │  方法正常返回
    │
    ▼
BankService.deposit() 正常返回
    │
    ▼
BankUI.deposit()
    │  打印 "✓ 存钱成功" + 新余额

13.7 设计模式清单

这个项目虽然小，但用了好几个经典设计模式：

设计模式 用在哪 一句话解释 单例模式 Bank.java 整个 JVM 只有一个 Bank 实例，getInstance() 控制创建 模板方法模式 Account + SavingAccount + CheckingAccount Account 定义 withdraw 骨架，子类重写部分逻辑。withdraw 的"检查金额>0"在父类，"判断余额够不够"在子类 策略模式（雏形） Account.getAccountType() 被多态调用 不同的 Account 类型返回不同的类型字符串，调用方不需要 if-else 判断 工厂模式（简化版） BankService.openCustomer() 等方法 Service 层负责创建对象，调用方不用 new，只用告诉 Service "我要开个客户" 不可变对象模式 Customer、SavingAccount、CheckingAccount 的 final 字段 firstName、interestRate、overdraftLimit 用 final 修饰，创建后不能改，数据安全

13.8 防污染设计：为什么 getAccounts() 返回 unmodifiableList

// Customer.java
public List<Account></account> getAccounts() {
    return Collections.unmodifiableList(accounts);
}

如果直接返回 accounts 会怎样？

Customer jane = bank.getCustomer(0);
List<Account></account> list = jane.getAccounts();

// 恶意代码：偷偷清空 Jane 的账户列表！
list.clear();

// 或者插入一个假账户
list.add(new SavingAccount(99999, 0.5));

用 Collections.unmodifiableList() 包一层后：

返回的列表是只读的，调 .add() .remove() .clear() 会抛 UnsupportedOperationException

但注意：这只是浅不可变，列表里的 Account 对象本身还是可以改的（比如 account.setBalance()）。不过我们的 Account 没有 setter，balance 只能通过 deposit/withdraw 改，所以实际上已经很安全了

第 14 节：逐行代码精讲

这一节把每个 Java 文件的每一行代码都解释清楚。建议你对照着源码，一行一行看。看不懂的地方反复看，直到完全明白。

14.1 OverdraftException.java 逐行精讲

package com.bank.exception;                         // 【行1】声明这个类属于 com.bank.exception 包
                                                    //       包名必须和文件夹路径一致：src/com/bank/exception/
                                                    //       所有 Java 文件第一行必须是 package（如果有的话）

public class OverdraftException extends Exception { // 【行3-4】public 表示公共类，谁都能访问
                                                    //       extends Exception 表示继承 Java 标准库里的 Exception 类
                                                    //       Exception 是所有 checked exception 的父类
                                                    //       继承它之后，这个类自动就有了异常的"身份"

    private final double deficit;                    // 【行6】final 字段，一旦赋值就不能改
                                                    //       deficit 记录"还差多少钱才能取到"
                                                    //       比如要取 800，可用额度 500，deficit = 300

    public OverdraftException(String message, double deficit) {  // 【行8】构造器，创建异常对象时用
        super(message);                             // 【行9】调用父类 Exception 的构造器，把 message 传进去
                                                    //       这个 message 会存在异常对象里，以后 e.getMessage() 就能取到
        this.deficit = deficit;                     // 【行10】用参数 deficit 给字段 deficit 赋值
                                                    //       this. 区分左边是字段，右边是参数
    }

    public double getDeficit() {                    // 【行12】getter 方法，外部通过它读取 deficit
        return deficit;                             // 【行13】返回 deficit 的值
    }                                               // 【行14】getter 结束
}                                                   // 【行15】类结束

为什么就这么短？ 因为 Exception 父类已经帮我们做了大部分事情——message、堆栈跟踪（stack trace）这些都在父类里，我们只需要加一个 deficit 字段。

14.2 Account.java 逐行精讲

package com.bank.domain;                            // 属于 domain 包

import com.bank.exception.OverdraftException;       // 导入 OverdraftException 类
                                                    //       不写这行的话，下面写 throws OverdraftException 就不认识

public abstract class Account {                     // 【行5】abstract 表示抽象类——不能 new Account()
                                                    //       只能 new 它的子类 SavingAccount 和 CheckingAccount

    protected double balance;                       // 【行7】protected 意味着：
                                                    //       ① 同包的类可以访问（比如 Customer 在 domain 包，不行）
                                                    //       ② 子类可以直接访问（SavingAccount 和 CheckingAccount 都行）
                                                    //       ③ 外部包的类不行（BankService 在 service 包，不行）
                                                    //       为什么不用 private？因为 CheckingAccount 里要直接读 balance 算透支

    public Account(double initialBalance) {         // 【行9】构造器，创建账户对象时必须传初始余额
        if (initialBalance < 0) {                   // 【行10】防御性检查：初始余额不能是负数
            throw new IllegalArgumentException(      // 【行11】直接抛出 IllegalArgumentException
                "初始余额不能为负数"                   //       这是 Java 标准异常，不需要我们自己定义
            );                                      //       IllegalArgumentException 是 unchecked exception
        }                                           // 【行12】if 结束
        this.balance = initialBalance;              // 【行13】用参数给字段赋值
    }

    public double getBalance() {                     // 【行16】getter，外部读取余额的唯一方式
        return balance;                             // 【行17】直接返回
    }

    public void deposit(double amount) {            // 【行20】存钱方法，没有返回值
        if (amount <= 0) {                           // 【行21】防御：存 0 或负数没意义
            throw new IllegalArgumentException(
                "存款金额必须大于 0"
            );
        }
        balance += amount;                          // 【行25】balance = balance + amount
    }

    public void withdraw(double amount)              // 【行28】取钱方法
        throws OverdraftException {                 //       throws 声明：我可能抛 OverdraftException
                                                    //       调用方必须 try-catch 或继续 throws
        if (amount <= 0) {                           // 【行30】先检查金额合法性
            throw new IllegalArgumentException(
                "取款金额必须大于 0"
            );
        }
        if (balance >= amount) {                     // 【行34】余额够不够？
            balance -= amount;                       // 【行35】够：直接减
        } else {                                     // 【行36】不够：
            throw new OverdraftException(            // 【行37】创建并抛出我们自定义的异常
                "余额不足",                           //       message 给人看
                amount - balance                     //       deficit = 差多少钱
            );
        }
    }

    public abstract String getAccountType();          // 【行42】抽象方法——没有方法体
                                                    //       子类必须实现这个方法，否则子类也得是 abstract
                                                    //       作用：让 Account 知道自己是什么类型
                                                    //       SavingAccount 返回 "储蓄账户"，CheckingAccount 返回 "支票账户"

    @Override                                        // 【行45】@Override 是注解，告诉编译器"我要重写父类方法"
    public String toString() {                       // 【行46】重写 Object 的 toString()
        return getAccountType()                      // 【行47】调用上面的抽象方法（多态！）
            + "{balance=" + balance + "}";           //       拼接字符串
    }
}

14.3 SavingAccount.java 逐行精讲

package com.bank.domain;

public class SavingAccount extends Account {         // 【行3】继承 Account，自动有了 balance、deposit、withdraw

    private final double interestRate;               // 【行5】利率，final 表示开户后不变
                                                    //       比如 0.03 表示年利率 3%

    public SavingAccount(double initialBalance, double interestRate) {  // 【行7】双参数构造器
        super(initialBalance);                       // 【行8】调用父类 Account 的构造器，必须在第一行！
                                                    //       super() 完成了 balance 字段的初始化
        if (interestRate < 0) {                      // 【行9】利率不能为负
            throw new IllegalArgumentException(
                "利率不能为负数"
            );
        }
        this.interestRate = interestRate;            // 【行13】给利率字段赋值
    }

    public double getInterestRate() {                // 【行15】getter
        return interestRate;
    }

    public double calculateMonthlyInterest() {        // 【行19】算月利息，不修改 balance
        return balance * interestRate / 12;          // 【行20】公式：余额 × 年利率 ÷ 12个月
                                                    //       比如 balance=1000, rate=0.03
                                                    //       月利息 = 1000 × 0.03 / 12 = 2.5
    }

    public void applyMonthlyInterest() {              // 【行23】把利息真的加到余额上
        balance += calculateMonthlyInterest();        // 【行24】先算出来，再加
                                                    //       注意：直接访问 balance 没问题
                                                    //       因为父类 Account 的 balance 是 protected
    }

    @Override
    public String getAccountType() {                  // 【行28】实现父类的抽象方法
        return "储蓄账户";                            // 【行29】固定返回 "储蓄账户"
    }
}

14.4 CheckingAccount.java 逐行精讲

package com.bank.domain;

import com.bank.exception.OverdraftException;        // 因为重写的 withdraw 要 throws OverdraftException

public class CheckingAccount extends Account {

    private final double overdraftLimit;              // 【行6】透支额度，比如 500 表示最多可以欠银行 500

    public CheckingAccount(double initialBalance, double overdraftLimit) {  // 【行8】双参数构造器
        super(initialBalance);
        if (overdraftLimit < 0) {
            throw new IllegalArgumentException(
                "透支额度不能为负数"
            );
        }
        this.overdraftLimit = overdraftLimit;
    }

    public CheckingAccount(double initialBalance) {   // 【行16】单参数构造器，方便不开透支时用
        this(initialBalance, 0);                      // 【行17】this(...) 调用本类的另一个构造器
                                                    //       等价于"额度为 0 的支票账户"
    }

    public double getOverdraftLimit() {               // 【行21】getter
        return overdraftLimit;
    }

    public double getAvailableCredit() {              // 【行25】计算当前还剩多少透支额度可用
        double used = Math.max(0, -balance);          // 【行26】这是整个类最巧妙的一行
                                                    //       Math.max(0, x) 意思是"如果 x 比 0 小，取 0"
                                                    //       情况 1: balance = 100 (正)
                                                    //              -balance = -100, max(0, -100) = 0 → 没用到透支
                                                    //       情况 2: balance = -200 (负，已经用了透支)
                                                    //              -balance = 200, max(0, 200) = 200 → 用了 200
        return overdraftLimit - used;                 // 【行27】剩余额度 = 总额度 - 已用
    }

    @Override
    public void withdraw(double amount)              // 【行30】重写父类的 withdraw！
        throws OverdraftException {                  //       签名和父类一样：返回值 void，参数 double，throws OverdraftException

        if (amount <= 0) {                           // 【行32】和父类一样的参数检查（重复但安全）
            throw new IllegalArgumentException(
                "取款金额必须大于 0"
            );
        }

        double available = balance + getAvailableCredit();  // 【行37】关键！可用总额 = 余额 + 剩余透支额度
                                                    //       比如 balance=200, 透支500, 已用0
                                                    //       available = 200 + 500 = 700 → 最多能取 700

        if (available >= amount) {                   // 【行39】可用够不够取？
            balance -= amount;                        // 【行40】够：正常扣钱（余额可能变成负数！透支了）
        } else {
            throw new OverdraftException(             // 【行42】不够：抛异常
                "超出可用额度（含透支）",
                amount - available                    //       deficit = 还缺多少
            );
        }
    }

    @Override
    public String getAccountType() {                  // 【行47】实现抽象方法
        return "支票账户";
    }
}

14.5 Customer.java 逐行精讲

package com.bank.domain;

import java.util.ArrayList;                          // 导入 ArrayList 类
import java.util.Collections;                        // 导入 Collections 工具类
import java.util.Iterator;                           // 导入迭代器接口
import java.util.List;                               // 导入 List 接口

public class Customer {

    private final String firstName;                   // 【行9】final，创建后不能改
    private final String lastName;                    // 【行10】final
    private final List<Account></account> accounts              // 【行11】accounts 字段声明时就初始化
        = new ArrayList<>();                         //       这样 accounts 永远不会是 null

    public Customer(String firstName, String lastName) {
        if (firstName == null || firstName.isEmpty()) {  // 【行14】null 检查 + 空串检查
            throw new IllegalArgumentException("名不能为空");
        }
        if (lastName == null || lastName.isEmpty()) {
            throw new IllegalArgumentException("姓不能为空");
        }
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String getFirstName() { return firstName; }   // 【行21】getter 一行搞定
    public String getLastName() { return lastName; }     // 【行22】getter

    public String getFullName() {                     // 【行24】合成方法
        return firstName + " " + lastName;            // 【行25】简单拼接
    }

    public void addAccount(Account account) {         // 【行27】给这个客户加一个账户
        if (account == null) {
            throw new IllegalArgumentException("账户不能为空");
        }
        accounts.add(account);                        // 【行31】ArrayList 的 add 方法，自动扩容
    }

    public int getAccountCount() {                    // 【行33】账户数量
        return accounts.size();                       // ArrayList.size() 返回元素个数
    }

    public Account getAccount(int index) {             // 【行36】按索引取账户
        if (index < 0 || index >= accounts.size()) {
            throw new IndexOutOfBoundsException(      // 【行38】越界检查
                "账户索引越界: " + index
            );
        }
        return accounts.get(index);                   // ArrayList.get(index)
    }

    public List<Account></account> getAccounts() {               // 【行43】返回整个账户列表
        return Collections.unmodifiableList(accounts);  // 【行44】防污染！返回只读视图
                                                    //       外部拿到这个 list 调 .add() 会抛 UnsupportedOperationException
    }

    public Iterator<Account></account> accountIterator() {       // 【行47】返回迭代器（本项目没用到，但提供了更灵活的遍历方式）
        return accounts.iterator();
    }

    @Override
    public String toString() {                         // 【行51】重写 Object 的 toString
        return "Customer{" + getFullName()            // 【行52】
            + ", accounts=" + accounts.size() + "}";
    }
}

14.6 Bank.java 逐行精讲

package com.bank.domain;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

public class Bank {

    private final List<Customer></customer> customers             // 【行9】和 Customer.accounts 一样，声明时初始化
        = new ArrayList<>();

    private Bank() {                                   // 【行12】⚠️ private 构造器！
                                                    //       这是单例模式的关键——外部不能 new Bank()
                                                    //       只能通过下面的 getInstance() 获取
    }

    private static final Bank INSTANCE                 // 【行14】静态常量，类加载时就创建好
        = new Bank();                                 //       因为是 static，整个 JVM 只有一个
                                                    //       因为是 final，创建后不能换

    public static Bank getInstance() {                 // 【行18】唯一的获取入口
        return INSTANCE;                               //       每次调用都返回同一个对象
    }

    public Customer addCustomer(String firstName, String lastName) {
        Customer customer = new Customer(firstName, lastName);  // 【行22】创建客户对象
        customers.add(customer);                       // 【行23】加进列表
        return customer;                               // 【行24】返回创建好的客户（方便调用方继续操作）
    }

    public int getCustomerCount() {
        return customers.size();
    }

    public Customer getCustomer(int index) {
        if (index < 0 || index >= customers.size()) {
            throw new IndexOutOfBoundsException("客户索引越界: " + index);
        }
        return customers.get(index);
    }

    public List<Customer></customer> getCustomers() {
        return Collections.unmodifiableList(customers);  // 同样的防污染设计
    }

    public Iterator<Customer></customer> customerIterator() {
        return customers.iterator();
    }

    public Customer findCustomerByName(String fullName) {  // 【行41】按全名查找
        for (Customer c : customers) {                   // 【行42】for-each 循环遍历所有客户
            if (c.getFullName().equals(fullName)) {       // 【行43】equals 比较字符串内容
                return c;                                //       找到了立刻返回
            }
        }
        return null;                                      // 【行46】遍历完没找到，返回 null
    }
}

14.7 CurrencyFormatter.java 逐行精讲

package com.bank.util;

import java.text.NumberFormat;                        // 数字格式化类
import java.util.Locale;                              // 地区/语言设置类

public final class CurrencyFormatter {                 // 【行5】final 类——不能被继承！
                                                    //       工具类通常设计成 final，防止别人继承后乱改

    private static final NumberFormat CNY              // 【行7】静态常量，中国人民币格式
        = NumberFormat.getCurrencyInstance(Locale.CHINA);
                                                    //       NumberFormat.getCurrencyInstance() 返回一个格式化器
                                                    //       Locale.CHINA 让它用中国习惯：¥ 符号、千分位逗号

    private CurrencyFormatter() {                      // 【行10】private 构造器
                                                    //       工具类不应该被 new，只应该调静态方法
                                                    //       private 防止外部误写 new CurrencyFormatter()
    }

    public static String format(double amount) {        // 【行13】静态方法，直接类名调
        return CNY.format(amount);                     // 【行14】比如 format(1234.5) → "¥1,234.50"
    }
}

14.8 BankService.java 逐行精讲

package com.bank.service;

import com.bank.domain.Account;
import com.bank.domain.Bank;
import com.bank.domain.CheckingAccount;
import com.bank.domain.Customer;
import com.bank.domain.SavingAccount;
import com.bank.exception.OverdraftException;
import com.bank.util.CurrencyFormatter;

public class BankService {

    private final Bank bank = Bank.getInstance();      // 【行12】持有 Bank 单例的引用
                                                    //       注意：不是 Bank bank = new Bank()！
                                                    //       因为 Bank 的构造器是 private，new 不了
                                                    //       必须 getInstance()

    public Customer openCustomer(String firstName, String lastName) {
        return bank.addCustomer(firstName, lastName);  // 【行15】直接委托给 Bank
                                                    //       Service 自己不创建 Customer
                                                    //       让 Bank 创建然后加进 customers 列表
    }

    public SavingAccount openSavingAccount(Customer customer, double balance, double rate) {
        SavingAccount account = new SavingAccount(balance, rate);  // 【行18】Service 负责 new 账户
        customer.addAccount(account);                  // 【行19】然后让 Customer 把账户加进自己的列表
        return account;                                // 【行20】返回新建的账户
    }

    public CheckingAccount openCheckingAccount(Customer customer, double balance) {
        return openCheckingAccount(customer, balance, 0);  // 【行23】单参数版本调用三参数版本，默认额度 0
    }

    public CheckingAccount openCheckingAccount(Customer customer, double balance, double overdraftLimit) {
        CheckingAccount account = new CheckingAccount(balance, overdraftLimit);
        customer.addAccount(account);
        return account;
    }

    public void deposit(Account account, double amount) {
        account.deposit(amount);                       // 【行31】直接调 account.deposit()
                                                    //       不 try-catch，因为 IllegalArgumentException
                                                    //       会直接往上抛到 UI 层
    }

    public boolean withdraw(Account account, double amount) {
        try {                                          // 【行36】这里 try-catch OverdraftException
            account.withdraw(amount);                  //       OverdraftException 是 checked exception
            return true;                               //       成功返回 true
        } catch (OverdraftException e) {               // 【行38】catch 到了
            System.out.println("取款失败: "             // 【行39】打印友好的失败信息
                + e.getMessage()                       //       "余额不足" 或 "超出可用额度"
                + " (差额 "
                + CurrencyFormatter.format(e.getDeficit())  // deficit 格式化成人能读的金额
                + ")");
            return false;                              // 【行43】失败返回 false
        }
    }

    public void applyMonthlyInterest() {               // 【行45】月末结算：给所有储蓄账户算利息
        for (Customer customer : bank.getCustomers()) {   // 【行46】遍历每个客户
            for (Account account : customer.getAccounts()) {  // 【行47】遍历客户的每个账户
                if (account instanceof SavingAccount) {  // 【行48】类型检查：是储蓄账户吗？
                    ((SavingAccount) account)             // 【行49】强转：因为 account 变量类型是 Account
                        .applyMonthlyInterest();         //       必须转成 SavingAccount 才能调 applyMonthlyInterest()
                }                                        //       注意：支票账户不计算利息，所以跳过
            }
        }
    }
}

14.9 Main.java 逐行精讲

package com.bank.ui;

public class Main {
    public static void main(String[] args) {          // 【行4】JVM 程序入口
        BankUI demo = new BankUI();                   // 【行5】创建 BankUI 对象
        demo.run();                                   // 【行6】调用 run() 启动交互
    }
}

Main.java 就这么简单。它是启动器，只负责"让程序跑起来"。所有真正的逻辑都在 BankUI 里。

14.10 BankUI.java 逐行精讲（关键方法）

BankUI.java 比较长（348行），只讲关键的几个方法：

构造器和字段：

public class BankUI {

    private final BankService service = new BankService();   // 【字段1】Service 层
    private final Bank bank = Bank.getInstance();             // 【字段2】Bank 单例（UI 层直接持有它，因为要读客户数量等）
    private final Scanner scanner = new Scanner(System.in);   // 【字段3】读用户输入的扫描器

run() 方法——整个程序的主循环：

public void run() {
    askLoadDemoData();            // 1. 问用户要不要加载演示数据
    while (true) {                // 2. 死循环，菜单一直显示直到用户选 0
        showMainMenu();           // 3. 显示菜单
        int choice = readIntSafe(); // 4. 安全读用户选择
        switch (choice) {         // 5. switch 分发
            case 1: openCustomer(); break;
            case 2: openAccount(); break;
            case 3: deposit(); break;
            case 4: withdraw(); break;
            case 5: printCustomerReport(); break;
            case 6: findCustomer(); break;
            case 7: applyMonthlyInterest(); break;
            case 0:
                System.out.println("感谢使用硅谷银行，再见！");
                return;          // return 直接退出 run()，程序结束
            default:
                System.out.println("无效的选择，请重试");
        }
    }
}

readIntSafe()——安全读整数：

private int readIntSafe() {
    while (true) {                 // 死循环直到读到合法整数
        String line = scanner.nextLine().trim();  // 读一行，去掉前后空格
        try {
            return Integer.parseInt(line);        // 尝试转成 int，成功就返回
        } catch (NumberFormatException e) {
            System.out.print("请输入整数: ");      // 转不了（比如用户输入了 "abc"），提示重新输入
        }
    }
}

selectCustomer()——让用户从列表选客户：

private Customer selectCustomer() {
    if (bank.getCustomerCount() == 0) {           // 先检查有没有客户
        System.out.println("当前没有任何客户，请先新开客户");
        return null;                              // 没客户，返回 null 表示"选不了"
    }
    System.out.println();
    System.out.println("请选择客户:");
    for (int i = 0; i < bank.getCustomerCount(); i++) {  // 遍历所有客户，列出来
        Customer c = bank.getCustomer(i);
        System.out.println("  " + (i + 1) + ". "      // 显示编号（从 1 开始，人看着舒服）
            + c.getFullName() + " (" + c.getAccountCount() + " 个账户)");
    }
    System.out.print("输入客户编号: ");
    int idx = readIntSafe() - 1;                  // 用户输入 1，实际索引是 0，所以减 1
    if (idx < 0 || idx >= bank.getCustomerCount()) {  // 越界检查
        System.out.println("✗ 无效的客户编号");
        return null;
    }
    return bank.getCustomer(idx);                 // 返回选中的客户
}

deposit()——存钱（含完整异常处理链路）：

这是整个项目中异常传播的完整示例，让我们看看一个负数输入是怎么被一层一层处理的：

private void deposit() {
    System.out.println();
    System.out.println("--- 存钱 ---");
    Customer customer = selectCustomer();
    if (customer == null) return;
    Account account = selectAccount(customer);
    if (account == null) return;

    System.out.println("当前余额: " + CurrencyFormatter.format(account.getBalance()));
    double amount = readDoubleSafe("存入金额: ");  // 【行5】安全读输入
                                                  //       readDoubleSafe 只防 "abc" 这种非数字
                                                  //       用户输入 -500 → readDoubleSafe 照样读到 -500
    try {                                          // 【行6】异常链路的起点！
        service.deposit(account, amount);          //       调 Service 层（它不 catch IllegalArgumentException）
        System.out.println("✓ 存钱成功");          //       这两行成功才会执行
        ...
    } catch (IllegalArgumentException e) {         // 【行9】UI 层兜住！
        System.out.println("✗ 存钱失败: " + e.getMessage());
    }                                              //       程序不会 crash，继续显示菜单
}

完整异常传播路径（假设用户输入 -500）：

UI: readDoubleSafe() 读到 amount = -500  ← 合法的 double，没被拦住
  ↓
UI: service.deposit(account, -500)
  ↓
BankService: account.deposit(-500)  ← Service 层不 catch，直接往下调
  ↓
Account: if (amount <= 0) throw new IllegalArgumentException("存款金额必须大于 0")
  ↓
异常对象冒泡回 BankService → 没人 catch → 继续冒泡回 UI
  ↓
UI: catch (IllegalArgumentException e)  ← 终于被兜住了！
  ↓
打印友好提示 "✗ 存钱失败: 存款金额必须大于 0"
  ↓
循环回到菜单，用户可以重试

为什么不在 Account.deposit() 里 try-catch 然后返回 boolean？

因为参数合法性检查应该早失败、早暴露。如果 Account.deposit() 静默处理非法参数，调用方（Service、UI）就不知道存失败了，还以为存成功了。抛异常就是"大声说：这个操作不合法！谁调用的谁负责处理！"

withdraw()——取钱（含 OverdraftException 处理）：

private void withdraw() {
    Customer customer = selectCustomer();
    Account account = selectAccount(customer);
    ...
    double amount = readDoubleSafe("取出金额: ");
    try {                                          // 【UI 层 catch IllegalArgumentException】
        boolean success = service.withdraw(account, amount);
        if (success) {
            System.out.println("✓ 取钱成功");
            ...
        }
        // 注意：OverdraftException 已经被 Service 层内部 catch 了，
        // 返回 false 表示失败，UI 层不用再 catch
    } catch (IllegalArgumentException e) {         // 【兜底】防止负数等非法输入
        System.out.println("✗ 取钱失败: " + e.getMessage());
    }
}

两个异常的不同处理策略：

异常类型 抛出位置 谁来 catch 原因 IllegalArgumentException Account.deposit/withdraw、CheckingAccount.withdraw UI 层 catch unchecked，编译器不强制，Service 选择让它冒泡到最顶层统一处理 OverdraftException CheckingAccount.withdraw Service 层 catch checked exception，Service 需要"余额不足"时返回 false，UI 根据 false 决定是否显示成功

第 15 节：常见错误排查表

这一节收集了初学者写这个项目时最容易踩到的坑。如果你的代码报错了或者行为不对，先查表！

15.1 编译错误（红线）速查表

错误提示 原因 怎么修 cannot find symbol: class OverdraftException 忘了 import 或者类名拼错 检查有没有 import com.bank.exception.OverdraftException;，或者看看文件是不是在正确的包目录下 package com.bank.exception does not exist 文件夹路径不对 确认 OverdraftException.java 在 src/com/bank/exception/ 下面 constructor Bank() is not visible Bank 构造器是 private，你写了 new Bank() 必须用 Bank.getInstance() abstract method cannot have a body 你给抽象方法写了花括号 抽象方法只有 ; 没有 { } missing method body or declare abstract 普通方法没写方法体 要么写 { } 里面加代码，要么加 abstract 关键字（但类也得 abstract） incompatible types: Account cannot be converted to SavingAccount 忘了强转 写 ((SavingAccount) account).getInterestRate()，加括号！ non-static variable this cannot be referenced from a static context 在 static 方法里用了 this 要么把方法改成非 static，要么创建对象来调 exception OverdraftException is not thrown in body of... 重写 withdraw 时签名没写 throws 子类的 withdraw 也要加 throws OverdraftException method does not override or implement a method from a supertype @Override 标了但实际上没重写对 检查方法名、参数类型、返回值是不是和父类完全一致 raw use of parameterized class 'List' 没写泛型 List 要写成 List<Account></account> 而不是 List unreported exception OverdraftException; must be caught or declared to be thrown 调了 withdraw 但没处理异常 要么 try-catch，要么方法签名加 throws OverdraftException

15.2 运行时错误速查表

错误现象 原因 怎么修 NullPointerException (NPE) 某个变量是 null 你还调了它的方法 找堆栈里的行号，看看是哪个对象。常见：findCustomerByName 返回 null 你直接用了 IndexOutOfBoundsException: Index 0 out of bounds for length 0 列表是空的你还 get(0) 调 .size() 先看有没有元素，或者先加客户再选 InputMismatchException 用 nextInt() 读输入但用户输入了字符串 用我们的 readIntSafe() / readDoubleSafe() 方法，或者用 hasNextInt() 先检查 IllegalArgumentException: 存款金额必须大于 0 用户输入了负数或 0（比如存 -500），没人 catch 这个 unchecked exception 在 BankUI 的 deposit() / withdraw() 里加 try-catch。readDoubleSafe() 只防非数字输入，不防负数 程序直接退出，没有任何输出 main 方法里什么都没做 检查 Main.java 是不是调了 demo.run() 取钱成功了但余额没变？ 你可能调错了对象 检查是不是 account.withdraw(amount)，而不是 new Account().withdraw(amount) 所有客户的账户列表都是空的？ addAccount 没调到 检查 BankService.openSavingAccount 里有没有 customer.addAccount(account)

15.3 逻辑错误（能运行但结果不对）

现象 原因 怎么修 储蓄账户取不了钱？ 你是不是没给 SavingAccount 写 withdraw 正常！SavingAccount 继承 Account.withdraw，没有透支，余额不够就抛异常。不用自己实现 CheckingAccount 取钱 2500 却没抛异常？ 余额 200，透支额度 500，可用 700，取 2500 应该失败 可用额度算错了。检查 available = balance + getAvailableCredit() 这行 月利息算出来是 0？ 利率是小数还是百分比？ 0.03 表示 3%，如果你填了 3 表示 300% customer.getAccount(0) 拿到的是支票不是储蓄？ addAccount 的顺序决定索引 setupDemoData 里先开的什么，index 0 就是什么。想用储蓄账户就调 getAccount(0) 前确保 0 号是储蓄 getFullName() 返回 "null null"？ firstName 和 lastName 没赋值 检查 Customer 构造器有没有正确执行 this.firstName = firstName Collections.unmodifiableList 返回的列表还能 add？ 不可能！真能 add 说明你没调 unmodifiableList 检查 getAccounts() 是不是直接 return accounts 了 Bank.getInstance() == Bank.getInstance() 返回 false？ 不可能！如果返回 false 说明你改了代码 检查 INSTANCE 字段是不是 static final Bank Scanner 读字符串后 nextInt 跳一行？ 典型问题：nextInt() 只读数字不读回车 我们用 nextLine() + parseInt() 就没这问题

15.4 初学者最常犯的 10 个错误

错误 1：用 private 代替 protected

// ❌ 错误
private double balance;

// ✅ 正确
protected double balance;

后果：CheckingAccount 和 SavingAccount 访问不到 balance 字段

错误 2：抽象类写了 public static void main 以外的普通方法没有花括号

// ❌ 错误（抽象方法也写成有花括号了）
public abstract String getAccountType() {
    return "";
}

// ✅ 正确（只有分号）
public abstract String getAccountType();

错误 3：子类构造器没调 super

// ❌ 错误：没有 super(initialBalance)
public SavingAccount(double initialBalance, double interestRate) {
    if (interestRate < 0) { ... }   // 这里 initialBalance 还没初始化 balance！
    this.interestRate = interestRate;
}

// ✅ 正确
public SavingAccount(double initialBalance, double interestRate) {
    super(initialBalance);          // 必须第一行调 super
    if (interestRate < 0) { ... }
    this.interestRate = interestRate;
}

错误 4：Bank 不是单例

// ❌ 错误：Bank 构造器不是 private
public Bank() { }

// ✅ 正确
private Bank() { }

错误 5：忘了重写 toString 就打印对象

// ❌ 错误：直接打印 Account 对象
Account a = new SavingAccount(1000, 0.03);
System.out.println(a);
// 输出：com.bank.domain.SavingAccount@1a2b3c4  ← 看不懂

// ✅ 正确：Account 已经重写了 toString，应该输出 "储蓄账户{balance=1000.0}"

错误 6：Scanner 混用 next() 和 nextLine()

// ❌ 错误
int choice = scanner.nextInt();      // 读了 1，但没读后面的回车
String name = scanner.nextLine();    // 读到的是空字符串！

错误 7：用 == 比较字符串

// ❌ 错误
if (name == "Jane") { }

// ✅ 正确
if (name.equals("Jane")) { }

== 比较的是内存地址，.equals() 比较的是内容

错误 8：findCustomerByName 返回 null 没检查

// ❌ 错误
Customer c = bank.findCustomerByName("Jane Simms");
Account sa = c.getAccount(0);   // 如果没找到 Jane，c 是 null → NullPointerException！

// ✅ 正确
Customer c = bank.findCustomerByName("Jane Simms");
if (c != null) {
    Account sa = c.getAccount(0);
}

错误 9：NumberFormatException 处理不对

// ❌ 错误
int n = Integer.parseInt(line);    // 可能抛异常，崩溃

// ✅ 正确
try {
    int n = Integer.parseInt(line);
} catch (NumberFormatException e) {
    System.out.println("请输入数字！");
}

错误 10：import 写错了

// ❌ 错误1：类名拼错
import com.bank.domain.Accounts;    // 没有 s！

// ❌ 错误2：路径错
import com.bank.Account;           // Account 在 domain 包里

// ❌ 错误3：用了同包类还 import
// 同包的类不需要 import

错误 11：unchecked exception 以为没人管就不会崩

// ❌ 错误：IllegalArgumentException 是 unchecked exception，
// 编译器不强制你 catch 或 throws，但它照样会导致程序崩溃！
double amount = -500;
account.deposit(amount);   // 这行直接抛 IllegalArgumentException
// 没人 catch → 程序直接 crash 退出！

// ✅ 正确：UI 层要 catch
try {
    account.deposit(amount);
} catch (IllegalArgumentException e) {
    System.out.println("输入错误: " + e.getMessage());
}

关键概念：Exception 分 checked 和 unchecked，但不管哪种，只要没被 catch 或继续 throws，就会让程序崩溃。unchecked 只是"编译器不提醒你"，不代表"不需要处理"。BankUI 里 readDoubleSafe() 只防"用户输入了 abc"这种非数字情况，但用户输入 -500 是合法的 double，readDoubleSafe 读得到，后面还得靠 try-catch 兜住。

第 16 节：单元测试入门 + 给项目写测试

单元测试是质量保证的基石。专业程序员写完功能后第一件事就是写测试。这一节教你用 JUnit 5 给我们的银行项目写测试。

16.1 什么是单元测试？

单元测试 = 写一段代码，自动检验另一段代码的输出对不对

// 你写的方法
public double calculateMonthlyInterest() {
    return balance * interestRate / 12;
}

// 单元测试：自动算一下预期值，对比实际值
@Test
void testMonthlyInterest() {
    SavingAccount a = new SavingAccount(1000, 0.03);
    double result = a.calculateMonthlyInterest();
    double expected = 2.5;   // 1000 × 0.03 / 12 = 2.5
    assertEquals(expected, result);
}

好处：

改了代码后跑一遍测试 → 立刻知道有没有改坏

写完方法立刻写测试 → 倒逼你想清楚边界条件

测试覆盖了各种极端情况 → 逻辑 bug 提前暴露

16.2 用 IDEA 快速建测试（不用手动装依赖）

IDEA 自带 JUnit 5！操作：

在项目根目录新建文件夹 test（和 src 平级）

右键 test → Mark Directory as → Test Sources Root

然后就能在里面写测试类了

enterprise/
├── src/          ← 源代码
│   └── com/bank/...
├── test/         ← 测试代码（新建这个）
│   └── com/bank/...
└── out/

16.3 写第一个测试类：AccountTest

文件位置：test/com/bank/domain/AccountTest.java

package com.bank.domain;

import com.bank.exception.OverdraftException;
import org.junit.jupiter.api.Test;   // JUnit 5 的 @Test 注解
import static org.junit.jupiter.api.Assertions.*;  // 断言方法

class AccountTest {

    // ====== 测试构造器 ======

    @Test
    void testInitialBalance() {
        SavingAccount account = new SavingAccount(500, 0.03);
        assertEquals(500, account.getBalance(), 0.001);  // 第三个参数是误差容忍度（double 比较必须有）
    }

    @Test
    void testNegativeInitialBalance() {
        assertThrows(IllegalArgumentException.class, () -> {
            new SavingAccount(-100, 0.03);
        });
    }

    // ====== 测试存款 ======

    @Test
    void testDeposit() {
        SavingAccount account = new SavingAccount(200, 0.03);
        account.deposit(150);
        assertEquals(350, account.getBalance(), 0.001);
    }

    @Test
    void testDepositZero() {
        SavingAccount account = new SavingAccount(200, 0.03);
        assertThrows(IllegalArgumentException.class, () -> {
            account.deposit(0);
        });
    }

    @Test
    void testDepositNegative() {
        SavingAccount account = new SavingAccount(200, 0.03);
        assertThrows(IllegalArgumentException.class, () -> {
            account.deposit(-50);
        });
    }

    // ====== 测试取款 ======

    @Test
    void testWithdrawSuccess() throws OverdraftException {
        SavingAccount account = new SavingAccount(300, 0.03);
        account.withdraw(200);
        assertEquals(100, account.getBalance(), 0.001);
    }

    @Test
    void testWithdrawAll() throws OverdraftException {
        SavingAccount account = new SavingAccount(300, 0.03);
        account.withdraw(300);
        assertEquals(0, account.getBalance(), 0.001);
    }

    @Test
    void testWithdrawFail() {
        SavingAccount account = new SavingAccount(100, 0.03);
        OverdraftException ex = assertThrows(OverdraftException.class, () -> {
            account.withdraw(150);
        });
        // 不仅验证抛了异常，还要验证异常里的 deficit 字段对不对
        assertEquals(50, ex.getDeficit(), 0.001);  // 差 50
        assertEquals("余额不足", ex.getMessage());
    }
}

16.4 写 CheckingAccountTest（重点测透支逻辑）

文件位置：test/com/bank/domain/CheckingAccountTest.java

package com.bank.domain;

import com.bank.exception.OverdraftException;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CheckingAccountTest {

    @Test
    void testNoOverdraftWithdrawFail() {
        // 无透支保护的支票账户，余额 200，取 250 应该失败
        CheckingAccount account = new CheckingAccount(200);
        assertThrows(OverdraftException.class, () -> {
            account.withdraw(250);
        });
    }

    @Test
    void testOverdraftWithdrawSuccess() throws OverdraftException {
        // 有 500 透支额度，余额 200，取 500 应该成功
        CheckingAccount account = new CheckingAccount(200, 500);
        account.withdraw(500);
        assertEquals(-300, account.getBalance(), 0.001);  // 余额变成 -300
    }

    @Test
    void testOverdraftLimitExactly() throws OverdraftException {
        // 余额 0，透支额度 500，取 500 刚好
        CheckingAccount account = new CheckingAccount(0, 500);
        account.withdraw(500);
        assertEquals(-500, account.getBalance(), 0.001);
        assertEquals(0, account.getAvailableCredit(), 0.001);  // 透支额度用完了
    }

    @Test
    void testOverdraftLimitExceeded() {
        // 余额 0，透支额度 500，取 501 应该失败
        CheckingAccount account = new CheckingAccount(0, 500);
        OverdraftException ex = assertThrows(OverdraftException.class, () -> {
            account.withdraw(501);
        });
        assertEquals(1, ex.getDeficit(), 0.001);  // 差 1 块钱
    }

    @Test
    void testAvailableCredit() {
        // 余额 200，透支额度 500，可用透支 = 500
        CheckingAccount account = new CheckingAccount(200, 500);
        assertEquals(500, account.getAvailableCredit(), 0.001);

        // 取 300 后（用了 100 透支），可用 = 400
        account.withdraw(300);
        assertEquals(400, account.getAvailableCredit(), 0.001);
    }

    @Test
    void testDepositAfterOverdraft() throws OverdraftException {
        // 先透支，再存钱
        CheckingAccount account = new CheckingAccount(0, 500);
        account.withdraw(300);         // balance = -300
        account.deposit(400);          // balance = 100
        assertEquals(100, account.getBalance(), 0.001);
    }
}

16.5 写 BankServiceTest

文件位置：test/com/bank/service/BankServiceTest.java

package com.bank.service;

import com.bank.domain.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class BankServiceTest {

    private BankService service;

    // 每个 @Test 方法执行前都会先跑 @BeforeEach
    @BeforeEach
    void setUp() {
        service = new BankService();
    }

    @Test
    void testOpenCustomer() {
        Customer customer = service.openCustomer("Alice", "Wang");
        assertNotNull(customer);
        assertEquals("Alice", customer.getFirstName());
        assertEquals("Wang", customer.getLastName());
    }

    @Test
    void testOpenSavingAccount() {
        Customer customer = service.openCustomer("Alice", "Wang");
        SavingAccount sa = service.openSavingAccount(customer, 1000, 0.03);
        assertEquals(1, customer.getAccountCount());
        assertEquals(sa, customer.getAccount(0));
        assertEquals(1000, customer.getAccount(0).getBalance(), 0.001);
    }

    @Test
    void testOpenCheckingAccountWithOverdraft() {
        Customer customer = service.openCustomer("Bob", "Li");
        CheckingAccount ca = service.openCheckingAccount(customer, 200, 500);
        assertEquals(500, ca.getOverdraftLimit(), 0.001);
    }

    @Test
    void testWithdrawSuccess() {
        Customer customer = service.openCustomer("Alice", "Wang");
        SavingAccount sa = service.openSavingAccount(customer, 1000, 0.03);
        assertTrue(service.withdraw(sa, 500));
        assertEquals(500, sa.getBalance(), 0.001);
    }

    @Test
    void testWithdrawFail() {
        Customer customer = service.openCustomer("Alice", "Wang");
        SavingAccount sa = service.openSavingAccount(customer, 100, 0.03);
        assertFalse(service.withdraw(sa, 200));   // 余额不够，返回 false
        assertEquals(100, sa.getBalance(), 0.001);  // 余额没变
    }

    @Test
    void testApplyMonthlyInterest() {
        Customer c1 = service.openCustomer("A", "B");
        service.openSavingAccount(c1, 1200, 0.03);  // 月利息 = 1200 × 0.03 / 12 = 3

        Customer c2 = service.openCustomer("C", "D");
        service.openCheckingAccount(c2, 500);       // 支票账户不算利息

        service.applyMonthlyInterest();

        // c1 的储蓄账户应该涨了 3 块
        assertEquals(1203, c1.getAccount(0).getBalance(), 0.001);
        // c2 的支票账户不变
        assertEquals(500, c2.getAccount(0).getBalance(), 0.001);
    }
}

16.6 怎么跑测试

在 IDEA 里有三种方式：

方式 1：单个测试方法

光标放在 @Test 标注的方法里

左边会出现绿色三角形 ▶️

点它 → Run 'testXXX()'

方式 2：整个测试类

打开测试类文件

左边的类名旁边有绿色三角形

点它 → Run 'XXXTest'

方式 3：所有测试

右键 test 文件夹

Run All Tests

成功标志：全部绿色 ✅。如果有红色 ❌，点进去看哪个断言失败了，对比 expected 和 actual。

16.7 测试断言速查表

方法 作用 示例 assertEquals(expected, actual) 两个值相等 assertEquals(500, account.getBalance(), 0.001) assertNotEquals(expected, actual) 不相等 assertNotEquals(0, result) assertTrue(condition) 条件为 true assertTrue(service.withdraw(account, 100)) assertFalse(condition) 条件为 false assertFalse(service.withdraw(account, 9999)) assertNull(object) 对象是 null assertNull(customer.findCustomerByName("Nobody")) assertNotNull(object) 对象不是 null assertNotNull(customer) assertThrows(ExceptionClass, executable) 抛了指定类型的异常 assertThrows(IllegalArgumentException.class, () -> {...}) assertDoesNotThrow(executable) 没抛异常 assertDoesNotThrow(() -> account.deposit(100))

关于 double 的 assertEquals 第三个参数：浮点数有精度问题。比如数学上 0.1 + 0.2 = 0.3，但计算机里 0.1 + 0.2 = 0.30000000000000004。所以比较 double 时必须指定误差范围，通常写 0.001（千分之一）。

第 17 节：Git 版本控制入门

版本控制是程序员的"后悔药"。改坏了代码？Git 帮你恢复到之前能跑的版本。这一节教你用 Git 管理这个项目。

17.1 为什么需要 Git？

想象一下你不用 Git 的生活：

改了一个类，结果程序跑不起来了 → 不知道改了什么

想回退到 3 天前能跑的状态 → 手动对比，改回去，改到什么时候

做功能 A 做到一半想先做功能 B → 复制粘贴做副本，两个版本混乱

Git 让这一切变成几行命令的事。

17.2 三个最重要的概念

工作区 (Working Directory)    ← 你正在改的文件（就是 enterprise/ 文件夹）
暂存区 (Staging Area)         ← git add 之后，修改被"标记"要提交
本地仓库 (Repository)         ← git commit 之后，修改被永久保存成一个版本

流程图：

修改文件 → git add → git commit
(工作区)    (暂存区)     (本地仓库)

17.3 第一次用 Git（对这个项目）

Step 1：检查电脑有没有装 Git

git --version

# 有版本号就行，比如 git version 2.39.3

# 没有就去 https://git-scm.com/download 下载安装

Step 2：初始化仓库（在项目目录下执行）

cd /home/liulixian/Desktop/idea-project/Bank/issue/01硅谷银行业务管理系统/enterprise
git init

执行后项目目录里会出现一个隐藏的 .git/ 文件夹——这就是 Git 仓库了。

Step 3：创建 .gitignore（告诉 Git 哪些文件不要管）

新建文件 .gitignore，内容：

# IDEA 配置

.idea/
*.iml

# 编译输出

out/
build/

# 系统文件

.DS_Store
Thumbs.db

这表示 .idea/、out/ 等文件 Git 会忽略。这些是 IDE 自动生成的，不该提交。

Step 4：第一次提交

git add .                    # 把所有文件加入暂存区
git status                   # 看看暂存了哪些（可选，推荐每次 commit 前都看一下）
git commit -m "初始提交：硅谷银行项目完成"   # commit 并写备注

Step 5：查看提交历史

git log                       # 看所有提交记录
git log --oneline             # 精简版，一行一个

17.4 日常开发的 Git 循环

改代码 → git status → git add (文件名) → git status → git commit -m "改了什么"

例子：你想加一个"打印所有客户总数"的功能

# 1. 改完 BankUI.java

git status                    # 看到 BankUI.java 是 modified

# 2. 只提交 BankUI.java（不要乱 add 所有）

git add src/com/bank/ui/BankUI.java

# 3. 再确认一下暂存区对不对

git status

# 4. commit

git commit -m "feat: 在主菜单显示当前客户总数"

Commit message 写好的习惯：

feat: 新功能

fix: 修 bug

refactor: 重构（不改行为，改代码结构）

docs: 文档更新

test: 加测试

chore: 杂项（格式化、加 .gitignore 等）

17.5 后悔了怎么办？——Git 核心救援命令

场景 1：改了文件但还没 add，想放弃修改

git checkout -- src/com/bank/ui/BankUI.java

# BankUI.java 恢复到上一次 commit 的状态

场景 2：已经 add 了（在暂存区），想拿出来

git reset HEAD src/com/bank/ui/BankUI.java

# 从暂存区退回工作区，文件内容不变

场景 3：已经 commit 了，想回到上一个版本

git log --oneline              # 先找 commit id

# 比如看到：a1b2c3d feat: 加客户总数显示

# e4f5g6h 初始提交

git reset --hard e4f5g6h       # 回到初始提交那版

# ⚠️ --hard 会丢掉那个 commit 之后所有未 commit 的修改！谨慎！

场景 4：想看看某一行代码是谁什么时候改的

git blame src/com/bank/ui/BankUI.java

# 每一行前面都会显示：commit id、作者、时间

17.6 Git 在 IDEA 里的使用

IDEA 内置了 Git 图形界面，不想敲命令行也可以：

1. 初始化：VCS → Enable Version Control Integration → 选 Git
2. 提交：右键文件 → Git → Add → 再右键 → Git → Commit File
3. 查看历史：右键文件 → Git → Show History
4. 回退：右键文件 → Git → Revert (撤销)

17.7 进阶：分支（Branch）

分支让你在不影响主线的情况下做实验：

# 创建并切换到新分支

git checkout -b feature/定期存款账户

# 在这个分支上改代码、commit、跑测试

# ...

# ...

# 改完了，切回主分支合并

git checkout main
git merge feature/定期存款账户

# 删掉已经合并的分支

git branch -d feature/定期存款账户

为什么用分支？比如你想加定期存款功能，但怕改坏现有逻辑 → 新开一个分支，在上面折腾，改完测试通过了再合并回主线。

第 18 节：代码重构实战

重构 = 不改变代码行为的前提下，让代码变得更好读、好维护。这一节给我们的银行项目做一次重构演示。

18.1 重构前 vs 重构后

重构前的 Customer.getAccountCount 相关代码

Customer.java（重构前）：

public int getAccountCount() {
    return accounts.size();
}

public Account getAccount(int index) {
    if (index < 0 || index >= accounts.size()) {
        throw new IndexOutOfBoundsException("账户索引越界: " + index);
    }
    return accounts.get(index);
}

问题：Bank 里也有一模一样的越界检查代码！这叫重复代码（DRY violation）——Don't Repeat Yourself。

重构方案：提取公共方法

我们可以写一个工具方法 checkIndexBounds(list, index, description)，两边都调它。

新建文件：src/com/bank/util/Preconditions.java

package com.bank.util;

import java.util.List;

public final class Preconditions {

    private Preconditions() {}

    public static void checkIndexBounds(int index, int size, String description) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException(
                description + " 索引越界: " + index + " (大小: " + size + ")"
            );
        }
    }
}

重构后的 Customer.java：

// import com.bank.util.Preconditions; 要加上

public Account getAccount(int index) {
    Preconditions.checkIndexBounds(index, accounts.size(), "账户");
    return accounts.get(index);
}

重构后的 Bank.java：

// import com.bank.util.Preconditions; 要加上

public Customer getCustomer(int index) {
    Preconditions.checkIndexBounds(index, customers.size(), "客户");
    return customers.get(index);
}

18.2 重构前后对比

指标 重构前 重构后 越界检查代码 2 处重复 1 处公共方法 加新的列表类 复制粘贴 if 判断 调 Preconditions.checkIndexBounds 改错误信息格式 改 2 处 改 1 处

18.3 更多重构思路给你练

重构手法 原代码 重构后 为什么好 提取方法 deposit() 和 withdraw() 都有 amount <= 0 检查 Account 里加一个 validatePositive(double amount, String desc) 方法，两个都调 不重复 方法重载 CheckingAccount 单参数构造器里写了 3 行 this(initialBalance, 0) 简洁 引入多态 BankUI.printCustomerReport 里一堆 if (account instanceof SavingAccount) 让 Account 自己实现 printReportLine() 方法 UI 层不用知道账户类型 配置常量 利息计算公式 balance * rate / 12 可以把 12 提为常量 MONTHS_PER_YEAR = 12 语义更清晰 引入 Optional findCustomerByName 返回 null 改成返回 Optional<Customer></customer> 强制调用方处理"没找到"的情况，减少 NPE

18.4 重构铁律

一次只改一件事——改完跑测试，确保行为没变

有测试保护的重构才安全——如果没有测试，重构前先写测试

小步快跑——大重构拆成好几步小的，每步都能编译通过

先编译通过，再跑测试通过——别改了一堆才想起编译

第 19 节：常见面试题汇总

这个项目里用到的知识点，全是 Java 面试高频考点。把这些背下来，面试遇到类似题就不会慌。

19.1 面向对象基础

Q1：Java 中 abstract class 和 interface 有什么区别？

对比项 抽象类 接口 实例化 不能 new 不能 new 构造器 有 没有 字段 任意 默认 public static final 方法实现 可以有具体方法 Java 8+ 可以有 default/static 方法 继承 单继承（一个类只能 extends 一个） 多实现（一个类可以 implements 多个） 设计意图 is-a（一种"是...的"关系） can-do（能做什么）

在我们的项目里：Account 是抽象类（SavingAccount "是一种" Account）。如果我们设计一个"可透支"的接口 Overdraftable，让 CheckingAccount implements 它，就是接口的用法。

Q2：重载（Overload）和重写（Override）有什么区别？

对比项 重载 Overload 重写 Override 位置 同一个类 父子类之间 方法名 相同 相同 参数列表 必须不同（类型/数量/顺序） 必须相同 返回值 可以不同 可以相同或子类（协变返回） 权限 可以不同 不能比父类更严格 异常 可以不同 不能比父类抛更多 checked exception 注解 不用 @Override

在我们的项目里：

CheckingAccount 的两个构造器是重载（参数不同）

CheckingAccount.withdraw() 是重写父类 Account.withdraw()

Q3：什么是多态？

一句话：父类引用指向子类对象，调用方法时表现出子类的行为。

Account a = new SavingAccount(1000, 0.03);
a.getAccountType();    // 返回 "储蓄账户" —— 因为实际对象是 SavingAccount

a = new CheckingAccount(500, 300);
a.getAccountType();    // 返回 "支票账户" —— 因为实际对象是 CheckingAccount

多态的三个必要条件：

继承（extends 或 implements）

重写（Override）

父类引用指向子类对象

Q4：this 和 super 有什么用？

this. 引用当前对象的字段和方法（区分同名参数）

this() 调用本类的另一个构造器（必须在第一行）

super. 引用父类的字段和方法

super() 调用父类的构造器（必须在第一行，不写也会自动加）

19.2 异常

Q5：Checked Exception 和 Unchecked Exception 有什么区别？

对比项 Checked Exception Unchecked Exception 继承 Exception（不包括 RuntimeException） RuntimeException 或 Error 编译器检查 ✅ 强制 try-catch 或 throws ❌ 不强制 场景 可以恢复的异常（网络断了、文件找不到） 编程错误（NPE、数组越界） 我们的项目 OverdraftException IllegalArgumentException、IndexOutOfBoundsException

Q6：throw 和 throws 有什么区别？

throw（动词）：抛出一个异常对象。写在方法内部

throws（声明）：声明这个方法可能抛哪些异常。写在方法签名上

// throws 声明
public void withdraw(double amount) throws OverdraftException {
    // throw 抛出
    if (available < amount) {
        throw new OverdraftException("余额不足", deficit);
    }
}

Q7：try-with-resources 是什么？

Java 7+ 引入的语法糖，让自动关闭资源（比如文件、数据库连接）：

try (BufferedReader reader = new BufferedReader(...)) {
    // 用 reader
}  // 出了 try 块自动 reader.close()，不用写 finally

我们的项目没用到，但面试经常问。

19.3 集合

Q8：List 和 Set 有什么区别？

List：有序、可重复。比如 [1, 2, 2, 3]

Set：无序、不重复。比如 {1, 2, 3}（放两个 2 会自动去重）

我们用的是 List，因为客户/账户有顺序（索引 0、1、2...）。

Q9：ArrayList 和 LinkedList 有什么区别？

对比项 ArrayList LinkedList 底层 动态数组 双向链表 随机访问 get(i) O(1) 快 O(n) 慢（要从头/尾遍历） 头部插入删除 O(n) 慢（后面元素都要挪） O(1) 快 尾部插入 O(1) 快（有扩容开销） O(1) 快 内存占用 少 多（每个节点存指针） 选择 大多数场景 频繁头部插入删除

Q10：为什么 Collections.unmodifiableList 能防修改？

它不是复制一份列表，而是返回一个代理对象。代理对象的 add() remove() set() 方法全部抛 UnsupportedOperationException，但它内部还是指向原来的列表。对"只读视图"场景非常合适，既安全又省内存。

19.4 单例模式

Q11：单例模式有几种写法？饿汉式 vs 懒汉式

我们用的是饿汉式：

private static final Bank INSTANCE = new Bank();
private Bank() {}
public static Bank getInstance() { return INSTANCE; }

写法 类加载时创建 线程安全 延迟加载 复杂度 饿汉式（我们用的） ✅ ✅ ❌ 低 懒汉式（synchronized） ❌ ✅ ✅ 中 双重检查锁 DCL ❌ ✅ ✅ 高 静态内部类 ❌ ✅ ✅ 中 枚举单例 ✅ ✅ ✅ 中

为什么选饿汉式？ 银行对象肯定要用，不在乎这点创建时间，饿汉式最简单、最安全。

19.5 基础细节

Q12：final 关键字有几种用法？

final 类：不能被继承。比如 CurrencyFormatter

final 方法：不能被子类重写

final 字段：赋值后不能改。比如 Customer.firstName

final 局部变量：赋值后不能改（少用）

Q13：static 关键字有几种用法？

static 字段：类级别，所有对象共享一份。比如 Bank.INSTANCE

static 方法：类级别，不用 new 对象就能调。比如 Bank.getInstance()

static 代码块：类加载时执行一次。比如想在类加载时做初始化

static 内部类：不持有外部类的引用（少见）

Q14：== 和 equals() 有什么区别？

==：比较内存地址（引用）

equals()：比较内容（Object 的 equals 默认也是比较地址，但 String、Integer 等重写了）

String s1 = new String("hello");
String s2 = new String("hello");
s1 == s2       // false（地址不同）
s1.equals(s2)  // true（内容相同）

第 20 节：项目扩展方向建议

学完了基础版？想继续挑战？这一节给你列出 N 个进阶方向，每个都能让你巩固更多 Java 技能。

20.1 难度分级

初级（改改现有类）  → 中级（加新功能）  → 高级（架构升级）
    ↓                    ↓                   ↓
改 InterestRate          加 Transaction      加数据库持久化
加账户排序功能           加 ATM 模拟          加 Spring Boot Web
                        加文件保存/读取       加多线程
                        加新账户类型           加网络传输

20.2 初级扩展（不改架构，加小功能）

扩展 1：InterestRate 可修改

// SavingAccount 把 final 去掉，加 setter
private double interestRate;  // 去掉 final

public void setInterestRate(double rate) {
    if (rate < 0) throw new IllegalArgumentException(...);
    this.interestRate = rate;
}

学什么：什么时候用 final、什么时候不用

扩展 2：银行名字可配置

Bank 加一个字段：

private final String bankName = "硅谷银行";
public String getBankName() { return bankName; }

然后 BankUI 里显示菜单时用到它。

扩展 3：账户列表支持排序

在 Customer 里加：

public List<Account></account> getAccountsSortedByBalance() {
    List<Account></account> sorted = new ArrayList<>(accounts);
    sorted.sort((a, b) -> Double.compare(a.getBalance(), b.getBalance()));
    return Collections.unmodifiableList(sorted);
}

学什么：Lambda 表达式、Comparator 接口、list.sort()

扩展 4：客户按账户数量排序打印报告

List<Customer></customer> sortedCustomers = new ArrayList<>(bank.getCustomers());
sortedCustomers.sort((a, b) -> b.getAccountCount() - a.getAccountCount());

20.3 中级扩展（加新类和新功能）

扩展 5：Transaction 交易流水

新建 domain/Transaction.java：

public class Transaction {
    private final LocalDateTime time;      // 交易时间
    private final String type;             // "deposit" 或 "withdraw"
    private final double amount;           // 金额
    private final Account account;         // 关联账户
    private final boolean success;         // 是否成功
    private final String note;             // 备注（失败原因等）
}

然后 Account 加一个 List<Transaction></transaction> transactions，每次 deposit/withdraw 都 append 一条。

学什么：LocalDateTime、构造器收集参数、关联对象、历史记录设计

扩展 6：定期存款账户 TimeDepositAccount

继承 Account：

public class TimeDepositAccount extends Account {
    private final double annualRate;
    private final int months;              // 存几个月
    private final LocalDateTime maturityDate;  // 到期时间
    private boolean matured = false;       // 是否已到期
}

没到期取钱要扣违约金（比如 3%）

到期了自动转成活期

需要一个方法 daysUntilMaturity()

学什么：LocalDate/LocalDateTime、时间计算、状态字段设计、新的 OverdraftException 变体

扩展 7：用文件保存/读取数据（Java IO）

保存：程序退出时把所有 Bank 数据写到文件

public void saveToFile(String path) throws IOException {
    try (PrintWriter writer = new PrintWriter(new FileWriter(path))) {
        for (Customer c : customers) {
            writer.println(c.getFullName());
            for (Account a : c.getAccounts()) {
                writer.println(a.getAccountType() + "," + a.getBalance());
            }
        }
    }
}

读取：启动时从文件恢复

public void loadFromFile(String path) throws IOException {
    try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
        String line;
        // ...逐行解析...
    }
}

学什么：File、FileReader、FileWriter、BufferedReader、PrintWriter、try-with-resources、IO 异常处理

扩展 8：加个 ATM 模拟界面

不用改成图形界面，加个"ATM 模式"菜单：

========== ATM 界面 ==========
请插卡（输入客户全名）: Jane Simms
请选择账户:

1. 储蓄账户 ¥500.00
2. 支票账户 ¥200.00
   请选择操作:
3. 查余额
4. 存钱
5. 取钱
6. 退卡

学什么：不同的 UI 风格设计、复用已有 domain/service 层

20.4 高级扩展（架构升级）

扩展 9：数据库持久化（JDBC）

用 SQLite（轻量、无需安装服务器）：

// 新建 util/DatabaseManager.java
public class DatabaseManager {
    private Connection conn;

    public void connect() throws SQLException {
        conn = DriverManager.getConnection("jdbc:sqlite:bank.db");
    }

    public void saveCustomer(Customer c) throws SQLException {
        PreparedStatement ps = conn.prepareStatement(
            "INSERT INTO customer (first_name, last_name) VALUES (?, ?)"
        );
        ps.setString(1, c.getFirstName());
        ps.setString(2, c.getLastName());
        ps.executeUpdate();
    }
}

学什么：JDBC、Connection、PreparedStatement、SQL、数据库表设计

扩展 10：Spring Boot REST API

把整个项目改成 Web 服务：

@RestController
@RequestMapping("/api/bank")
public class BankController {

    @GetMapping("/customers")
    public List<Customer></customer> listCustomers() {
        return Bank.getInstance().getCustomers();
    }

    @PostMapping("/customers")
    public Customer createCustomer(@RequestBody CustomerRequest req) {
        return service.openCustomer(req.getFirstName(), req.getLastName());
    }
}

然后前端用 Postman 或浏览器就能调：

curl http://localhost:8080/api/bank/customers

# 返回 JSON 数据

学什么：Spring Boot、REST API、HTTP 方法、JSON 序列化、Web 开发基础

扩展 11：加 JUnit 5 + Mockito 做更完整的测试

// Mockito 模拟 Bank 单例的测试
@ExtendWith(MockitoExtension.class)
class BankServiceTest {

    @Mock
    private Bank bank;

    @InjectMocks
    private BankService service;  // service 里的 bank 字段会被上面的 mock 注入

    @Test
    void testOpenCustomer() {
        Customer mockCustomer = mock(Customer.class);
        when(bank.addCustomer("Alice", "Wang")).thenReturn(mockCustomer);

        Customer result = service.openCustomer("Alice", "Wang");

        assertSame(mockCustomer, result);
        verify(bank).addCustomer("Alice", "Wang");
    }
}

学什么：Mock、测试隔离、单元测试 vs 集成测试

扩展 12：加并行计算（CompletableFuture）

比如同时计算所有储蓄账户的月利息：

List<CompletableFuture<Void></void>> futures = bank.getCustomers().stream()
    .flatMap(c -> c.getAccounts().stream())
    .filter(a -> a instanceof SavingAccount)
    .map(a -> CompletableFuture.runAsync(() -> {
        ((SavingAccount) a).applyMonthlyInterest();
    }))
    .collect(Collectors.toList());

CompletableFuture.allOf(futures.toArray(new CompletableFuture[0])).join();

学什么：Stream API、CompletableFuture、Lambda、函数式编程、并发编程基础

第 21 节：完整知识图谱

把整个项目涉及的所有知识点串成一张网。学完项目后回看这张图，查漏补缺。

                    ┌─────────────────────────────┐
                    │     Java 基础语法（必修）      │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
          ▼                        ▼                        ▼
   ┌─────────────┐          ┌──────────────┐          ┌──────────────┐
   │ 变量/类型    │          │ 方法          │          │ 控制流        │
   │ int double  │          │ 签名/参数     │          │ if/else      │
   │ String      │          │ 返回值        │          │ for-each     │
   │ final       │          │ 重载/重写      │          │ while(true)  │
   └──────┬──────┘          └──────┬───────┘          └──────┬───────┘
          │                        │                        │
          └────────────────────────┼────────────────────────┘
                                   ▼
                    ┌─────────────────────────────┐
                    │     面向对象核心（重点）        │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────┬───────────┼───────────┬────────────┐
          │            │           │           │            │
          ▼            ▼           ▼           ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ 类/对象   │ │ 继承      │ │ 多态      │ │ 抽象类    │ │ 接口      │
   │ new      │ │ extends  │ │ instanceof│ │ abstract  │ │ (预留学)  │
   │ 构造器   │ │ super    │ │ 强转      │ │ 模板方法  │ │          │
   │ this     │ │ protected│ │ 父引用    │ │           │ │          │
   └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │     异常处理（重要）           │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────┬───────────┼───────────┬────────────┐
          │            │           │           │            │
          ▼            ▼           ▼           ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ Exception │ │ RuntimeException │ │ throw     │ │ throws    │ │ try-catch │
   │ vs Error │ │ checked vs unchecked │ │ catch     │ │ finally   │ │ 自定义异常 │
   └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │     集合框架（核心工具）       │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────┬───────────┼───────────┬────────────┐
          │            │           │           │            │
          ▼            ▼           ▼           ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ List<E></e>  │ │ ArrayList│ │ Iterator  │ │ 泛型      │ │ Collections│
   │ Set      │ │ Map      │ │ for-each  │ │ <> 语法   │ │ .unmodifiable│
   └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │     设计模式（实战）           │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────┬───────────┼───────────┬────────────┐
          │            │           │           │            │
          ▼            ▼           ▼           ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ 单例模式  │ │ 模板方法  │ │ 工厂模式  │ │ 不可变对象│ │ 只读视图  │
   │ 饿汉式   │ │ Account  │ │ Bank     │ │ final字段 │ │ unmodifiable│
   │ private  │ │ withdraw │ │ Service  │ │          │ │          │
   └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │     开发工具（效率）           │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────┬───────────┼───────────┬────────────┐
          │            │           │           │            │
          ▼            ▼           ▼           ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ IDEA     │ │ Git      │ │ JUnit 5  │ │ javac    │ │ Scanner  │
   │ 调试     │ │ commit   │ │ @Test    │ │ java -cp │ │ .nextLine│
   │ 断点     │ │ 回退     │ │ assert   │ │ 类路径    │ │ .parseInt│
   └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │     进阶方向（可选）           │
                    └──────────────┬──────────────┘
                                   │
          ┌────────────┬───────────┼───────────┬────────────┐
          │            │           │           │            │
          ▼            ▼           ▼           ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ Stream   │ │ Lambda   │ │ JDBC/SQL │ │ Spring   │ │ 多线程    │
   │ API      │ │ 表达式   │ │ 文件IO   │ │ Boot     │ │ 并发      │
   └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘

第 22 节：从零开始学习路线图

给你规划一个 4 周 的完整学习计划，每周有明确目标。按计划走，学完就能独立写类似的小项目。

第 0 天：环境准备（4 小时）

时间 任务 验收标准 1h 下载安装 JDK 17+ java -version 显示版本号 1h 下载安装 IDEA Community 能新建 Java 项目 1h 写 HelloWorld 跑通 控制台输出 "Hello, Java!" 1h 过一遍预备知识大节 不要求全懂，扫一遍标题

第 1-2 天：写 domain 层（10 小时）

时间 任务 验收标准 2h 写 OverdraftException 能 new 出来并 catch 3h 写 Account 抽象类 new SavingAccount 能跑通 Account 的 deposit/withdraw 2h 写 SavingAccount 月利息算出来正确（1000×0.03/12=2.5） 2h 写 CheckingAccount 透支计算正确，边界情况（刚好用完额度） 1h 写 JUnit 测试 跑通 AccountTest、SavingAccountTest、CheckingAccountTest

第 3-4 天：写 domain/service 剩余（8 小时）

时间 任务 验收标准 3h 写 Customer + Bank 单例 Bank.getInstance() == Bank.getInstance() 为 true 1h 写 CurrencyFormatter format(1234.5) → "¥1,234.50" 2h 写 BankService service.withdraw(account, 800) 在余额 200、透支 500 时返回 false 2h 写 BankServiceTest 所有测试通过

第 5-6 天：UI 层 + 整体联调（8 小时）

时间 任务 验收标准 2h 写 Main + BankUI（自动演示版） 编译运行看到完整输出 4h 改成交互式菜单版 可以手动存钱取钱查余额 2h 测试各种边界情况 余额 0、透支刚好用完、名字为空等

第 7 天：复盘 + 重构（4 小时）

时间 任务 验收标准 1h 给关键类加 JavaDoc 注释 每个 public 方法有说明 1h 写 Preconditions 工具类，消除重复检查 提取公共方法 1h Git 初始化，提交初始版本 能 git log 看到提交记录 1h 翻一遍面试题 + 知识图谱 把不熟的知识点标出来

第 2 周：扩展练习（自选，10-15 小时）

选项 任务 学什么 基础版 加 Transaction 交易流水 持久化思维、历史记录 进阶版 加定期存款账户 继承、新的业务规则 挑战版 接 SQLite 数据库 JDBC、SQL 专家版 加 Spring Boot 做 REST API Web 开发、前后端分离

学习节奏建议

第 1-2 天：慢！把每行代码都琢磨清楚
第 3-4 天：稍快，重点在 Bank 单例和 Customer 关系
第 5-6 天：动手写交互，Scanner + while(true) 是你的朋友
第 7 天：查漏补缺，跑一遍所有测试
第 2 周开始：选一个扩展方向，边学边做
遇到卡壳的地方：先 Google，再看教程"逐行精讲"和"常见错误表"

必须记住的 10 个 Java 句子

public/private/protected 是访问权限，static 是类级别

extends 是继承，super() 必须在第一行

abstract 方法只有分号没有花括号

多态 = 父类引用指向子类对象

异常要么 try-catch 要么 throws，不能不管

List 用 ArrayList，集合框架比数组好用

final 类不能继承，final 字段不能改

== 比地址，.equals() 比内容

单例三要素：private 构造器 + static 字段 + getInstance()

分层架构改 UI 不动底层，这就是好处

附录 A：完整项目代码行数统计

文件                       行数    职责
─────────────────────────────────────────────────────────
OverdraftException.java     15    自定义异常
Account.java                44    抽象基类
SavingAccount.java          31    储蓄账户
CheckingAccount.java        52    支票账户
Customer.java               67    客户
Bank.java                   54    银行单例
CurrencyFormatter.java      16    工具类
BankService.java            59    业务服务
BankUI.java                348    交互式界面
Main.java                    8    入口
─────────────────────────────────────────────────────────
合计                       694    （不含空行和注释约 500 行有效代码）

附录 B：编译和运行速查

# 编译（Mac/Linux/Git Bash）

mkdir -p out
find src -name "*.java" | xargs javac -d out

# 运行

java -cp out com.bank.ui.Main

# Windows cmd 编译

mkdir out
for /r src %f in (*.java) do javac -d out %f

# 清理

rm -rf out

附录 C：IDEA 快捷键速查

快捷键 功能 Ctrl + Shift + F 全局搜索文本 Ctrl + Shift + N 快速打开文件 Ctrl + B / Ctrl + 点击 跳转到定义 Alt + 左右箭头 在跳转历史中前进/后退 Ctrl + O 显示类的所有方法 Ctrl + / 注释/取消注释（单行） Ctrl + Shift + / 注释/取消注释（多行） Ctrl + F9 编译 Shift + F10 运行 Shift + F9 Debug F7 Debug 单步进入（Step Into） F8 Debug 单步跳过（Step Over） F9 Debug 继续运行到下一个断点 Ctrl + Alt + T 选中代码包一层 try-catch/if 等

结语：恭喜你把整个项目从头到尾写完、跑通、理解了！这个项目虽然小，但涵盖了面向对象编程的几乎所有核心概念。接下来不管学 Python、C#、Go，你会发现类、对象、异常、集合、分层架构这些思想都是一样的——只是语法变了。祝编程之路一帆风顺！
