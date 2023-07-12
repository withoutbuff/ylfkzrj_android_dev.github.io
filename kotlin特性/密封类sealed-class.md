# 一、什么是kotlin密封类？

密封类是一种特殊的类，它用来表示受限的类继承结构，即一个类只能有有限的几种子类，而不能有任何其他类型的子类。

密封类使用sealed关键字声明，在Kotlin 1.0中，密封类的所有子类必须嵌套在密封类内部；在Kotlin 1.1中，这个限制放宽了，允许将子类定义在同一个文件中；在Kotlin 1.5中，这个限制进一步放宽了，允许将子类定义在任何地方，只要保证子类可见性不高于父类。

密封类最大的优点是可以配合when表达式实现完备性检查（exhaustive check），即编译器可以检测出是否覆盖了所有可能的分支情况，如果没有则会报错或提示警告。这样可以避免遗漏某些情况导致逻辑错误或运行时异常。

例如，我们可以定义一个表示运算符的密封类，以及它的四个子类：
```kotlin
//定义一个密封类
sealed class Operator

//定义四个子类，分别表示加、减、乘、除
class Add : Operator()
class Subtract : Operator()
class Multiply : Operator()
class Divide : Operator()
```

# 二、kotlin密封类的优缺点

## 2.1 优点

- 密封类可以让我们在编译时就确定所有可能的子类类型，从而提高代码的安全性和可读性。
- 密封类可以让我们在使用when表达式时，不需要写else分支，因为所有的情况都已经覆盖了，这样可以减少代码的冗余和错误。
- 密封类可以让我们在设计模式中，更方便地实现一些模式，例如状态模式、访问者模式等。

## 2.2 缺点

- 密封类的子类必须在同一个文件中声明，或者嵌套在密封类的声明中，这样可能会导致文件过长或者结构不清晰。
- 密封类的子类不能被其他类继承，这样可能会限制了代码的复用性和扩展性。

# 三、kotlin密封类的使用方式

密封类适合用于表示有限的几种类型的情况，例如：

- 表示状态或者结果，例如成功、失败、加载中等。
- 表示运算符或者表达式，例如加、减、乘、除等。
- 表示事件或者动作，例如点击、滑动、拖拽等。

下面我们用一个简单的示例来演示如何使用密封类。我们定义一个表示计算器的密封类，它有两个子类，分别表示数字和运算符：

```kotlin
//定义一个表示计算器的密封类
sealed class Calculator

//定义两个子类，分别表示数字和运算符
data class Number(val value: Int) : Calculator()
data class Operator(val op: String) : Calculator()
```

然后我们定义一个函数，用来根据输入的密封类对象，计算结果：

```kotlin

//定义一个函数，用来根据输入的密封类对象，计算结果
fun calculate(a: Calculator, b: Calculator): Int {
    //使用when表达式，根据不同的子类类型，进行不同的操作
    return when (a) {
        //如果a是数字，再判断b的类型
        is Number -> when (b) {
            //如果b也是数字，就根据a的值和b的值进行运算
            is Number -> when (a.value) {
                //如果a的值是0，就返回0
                0 -> 0
                //如果a的值是1，就返回b的值
                1 -> b.value
                //否则，就根据b的运算符进行运算
                else -> when (b.op) {
                    //如果b的运算符是加号，就返回a的值加b的值
                    "+" -> a.value + b.value
                    //如果b的运算符是减号，就返回a的值减b的值
                    "-" -> a.value - b.value
                    //如果b的运算符是乘号，就返回a的值乘b的值
                    "*" -> a.value * b.value
                    //如果b的运算符是除号，就返回a的值除b的值
                    "/" -> a.value / b.value
                    //其他情况，就抛出异常
                    else -> throw IllegalArgumentException("Invalid operator: ${b.op}")
                }
            }
            //如果b是运算符，就抛出异常
            is Operator -> throw IllegalArgumentException("Cannot calculate two operators")
        }
        //如果a是运算符，就抛出异常
        is Operator -> throw IllegalArgumentException("Cannot calculate an operator and a number")
    }
}
```



#四、密封类与设计模式
##1.状态模式
状态模式是一种行为型设计模式，它允许一个对象在其内部状态改变时改变它的行为。状态模式将状态封装成独立的类，并将请求委托给当前的状态对象，从而实现状态的切换和行为的变化。

