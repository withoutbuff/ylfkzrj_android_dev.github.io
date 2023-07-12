# okhttp使用的简单举例

## 一、什么是okhttp

okhttp用于替代HttpUrlConnection和Apache HttpClient (android API23 6.0里已移除HttpClient)的网络框架。它是一个高效、简洁、易用的HTTP客户端，支持HTTP和HTTPS协议，能够处理异步请求和文件上传下载等操作。

okhttp具有如下优点：

- 允许连接到同一个主机地址的所有请求,提高请求效率
- 共享Socket,减少对服务器的请求次数
- 通过连接池,减少了请求延迟
- 缓存响应数据来减少重复的网络请求
- 减少了对数据流量的消耗
- 自动处理GZip压缩


| 名称 | 优点 | 缺点 | 使用场景 |
| :---: | :---: | :---: | :---: |
| okhttp | - 高效、简洁、易用<br>- 支持HTTP和HTTPS协议<br>- 支持异步请求和文件上传下载<br>- 支持连接复用、缓存、压缩等优化机制 | - 需要切换到主线程<br>- 传入调用比较复杂<br>- 不支持自定义的请求方法<br>- 不支持自动重定向 | - 高性能网络请求<br>- 文件上传下载<br>- 压缩和缓存控制<br>- 拦截器和HTTPS处理 |
| HttpUrlConnection | - 简单易用，不需要引入第三方库<br>- 灵活可控，可以自定义各种参数和设置<br>- 兼容性好，适用于不同版本的Android系统 | - 代码冗长，需要处理各种异常和输入输出流<br>- 性能较差，没有连接池和压缩等优化机制<br>- 功能较弱，没有支持异步请求和文件上传下载等操作 | - 简单灵活的网络请求<br>- 兼容不同版本的Android系统<br>- 自动跟随重定向 |
| Apache HttpClient | - 功能强大，提供了丰富的API和工具类<br>- 性能优良，提供了连接池和压缩等优化机制<br>- 稳定可靠，经过了长期的测试和使用 | - API过于复杂，使用起来不够简洁<br>- 依赖第三方库，需要引入多个jar包<br>- 兼容性差，Android 6.0已经移除了HttpClient的支持 | - 功能强大、稳定可靠的网络请求<br>- 自定义SSL和重定向策略 |

## 二、如何使用okhttp

### 2.1 添加依赖

在Android studio中新建Android项目后，打开project视图，app目录下的build.gradle文件，在dependencies { }中添加Okhttp依赖。

```gradle
implementation 'com.squareup.okhttp3:okhttp:3.10.0'
```

完成后记得 Sync now。

### 2.2 发送GET请求

GET请求是最常见的一种网络请求方式，用于从服务器获取数据。使用okhttp发送GET请求的基本步骤如下¹：

- 创建OkHttpClient对象
- 创建Request对象，设置URL和其他参数
- 调用OkHttpClient的newCall方法，传入Request对象，得到Call对象
- 调用Call对象的execute方法，发送同步请求，或者调用enqueue方法，发送异步请求
- 处理响应结果，根据Response对象的方法获取响应头、响应体等信息

以下是一个简单的示例代码，使用okhttp发送一个GET请求，并打印响应结果：

```java
//创建OkHttpClient对象
OkHttpClient client = new OkHttpClient();
//创建Request对象
Request request = new Request.Builder()
        .url("http://www.baidu.com") //设置URL
        .build();
//创建Call对象
Call call = client.newCall(request);
//发送同步请求
try {
    //获取响应对象
    Response response = call.execute();
    //判断是否成功
    if (response.isSuccessful()) {
        //获取响应体内容
        String data = response.body().string();
        //打印结果
        System.out.println(data);
    } else {
        //处理错误情况
        System.out.println("请求失败：" + response.code());
    }
} catch (IOException e) {
    //处理异常情况
    e.printStackTrace();
}
```

需要注意的是：由于网络连接非常耗时，Android不允许在主线程进行网络连接！因此我们需要在子线程中发送网络请求，并通过Handler或者runOnUiThread等方式回到主线程更新UI界面。

### 2.3 发送POST请求

POST请求是另一种常见的网络请求方式，用于向服务器提交数据。使用okhttp发送POST请求的基本步骤如下⁴：

- 创建OkHttpClient对象
- 创建RequestBody对象，设置要提交的数据和内容类型（MediaType）
- 创建Request对象，设置URL和RequestBody对象
- 调用OkHttpClient的newCall方法，传入Request对象，得到Call对象
- 调用Call对象的execute方法，发送同步请求，或者调用enqueue方法，发送异步请求
- 处理响应结果，根据Response对象的方法
获取响应头、响应体等信息

