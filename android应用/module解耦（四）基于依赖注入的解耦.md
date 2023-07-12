# 安卓开发中基于依赖注入（DI）的模块解耦

## 1\. 什么是依赖注入（DI）

依赖注入（Dependency Injection，简称 DI）是一种设计模式，用于实现控制反转（Inversion of Control，简称 IoC）。控制反转是指将对象之间的依赖关系由程序代码中定义转移到外部容器中管理，从而降低对象之间的耦合度，提高代码的可维护性和可测试性

依赖注入的核心思想是：一个类不需要自己创建或获取它所依赖的其他类的实例，而是通过构造函数、属性或者工厂模式等方法，由外部容器（如 DI 框架）将它所依赖的其他类的实例注入到它内部

例如，假设有一个名为 Car 的类，它需要一个名为 Engine 的类作为它的属性。如果不使用依赖注入，Car 类可能会在自己的构造函数中创建或获取一个 Engine 类的实例：

```
public class Car {
    private Engine engine;

    public Car() {
        // 不使用依赖注入
        this.engine = new Engine(); // 或者从其他地方获取
    }
}

```

这样做会导致 Car 类和 Engine 类之间产生强耦合关系，如果 Engine 类发生变化或者需要替换成其他类型的引擎，Car 类也需要相应地修改代码。而且这样做也不利于对 Car 类进行单元测试，因为无法轻易地将 Engine 类替换成测试替身

如果使用依赖注入，则可以将 Engine 类作为 Car 类构造函数的参数传入：

```
public class Car {
    private Engine engine;

    public Car(Engine engine) {
        // 使用依赖注入
        this.engine = engine; // 由外部容器提供
    }
}

```

这样做就可以解除 Car 类和 Engine 类之间的强耦合关系，Car 类不需要关心 Engine 类如何创建或获取，只需要使用它提供的功能即可。同时也可以方便地对 Car 类进行单元测试，只需要传入一个名为 FakeEngine 的 Engine 测试替身，并针对不同的测试进行配置

## 2\. 如何在安卓开发中使用 DI

在安卓开发中，有许多场景可以使用 DI 来实现模块解耦。例如，在企业级 APP 中， 路由是一种常见的功能， 用于将 Intent 页面跳转的强依赖关系解耦， 同时减少跨团队开发时互相依赖问题。 又例如，在大型 APP 中， 通常会采用模块化或组件化方式开发， 对于模块间解耦要求更高。 还有例如，在 MVP 或 MVVM 等架构模式中， 可以使用 DI 来管理 View 和 Presenter 或 ViewModel 之间以及各种服务和数据源之间的依赖关系

要在安卓开发中使用 DI ，我们可以借助一些开源库来简化操作和提高效率。 目前比较流行和成熟的 DI 开源库有 Dagger2 和 Dagger Hilt 等

 # 安卓开发中基于依赖注入（DI）的模块解
 ## 1\. 什么是依赖注入（DI） 
依赖注入（Dependency Injection，简称 DI）是一种设计模式，用于实现控制反转（Inversion of Control，简称 IoC）。控制反转是指将对象之间的依赖关系由程序代码中定义转移到外部容器中管理，从而降低对象之间的耦合度，提高代码的可维护性和可测试性 。 
依赖注入的核心思想是：一个类不需要自己创建或获取它所依赖的其他类的实例，而是通过构造函数、属性或者工厂模式等方法，由外部容器（如 DI 框架）将它所依赖的其他类的实例注入到它内部。 
例如，假设有一个名为 Car 的类，它需要一个名为 Engine 的类作为它的属性。如果不使用依赖注入，Car 类可能会在自己的构造函数中创建或获取一个 Engine 类的实例： 
```java
 public class Car { 
private Engine engine; public Car() { 
// 不使用依赖注入 this.engine = new Engine(); 
// 或者从其他地方获取 
} 
}
 ``` 
这样做会导致 Car 类和 Engine 类之间产生强耦合关系，如果 Engine 类发生变化或者需要替换成其他类型的引擎，Car 类也需要相应地修改代码。而且这样做也不利于对 Car 类进行单元测试，因为无法轻易地将 Engine 类替换成测试替身。
 如果使用依赖注入，则可以将 Engine 类作为 Car 类构造函数的参数传入：
 ```java 
public class Car {
 private Engine engine;
 public Car(Engine engine) { // 使用依赖注入
 this.engine = engine; // 由外部容器提供
 } 
} 
``` 
这样做就可以解除 Car 类和 Engine 类之间的强耦合关系，Car 类不需要关心 Engine 类如何创建或获取，只需要使用它提供的功能即可。同时也可以方便地对 Car 类进行单元测试，只需要传入一个名为 FakeEngine 的 Engine 测试替身，并针对不同的测试进行配置。 
## 2\. 如何在安卓开发中使用 DI 
在安卓开发中，有许多场景可以使用 DI 来实现模块解耦。例如，在企业级 APP 中， 路由是一种常见的功能， 用于将 Intent 页面跳转的强依赖关系解耦， 同时减少跨团队开发时互相依赖问题。 又例如，在大型 APP 中， 通常会采用模块化或组件化方式开发， 对于模块间解耦要求更高。 还有例如，在 MVP 或 MVVM 等架构模式中， 可以使用 DI 来管理 View 和 Presenter 或 ViewModel 之间以及各种服务和数据源之间的依赖关系 。 要在安卓开发中使用 DI ，我们可以借助一些开源库来简化操作和提高效率。 目前比较流行和成熟的 DI 开源库有 Dagger2 和 Dagger Hilt 等

