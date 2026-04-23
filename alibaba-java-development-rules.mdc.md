---
description: 阿里巴巴 Java 开发规范，适用于本项目所有 Java 代码的编写、审查和重构。凡涉及 Java 代码生成、命名规范、异常处理、日志规范、SQL 编写、并发编程、集合使用、OOP 设计及安全编码等场景，必须遵循本规范。
globs:
  - "**/*.java"
alwaysApply: false
---

# 阿里巴巴 Java 开发手册规约

基于《阿里巴巴Java开发手册》（嵩山版）整理，适用于本项目所有 Java 代码编写。规约分为【强制】、【推荐】、【参考】三个级别。

> **使用说明**：
> - **【强制】** — 必须遵守，违反将导致代码审查不通过
> - **【推荐】** — 尽量遵守，有利于代码质量提升
> - **【参考】** — 理解背景后酌情采纳

---

## 一、编程规约 - 命名风格

### 【强制】规则

1. 代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。
   - 反例：`_name` / `__name` / `$Object` / `name_` / `name$` / `Object$`

2. 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。
   - 正例：`alibaba` / `taobao` / `youku` / `hangzhou` 等国际通用的名称，可视同英文。
   - 反例：`DaZhePromotion` [打折] / `getPingfenByName()` [评分] / `int 某变量 = 3`

3. 类名使用 UpperCamelCase 风格，但 DO / BO / DTO / VO / AO 除外。
   - 正例：`MarcoPolo` / `UserDO` / `XmlService` / `TcpUdpDeal` / `TaPromotion`
   - 反例：`macroPolo` / `UserDo` / `XMLService` / `TCPUDPDeal` / `TAPromotion`

4. 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格。
   - 正例：`localValue` / `getHttpMessage()` / `inputUserId`

5. 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。
   - 正例：`MAX_STOCK_COUNT`
   - 反例：`MAX_COUNT`

6. 抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类命名以它要测试的类的名称开始，以 Test 结尾。

7. 中括号是数组类型的一部分，数组定义如下：`String[] args`。
   - 反例：使用 `String args[]` 的方式来定义。

8. POJO 类中布尔类型的变量，都不要加 is，否则部分框架解析会引起序列化错误。
   - 反例：定义为基本数据类型 `Boolean isDeleted`，其方法也是 `isDeleted()`，RPC 框架在反向解析时"以为"对应的属性名称是 `deleted`，导致属性获取不到。

9. 包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。
   - 正例：应用工具类包名为 `com.alibaba.open.util`、类名为 `MessageUtils`

10. 杜绝不完全规范的缩写，避免望文不知义。
    - 反例：`AbstractClass` "缩写"命名成 `AbsClass`；`condition` "缩写"命名成 `condi`

11. Service 和 DAO 类，基于 SOA 的理念，暴露出来的服务一定是接口，内部的实现类用 Impl 的后缀与接口区别。
    - 正例：`CacheServiceImpl` 实现 `CacheService` 接口。

### 【推荐】规则

12. 如果使用到了设计模式，建议在类名中体现出具体模式。
    - 正例：`public class OrderFactory;` / `public class LoginProxy;` / `public class ResourceObserver;`

13. 接口类中的方法和属性不要加任何修饰符号（public 也不要加），保持代码的简洁性，并加上有效的 Javadoc 注释。尽量不要在接口里定义变量。
    - 正例：接口方法签名 `void f();`，接口基础常量 `String COMPANY = "alibaba";`
    - 反例：接口方法定义 `public abstract void f();`

14. 如果是形容能力的接口名称，取对应的形容词做接口名（通常是 –able 的形式）。
    - 正例：`AbstractTranslator` 实现 `Translatable`。

### 【参考】规则

15. 枚举类名建议带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开。
    - 正例：枚举名字 `DealStatusEnum`，成员名称：`SUCCESS` / `UNKOWN_REASON`

16. 各层命名规约：
    - Service/DAO 层方法命名：获取单个对象用 `get` 前缀；获取多个对象用 `list` 前缀；获取统计值用 `count` 前缀；插入用 `save`（推荐）或 `insert` 前缀；删除用 `remove`（推荐）或 `delete` 前缀；修改用 `update` 前缀。
    - 领域模型命名：数据对象 `xxxDO`；数据传输对象 `xxxDTO`；展示对象 `xxxVO`；POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 `xxxPOJO`。

---

## 二、编程规约 - 常量定义

### 【强制】规则

1. 不允许任何魔法值（即未经定义的常量）直接出现在代码中。
   - 反例：
     ```java
     String key = "Id#taobao_" + tradeId;
     cache.put(key, value);
     ```

2. long 或者 Long 初始赋值时，必须使用大写的 L，不能是小写的 l，小写容易跟数字 1 混淆。
   - 说明：`Long a = 2l;` 写的是数字的 21，还是 Long 型的 2？

### 【推荐】规则

3. 不要使用一个常量类维护所有常量，应该按常量功能进行归类，分开维护。
   - 正例：缓存相关的常量放在类 `CacheConsts` 下；系统配置相关的常量放在类 `ConfigConsts` 下。