以下是一个简单的示例代码，使用okhttp发送一个POST请求，并打印响应结果：

```java
//创建OkHttpClient对象
OkHttpClient client = new OkHttpClient();
//创建RequestBody对象，设置要提交的数据和内容类型（MediaType）
RequestBody body = RequestBody.create("name=张三&age=18", MediaType.parse("application/x-www-form-urlencoded"));
//创建Request对象，设置URL和RequestBody对象
Request request = new Request.Builder()
        .url("http://www.example.com/post") //设置URL
        .post(body) //设置RequestBody对象
        .build();
//创建Call对象
Call call = client.newCall(request);
//发送同步请求
try {
    //获取响应对象
    Response response = call.execute();
    //判断是否成功
    if (response.isSuccessful()) {
        //获取响应体内容
        String data = response.body().string();
        //打印结果
        System.out.println(data);
    } else {
        //处理错误情况
        System.out.println("请求失败：" + response.code());
    }
} catch (IOException e) {
    //处理异常情况
    e.printStackTrace();
}
```

同样，我们需要在子线程中发送网络请求，并通过Handler或者runOnUiThread等方式回到主线程更新UI界面。

### 2.4 上传文件

使用okhttp上传文件的基本步骤如下：

- 创建OkHttpClient对象
- 创建MultipartBody对象，设置要上传的文件和其他参数（如文件名、文件类型等）
- 创建Request对象，设置URL和MultipartBody对象
- 调用OkHttpClient的newCall方法，传入Request对象，得到Call对象
- 调用Call对象的execute方法，发送同步请求，或者调用enqueue方法，发送异步请求
- 处理响应结果，根据Response对象的方法获取响应头、响应体等信息

以下是一个简单的示例代码，使用okhttp上传一个图片文件：

```java
//创建OkHttpClient对象
OkHttpClient client = new OkHttpClient();
//创建File对象，指定要上传的文件路径
File file = new File("/sdcard/pic.jpg");
//创建RequestBody对象，设置文件类型和文件内容
RequestBody fileBody = RequestBody.create(MediaType.parse("image/jpeg"), file);
//创建MultipartBody对象，设置要上传的文件和其他参数（如文件名、文件类型等）
MultipartBody body = new MultipartBody.Builder()
        .setType(MultipartBody.FORM) //设置表单类型
        .addFormDataPart("file", file.getName(), fileBody) //添加表单数据，包括文件名、文件类型和文件内容
        .addFormDataPart("name", "张三") //添加其他表单数据
        .build();
//创建Request对象，设置URL和MultipartBody对象
Request request = new Request.Builder()
        .url("http://www.example.com/upload") //设置URL
        .post(body) //设置MultipartBody对象
        .build();
//创建Call对象
Call call = client.newCall(request);
//发送异步请求
call.enqueue(new Callback() {
    @Override
    public void onFailure(Call call, IOException e) {
        //处理失败情况
        e.printStackTrace();
    }

    @Override
    public void onResponse(Call call, Response response) throws IOException {
        //处理成功情况
        if (response.isSuccessful()) {
            //获取响应体内容
            String data = response.body().string();
            //打印结果
            System.out.println(data);
        } else {
            //处理错误情况
            System.out.println("请求失败：" + response.code());
        }
    }
});
```

## 三、okhttp的优缺点

### 3.1 优点

okhttp有以下几个优点：

- 支持SPDY, 可以合并多个到同一个主机的请求，使用连接池技术减少请求的延迟 (如果SPDY是可用的话)
- 使用GZIP压缩减少传输的数据量
- 缓存响应避免重复的网络请求、拦截器等等

### 3.2 缺点

okhttp也有以下几个缺点：

- 消息回来需要切到主线程，主线程要自己去写
- 传入调用比较复杂，需要创建多个对象和方法
- 不支持自定义的请求方法，只能使用GET、POST、PUT、DELETE等标准方法
- 不支持自动重定向，需要手动处理

## 四、okhttp的使用场景

okhttp适合用于以下几种使用场景：

- 需要高效、高性能的网络请求，支持HTTP/2和连接池技术
- 需要处理文件上传下载，支持多部分表单和进度监听
- 需要处理GZIP压缩和缓存策略，支持透明的压缩和缓存控制
- 需要处理拦截器，支持添加、移除或转换请求或响应的头部信息
- 需要处理HTTPS，支持自定义证书和主机名验证


