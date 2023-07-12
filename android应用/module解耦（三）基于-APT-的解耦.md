# Android 通过 APT 解耦模块依赖

## 一、APT 是什么？

APT（Annotation Process Tool）是注解处理工具，它可以在编译期间扫描和处理注解，并生成相应的 Java 代码。APT 是 Java 的一个特性，但在 Android 开发中也有广泛的应用
APT 的优点是：

*   可以在编译期间检查代码的正确性，避免运行时出现错误
*   可以减少手写代码的数量，提高开发效率和可读性
*   可以实现模块间的解耦，降低耦合度和依赖关系

APT 的缺点是：

*   需要额外配置和使用注解处理器
*   生成的代码可能不易理解和维护
*   对于动态变化的数据不适用

## 二、APT 在 Android 中的应用场景

APT 在 Android 中有很多应用场景，例如：

*   ButterKnife：使用注解绑定视图和事件，简化 UI 开发
*   Dagger2：使用注解实现依赖注入，实现模块间的解耦
*   EventBus：使用注解注册和发送事件，简化组件间的通信
*   Retrofit：使用注解定义网络请求接口，简化网络开发
*   ARouter：使用注解定义路由表，实现模块间的跳转

本文将重点介绍 APT 在 Android 模块化开发中如何实现模块间的解耦。

## 三、APT 如何实现模块间的解耦

在 Android 模块化开发中，通常会遇到以下问题：

*   模块之间不能直接引用对方的类或资源，否则会造成循环依赖或冲突
*   模块之间需要通过接口或抽象类来定义通信协议，但这样会增加代码量和复杂度
*   模块之间需要通过反射或动态代理来调用对方的方法或服务，但这样会影响性能和安全性

为了解决这些问题，我们可以利用 APT 来生成作为跨模块转发层的中间类具体步骤如下：

1.  定义一个公共库（common）作为基础模块，提供一些基础功能和通用接口。
2.  定义一个注解库（annotation）作为公共库的子模块，在其中定义一些自定义注解。
3.  定义一个编译器库（compiler）作为公共库的子模块，在其中定义一个自定义注解处理器。
4.  在业务模块（moduleA、moduleB等）中引入公共库，并使用自定义注解标记需要跨模块调用或提供服务的类或方法。
5.  在编译期间，自定义注解处理器会扫描所有业务模块中标记了自定义注解的类或方法，并根据规则生成相应的中间类。
6.  在运行期间，业务模块可以通过中间类来调用其他业务模块提供的服务或方法。

下面我们以一个简单示例来说明这个过程。

### 3.1 公共库 common

公共库 common 提供了以下功能：

*   定义了一个 ServiceManager 类来管理所有跨模块提供或调

#Android 通过 APT 解耦模块依赖 
## 一、APT 是什么？ 
APT（Annotation Process Tool）是注解处理工具，它可以在编译期间扫描和处理注解，并生成相应的 Java 代码。APT 是 Java 的一个特性，但在 Android 开发中也有广泛的应用 。 APT 的优点是： - 可以在编译期间检查代码的正确性，避免运行时出现错误 - 可以减少手写代码的数量，提高开发效率和可读性 - 可以实现模块间的解耦，降低耦合度和依赖关系 APT 的缺点是： - 需要额外配置和使用注解处理器 - 生成的代码可能不易理解和维护 - 对于动态变化的数据不适用 
## 二、APT 在 Android 中的应用场景 
APT 在 Android 中有很多应用场景，例如： - ButterKnife：使用注解绑定视图和事件，简化 UI 开发 - Dagger2：使用注解实现依赖注入，实现模块间的解耦 - EventBus：使用注解注册和发送事件，简化组件间的通信 - Retrofit：使用注解定义网络请求接口，简化网络开发 - ARouter：使用注解定义路由表，实现模块间的跳转 本文将重点介绍 APT 在 Android 模块化开发中如何实现模块间的解耦。 
## 三、APT 如何实现模块间的解耦 
在 Android 模块化开发中，通常会遇到以下问题： - 模块之间不能直接引用对方的类或资源，否则会造成循环依赖或冲突 - 模块之间需要通过接口或抽象类来定义通信协议，但这样会增加代码量和复杂度 - 模块之间需要通过反射或动态代理来调用对方的方法或服务，但这样会影响性能和安全性 为了解决这些问题，我们可以利用 APT 来生成作为跨模块转发层的中间类。
具体步骤如下： 
1\. 定义一个公共库（common）作为基础模块，提供一些基础功能和通用接口。
 2\. 定义一个注解库（annotation）作为公共库的子模块，在其中定义一些自定义注解。 