4. 常量的复用层次有五层：跨应用共享常量、应用内共享常量、子工程内共享常量、包内共享常量、类内共享常量。

5. 如果变量值仅在一个范围内变化，且带有名称之外的延伸属性，定义为枚举类。

---

## 三、编程规约 - 代码格式

### 【强制】规则

1. 大括号的使用约定。如果是大括号内为空，则简洁地写成 `{}` 即可；如果是非空代码块则：左大括号前不换行，左大括号后换行，右大括号前换行，右大括号后还有 else 等代码则不换行，表示终止的右大括号后必须换行。

2. 左小括号和字符之间不出现空格；右小括号和字符之间也不出现空格。
   - 反例：`if (空格 a == b 空格)`

3. if / for / while / switch / do 等保留字与括号之间都必须加空格。

4. 任何二目、三目运算符的左右两边都需要加一个空格，包括赋值运算符 `=`。

5. 采用 4 个空格缩进，禁止使用 Tab 字符。
   - 说明：如果使用 Tab 缩进，必须设置 1 个 Tab 为 4 个空格。IDEA 设置 Tab 为 4 个空格时，请勿勾选 `Use tab character`。

6. 单行字符数限制不超过 120 个，超出需要换行，换行时遵循如下原则：
   - 第二行相对第一行缩进 4 个空格，从第三行开始不再继续缩进。
   - 运算符与下文一起换行。
   - 点号与下文一起换行。
   - 调用方法的多个参数需要换行时，在逗号后进行。
   - 在括号前不要换行。

7. 方法参数在定义和传入时，多个参数逗号后边必须加空格。
   - 正例：`method(args1, args2);`
   - 说明：下例中实参和逗号之间有空格。

8. IDE 的 text file encoding 设置为 UTF-8；IDE 中文件的换行符使用 Unix 格式，不要使用 Windows 格式。

9. 没有必要增加若干空行来隔开代码逻辑，不同逻辑、不同语义之间插入一个空行即可。

---

## 四、编程规约 - OOP 规约

### 【强制】规则

1. 避免通过一个类的对象引用来访问此类的静态变量或静态方法，无谓增加编译器解析成本，直接用类名来访问即可。

2. 所有的覆写方法，必须加 `@Override` 注解。
   - 说明：`getObject()` 与 `get0bject()` 的问题。一个是字母 O，一个是数字 0，加 `@Override` 注解可以准确判断是否覆盖成功。

3. 相同参数类型，同等业务含义，才可以使用 Java 的可变参数，避免使用 Object，可变参数必须放置在参数列表的最后。

4. 外部正在调用或者二方库依赖的接口，不允许修改方法签名，避免对接口调用方产生影响。接口过时必须加 `@Deprecated` 注解，并清晰地说明采用的新接口或者新服务是什么。

5. 不能使用过时的类或方法。
   - 说明：`java.net.URLDecoder` 中的方法 `decode(String encodeStr)` 这个方法已经过时，应该使用双参数 `decode(String source, String encode)`。

6. `Object` 的 `equals` 方法容易抛空指针异常，应使用常量或确定有值的对象来调用 `equals`。
   - 正例：`"test".equals(object);`
   - 反例：`object.equals("test");`
   - 说明：推荐使用 `java.util.Objects#equals`（JDK7 引入的工具类）

7. 所有整型包装类对象之间值的比较，全部使用 `equals` 方法比较。
   - 说明：对于 `Integer var = ?` 在 -128 至 127 范围内的赋值，`Integer` 对象是在 `IntegerCache.cache` 产生，会复用已有对象，这个区间内的 `Integer` 值可以使用 `==` 进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用 `equals` 方法进行判断。

8. 浮点数之间的等值判断，基本数据类型不能用 `==` 来比较，包装数据类型不能用 `equals` 来判断。
   - 说明：浮点数采用"尾数+阶码"的编码方式，类似于科学计数法的"有效数字+指数"的表示方式。二进制无法精确表示大部分的十进制小数。
   - 正例：使用 `BigDecimal` 来进行浮点数的比较。

9. 定义 DO / DTO / VO 等 POJO 类时，不要设定任何属性默认值。
   - 反例：POJO 类的 `gmtCreate` 默认值为 `new Date()`，但是这个属性在数据提取时并没有置入具体值，那么这个属性将无法从数据中取出。

10. 序列化类新增属性时，请不要修改 `serialVersionUID` 字段，避免反序列失败；如果完全不兼容升级，避免反序列化混乱，那么请修改 `serialVersionUID` 值。
    - 说明：注意 `serialVersionUID` 不一致会抛出序列化运行时异常。

11. 构造方法里面禁止加入任何业务逻辑，如果有初始化逻辑，请放在 `init` 方法中。

12. POJO 类必须写 `toString` 方法。使用 IDE 中的工具 `source > generate toString` 时，如果继承了另一个 POJO 类，注意在前面加一下 `super.toString`。
    - 说明：在方法执行抛出异常时，可以直接调用 POJO 的 `toString()` 方法打印其属性值，便于排查问题。

13. 禁止在 POJO 类中，同时存在对应属性 `xxx` 的 `isXxx()` 和 `getXxx()` 方法，框架在调用属性 `xxx` 的提取方法时，不确定哪一个方法是被优先调用的。