#####1.1kotlin实现
密封类可以表示一个有限的状态集合，而且可以在when表达式中进行完备的检查，不需要使用else分支。例如，假设有一个电灯类，它有两种状态：开和关，每种状态下可以执行不同的操作。可以用以下的代码来实现：

```kotlin
// 定义一个密封类表示状态
sealed class State {
    // 定义一个抽象方法表示操作
    abstract fun operate()
}

// 定义一个开状态的子类，继承自State
class On : State() {
    // 重写操作方法，打印信息并切换到关状态
    override fun operate() {
        println("The light is on, turn it off.")
        Light.state = Off()
    }
}

// 定义一个关状态的子类，继承自State
class Off : State() {
    // 重写操作方法，打印信息并切换到开状态
    override fun operate() {
        println("The light is off, turn it on.")
        Light.state = On()
    }
}

// 定义一个电灯类，它有一个状态属性，初始为关状态
object Light {
    var state: State = Off()

    // 定义一个方法，根据当前状态执行操作
    fun switch() {
        state.operate()
    }
}

// 测试代码
fun main() {
    // 创建一个电灯对象
    val light = Light
    // 调用switch方法，根据当前状态执行操作
    light.switch() // The light is off, turn it on.
    light.switch() // The light is on, turn it off.
    light.switch() // The light is off, turn it on.
}
```

#####1.2java实现
在java中，实现状态模式的一种方式是使用枚举类，因为枚举类也可以表示一个有限的状态集合，而且可以实现接口或抽象类，从而定义不同的行为。例如，用java实现上面的例子，可以用以下的代码：

```java
// 定义一个接口表示状态
interface State {
    // 定义一个抽象方法表示操作
    void operate();
}

// 定义一个枚举类表示状态，实现State接口
enum LightState implements State {
    // 定义两个枚举常量，分别表示开和关状态
    // 在每个枚举常量的构造器中，传入一个State对象，表示该状态下的行为
    ON(new State() {
        // 重写操作方法，打印信息并切换到关状态
        @Override
        public void operate() {
            System.out.println("The light is on, turn it off.");
            Light.state = OFF;
        }
    }),
    OFF(new State() {
        // 重写操作方法，打印信息并切换到开状态
        @Override
        public void operate() {
            System.out.println("The light is off, turn it on.");
            Light.state = ON;
        }
    });

    // 定义一个私有的State属性，表示该枚举常量对应的状态对象
    private final State state;

    // 定义一个私有的构造器，接收一个State对象作为参数，赋值给state属性
    private LightState(State state) {
        this.state = state;
    }

    // 定义一个公共的方法，调用state属性的operate方法
    public void operate() {
        state.operate();
    }
}

// 定义一个电灯类，它有一个状态属性，初始为关状态
class Light {
    // 定义一个静态的LightState属性，表示电灯的状态，初始为OFF
    public static LightState state = LightState.OFF;

    // 定义一个方法，根据当前状态执行操作
    public void switch() {
        state.operate();
    }
}

// 测试代码
public class Main {
    public static void main(String[] args) {
        // 创建一个电灯对象
        Light light = new Light();
        // 调用switch方法，根据当前状态执行操作
        light.switch(); // The light is off, turn it on.
        light.switch(); // The light is on, turn it off.
        light.switch(); // The light is off, turn it on.
    }
}
```

可以看出，使用kotlin的密封类实现状态模式的优点是：

- 代码更简洁，不需要定义接口或抽象类，也不需要使用匿名内部类
- 代码更安全，不需要担心在其他地方出现未知的状态，也不需要使用else分支处理默认情况
- 代码更易读，可以清楚地看出状态之间的层次关系和转换逻辑

使用java的枚举类实现状态模式的优点是：

- 代码更统一，不需要在不同的类中定义状态，也不需要使用对象来表示状态
- 代码更高效，不需要创建多个状态对象，也不需要使用多态来调用操作方法

##2. 访问者模式

访问者模式是一种行为型设计模式，它允许在不修改已有类的结构的情况下，定义作用于这些类的新操作。访问者模式将元素的结构和元素的操作分离，使得操作可以根据不同的元素类型而变化。

