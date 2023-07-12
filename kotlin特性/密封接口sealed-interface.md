### 什么是密封接口？

密封接口（sealed interface）是kotlin 1.5引入的一个新特性，它可以让我们定义一个**限制性的类层次结构**，也就是说，我们可以在编译时就知道一个密封接口有哪些可能的子类型。这样，我们就可以更好地控制继承关系，避免出现意外的子类型。

密封接口与密封类（sealed class）类似，都可以用来表示一组有限的可能性。但是，密封类只能有一个实例，而密封接口的子类型可以有多个实例。此外，密封类只能被类继承，而密封接口可以被类和枚举类（enum class）实现。

要声明一个密封接口，我们需要在interface关键字前加上sealed修饰符：

```kotlin
sealed interface Error // 密封接口
```

一个密封接口可以有抽象或默认实现的方法，也可以有属性：

```kotlin
sealed interface Shape { // 密封接口
    val area: Double // 属性
    fun draw() // 抽象方法
    fun printArea() { // 默认实现方法
        println("The area is $area")
    }
}
```

### 密封接口的优点

使用密封接口有以下几个优点：

- **类型安全**：由于密封接口的子类型是固定的，我们可以在编译时就检查是否覆盖了所有可能的情况。这样，我们就不会遗漏某些分支或者处理错误的类型。
- **可读性**：使用密封接口可以让我们清楚地看到一个类型有哪些变种。这样，我们就可以更容易地理解和维护代码。
- **灵活性**：使用密封接口可以让我们定义更多样化的子类型。我们可以使用数据类（data class），对象（object），普通类（class），或者另一个密封类（sealed class）作为子类型。我们还可以在不同的文件或模块中定义子类型。
- **表达力**：使用密封接口可以让我们利用多态（polymorphism）和继承（inheritance）来实现更复杂和优雅的设计模式。

### 密封接口在设计模式中的应用

设计模式是一些经过验证和总结的解决特定问题的代码结构和技巧。使用设计模式可以让我们编写出更高效，更可复用，更易扩展的代码。

下面，我们将介绍几种常见的设计模式，并展示如何使用密封接口来实现它们。

#### 策略模式

策略模式（Strategy Pattern）是一种行为型设计模式，它可以让我们在运行时根据不同的情况选择不同的算法或策略。这样，我们就可以将算法的定义和使用分离，提高代码的灵活性和可维护性。

要实现策略模式，我们可以使用密封接口来定义一个策略的抽象，然后使用不同的子类型来实现具体的策略。例如，我们可以定义一个排序策略的密封接口，然后使用不同的排序算法作为子类型：

```kotlin
sealed interface SortStrategy { // 密封接口
    fun sort(list: List<Int>): List<Int> // 抽象方法
}

object BubbleSort : SortStrategy { // 对象
    override fun sort(list: List<Int>): List<Int> {
        // 实现冒泡排序
    }
}

object QuickSort : SortStrategy { // 对象
    override fun sort(list: List<Int>): List<Int> {
        // 实现快速排序
    }
}

object MergeSort : SortStrategy { // 对象
    override fun sort(list: List<Int>): List<Int> {
        // 实现归并排序
    }
}
```

然后，我们可以定义一个上下文类（Context Class），它可以持有一个策略的引用，并根据需要切换不同的策略：

```kotlin
class Sorter(var strategy: SortStrategy) { // 上下文类
    fun sort(list: List<Int>): List<Int> {
        return strategy.sort(list) // 调用策略的方法
    }
}
```

最后，我们可以在客户端代码中使用上下文类来执行不同的策略：

```kotlin
fun main() {
    val list = listOf(5, 3, 7, 1, 9)
    val sorter = Sorter(BubbleSort) // 创建上下文类，并指定初始策略
    println(sorter.sort(list)) // 使用冒泡排序
    sorter.strategy = QuickSort // 切换策略
    println(sorter.sort(list)) // 使用快速排序
    sorter.strategy = MergeSort // 切换策略
    println(sorter.sort(list)) // 使用归并排序
}
```

使用密封接口实现策略模式的优点是：

- 我们可以在编译时就知道有哪些可用的策略，避免出现无效或未知的策略。
- 我们可以使用数据类，对象，普通类或密封类作为子类型，根据不同的策略需要定义不同的属性和方法。
- 我们可以在不同的文件或模块中定义子类型，提高代码的模块化和可读性。

如果使用java实现策略模式，我们可能需要定义一个接口来表示策略，然后使用不同的类来实现接口：

```java
interface SortStrategy { // 接口
    List<Integer> sort(List<Integer> list); // 抽象方法
}

class BubbleSort implements SortStrategy { // 类
    @Override
    public List<Integer> sort(List<Integer> list) {
        // 实现冒泡排序
    }
}

class QuickSort implements SortStrategy { // 类
    @Override
    public List<Integer> sort(List<Integer> list) {
        // 实现快速排序
    }
}

class MergeSort implements SortStrategy { // 类
    @Override
    public List<Integer> sort(List<Integer> list) {
        // 实现归并排序
    }
}
```

然后，我们也需要定义一个上下文类来持有和切换策略：

```java
class Sorter { // 上下文类
    private SortStrategy strategy; // 策略引用

    public Sorter(SortStrategy strategy) { // 构造函数
        this.strategy = strategy;
    }

    public void setStrategy(SortStrategy strategy) { // 设置策略方法
        this.strategy = strategy;
    }

    public List<Integer> sort(List<Integer> list) {
        return strategy.sort(list); // 调用策略的方法
    }
}
```

最后，我们也可以在客户端代码中使用上下文类来执行不同的策略：

