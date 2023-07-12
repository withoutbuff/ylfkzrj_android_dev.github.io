# Mavericks + RxJava2

MvRx 1.x被设计成能与RxJava 2很好地兼容。 然而，Mavericks现在在内部使用coroutines。

RxJava 2的兼容性仍然可以通过`mavericks-rxjava2`工件来实现。

要使用它，请使用`BaseMvRxViewModel`而不是`MavericksViewModel`。这样做将使你能够访问`Single<T>`和`Observable<T>`上的`execute`扩展以及所有其他MvRx 1.x API。
# Mavericks + RxJava2

MvRx 1.x was designed to work well with RxJava 2. However, Mavericks now uses coroutines internally.

RxJava 2 compatibility is still available via the `mavericks-rxjava2` artifact.

To use it, use `BaseMvRxViewModel` instead of `MavericksViewModel`. Doing so will give you access to `execute` extensions on `Single<T>` and `Observable<T>` as well as all other MvRx 1.x APIs.