### 2.1 Dagger2

Dagger2 是一个由 Google 和 Square 共同开发的 DI 开源库，它基于 Java 注解和编译时代码生成技术，可以在编译期间检查依赖关系的完整性和正确性，从而提高运行时的性能和稳定性

要使用 Dagger2 ，我们需要遵循以下几个步骤：

①   定义依赖提供者（Provider），即用 @Provides 注解标注一个方法，表示该方法可以提供某个类型的依赖实例。例如：

```
@Module
public class EngineModule {
    @Provides
    public Engine provideEngine() {
        return new Engine();
    }
}

```

②   定义依赖需求者（Consumer），即用 @Inject 注解标注一个构造函数、属性或方法，表示该构造函数、属性或方法需要注入某个类型的依赖实例。例如：

```
public class Car {
    @Inject
    Engine engine;

    public Car() {
        // 省略其他代码
    }
}

```

③   定义组件（Component），即用 @Component 注解标注一个接口，表示该接口可以连接依赖提供者和依赖需求者，并定义一些抽象方法来获取或注入依赖实例。例如：

```
@Component(modules = {EngineModule.class})
public interface CarComponent {
    // 获取依赖实例
    Engine getEngine();

    // 注入依赖实例
    void inject(Car car);
}

```

④  生成并使用组件实例，即通过 Dagger 开头的类来获取组件实例，并调用其抽象方法来获取或注入依赖实例。例如：

```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 生成组件实例
        CarComponent carComponent = DaggerCarComponent.create();

        // 获取依赖实例
        Engine engine = carComponent.getEngine();

        // 注入依赖实例
        Car car = new Car();
        carComponent.inject(car);

        // 省略其他代码

    }
}

```

Dagger2 的优点有：

*   编译时检查，避免运行时错误。
*   无反射，提高性能。
*   支持作用域管理，避免内存泄漏。
*   支持多模块开发

Dagger2 的缺点有：

*   学习曲线陡峭，注解繁多。
*   需要手动创建和管理组件。
*   需要适配 Android 生命周期

Dagger2 的使用场景有：

*   需要高性能和稳定性的项目。
*   需要灵活控制依赖关系的项目。
*   需要支持多模块开发的项目

### 2.2 Dagger Hilt

Dagger Hilt 是一个由 Google 开发的 DI 开源库， 它是对 Dagger2 的二次封装， 专门针对 Android 平台制定了一系列规则， 大大简化了 Dagger2 的使用

要使用 Dagger Hilt ，我们需要遵循以下几个步骤：

① 定义依赖提供者（Provider），即用 @Provides 注解标注一个方法，表示该方法可以提供某个类型的依赖实例。与 Dagger2 不同的是，我们不需要自己创建模块类，而是用 @InstallIn 注解标注该方法所属的组件范围。例如：

```
@InstallIn(SingletonComponent.class)
@Module
public class EngineModule {
    @Provides
    public Engine provideEngine() {
        return new Engine();
    }
}

```

②   定义依赖需求者（Consumer），即用 @Inject 注解标注一个构造函数、属性或方法，表示该构造函数、属性或方法需要注入某个类型的依赖实例。与 Dagger2 不同的是，我们不需要自己创建和使用组件接口，而是用 @AndroidEntryPoint 注解标注 Android 系统组件（如 Activity、Fragment 等），表示该组件可以使用 Hilt 的依赖注入功能。例如：

```
@AndroidEntryPoint
public class MainActivity extends AppCompatActivity {

    @Inject
    Engine engine;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 省略其他代码

    }
}

```

③   生成并使用组件实例，这一步由 Hilt 自动完成，我们不需要手动调用任何方法来获取或注入依赖实例。

Dagger Hilt 的优点有：

*   简单易用，减少了大量的模板代码。
*   提供了 Android 专属的 API ，适配了 Android 生命周期。
*   支持作用域管理，避免内存泄漏。
*   兼容 Dagger2 ，可以无缝迁移 。

Dagger Hilt 的缺点有：

*   仍然基于 Dagger2 ，可能存在一些潜在的问题和限制。
*   相对于 Dagger2 ，灵活性较低，不能自定义组件和模块 。

Dagger Hilt 的使用场景有：

*   需要快速上手和简化开发流程的项目。
*   需要适配 Android 生命周期和系统组件的项目。
*   需要兼容 Dagger2 的项目 。