```java
public static void main(String[] args) {
    List<Integer> list = Arrays.asList(5, 3, 7, 1, 9);
    Sorter sorter = new Sorter(new BubbleSort()); // 创建上下文类，并指定初始策略
    System.out.println(sorter.sort(list)); // 使用冒泡排序
    sorter.setStrategy(new QuickSort()); // 切换策略
    System.out.println(sorter.sort(list)); // 使用快速排序
    sorter.setStrategy(new MergeSort()); // 切换策略
    System.out.println(sorter.sort(list)); // 使用归并排序
}
```

使用java实现策略模式的缺点是：

- 我们不能在编译时就知道有哪些可用的策略，因为任何类都可以实现接口。
- 我们只能使用类作为子类型，不能使用数据类或对象。
- 我们必须在同一个包中定义子类型，不能在不同的文件或模块中。

#### 访问者模式

访问者模式（Visitor Pattern）是一种行为型设计模式，它可以让我们在不修改原有类结构的情况下，为类添加新的操作或功能。这样，我们就可以将数据结构和操作分离，提高代码的扩展性和复用性。

要实现访问者模式，我们可以使用密封接口来定义一个元素（Element）的抽象，然后使用不同的子类型来实现具体的元素。例如，我们可以定义一个表达式（Expression）的密封接口，然后使用不同的子类型来表示不同的表达式：

```kotlin
sealed interface Expression { // 密封接口
    fun accept(visitor: Visitor): Any // 抽象方法，接受访问者
}

data class Number(val value: Int) : Expression { // 数据类
    override fun accept(visitor: Visitor): Any {
        return visitor.visitNumber(this) // 调用访问者的方法
    }
}

data class Sum(val left: Expression, val right: Expression) : Expression { // 数据类
    override fun accept(visitor: Visitor): Any {
        return visitor.visitSum(this) // 调用访问者的方法
    }
}

data class Product(val left: Expression, val right: Expression) : Expression { // 数据类
    override fun accept(visitor: Visitor): Any {
        return visitor.visitProduct(this) // 调用访问者的方法
    }
}
```

然后，我们可以定义一个访问者（Visitor）的接口，它可以为每种元素提供一个访问方法：

```kotlin
interface Visitor { // 访问者接口
    fun visitNumber(number: Number): Any // 访问数字表达式
    fun visitSum(sum: Sum): Any // 访问加法表达式
    fun visitProduct(product: Product): Any // 访问乘法表达式
}
```

最后，我们可以定义不同的访问者实现类，它们可以为元素提供不同的操作或功能。例如，我们可以定义一个求值（Evaluate）访问者，它可以计算表达式的值：

```kotlin
class Evaluate : Visitor { // 求值访问者
    override fun visitNumber(number: Number): Any {
        return number.value // 返回数字本身
    }

    override fun visitSum(sum: Sum): Any {
        return (sum.left.accept(this) as Int) + (sum.right.accept(this) as Int) // 返回左右子表达式之和
    }

    override fun visitProduct(product: Product): Any {
        return (product.left.accept(this) as Int) * (product.right.accept(this) as Int) // 返回左右子表达式之积
    }
}
```

我们还可以定义一个打印（Print）访问者，它可以打印表达式的字符串表示：

```kotlin
class Print : Visitor { // 打印访问者
    override fun visitNumber(number: Number): Any {
        return number.value.toString() // 返回数字的字符串
    }

    override fun visitSum(sum: Sum): Any {
        return "(${sum.left.accept(this)}) + (${sum.right.accept(this)})" // 返回加法的字符串
    }

    override fun visitProduct(product: Product): Any {
        return "(${product.left.accept(this)}) * (${product.right.accept(this)})" // 返回乘法的字符串
    }
}
```

使用密封接口实现访问者模式的优点是：

- 我们可以在编译时就知道有哪些可用的元素，避免出现无效或未知的元素。
- 我们可以使用数据类，对象，普通类或密封类作为子类型，根据不同的元素需要定义不同的属性和方法。
- 我们可以在不同的文件或模块中定义子类型，提高代码的模块化和可读性。
- 我们可以在不修改元素类的情况下，为它们添加新的访问者和操作。

如果使用java实现访问者模式，我们可能需要定义一个抽象类来表示元素，然后使用不同的子类来继承元素：

```java
abstract class Expression { // 抽象类
    public abstract Object accept(Visitor visitor); // 抽象方法，接受访问者
}

class Number extends Expression { // 子类
    private int value; // 属性

    public Number(int value) { // 构造函数
        this.value = value;
    }

    public int getValue() { // 获取属性值方法
        return value;
    }

    @Override
    public Object accept(Visitor visitor) {
        return visitor.visitNumber(this); // 调用访问者的方法
    }
}

class Sum extends Expression { // 子类
    private Expression left; // 属性
    private Expression right; // 属性

    public Sum(Expression left, Expression right) { // 构造函数
        this.left = left;
        this.right = right;
    }

    public Expression getLeft() { // 获取属性值方法
        return left;
    }

    public Expression getRight() { // 获取属性值方法
        return right;
    }

    @Override
    public Object accept(Visitor visitor) {
        return visitor.visitSum(this); // 调用访问者的方法
    }
}

class Product extends Expression { // 子类
    private Expression left; // 属性
    private Expression right; // 属性

    public Product(Expression left, Expression right) { // 构造函数
        this.left = left;
        this.right = right;
    }

    public Expression getLeft() { // 获取属性值方法
        return left;
    }

    public Expression getRight() { // 获取属性值方法
        return right;
    }

    @Override
    public Object accept(Visitor visitor) {
        return visitor.visitProduct(this); // 调用访问者的方法
    }
}