3\. 定义一个编译器库（compiler）作为公共库的子模块，在其中定义一个自定义注解处理器。 
4\. 在业务模块（moduleA、moduleB等）中引入公共库，并使用自定义注解标记需要跨模块调用或提供服务的类或方法。 
5\. 在编译期间，自定义注解处理器会扫描所有业务模块中标记了自定义注解的类或方法，并根据规则生成相应的中间类。 
6\. 在运行期间，业务模块可以通过中间类来调用其他业务模块提供的服务或方法。 下面我们以一个简单示例来说明这个过程。 
### 3.1 公共库 common 公共库 
common 提供了以下功能： 
- 定义了一个 ServiceManager 类来管理所有跨模块提供或调


### 3.2 注解库 annotation

注解库 annotation 定义了以下注解：

*   @Service：用于标记需要跨模块提供服务的类，注解中可以指定服务的名称和描述
*   @Method：用于标记需要跨模块调用的方法，注解中可以指定方法的名称和参数类型
*   @Param：用于标记方法的参数，注解中可以指定参数的名称和类型

例如：

```
// 定义一个跨模块提供服务的类
@Service(name = "user", desc = "用户服务")
public class UserService {

    // 定义一个跨模块调用的方法
    @Method(name = "login", params = {@Param(name = "username", type = String.class), @Param(name = "password", type = String.class)})
    public boolean login(String username, String password) {
        // 省略登录逻辑
        return true;
    }
}

```

### 3.3 编译器库 compiler

编译器库 compiler 定义了一个自定义注解处理器 ServiceProcessor，它继承了 AbstractProcessor 类，并重写了以下方法：

*   init：初始化注解处理器，获取一些工具类对象，如 Elements、Types、Filer 等
*   getSupportedAnnotationTypes：返回支持处理的注解类型集合，如 Service、Method、Param 等
*   process：处理标记了支持的注解类型的元素（Element），并根据规则生成相应的中间类

例如：

```
// 自定义注解处理器
public class ServiceProcessor extends AbstractProcessor {

    // 初始化工具类对象
    private Elements elementUtils;
    private Types typeUtils;
    private Filer filer;

    @Override
    public synchronized void init(ProcessingEnvironment processingEnv) {
        super.init(processingEnv);
        elementUtils = processingEnv.getElementUtils();
        typeUtils = processingEnv.getTypeUtils();
        filer = processingEnv.getFiler();
    }

    // 返回支持处理的注解类型集合
    @Override
    public Set<String> getSupportedAnnotationTypes() {
        Set<String> types = new LinkedHashSet<>();
        types.add(Service.class.getCanonicalName());
        types.add(Method.class.getCanonicalName());
        types.add(Param.class.getCanonicalName());
        return types;
    }

```


 ### 3.2 注解库 
annotation 注解库 annotation 定义了以下注解： 
- @Service：用于标记需要跨模块提供服务的类，注解中可以指定服务的名称和描述
 - @Method：用于标记需要跨模块调用的方法，注解中可以指定方法的名称和参数类型 
- @Param：用于标记方法的参数，注解中可以指定参数的名称和类型 
例如：
 ```java 
// 定义一个跨模块提供服务的类 @Service(name = "user", desc = "用户服务") public class UserService { // 定义一个跨模块调用的方法 @Method(name = "login", params = {@Param(name = "username", type = String.class), @Param(name = "password", type = String.class)}) public boolean login(String username, String password) { // 省略登录逻辑 return true; } } 
``` 
### 3.3 编译器库 compiler 编译器库 compiler 定义了一个自定义注解处理器 ServiceProcessor，它继承了 AbstractProcessor 类，并重写了以下方法：
 - init：初始化注解处理器，获取一些工具类对象，如 Elements、Types、Filer 等
 - getSupportedAnnotationTypes：返回支持处理的注解类型集合，如 Service、Method、Param 等
 - process：处理标记了支持的注解类型的元素（Element），并根据规则生成相应的中间类
 例如： 
```java
 // 自定义注解处理器 
public class ServiceProcessor extends AbstractProcessor { // 初始化工具类对象 private Elements elementUtils; private Types typeUtils; private Filer filer; @Override public synchronized void init(ProcessingEnvironment processingEnv) { super.init(processingEnv); elementUtils = processingEnv.getElementUtils(); typeUtils = processingEnv.getTypeUtils(); filer = processingEnv.getFiler(); } // 返回支持处理的注解类型集合 @Override public Set<String> getSupportedAnnotationTypes() { Set<String> types = new LinkedHashSet<>(); types.add(Service.class.getCanonicalName()); types.add(Method.class.getCanonicalName()); types.add(Param.class.getCanonicalName()); return types; }
```
### 3.4 运行时库 runtime

运行时库 runtime 定义了一个 ServiceManager 类，它负责管理所有跨模块提供的服务类和方法，以及加载生成的中间类。它提供了以下方法：

*   init：初始化 ServiceManager，扫描所有已注册的服务类，并加载对应的中间类
*   register：注册一个服务类，需要传入服务类的全限定名
*   invoke：调用一个服务方法，需要传入服务名称、方法名称和参数列表