14. 使用索引访问用 `String` 的 `split` 方法得到的数组时，需做最后一个分隔符后有无内容的检查，否则会有抛 `IndexOutOfBoundsException` 的风险。

15. 当一个类有多个构造方法，或者多个同名方法，这些方法应该按顺序放置在一起，便于阅读。

16. 类内方法定义的顺序依次是：公有方法或保护方法 > 私有方法 > getter / setter 方法。
    - 说明：公有方法是类的接口和维护者，对用户更关注，所以应先展示出来。

17. setter 方法中，参数名称与类成员变量名称一致，`this.成员名 = 参数名`。在 getter / setter 方法中，不要增加业务逻辑，增加排查问题的难度。

18. 循环体内，字符串的连接方式，使用 `StringBuilder` 的 `append` 方法进行扩展。
    - 反例：
      ```java
      String str = "start";
      for (int i = 0; i < 100; i++) {
          str = str + "hello";
      }
      ```
    - 说明：反编译出的字节码文件显示每次循环都会 `new` 出一个 `StringBuilder` 对象，然后进行 `append` 操作，最后通过 `toString` 方法返回 `String` 对象，造成内存资源浪费。

19. `final` 可以声明类、成员变量、方法、以及本地变量，下列情况使用 `final` 关键字：
    - 不允许被继承的类，如 `String` 类。
    - 不允许修改引用的域对象，如 POJO 类的成员变量。
    - 不允许被覆写的方法，如 POJO 类的 setter 方法。
    - 不允许运行过程中重新赋值的局部变量。
    - 避免上下文重复使用一个变量，使用 `final` 关键字可以保证重新赋值时报错，强制重新定义变量，方便命名。

20. 慎用 `Object` 的 `clone` 方法来拷贝对象。
    - 说明：对象 `clone` 方法默认是浅拷贝，若想实现深拷贝需重写 `clone` 方法实现域对象的深度遍历式拷贝。

### 【推荐】规则

21. 类成员与方法访问控制从严：
    - 如果不允许外部直接通过 `new` 来创建对象，那么构造方法必须是 `private`。
    - 工具类不允许有 `public` 或 `default` 构造方法。
    - 类非 `static` 成员变量并且与子类共享，必须是 `protected`。
    - 类非 `static` 成员变量并且仅在本类使用，必须是 `private`。
    - 类 `static` 成员变量如果仅在本类使用，必须是 `private`。
    - 若是 `static` 成员变量，必须考虑是否为 `final`。
    - 类成员方法只供类内部调用，必须是 `private`。
    - 类成员方法只对继承类公开，那么限制为 `protected`。
    - 说明：任何类、方法、参数、变量，严控访问范围。过于宽泛的访问范围，不利于模块解耦。

22. 使用 `String.valueOf(value)` 代替 `"" + value`。
    - 说明：当 `value` 为 `null` 时，`"" + value` 返回 `"null"` 字符串，而 `String.valueOf(value)` 返回 `"null"` 更符合语义。实际上两者在 `value` 为 `null` 时行为一致，但 `String.valueOf()` 更明确表达意图。

---

## 五、编程规约 - 集合处理

### 【强制】规则

1. 关于 `hashCode` 和 `equals` 的处理，遵循如下规则：
   - 只要覆写 `equals`，就必须覆写 `hashCode`。
   - 因为 `Set` 存储的是不重复的对象，依据 `hashCode` 和 `equals` 进行判断，所以 `Set` 存储的对象必须覆写这两种方法。
   - 如果自定义对象作为 `Map` 的键，那么必须覆写 `hashCode` 和 `equals`。
   - 说明：`String` 因为覆写了 `hashCode` 和 `equals` 方法，所以可以愉快地将 `String` 作为 `Map` 的 key 来使用。

2. `ArrayList` 的 `subList` 结果不可强转成 `ArrayList`，否则会抛出 `ClassCastException` 异常。
   - 说明：`subList` 返回的是 `ArrayList` 的内部类 `SubList`，并不是 `ArrayList` 而是 `ArrayList` 的一个视图，对于 `SubList` 子列表的所有操作最终会反映到原列表上。

3. 在 `subList` 场景中，高度注意对原集合元素的增加或删除，均会导致子列表的遍历、增加或删除产生 `ConcurrentModificationException` 异常。

4. 使用集合转数组的方法，必须使用集合的 `toArray(T[] array)`，传入的是类型完全一致、长度为 0 的空数组。
   - 正例：
     ```java
     List<String> list = new ArrayList<>(2);
     list.add("guan");
     list.add("bao");
     String[] array = list.toArray(new String[0]);
     ```
   - 说明：使用 `toArray` 带参方法，入参分配的空数组运行时内存大小正合所需。不需要预先分配或传入指定大小。

5. 使用工具类 `Arrays.asList()` 把数组转换成集合时，不能使用其修改集合相关的方法，它的 `add` / `remove` / `clear` 方法会抛出 `UnsupportedOperationException` 异常。
   - 说明：`asList` 的返回对象是一个 `Arrays` 内部类，并没有实现集合的修改方法。`Arrays.asList` 体现的是适配器模式，只是转换接口，后台的数据仍是数组。

