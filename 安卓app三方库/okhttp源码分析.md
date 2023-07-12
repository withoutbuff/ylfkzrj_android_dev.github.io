# OkHttp源码分析

OkHttp是一个流行的开源网络库，它可以在Java和Kotlin中使用，提供了高效、灵活、易用的HTTP客户端功能。本文将从以下几个方面来分析OkHttp的源码：

- 基本架构
- 重要组件
- 缓存策略和实现方式
- 链接策略和实现方式
- 拦截器的设计思路和实现方式

## 1. 基本架构

OkHttp的基本架构可以用下图来表示：

![框架图](https://upload-images.jianshu.io/upload_images/24860325-87e5d8ff099bd4b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


从图中可以看出，OkHttp主要由以下几个部分组成：

- OkHttpClient：这是OkHttp的核心管理类，它负责创建和配置HTTP请求，以及管理连接池、缓存、拦截器等组件。
- Request：这是HTTP请求的封装类，它包含了URL、Header、Method、Body等常见的参数。
- Response：这是HTTP响应的封装类，它包含了Code、Message、Header、Body等常见的参数。
- RealCall：这是请求的调度类，它负责根据同步或异步的方式来执行请求，并构造内部逻辑责任链。
- Interceptor：这是拦截器的接口，它定义了一个方法proceed，用于在责任链中传递请求和响应。OkHttp内部提供了多种拦截器，用于实现重试、重定向、缓存、连接等功能。
- RealInterceptorChain：这是拦截器链的实现类，它维护了一个拦截器列表和一个索引，用于按顺序调用拦截器的proceed方法，并处理异常情况。
- ConnectionPool：这是连接池的实现类，它负责管理复用和回收HTTP连接，以提高性能和节省资源。
- Cache：这是缓存的实现类，它负责存储和读取HTTP响应，以减少网络请求和流量消耗。

## 2. 重要组件

### 2.1 OkHttpClient

OkHttpClient是OkHttp的核心管理类，它通过Builder模式来创建和配置。它有很多属性和方法，这里只介绍一些比较重要的：

- interceptors：这是一个拦截器列表，用于添加应用层面的拦截器，比如日志、压缩、加密等。
- networkInterceptors：这是一个拦截器列表，用于添加网络层面的拦截器，比如重试、重定向、缓存等。
- connectTimeoutMillis：这是连接超时时间，单位是毫秒。
- readTimeoutMillis：这是读取超时时间，单位是毫秒。
- writeTimeoutMillis：这是写入超时时间，单位是毫秒。
- connectionPool：这是连接池对象，用于管理复用和回收HTTP连接。
- cache：这是缓存对象，用于存储和读取HTTP响应。
- cookieJar：这是Cookie管理对象，用于存储和发送Cookie。
- newCall(Request request)：这是创建一个新的HTTP请求的方法，它返回一个Call对象，用于执行同步或异步请求。

### 2.2 Request

Request是HTTP请求的封装类，它包含了URL、Header、Method、Body等常见的参数。它通过Builder模式来创建和配置。它有以下几个属性和方法：

- url：这是请求的URL对象，包含了协议、域名、端口、路径、查询参数等信息。
- method：这是请求的方法，比如GET、POST、PUT等。
- headers：这是请求的头部信息，包含了一些键值对，比如User-Agent、Content-Type等。
- body：这是请求的正文信息，包含了一些字节数据，比如表单、文件、JSON等。
- tag：这是请求的标签对象，用于给请求添加一些额外的信息，比如取消标识、优先级等。

### 2.3 Response

Response是HTTP响应的封装类，它包含了Code、Message、Header、Body等常见的参数。它有以下几个属性和方法：

- request：这是响应对应的请求对象。
- protocol：这是响应使用的协议，比如HTTP/1.1、HTTP/2等。
- code：这是响应的状态码，比如200、404、500等。
- message：这是响应的状态消息，比如OK、Not Found、Internal Server Error等。
- headers：这是响应的头部信息，包含了一些键值对，比如Content-Length、Content-Type等。
- body：这是响应的正文信息，包含了一些字节数据，比如HTML、图片、JSON等。
- cacheResponse：这是缓存中获取到的响应对象，如果没有缓存，则为null。
- networkResponse：这是网络中获取到的响应对象，如果没有网络，则为null。
- priorResponse：这是重定向或重试前获取到的响应对象，如果没有重定向或重试，则为null。

## 3. 缓存策略和实现方式

OkHttp提供了一个缓存功能，可以将HTTP响应存储在本地，并在需要时读取出来，以减少网络请求和流量消耗。OkHttp的缓存策略遵循了RFC 7234规范，并且可以通过拦截器和头部信息来自定义。

OkHttp的缓存策略主要由以下几个因素决定：

- Cache-Control：这是一个头部信息，用于指定缓存的指令，比如max-age、no-cache、no-store等。它可以在请求和响应中设置，用于控制缓存的有效期和行为。
- ETag：这是一个头部信息，用于标识资源的版本，比如"5d8c72a5edda"。它可以在响应中设置，用于验证缓存是否过期。
- Last-Modified：这是一个头部信息，用于标识资源的最后修改时间，比如"Wed, 21 Oct 2015 07:28:00 GMT"。它可以在响应中设置，用于验证缓存是否过期。
- If-None-Match：这是一个头部信息，用于发送ETag值，比如"5d8c72a5edda"。它可以在请求中设置，用于向服务器询问缓存是否有效。
- If-Modified-Since：这是一个头部信息，用于发送Last-Modified值，比如"Wed, 21 Oct 2015 07:28:00 GMT"。它可以在请求中设置，用于向服务器询问缓存是否有效。

根据以上因素，OkHttp的缓存策略大致可以分为以下几种情况：

- 如果请求中没有Cache-Control或者只有max-age指令，并且响应中有Cache-Control或者ETag或者Last-Modified，那么OkHttp会将响应缓存起来，并根据Cache-Control或者ETag或者Last-Modified来判断缓存的有效期。
- 如果请求中有Cache-Control并且有no-cache或者no-store指令，那么OkHttp不会使用缓存，而是直接向服务器发送请求，并将响应缓存起来。
- 如果请求中有Cache-Control并且有only-if-cached指令，那么OkHttp只会使用缓存，而不会向服务器发送请求，如果缓存不存在或者过期，那么返回504错误。
- 如果请求中有Cache-Control并且有max-stale指令，那么OkHttp会使用过期的缓存，但是不会超过max-stale指定的时间，如果没有合适的缓存，那么向服务器发送请求，并将响应缓存起来。
- 如果请求中有Cache-Control并且有min-fresh指令，那么OkHttp会使用未过期的缓存，并且保证缓存的新鲜度不低于min-fresh指定的时间，如果没有合适的缓存，那么向服务器发送请求，并将响应缓存起来。
- 如果请求中有If-None-Match或者If-Modified-Since，那么OkHttp会向服务器发送验证请求，并附上ETag或者Last-Modified值，如果服务器返回304状态码，表示缓存有效，那么OkHttp会使用缓存，并更新缓存的头部信息；如果服务器返回其他状态码，表示缓存无效，那么OkHttp会使用新的响应，并将其缓存起来。

OkHttp的缓存实现方式主要由以下几个类完成：

- Cache：这是缓存的实现类，它负责存储和读取HTTP响应。它内部使用了DiskLruCache来实现磁盘缓存，并提供了一些方法来操作缓存，比如put、get、remove、update、evict等。
- CacheInterceptor：这是一个拦截器类，它负责实现缓存策略和逻辑。它在责任链中的位置是在ConnectInterceptor之前，在CallServerInterceptor之后。它根据请求和响应的头部信息来判断是否使用缓存或者向服务器发送验证请求，并调用Cache类的方法来操作缓存。
- CacheStrategy：这是一个辅助类，它负责根据请求和响应的头部信息来生成一个最优的缓存策略对象。它包含了两个属性：networkRequest和cacheResponse，分别表示是否需要向服务器发送请求和是否需要从缓存中读取响应。

## 4. 链接策略和实现方式

OkHttp提供了一个链接策略，可以根据URL、代理、协议等因素来选择最优的HTTP连接，并实现连接的复用和回收。OkHttp的链接策略主要由以下几个因素决定：

- URL：这是请求的URL对象，包含了协议、域名、端口、路径、查询参数等信息。
- Proxy：这是代理对象，用于指定是否通过代理服务器来发送请求。
- Protocol：这是协议对象，用于指定使用哪种协议来发送请求，比如HTTP/1.1、HTTP/2等。
- ConnectionSpec：这是连接规范对象，用于指定连接的安全性和兼容性，比如是否支持TLS、SSL等。
- Route：这是路由对象，用于表示一个连接的目标地址和代理地址。它由URL、Proxy和InetSocketAddress组成。
- RouteSelector：这是路由选择器对象，用于根据URL、Proxy和ConnectionSpec来生成一个路由列表，并按顺序尝试每个路由，直到找到一个可用的连接或者失败。
- Connection：这是连接对象，用于表示一个实际的HTTP连接。它包含了Socket、Stream、Protocol等信息，并提供了一些方法来操作连接，比如connect、write、read、close等。
- ConnectionPool：这是连接池对象，用于管理复用和回收HTTP连接。它内部使用了一个双向链表来存储连接，并提供了一些方法来操作连接池，比如get、put、evictAll等。

OkHttp的链接策略主要由以下几个类完成：

- ConnectInterceptor：这是一个拦截器类，它负责实现链接策略和逻辑。它在责任链中的位置是在CacheInterceptor之后，在CallServerInterceptor之前。它根据请求的URL、Proxy和ConnectionSpec来创建一个RouteSelector对象，并调用其next方法来获取一个可用的连接，并将其传递给下一个拦截器。
- StreamAllocation：这是一个辅助类，它负责管理一个请求所使用的连接和流。它包含了一个RouteSelector对象和一个ConnectionPool对象，并提供了一些方法来操作连接和流，比如newStream、release、noNewStreams等。
- RealConnection：这是一个继承自Connection的类，它负责实现连接的具体逻辑。它包含了一个Route对象和一个Socket对象，并提供了一些方法来操作连接，比如connectSocket、connectTls、connectTunnel等。

## 5. 拦截器的设计思路和实现方式

OkHttp使用了拦截器模式来组织内部的逻辑，将复杂的请求过程切分成多个独立的模块，并通过责任链模式将它们串联起来。这样做的好处有以下几点：

- 降低耦合度，每个拦截器只关注自己的功能，不需要知道其他拦截器的存在和实现。
- 提高可扩展性，可以方便地添加或删除拦截器，或者改变拦截器的顺序和逻辑。
- 提高可复用性，可以在不同的场景下重用同一个拦截器，或者将拦截器封装成库供其他项目使用。

OkHttp的拦截器设计思路主要有以下几点：

- 定义一个拦截器接口Interceptor，它只有一个方法proceed，用于接收一个请求并返回一个响应。
- 定义一个拦截器链接口Chain，它有两个方法request和proceed，分别用于获取当前请求和调用下一个拦截器。
- 定义一个拦截器链实现类RealInterceptorChain，它维护了一个拦截器列表和一个索引，并实现了Chain接口的方法。在proceed方法中，它会根据索引从列表中取出一个拦截器，并调用其proceed方法，并将自身作为参数传递进去。这样就形成了一个递归调用的过程，直到所有拦截器都被执行完毕。
- 定义多个具体的拦截器类，比如RetryAndFollowUpInterceptor、BridgeInterceptor、CacheInterceptor、ConnectInterceptor、CallServerInterceptor等，并实现Interceptor接口的方法。在proceed方法中，它们会根据自己的功能和逻辑来处理请求或响应，并调用Chain参数的proceed方法来传递给下一个拦截器。

下面我们来看一下OkHttp内部提供了哪些拦截器，以及它们的功能和作用。

### 5.1 RetryAndFollowUpInterceptor

这是一个应用层面的拦截器，它负责处理请求的重试和重定向。它在责任链中的位置是最前面的一个。它有以下几个功能：

- 根据请求的URL、代理、协议等因素来选择最优的连接，并创建一个StreamAllocation对象来管理连接和流。
- 根据请求和响应的头部信息来判断是否需要重试或重定向，并更新请求的URL、代理、协议等信息。
- 根据请求和响应的头部信息来判断是否需要取消请求或关闭连接，并释放StreamAllocation对象。

### 5.2 BridgeInterceptor

这是一个应用层面的拦截器，它负责处理请求和响应的头部信息。它在责任链中的位置是在RetryAndFollowUpInterceptor之后，在CacheInterceptor之前。它有以下几个功能：

- 在请求中添加一些默认的头部信息，比如User-Agent、Content-Type、Content-Length等。
- 在请求中添加一些Cookie信息，从CookieJar中获取Cookie并发送给服务器。
- 在响应中解析一些头部信息，比如Content-Encoding、Transfer-Encoding等，并对响应进行解压缩或分块处理。
- 在响应中解析一些Cookie信息，从响应中获取Cookie并存储到CookieJar中。

### 5.3 CacheInterceptor

这是一个网络层面的拦截器，它负责实现缓存策略和逻辑。它在责任链中的位置是在BridgeInterceptor之后，在ConnectInterceptor之前。它有以下几个功能：

- 根据请求和响应的头部信息来判断是否使用缓存或者向服务器发送验证请求，并调用Cache类的方法来操作缓存。
- 根据请求和响应的头部信息来生成一个最优的缓存策略对象，并使用CacheStrategy类来辅助。
- 根据缓存策略对象来获取缓存响应或者网络响应，并将其传递给下一个拦截器。

### 5.4 ConnectInterceptor

这是一个网络层面的拦截器，它负责实现链接策略和逻辑。它在责任链中的位置是在CacheInterceptor之后，在CallServerInterceptor之前。它有以下几个功能：

- 根据请求的URL、代理、协议等因素来创建一个RouteSelector对象，并调用其next方法来获取一个可用的连接，并将其传递给下一个拦截器。
- 根据请求和响应的头部信息来判断是否需要升级协议，比如从HTTP/1.1升级到HTTP/2，并更新连接的协议信息。

### 5.5 CallServerInterceptor

这是一个网络层面的拦截器，它负责实现与服务器的通信。它在责任链中的位置是最后面的一个。它有以下几个功能：

- 根据连接对象来创建一个HttpCodec对象，并使用其方法来编码请求和解码响应。
- 根据请求对象来创建一个RequestBody对象，并使用其方法来写入请求正文数据。
- 根据响应对象来创建一个ResponseBody对象，并使用其方法来读取响应正文数据。
- 根据请求和响应的头部信息来判断是否需要关闭连接或者回收连接，并调用ConnectionPool类的方法来操作连接池。

以上就是OkHttp内部提供的拦截器，它们按照一定的顺序和逻辑构成了一个完整的请求过程。当然，我们也可以自定义一些拦截器，比如日志拦截器、压缩拦截器、加密拦截器等，并添加到OkHttpClient中，以实现更多的功能和需求。

