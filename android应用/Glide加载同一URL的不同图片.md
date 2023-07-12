Glide加载图片时，会使用缓存来提升加载速度，此时如果加载的URL不变，Glide不会主动请求URL资源是否更新，所以此时需要我们做HTTP请求，倘若资源已更新，就通知Glide重新加载。

如果简单粗暴的禁用Glide缓存，那么每次加载图片， imageView都会闪烁，即消耗性能，也不美观。

下面是需要用到的知识：

##1. HTTP资源更新标识

1.首先发送HTTP get请求，在请求头中加入参数If-Modified-Since: Thu, 30 Oct 2014 22:36:24 GMT(时间戳早于等于当前时间都可以)

2.此时有两种可能性（不考虑异常情况404，501等）如果收到304，说明资源未更新。但此处还需要做一次判断，响应头中的 Last-Modified>请求头的If-Modified-Since，此时需要再请求一次资源
![示例](https://upload-images.jianshu.io/upload_images/24860325-85165cb1684b52a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![示例](https://upload-images.jianshu.io/upload_images/24860325-f9621ea8f042996f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


3.收到200，说明资源已经更新。

##2. Glide的signature机制

Glide使用signature机制，来判断是否重新请求URL资源，这里我们直接用Last-Modified作为key就可以了。

##3. HTTP请求库OKHTTP

##4. Android持久化数据sharedPreference

#下面是全部代码：
```JAVA
**private static** String *IF_MODIFIED_SINCE* = **"If-Modified-Since"**;  **private static** String *SP_FILE_COMMON* = **"sp_common"**;  **private static boolean** *isCheckAlbumChange* = **false**;

    **private void** checkUrlUpdated(String url, ImageView imageView) {          *//当url为空时，不请求网络,加载默认图片***if** (TextUtils.*isEmpty*(url)) {              MainActivity.**this**.runOnUiThread(() -> {                  Glide.*with*(MainActivity.**this**).load(R.drawable.***ic_prisma***).into(imageView);              });  } **else** {              *//获取url对应存储在sp中的Last-Modified或Etag*String key = SharedPreferencesUtils.*getParam*(MainActivity.**this**, *SP_FILE_COMMON*, **""**).toString();              **if** (!TextUtils.*isEmpty*(key)) {                  *//key非空，即本地存在缓存，先加载本地缓存*MainActivity.**this**.runOnUiThread(() -> {                      glideLoadImg(MainActivity.**this**, key, url, imageView);                  });  } **else** {                  *//key为空，不存在缓存，加载默认图片*MainActivity.**this**.runOnUiThread(() -> {                      Glide.*with*(MainActivity.**this**).load(R.drawable.***ic_prisma***).into(imageView);                  });              }  HashMap<String, String> p = **new** HashMap<>();              p.put(**"If-Modified-Since"**, key);              OkHttpManager.*getResponse*(url, p, **new** Callback() {                  @Override                  **public void** onFailure(@NotNull Call call, @NotNull IOException e) {  *//                    Log.d(TAG, "onFailure: " + e.toString());*}                    @Override                  **public void** onResponse(@NotNull Call call, @NotNull Response response) **throws** IOException {                      Log.*d*(***TAG***, **"onResponse: "**);                      *//Last-Modified检测一致***if** (!SharedPreferencesUtils.*getParam*(MainActivity.**this**, *SP_FILE_COMMON*, **""**).toString().equals(response.header(**"Last-Modified"**))) {                          *isCheckAlbumChange* = **false**;                          **if** (response.header(**"Last-Modified"**) != **null** && response.header(**"Last-Modified"**).getClass().getSimpleName() != **null**) {                              SharedPreferencesUtils.*setParam*(MainActivity.**this**, *SP_FILE_COMMON*, response.header(**"Last-Modified"**));  } **else** {                              MainActivity.**this**.runOnUiThread(()->{                                  Glide.*with*(MainActivity.**this**)                                          .load(url)                                          .placeholder(R.drawable.***ic_prisma***)                                          .error(R.drawable.***ic_prisma***)                                          .into(imageView);                              });                              **return**;                          }                          **if** (!*isCheckAlbumChange*) {                              *//不一致重新请求一次，只一次*Log.*d*(***TAG***, **"onResponse: recursion checkUrlUpdated"**);                              checkUrlUpdated(url, imageView);                          }  } **else** {                          *isCheckAlbumChange* = **true**;                          **if** (response.code() == 304) {                              *//未改变加载缓存*Log.*d*(***TAG***, **"onResponse: 304"**);                              Log.*d*(***TAG***, **"onResponse: 304"** + response.header(**"Last-Modified"**));                              MainActivity.**this**.runOnUiThread(() -> {                                  glideLoadImg(MainActivity.**this**, key, url, imageView);                              });  } **else if** (response.code() == 200) {                              *//改变重新请求*Log.*d*(***TAG***, **"onResponse: 200"**);                              Log.*d*(***TAG***, **"onResponse: 200"** + response.header(**"Last-Modified"**));                              MainActivity.**this**.runOnUiThread(() -> {                                  glideLoadImg(MainActivity.**this**, response.header(**"Last-Modified"**), url, imageView);                              });                          }                      }                  }              });          }      }        **private void** glideLoadImg(Context context, String key, String url, ImageView imageView) {          MainActivity.**this**.runOnUiThread(() -> {              Log.*d*(***TAG***, **"glideLoadImg: "** + key);  *//            Glide.with(context).load(imageView.getDrawable())**//                    .signature(new ObjectKey(signature))**//                    .load(url)**//                    .placeholder(imageView.getDrawable())**//                    .error(R.drawable.ic_prisma)**//                    .into(imageView);*Glide.*with*(context).setDefaultRequestOptions(RequestOptions.*noAnimation*()                      *//图片签名信息，相同url下如果需要刷新图片，signature不同则会加载网络端的图片资源*.signature(**new** ObjectKey(key)).placeholder(imageView.getDrawable()))                      .load(url)                      .into(imageView);            });      }
```