6. 泛型通配符 `<? extends T>` 来接收返回的数据，此写法的泛型集合不能使用 `add` 方法，而 `<? super T>` 不能使用 `get` 方法，作为接口调用赋值时易出错。
   - 说明：扩展说一下 PECS（Producer Extends Consumer Super）原则：第一、频繁往外读取内容的，适合用 `<? extends T>`。第二、经常往里插入的，适合用 `<? super T>`。

7. 在无泛型限制定义的集合赋值给泛型限制的集合时，在使用集合元素时，不可避免地需要强制类型转换（向下转型），若类型不一致则抛出 `ClassCastException` 异常。

8. 不要在 `foreach` 循环里进行元素的 `remove` / `add` 操作。`remove` 元素请使用 `Iterator` 方式，如果并发操作，需要对 `Iterator` 对象加锁。
   - 正例：
     ```java
     Iterator<String> iterator = list.iterator();
     while (iterator.hasNext()) {
         String item = iterator.next();
         if (删除元素的条件) {
             iterator.remove();
         }
     }
     ```
   - 反例：
     ```java
     for (String item : list) {
         if ("1".equals(item)) {
             list.remove(item);
         }
     }
     ```

9. 在 JDK7 版本及以上，`Comparator` 实现类要满足如下三个条件，不然 `Arrays.sort`、`Collections.sort` 会抛出 `IllegalArgumentException` 异常。
   - 三个条件如下：1）x，y 的比较结果和 y，x 的比较结果相反。2）x > y，y > z，则 x > z。3）x = y，则 x，z 的比较结果和 y，z 的比较结果一致。
   - 说明：反例中使用 `Comparator` 的第一个条件就是违背了：x，y 比较结果和 y，x 的比较结果相反，即比较逻辑不对称。

10. 泛型集合使用时，在 JDK7 及以上版本使用 diamond 语法，即 `<>` 不需要指定泛型类型。
    - 正例：`List<String> list = new ArrayList<>();`

### 【推荐】规则

11. 集合初始化时，指定集合初始值大小。
    - 说明：`HashMap` 使用 `HashMap(int initialCapacity)` 初始化，如果暂时无法确定集合大小，请指定默认值 16（即 `new HashMap<>(16)`）。
    - 正例：`initialCapacity = (需要存储的元素个数 / 负载因子) + 1`。注意负载因子（即 loader factor）默认为 0.75，如果暂时无法确定初始值大小，请设置为 16（即默认值）。

12. 使用 `entrySet` 遍历 `Map` 类集合 KV，而不是 `keySet` 方式进行遍历。
    - 说明：`keySet` 其实是遍历了两次，一次是转为 `Iterator` 对象，另一次是从 `HashMap` 中取出 `key` 所对应的 `value`。而 `entrySet` 只是遍历了一次就把 `key` 和 `value` 都放到了 `Entry` 中，效率更高。如果是 JDK8，使用 `Map.forEach` 方法。

13. 高度注意 `Map` 类集合 K / V 能不能存储 `null` 值的情况：
    - `HashMap` 的 K/V 均可以为 `null`。
    - `ConcurrentHashMap` 的 K/V 均不可以为 `null`。
    - `TreeMap` 的 K 不可以为 `null`，V 可以为 `null`。

---

## 六、编程规约 - 并发处理

### 【强制】规则

1. 获取单例对象需要保证线程安全，其中的方法也要保证线程安全。
   - 说明：资源驱动类、工具类、单例工厂类都需要注意。

2. 创建线程或线程池时请赋予有意义的名称，方便出错时回溯。
   - 正例：自定义线程工厂，并且外部调用时命名线程名。
     ```java
     public class UserThreadFactory implements ThreadFactory {
         private final String namePrefix;
         private final AtomicInteger nextId = new AtomicInteger(1);
         public UserThreadFactory(String whatFeatureOfGroup) {
             namePrefix = "From UserThreadFactory's " + whatFeatureOfGroup + "-Worker-";
         }
         @Override
         public Thread newThread(Runnable r) {
             String name = namePrefix + nextId.getAndIncrement();
             return new Thread(null, r, name);
         }
     }
     ```

3. 线程资源必须通过线程池提供，不允许在应用中自行显式创建线程。
   - 说明：使用线程池的好处是减少在创建和销毁线程上所消耗的时间以及系统资源的开销，解决资源不足的问题。如果不使用线程池，有可能造成系统创建大量同类线程而导致消耗完内存或者"过度切换"的问题。

4. 线程池不允许使用 `Executors` 去创建，而是通过 `ThreadPoolExecutor` 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。
   - 说明：`Executors` 返回的线程池对象的弊端如下：1）`FixedThreadPool` 和 `SingleThreadPool`：允许的请求队列长度为 `Integer.MAX_VALUE`，可能会堆积大量的请求，从而导致 OOM。2）`CachedThreadPool`：允许的创建线程数量为 `Integer.MAX_VALUE`，可能会创建大量的线程，从而导致 OOM。