例如：

```
// 初始化 ServiceManager
ServiceManager.init();

// 注册一个服务类
ServiceManager.register("com.example.UserService");

// 调用一个服务方法
boolean result = (boolean) ServiceManager.invoke("user", "login", "admin", "123456");

```

### 3.5 使用示例

假设我们有两个模块：app 和 user。app 模块依赖于 user 模块，user 模块提供了一个 UserService 类，用于实现用户相关的功能。我们想要在 app 模块中调用 UserService 的 login 方法，但是不想直接引用 user 模块。

我们可以使用 APT 来解耦这两个模块的依赖关系，具体步骤如下：

1.  在 user 模块中定义 UserService 类，并使用 @Service 和 @Method 注解标记该类和 login 方法。
2.  在 app 模块中添加 annotation、compiler 和 runtime 库的依赖，并在 build.gradle 中配置 annotationProcessor。
3.  在 app 模块中注册 UserService 类，并使用 ServiceManager 调用 login 方法。

具体代码如下：

```
// user 模块中定义 UserService 类
@Service(name = "user", desc = "用户服务")
public class UserService {

    @Method(name = "login", params = {@Param(name = "username", type = String.class), @Param(name = "password", type = String.class)})
    public boolean login(String username, String password) {
        // 省略登录逻辑
        return true;
    }
}

// app 模块中添加依赖和配置
dependencies {
    implementation project(':user')
    implementation 'com.kymjs:annotation:1.0'
    annotationProcessor 'com.kymjs:compiler:1.0'
    implementation 'com.kymjs:runtime:1.0'
}

// app 模块中注册并调用 UserService 类
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 初始化 ServiceManager
        ServiceManager.init();

        // 注册 UserService 类
        ServiceManager.register("com.example.UserService");

        // 调用 login 方法
        boolean result = (boolean) ServiceManager.invoke("user", "login", "admin", "123456");

        // 显示结果
        Toast.makeText(this, result ? "登录成功" : "登录失败", Toast.LENGTH_SHORT).show();
    }
}

```

这样就实现了通过 APT 解耦模块依赖的功能。

### 3.4 运行时库 runtime 
运行时库 runtime 定义了一个 ServiceManager 类，它负责管理所有跨模块提供的服务类和方法，以及加载生成的中间类。它提供了以下方法： - init：初始化 ServiceManager，扫描所有已注册的服务类，并加载对应的中间类 - register：注册一个服务类，需要传入服务类的全限定名 - invoke：调用一个服务方法，需要传入服务名称、方法名称和参数列表 
例如： 
```java 
// 初始化 ServiceManager ServiceManager.init(); // 注册一个服务类 ServiceManager.register("com.example.UserService"); // 调用一个服务方法 boolean result = (boolean) ServiceManager.invoke("user", "login", "admin", "123456"); 
```
 ### 3.5 使用示例 
假设我们有两个模块：app 和 user。app 模块依赖于 user 模块，user 模块提供了一个 UserService 类，用于实现用户相关的功能。我们想要在 app 模块中调用 UserService 的 login 方法，但是不想直接引用 user 模块。 我们可以使用 APT 来解耦这两个模块的依赖关系，具体步骤如下： 
1\. 在 user 模块中定义 UserService 类，并使用 @Service 和 @Method 注解标记该类和 login 方法。 
2\. 在 app 模块中添加 annotation、compiler 和 runtime 库的依赖，并在 build.gradle 中配置 annotationProcessor。 
3\. 在 app 模块中注册 UserService 类，并使用 ServiceManager 调用 login 方法。 
具体代码如下： 
```java
 // user 模块中定义 UserService 类 @Service(name = "user", desc = "用户服务") public class UserService { @Method(name = "login", params = {@Param(name = "username", type = String.class), @Param(name = "password", type = String.class)}) public boolean login(String username, String password) { // 省略登录逻辑 return true; } } // app 模块中添加依赖和配置 dependencies { implementation project(':user') implementation 'com.kymjs:annotation:1.0' annotationProcessor 'com.kymjs:compiler:1.0' implementation 'com.kymjs:runtime:1.0' } // app 模块中注册并调用 UserService 类 public class MainActivity extends AppCompatActivity { @Override protected void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); setContentView(R.layout.activity_main); // 初始化 ServiceManager ServiceManager.init(); // 注册 UserService 类 ServiceManager.register("com.example.UserService"); // 调用 login 方法 boolean result = (boolean) ServiceManager.invoke("user", "login", "admin", "123456"); // 显示结果 Toast.makeText(this, result ? "登录成功" : "登录失败", Toast.LENGTH_SHORT).show(); } }
 ``` 
这样就实现了通过 APT 解耦模块依赖的功能。
