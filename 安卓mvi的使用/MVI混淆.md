# Proguard/Dexguard/R8

Mavericks库本身有一个配置Proguard的消费者Proguard文件。由于主库使用Kotlin，而可选的附加模块使用Kotlin反射和RxJava，你可能需要在应用程序本身的Proguard规则中添加处理这些规则。(如果你没有为其他目的添加它们的话）。这些并不是消费者Proguard文件的一部分，因为它们并不是特定于lib的Proguard配置的一部分。这就是我们的示例应用程序的配置部分的样子：

```
# 这些类是通过kotlin反射使用的，一旦Proguard支持Kotlin反射，就不再需要保留这些类了。
# Kotlin反射直接使用。
-keep class kotlin.reflect.jvm.internal.impl.builtInsLoaderImpl
-keep class kotlin.reflect.jvm.internal.impl.serialization.deserialization.buildins.BuiltInsLoaderImpl
-keep class kotlin.reflect.jvm.internal.impl.load.java.FieldOverridabilityCondition
-keep class kotlin.reflect.jvm.internal.impl.load.java.ErasedOverridabilityCondition.
-keep class kotlin.reflect.jvm.internal.impl.load.java.JavaIncompatibilityRulesOverridabilityCondition

# 如果同伴对象是通过Kotlin反射实例化的，并且它们扩展/实现了Proguard
# 会被删除或内联，我们就会遇到麻烦，因为继承性仍在Metadata注解中。
# 由Kotlin反射读取。
# FIXME 如果Pro/Dexguard支持Kotlin反射，则删除该类。
-if class **$Companion extends **
-keep class <2>
-if class **$Companion implements **
-keep class <2>

# https://medium.com/@AthorNZ/kotlin-metadata-jacksonand-proguard-f64f51e5ed32
-keep class kotlin.Metadata { *; }

# https://stackoverflow.com/questions/33547643/how-to-use-kotlin-with-proguard
-dontwarn kotlin.**

# RxJava
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
long producerIndex；
long consumerIndex；
}
```

#### Kotlin反射与Proguard/Dexguard/R8

为lib提供的配置假定你的收缩器能够正确处理Kotlin `@Metadata`注解（例如Dexguard 8.6++），如果它不能（例如Proguard和当前版本的R8），你需要在你应用程序的proguard配置中添加这些规则：
```
# MavericksViewModel通过反射加载Companion类，因此我们需要确保保持
# Companion对象的名字。
-keepclassmembers class ** extends com.airbnb.mvrx.MavericksViewModel {
** Companion；
}

# 在Mavericks中作为状态使用的Kotlin数据类的成员是通过Kotlin反射来读取的，这就会造成麻烦
# 如果不保留它们，就会给Proguard带来麻烦。
# 在反射缓存升温过程中，类型也是通过反射访问的。也需要保留它们。
-keepclassmembers,includedescriptorclasses,allowobfuscation class ** implements com.airbnb.mvrx.MavericksState {
*;
}

# MavericksState对象和实现MavericksState接口的名称类需要被
# 保留，因为它们是通过反射访问的。
-keepnames class com.airbnb.mvrx.MavericksState
-keepnames class * implements com.airbnb.mvrx.MavericksState

# MavericksViewModelFactory通过反射使用同伴的类名进行引用。
-keepnames class * implements com.airbnb.mvrx.MavericksViewModelFactory
```

[Translated with DeepL](https://www.deepl.com/translator?utm_source=windows&utm_medium=app&utm_campaign=windows-share)

# Proguard/Dexguard/R8

The Mavericks lib itself has a consumer Proguard file that configures Proguard. As the main lib uses Kotlin, and optional add-on modules use Kotlin reflection and RxJava, you might have to add rules to handle those to the Proguard rules of your app itself. (If you did not add them for other purposes already). These are not part of the consumer Proguard file, as they are not part of the lib specific Proguard configuration. This is what that part of the configuration looks like for our sample app:

```
# These classes are used via kotlin reflection and the keep might not be required anymore once Proguard supports
# Kotlin reflection directly.
-keep class kotlin.reflect.jvm.internal.impl.builtins.BuiltInsLoaderImpl
-keep class kotlin.reflect.jvm.internal.impl.serialization.deserialization.builtins.BuiltInsLoaderImpl
-keep class kotlin.reflect.jvm.internal.impl.load.java.FieldOverridabilityCondition
-keep class kotlin.reflect.jvm.internal.impl.load.java.ErasedOverridabilityCondition
-keep class kotlin.reflect.jvm.internal.impl.load.java.JavaIncompatibilityRulesOverridabilityCondition

# If Companion objects are instantiated via Kotlin reflection and they extend/implement a class that Proguard
# would have removed or inlined we run into trouble as the inheritance is still in the Metadata annotation
# read by Kotlin reflection.
# FIXME Remove if Kotlin reflection is supported by Pro/Dexguard
-if class **$Companion extends **
-keep class <2>
-if class **$Companion implements **
-keep class <2>

# https://medium.com/@AthorNZ/kotlin-metadata-jackson-and-proguard-f64f51e5ed32
-keep class kotlin.Metadata { *; }

# https://stackoverflow.com/questions/33547643/how-to-use-kotlin-with-proguard
-dontwarn kotlin.**

# RxJava
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
   long producerIndex;
   long consumerIndex;
}
```

#### Kotlin reflection with Proguard/Dexguard/R8

The configuration provided for the lib assumes that your shrinker can correctly handle Kotlin `@Metadata` annotations (e.g. Dexguard 8.6++) if it cannot (e.g. Proguard and current versions of R8) you need to add these rules to the proguard configuration of your app:
```
# MavericksViewModel loads the Companion class via reflection and thus we need to make sure we keep
# the name of the Companion object.
-keepclassmembers class ** extends com.airbnb.mvrx.MavericksViewModel {
    ** Companion;
}

# Members of the Kotlin data classes used as the state in Mavericks are read via Kotlin reflection which cause trouble
# with Proguard if they are not kept.
# During reflection cache warming also the types are accessed via reflection. Need to keep them too.
-keepclassmembers,includedescriptorclasses,allowobfuscation class ** implements com.airbnb.mvrx.MavericksState {
   *;
}

# The MavericksState object and the names classes that implement the MavericksState interface need to be
# kept as they are accessed via reflection.
-keepnames class com.airbnb.mvrx.MavericksState
-keepnames class * implements com.airbnb.mvrx.MavericksState

# MavericksViewModelFactory is referenced via reflection using the Companion class name.
-keepnames class * implements com.airbnb.mvrx.MavericksViewModelFactory
```