5. `SimpleDateFormat` 是线程不安全的类，一般不要定义为 `static` 变量，如果定义为 `static`，必须加锁，或者使用 `DateUtils` 工具类。
   - 正例：注意线程安全，使用 `ThreadLocal` 或 `DateTimeFormatter`（JDK8）。
     ```java
     private static final ThreadLocal<SimpleDateFormat> df = ThreadLocal.withInitial(
         () -> new SimpleDateFormat("yyyy-MM-dd")
     );
     ```

6. 必须回收自定义的 `ThreadLocal` 变量，尤其在线程池场景下，线程经常会被复用，如果不清理自定义的 `ThreadLocal` 变量，可能会影响后续业务逻辑和造成内存泄露等问题。尽量在代理中使用 `try-finally` 块进行回收。
   - 正例：
     ```java
     objectThreadLocal.set(userInfo);
     try {
         // ...
     } finally {
         objectThreadLocal.remove();
     }
     ```

7. 高并发时，同步调用应该去考量锁的性能损耗。能用无锁数据结构，就不要用锁；能锁区块，就不要锁整个方法体；能用对象锁，就不要用类锁。
   - 说明：尽可能使加锁的代码块工作量尽可能小，避免在锁代码块中调用 RPC 方法。

8. 对多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则可能会造成死锁。
   - 说明：线程一需要对表 A、B、C 依次全部加锁后才可以进行更新操作，那么线程二的加锁顺序也必须是 A、B、C，否则可能出现死锁。

9. 在使用阻塞等待获取锁的方式中，必须在 `try` 代码块之外，并且在加锁方法与 `try` 代码块之间没有任何可能抛出异常的方法调用，避免加锁成功后，在 `finally` 中解锁失败。
   - 正例：
     ```java
     Lock lock = new ReentrantLock();
     lock.lock();
     try {
         // do something
     } finally {
         lock.unlock();
     }
     ```

10. 在使用尝试机制来获取锁的方式中，进入业务代码块之前，必须先判断当前线程是否持有锁。锁释放时，不判断当前线程是否持有锁而直接释放锁也是不允许的。
    - 正例：
      ```java
      Lock lock = new ReentrantLock();
      if (lock.tryLock()) {
          try {
              // do something
          } finally {
              lock.unlock();
          }
      }
      ```

11. 并发修改同一记录时，避免更新丢失，需要加锁。要么在应用层加锁，要么在缓存加锁，要么在数据库层使用乐观锁，使用 version 作为更新依据。
    - 说明：如果每次访问冲突概率小于 20%，推荐使用乐观锁，否则使用悲观锁。乐观锁的重试次数不得小于 3 次。

12. 多线程并行处理定时任务时，`Timer` 运行多个 `TimeTask` 时，只要其中之一没有捕获抛出的异常，其它任务便会自动终止运行，使用 `ScheduledExecutorService` 则没有这个问题。

### 【推荐】规则

13. 资金相关的金融敏感信息，使用悲观锁策略。
    - 说明：乐观锁在获取锁的同时进行了数据修改，所以可能出现获取锁失败后重试，更新操作将改变最终结果。悲观锁先获取锁再修改，符合业务上"锁"的语义。

14. 使用 `CountDownLatch` 进行异步转同步操作，每个线程退出前必须调用 `countDown`，线程执行代码注意 `catch` 异常，确保 `countDown` 方法可以执行，避免主线程无法执行至 `await` 方法，直到超时才返回。
    - 说明：注意，子线程抛出异常堆栈，不能在主线程 `try-catch` 到。

15. 避免 `Random` 实例被多线程使用，虽然共享该实例是线程安全的，但会因竞争同一 `seed` 导致性能下降。
    - 说明：`Random` 实例包括 `java.util.Random` 的实例或者 `Math.random()` 的方式。正例：使用 `ThreadLocalRandom`。

---

## 七、编程规约 - 控制语句

### 【强制】规则

1. 在一个 `switch` 块内，每个 `case` 要么通过 `break` / `return` 等来终止，要么注释说明程序将继续执行到哪一个 `case` 为止；在一个 `switch` 块内，都必须包含一个 `default` 语句并且放在最后，即使它什么代码也没有。

2. 在 if / else / for / while / do 语句中必须使用大括号。
   - 说明：即使只有一行代码，也禁止不使用大括号。`if (condition) return;` 是错误写法。

3. 在表达异常的分支时，少用 if-else 方式，这种方式可以改写成：
   ```java
   if (condition) {
       ...
       return obj;
   }
   // 接着写 else 的业务逻辑代码;
   ```
   - 说明：如果非使用 if-else 不可，请勿超过 3 层，超过请使用状态模式或策略模式。

4. 除常用方法（如 `getXxx` / `isXxx`）等外，不要在条件判断中执行其它复杂的语句，将复杂逻辑判断的结果赋值给一个有意义的布尔变量名，以提高可读性。
   - 正例：
     ```java
     // 伪代码
     boolean existed = (file.open(fileName, "w") != null) && (...) || (...);
     if (existed) {
         ...
     }
     ```

### 【推荐】规则

5. 方法参数个数不应超过 5 个，超过则应考虑封装成对象。

