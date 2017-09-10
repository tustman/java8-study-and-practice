# Java8新特性总结(Lambda表达式,StreamAPI,Optional 容器类,全新日期操作类)
<hr>
## Lambda 表达式的基础语法(上联：左右遇一括号省 下联：左侧推断类型省 横批：能省则省)

### Lambda 表达式的基础语法：
    Java8中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符箭头操作符将 Lambda 表达式拆分成两部分：
    - 左侧：Lambda 表达式的参数列表
    - 右侧：Lambda 表达式中所需执行的功能， 即 Lambda 体

### 语法格式一：无参数，无返回值
 `() -> System.out.println("Hello Lambda!");`
### 语法格式二：有一个参数，并且无返回值
 `(x) -> System.out.println(x)`
### 语法格式三：若只有一个参数，小括号可以省略不写
`x -> System.out.println(x)`
### 语法格式四：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
```java
Comparator<Integer> com = (x, y) -> {
    System.out.println("函数式接口");
    return Integer.compare(x, y);
};
```
### 语法格式五：若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写
 `Comparator<Integer> com = (x, y) -> Integer.compare(x, y);`

### 语法格式六：Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断”
 `(Integer x, Integer y) -> Integer.compare(x, y);`

### Lambda 表达式需要“函数式接口”的支持
    函数式接口：接口中只有一个抽象方法的接口，称为函数式接口。 可以使用注解 @FunctionalInterface 修饰可以检查是否是函数式接口

### Java8 内置的四大核心函数式接口

- Consumer<T> : 消费型接口
		void accept(T t);

- Supplier<T> : 供给型接口
		T get();

- Function<T, R> : 函数型接口
		R apply(T t);

- Predicate<T> : 断言型接口
		boolean test(T t);

### 方法引用：若 Lambda 体中的功能，已经有方法提供了实现，可以使用方法引用（可以将方法引用理解为 Lambda 表达式的另外一种表现形式）

- 对象的引用 :: 实例方法名

- 类名 :: 静态方法名

- 类名 :: 实例方法名

> 注意：
 - 方法引用所引用的方法的参数列表与返回值类型，需要与函数式接口中抽象方法的参数列表和返回值类型保持一致！
 - 若Lambda 的参数列表的第一个参数，是实例方法的调用者，第二个参数(或无参)是实例方法的参数时，格式： ClassName::MethodName

### 构造器引用 :构造器的参数列表，需要与函数式接口中参数列表保持一致！

- 类名 :: new

### 数组引用

- 类型[] :: new;

<hr>

## Stream API 的操作步骤

1. 创建 Stream
2. 中间操作
3. 终止操作(终端操作), 注意：流进行了终止操作后，不能再次使用

### 筛选与切片
- filter——接收 Lambda ， 从流中排除某些元素。
- limit——截断流，使其元素不超过给定数量。
- skip(n) —— 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
- distinct——筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素

### 映射
- map——接收 Lambda ， 将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
- flatMap——接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流

### 排序
- sorted()——自然排序
- sorted(Comparator com)——定制排序

### 终止操作
- allMatch——检查是否匹配所有元素
- anyMatch——检查是否至少匹配一个元素
- noneMatch——检查是否没有匹配的元素
- findFirst——返回第一个元素
- findAny——返回当前流中的任意元素
- count——返回流中元素的总个数
- max——返回流中最大值
- min——返回流中最小值
- reduce(T identity, BinaryOperator) / reduce(BinaryOperator); ——归约:可以将流中元素反复结合起来，得到一个值。
- collect ——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法

<hr>

## Optional 容器类：用于尽量避免空指针异常
- Optional.of(T t) : 创建一个 Optional 实例
- Optional.empty() : 创建一个空的 Optional 实例
- Optional.ofNullable(T t):若 t 不为 null,创建 Optional 实例,否则创建空实例
- isPresent() : 判断是否包含值
- orElse(T t) :  如果调用对象包含值，返回该值，否则返回t
- orElseGet(Supplier s) :如果调用对象包含值，返回该值，否则返回 s 获取的值
- map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回 Optional.empty()
- flatMap(Function mapper):与 map 类似，要求返回值必须是Optional

<hr>

## 全新日期操作接口及以上功能示例,参见本项目代码示例.