#####2.1kotlin实现
在kotlin中，可以使用密封类来实现访问者模式，因为密封类可以表示一个有限的元素集合，而且可以在when表达式中进行完备的检查，不需要使用else分支。例如，假设有一个表达式类，它有两种子类：数字和加法，每种子类都可以被访问者访问，执行不同的操作。可以用以下的代码来实现：

```kotlin
// 定义一个密封类表示表达式
sealed class Expr {
    // 定义一个抽象方法表示接受访问者的访问
    abstract fun <R> accept(visitor: Visitor<R>): R
}
    // 定义一个数字的子类，继承Expr类，有一个value属性表示数字的值
    data class Num(val value: Int) : Expr() {
        // 重写accept方法，调用访问者的visitNum方法，传入自己作为参数
        override fun <R> accept(visitor: Visitor<R>): R {
            return visitor.visitNum(this)
        }
    }

    // 定义一个加法的子类，继承Expr类，有两个Expr属性表示左右操作数
    data class Sum(val left: Expr, val right: Expr) : Expr() {
        // 重写accept方法，调用访问者的visitSum方法，传入自己作为参数
        override fun <R> accept(visitor: Visitor<R>): R {
            return visitor.visitSum(this)
        }
    }

    // 定义一个访问者接口，泛型参数R表示访问的结果类型
    interface Visitor<R> {
        // 定义一个访问数字的方法，接收一个Num对象作为参数，返回一个R类型的结果
        fun visitNum(num: Num): R
        // 定义一个访问加法的方法，接收一个Sum对象作为参数，返回一个R类型的结果
        fun visitSum(sum: Sum): R
    }

    // 定义一个求值的访问者类，实现Visitor接口，泛型参数为Int
    class EvalVisitor : Visitor<Int> {
        // 重写访问数字的方法，返回数字的值
        override fun visitNum(num: Num): Int {
            return num.value
        }
        // 重写访问加法的方法，返回左右操作数的求值结果的和
        override fun visitSum(sum: Sum): Int {
            return sum.left.accept(this) + sum.right.accept(this)
        }
    }

    // 定义一个打印的访问者类，实现Visitor接口，泛型参数为String
    class PrintVisitor : Visitor<String> {
        // 重写访问数字的方法，返回数字的字符串表示
        override fun visitNum(num: Num): String {
            return num.value.toString()
        }
        // 重写访问加法的方法，返回加法的字符串表示，用括号括起来
        override fun visitSum(sum: Sum): String {
            return "(${sum.left.accept(this)} + ${sum.right.accept(this)})"
        }
    }

    // 测试代码
    fun main() {
        // 创建一个表达式对象，表示1 + (2 + 3)
        val expr = Sum(Num(1), Sum(Num(2), Num(3)))
        // 创建一个求值的访问者对象
        val evalVisitor = EvalVisitor()
        // 创建一个打印的访问者对象
        val printVisitor = PrintVisitor()
        // 调用表达式的accept方法，传入求值的访问者，打印求值的结果
        println(expr.accept(evalVisitor)) // 6
        // 调用表达式的accept方法，传入打印的访问者，打印表达式的字符串表示
        println(expr.accept(printVisitor)) // (1 + (2 + 3))
    }
```

可以看出，使用kotlin的密封类实现访问者模式的优点是：