6. 下列情形，需要进行参数校验：
   - 调用频次低的方法。
   - 执行时间开销很大的方法。此情形中，参数校验时间几乎可以忽略不计，但如果因为参数错误导致中间执行回退，或者错误，那得不偿失。
   - 需要极高稳定性和可用性的方法。
   - 对外提供的开放接口，不管是 RPC / API / HTTP 接口。
   - 敏感权限入口。

7. 下列情形，不需要进行参数校验：
   - 极有可能被循环调用的方法。但在方法说明里必须注明外部参数检查。
   - 底层调用频度较高的方法。毕竟像纯 `filter` 映射一般只是个通用方法，不需要做参数校验。
   - 被声明成 `private` 只能被自己代码调用的方法。

---

## 八、异常日志 - 异常处理

### 【强制】规则

1. Java 类库中定义的可以通过预检查方式规避的运行时异常，不应该通过 `catch` 的方式来处理，比如：`NullPointerException`、`IndexOutOfBoundsException` 等等。
   - 说明：无法通过预检查的异常除外，比如，在解析字符串形式的数字时，不得不通过 `catch` 来处理 `NumberFormatException`。正例：`if (obj != null) {...}` 而不是 `try { obj.method() } catch (NullPointerException e) {...}`

2. 异常捕获后不要用来做流程控制、条件控制。
   - 说明：异常设计的初衷是解决程序运行中的各种意外情况，且异常的处理效率比条件判断方式要低很多。

3. `catch` 时请分清稳定代码和非稳定代码，稳定代码指的是无论如何不会出错的代码。对于非稳定代码的 `catch` 尽可能进行区分异常类型，再做对应的异常处理。
   - 说明：对大段代码进行 `try-catch`，将使得无法根据异常来定位具体的出错代码位置，也无法做出针对性的处理。

4. 捕获异常是为了处理它，不要捕获了却什么都不处理而抛弃之。如果不想处理它，请将该异常抛给它的调用者。最外层的业务使用者，必须处理异常，将其转化为用户可以理解的内容。

5. 事务场景中，抛出异常被 `catch` 后，如果需要回滚，一定要注意手动回滚事务。

6. `finally` 块必须对资源对象、流对象进行关闭，如果有异常则做 `try-catch`。
   - 说明：如果 JDK7 及以上，可以使用 `try-with-resources` 方式。

7. 不要在 `finally` 块中使用 `return`。
   - 说明：`finally` 块中的 `return` 返回后方法结束执行，不会再执行 `try` 块中的 `return` 语句。

8. 捕获异常与抛异常，必须是完全匹配，或者捕获异常是抛异常的父类。

9. 在调用 RPC、二方包或动态生成类的相关方法时，捕捉异常必须使用 `Throwable` 类来进行拦截。
   - 说明：通过反射机制来调用方法时，如果找不到方法，抛出 `NoSuchMethodException`。在 RPC 框架中，通常抛出框架自定义的异常，这些异常继承自 `Exception`，但也有可能抛出 `Error`，因此使用 `Throwable` 拦截更安全。

### 【推荐】规则

10. 方法的返回值可以为 `null`，不强制返回空集合或者空对象等，但必须添加注释充分说明什么情况下会返回 `null` 值。

11. 防止 NPE（空指针异常），是程序员的基本修养，注意 NPE 产生的场景：
    - 返回类型为基本数据类型，`return` 包装数据类型的对象时，自动拆箱有可能产生 NPE。
    - 数据库的查询结果可能为 `null`。
    - 集合里的元素即使 `isNotEmpty`，取出的数据元素也可能为 `null`。
    - 远程调用返回对象时，一律进行空指针判断，防止 NPE。
    - 对于 `Session` 中获取的数据，建议进行 NPE 检查，避免空指针。
    - 级联调用 `obj.getA().getB().getC()`，一连串调用，易产生 NPE。
    - 使用 JDK8 的 `Optional` 类来防止 NPE。

12. 定义时区分 `unchecked` / `checked` 异常，避免直接抛出 `RuntimeException`，更不允许抛出 `Exception` 或者 `Throwable`，应使用有业务含义的自定义异常来替代。

---

## 九、异常日志 - 日志规约

### 【强制】规则

1. 应用中不可直接使用日志系统（Log4j、Logback）中的 API，而应依赖使用日志框架（SLF4J）中的 API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。
   - 正例：`import org.slf4j.Logger; import org.slf4j.LoggerFactory; private static final Logger logger = LoggerFactory.getLogger(Abc.class);`

2. 所有日志文件至少保存 15 天，因为有些异常具备以"周"为频次的特征。

3. 应用中的扩展日志（如打点、临时监控、访问日志等）命名方式：`appName_logType_logName.log`。logType 为日志类型，推荐分类有 stats / desc / monitor / visit 等；logName 为日志描述。这种命名的好处：通过文件名就可以知道日志文件属于什么应用，什么类型，什么目的，也有利于归类查找。
   - 正例：`mppserver_monitor_timeZoneConvert.log` 说明：推荐对日志进行分类，如将错误日志和业务日志分开存储，方便开发人员查看，也方便通过日志对系统进行及时监控。