- 代码更简洁，不需要定义抽象方法或接口，也不需要使用类型转换或类型检查
- 代码更安全，不需要担心在其他地方出现未知的表达式类型，也不需要使用else分支处理默认情况
- 代码更易读，可以清楚地看出表达式之间的层次关系和访问者的操作逻辑
#####2.2 java实现
```
// 抽象元素
interface Animal {
    void accept(Visitor visitor); // 接受一个抽象访问者
}

// 具体元素Lion
class Lion implements Animal {
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this); // 调用具体访问者对自己进行操作
    }

    public String roar() {
        return "Roar!";
    }
}

// 具体元素Tiger
class Tiger implements Animal {
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this); // 调用具体访问者对自己进行操作
    }

    public String growl() {
        return "Growl!";
    }
}

// 具体元素Elephant
class Elephant implements Animal {
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this); // 调用具体访问者对自己进行操作
    }

    public String trumpet() {
        return "Trumpet!";
    }
}

// 抽象访问者
interface Visitor {
    void visit(Lion lion); // 访问具体元素Lion

    void visit(Tiger tiger); // 访问具体元素Tiger

    void visit(Elephant elephant); // 访问具体元素Elephant
}

// 具体访问者Feed
class Feed implements Visitor {
  @Override 
  public void visit(Lion lion) { 
      System.out.println("Feed the lion with meat.");
  } 

  @Override 
  public void visit(Tiger tiger) { 
      System.out.println("Feed the tiger with chicken.");
  } 

  @Override 
  public void visit(Elephant elephant) { 
      System.out.println("Feed the elephant with banana.");
  } 
}

// 具体访问者Observe
class Observe implements Visitor { 
  @Override 
  public void visit(Lion lion) { 
      System.out.println("Observe the lion: " + lion.roar());
  } 

  @Override 
  public void visit(Tiger tiger) { 
      System.out.println("Observe the tiger: " + tiger.growl());
  } 

  @Override 
  public void visit(Elephant elephant) { 
      System.out.println("Observe the elephant: " + elephant.trumpet());
  }  
}

// 具体访问者Train
class Train implements Visitor {  
   @Override  
   public void visit(Lion lion) {  
       System.out.println("Train the lion to jump through a hoop.");  
   }  

   @Override  
   public void visit(Tiger tiger) {  
       System.out.println("Train the tiger to stand on a ball.");  
   }  

   @Override  
   public void visit(Elephant elephant) {  
       System.out.println("Train the elephant to sit on a stool.");  
   }   
}  

// 对象结构类Zoo

class Zoo {

 private List<Animal> animals = new ArrayList<>();

 // 添加一个新动物   
public void add(Animal animal) {   
     animals.add(animal);   
}   

 // 移除一个已有动物   
public void remove(Animal animal) {   
     animals.remove(animal);   
}   

 // 接受一个抽象访问者，并将所有动物传递给它进行处理    
public void accept(Visitor visitor) {    
     for (Animal animal : animals) {    
         animal.accept(visitor);    
     }    
}   
}
```
测试代码：
```
public class Test {

   public static void main(String[] args) {

       Zoo zoo = new Zoo();

       zoo.add(new Lion());
       zoo.add(new Tiger());
       zoo.add(new Elephant());

       Visitor feed = new Feed();
       Visitor observe = new Observe();
       Visitor train = new Train();

       zoo.accept(feed);
       zoo.accept(observe);
       zoo.accept(train);
   }
}
```
运行测试代码并显示输出结果：
```
Feed the lion with meat.
Feed the tiger with chicken.
Feed the elephant with banana.
Observe the lion: Roar!
Observe the tiger: Growl!
Observe the elephant: Trumpet!
Train the lion to jump through a hoop.
Train the tiger to stand on a ball.
Train the elephant to sit on a stool.
```
















#五、与final类对比
| 特性 | 密封类 | final类 |
| :---: | :---: | :---: |
| 可继承性 | 部分可继承，只能被指定的类继承| 不可继承 |
| 受保护成员或虚成员 | 不允许，因为密封类的子类必须是密封类或final类 | 允许，因为final类没有子类 |
| 抽象性 | 允许，因为密封类可以是抽象的或具体的| 不允许，因为抽象类必须被继承 |
| 相似之处 | 都是限制类的继承，都不能声明为抽象的，都可以继承别的类或接口 | 都是限制类的继承，都不能声明为抽象的，都可以继承别的类或接口 |
| 不同之处 | 可以指定哪些类可以作为其子类，可以实现多态性，可以用于一些特定的设计模式 | 不能有任何子类，不能实现多态性，不能用于一些特定的设计模式 |
| 优点 | 可以保证封装性和多态性，可以提高代码的可读性和可维护性，可以避免不必要的继承或实现 | 可以保证封装性和不变性，可以提高代码的执行效率，可以避免类的滥用 |
| 缺点 | 可能增加代码的复杂度和冗余，可能限制类的扩展性和灵活性，可能与一些框架或库不兼容 | 可能增加代码的耦合度和僵化度，可能限制类的扩展性和灵活性，可能与一些框架或库不兼容 |
| 应用场景 | 可以用于实现一些特定的设计模式，例如状态模式、策略模式、访问者模式等，这些模式需要明确地定义一组有限的子类或实现类 | 可以用于实现一些不需要继承的类，例如工具类、常量类、单例类等，这些类需要保证其不变性和唯一性 |