4. 对 `trace` / `debug` / `info` 级别的日志输出，必须使用日志占位符的方式，不要进行字符串拼接。
   - 正例：`logger.debug("Processing trade with id: {}", id);`
   - 说明：如果 `debug` 级别关闭了，但是仍然做了字符串拼接操作，产生了浪费。

5. 避免重复打印日志，浪费磁盘空间。务必在日志配置文件中设置 `additivity=false`。
   - 正例：`<logger name="com.taobao.dubbo.config" additivity="false">`

6. 异常信息应该包括两类信息：案发现场信息和异常堆栈信息。如果不处理，那么通过关键字 `throws` 往上抛出。
   - 正例：`logger.error("inputParams: {} and it is an error", e);` 或 `logger.error("inputParams: {} and it is an error", new Object[]{e});`
   - 说明：只记录 `e.getMessage()` 会丢失重要的堆栈信息，不利于问题排查。正确做法是将整个异常对象传入。

### 【推荐】规则

7. 谨慎地记录日志。生产环境禁止输出 `debug` 日志；有选择地输出 `info` 日志；如果使用 `warn` 来记录刚上线时的业务行为信息，一定要注意日志输出量的问题，避免把服务器磁盘撑爆，并记得及时删除这些观察日志。

8. 可以使用 `warn` 日志级别来记录用户输入参数错误的情况，避免用户投诉时，无所适从。如非必要，请不要在此场景打出 `error` 级别，避免频繁报警。

9. `error` 级别只记录系统逻辑出错、异常或重要的错误信息。

---

## 十、MySQL 数据库 - 建表规约

### 【强制】规则

1. 表达是与否概念的字段，必须使用 `is_xxx` 的方式命名，数据类型是 `unsigned tinyint`（1 表示是，0 表示否）。
   - 说明：任何字段如果为非负数，必须是 `unsigned`。注意：POJO 类中的布尔属性不能加 `is` 前缀，数据库中必须加 `is` 前缀，需要在 `<resultMap>` 中进行字段与属性之间的映射。

2. 表名、字段名必须使用小写字母或数字，单词间用下划线分隔，禁止出现数字开头，禁止两个下划线中间只出现数字。
   - 正例：`getter_admin`，`task_config`，`level3_name`
   - 反例：`GetterAdmin`，`taskConfig`，`level_3_name`

3. 表名不使用复数名词。
   - 说明：表名应该仅仅表示表里面的实体内容，不应该表示实体数量。

4. 禁用保留字，如 `desc`、`range`、`match`、`delayed` 等，请参考 MySQL 官方保留字。

5. 主键索引名为 `pk_字段名`；唯一索引名为 `uk_字段名`；普通索引名为 `idx_字段名`。
   - 说明：`pk_` 即 primary key；`uk_` 即 unique key；`idx_` 即 index 的简称。

6. 小数类型为 `decimal`，禁止使用 `float` 和 `double`。
   - 说明：`float` 和 `double` 在存储时存在精度损失。如果存储的数据范围超过 `decimal` 的范围，建议将数据拆成整数和小数分开存储。

7. 如果存储的字符串长度几乎相等，使用 `char` 定长字符串类型。

8. `varchar` 是可变长字符串，不预先分配存储空间，长度不要超过 5000，如果存储长度大于此值，定义字段类型为 `text`，独立出来一张表，用主键来对应，避免影响其它字段索引效率。

9. 表必备三字段：`id`、`create_time`、`update_time`。
   - 说明：其中 `id` 必为主键，类型为 `bigint unsigned`、单表时自增、步长为 1。`create_time` 和 `update_time` 的类型均为 `datetime`。

10. 单表行数超过 500 万行或者单表容量超过 2 GB，才推荐进行分库分表。
    - 说明：如果预计三年后的数据量根本达不到这个级别，请不要在创建表时就分库分表。

---

## 十一、MySQL 数据库 - SQL 语句

### 【强制】规则

1. 不要使用 `count(列名)` 或 `count(常量)` 来替代 `count(*)`，`count(*)` 是 SQL92 定义的标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。
   - 说明：`count(*)` 会统计值为 NULL 的行，而 `count(列名)` 不会统计此列为 NULL 值的行。

2. `count(distinct col)` 计算该列除 NULL 之外的不重复行数，注意 `count(distinct col1, col2)` 如果其中一列全为 NULL，那么即使另一列有不同的值，也返回 0。

3. 当某一列的值全是 NULL 时，`count(col)` 的返回结果为 0，但 `sum(col)` 的返回结果为 NULL，因此使用 `sum()` 时需注意 NPE 问题。
   - 正例：可以使用如下方式来避免 NPE：`SELECT IFNULL(SUM(column), 0) FROM table;`

4. 使用 `ISNULL()` 来判断是否为 NULL 值。
   - 说明：NULL 与任何值的直接比较都为 NULL。

5. 代码中写分页查询逻辑时，若 `count` 为 0 应直接返回，避免执行后面的分页语句。

6. 不得使用外键与级联，一切外键概念必须在应用层解决。
   - 说明：以学生和成绩的关系为例，学生表中的 `student_id` 是主键，那么成绩表中的 `student_id` 为外键。如果更新学生表中的 `student_id`，同时触发成绩表中的 `student_id` 更新，即为级联更新。外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴风险；外键影响数据库的插入速度。

7. 禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。

8. 数据订正（特别是删除或修改记录操作）时，要先 `SELECT`，避免出现误删除，确认无误才能执行更新语句。

9. `in` 操作后边的集合元素数量限制在 1000 个之内。

---

## 十二、工程结构 - 应用分层

### 【推荐】规则

1. 推荐分层结构：
   - **开放 API 层**：可直接封装 Service 接口暴露成 RPC 接口；通过 Web 封装成 HTTP 接口；网关控制层等。
   - **终端显示层**：各个端的模板渲染执行过的层。当前主要是 velocity 渲染，JS 渲染，JSP 渲染，移动端展示等。
   - **Web 层**：主要是对访问控制进行转发，各类基本参数校验，或者不复用的业务简单处理等。
   - **Service 层**：相对具体的业务逻辑服务层。
   - **Manager 层**：通用业务处理层，它有如下特征：1）对第三方平台封装的层，预处理返回结果及转化异常信息，适配上层接口。2）对 Service 层通用能力的下沉，如缓存方案、中间件通用处理。3）与 DAO 层交互，对多个 DAO 的组合复用。
   - **DAO 层**：数据访问层，与底层 MySQL、Oracle、Hbase、OB 等进行数据交互。
   - **外部接口或第三方平台**：包括其它部门 RPC 开放接口，基础平台，其它公司的 HTTP 接口。

2. （分层异常处理规约）在 DAO 层，产生的异常类型有很多，无法用细粒度的异常进行 `catch`，使用 `catch(Exception e)` 方式，并 `throw new DAOException(e)`，不需要打印日志，因为日志在 Manager / Service 层一定需要捕获并打印到日志文件中去，如果再在 DAO 层打印一次，那么日志重复记录了。

3. （分层领域模型规约）DO 对象在 Manager / DAO 层使用，DTO 对象在 Manager / Service 层转换使用，VO 对象在 Web 层转换使用。

---

## 十三、工程结构 - 二方库依赖

### 【强制】规则

1. 线上应用不要依赖 SNAPSHOT 版本（安全包除外）；正式发布的二方库必须先去中央仓库，请勿把公司内部的二方库发布到中央仓库，以免造成重大损失。

2. 二方库的包命名规则：`groupId` + `artifactId` + `version`。
   - `groupId` 格式：`com.{公司}.biz.{业务线}`，最多不要超过 5 级。
   - `artifactId` 格式：`产品线名-模块名`，语义不冲突不重复。

3. 二方库版本号命名方式：`主版本号.次版本号.修订号`
   - 主版本号：产品方向改变，或者大规模 API 不兼容，或者架构不兼容升级。
   - 次版本号：保持相对兼容性，增加主要功能特性，影响范围极小的 API 不兼容修改。
   - 修订号：保持完全兼容性，修复 BUG、新增次要功能特性等。

4. 线上应用不要依赖 SNAPSHOT 版本（安全包除外）；不依赖新版的 SNAPSHOT 包意味着所有依赖包都是稳定版，线上操作就有保证，并且基于稳定版的依赖是可以传递的。

---

## 十四、安全规约

### 【强制】规则

1. 隶属于用户个人的页面或者功能必须进行权限控制校验。
   - 说明：防止没有做水平权限校验就可随意访问、修改、删除别人的数据，比如查看他人的私信内容。

2. 用户敏感数据禁止直接展示，必须对展示数据进行脱敏。
   - 说明：中国大陆个人手机号码显示：`139****1219`，隐藏中间 4 位，防止隐私泄露。

3. 用户输入的 SQL 参数严格使用参数绑定或者 METADATA 校验，防止 SQL 注入，禁止字符串拼接 SQL 访问数据库。

4. 用户请求传入的任何参数必须做有效性验证。
   - 说明：忽略参数校验可能导致：`page size` 过大导致内存溢出；`order by` 导致慢查询；因为 SQL 注入导致数据泄露等。

5. 禁止向 HTML 页面输出未经安全过滤或未正确转义的用户数据。

6. 表单、AJAX 提交必须执行 CSRF 安全验证。
   - 说明：CSRF（Cross-site Request Forgery）跨站请求伪造是一类常见攻击，指的是用户已登录网站 A 的情况下，被诱导访问了危险网站 B，网站 B 向网站 A 发起恶意请求，利用用户在网站 A 的登录状态执行攻击。

7. 在使用平台资源，譬如短信、邮件、电话、下单、支付，必须实现正确的防重放的机制，如数量限制、疲劳度控制、验证码校验，避免被滥刷而导致资损。
   - 说明：如注册时发送验证码到手机，如果没有图片验证码或次数限制，则可以被用于短信轰炸。

8. 发贴、评论、发送即时消息等用户生成内容的场景必须实现防刷控制，严格限制生成频率，否则会被恶意大量生成垃圾信息。

---

## 附：本项目的特别注意事项

基于本项目（****）的实际情况，额外强调：

1. 这里可以写上本项目的一些附加规范
