# js交互组件

#### 请移步[js交互组件gitlab](http://172.28.24.128/Yesway-Framework-Project/SDK/HSWebViewJSBridgeManager/blob/master/README.md),以下可能不是最新版本readme



#### 通过 CocoaPods 导入

```objc
pod 'HSWebViewJSBridgeManager' , :git => 'http://172.28.24.128/Yesway-Framework-Project/SDK/HSWebViewJSBridgeManager.git'
```

<font color=gray>pods文件夹中包括以下内容</font>

```objc
1.HSWebJSBridgeManager.h/m
2.WebViewJavascriptBridge相关文件
```



#### API列表

```objc
/// 初始化WebViewJavascriptBridge
/// @param webView 传入webview来实现桥接
/// @param htmlName 本地html文件名（不用写.html） 可传nil 或 @"" 但不可两个参数同时传空
/// @param htmlUrl 网络的html地址 可传nil 或 @"" 但不可两个参数同时传空
/// @param delegate controller
- (void)initBridgeWithWebView:(WKWebView *)webView AndInnerWebView:(NSString * _Nullable)htmlName OrHtmlUrl:(NSString * _Nullable)htmlUrl delegate:(id)delegate;


/// JS调用OC注册方法事件(无参)
/// @param handlerName 事件名称
/// @param dataCallback 给OC的传值
- (void)JS2OCRegisterHandlerWithHandlerName:(NSString *)handlerName dataCallback:(nullable void(^)(NSDictionary * backDic))dataCallback ;


/// JS调用OC注册方法事件(有参)
/// @param handlerName 事件名称
/// @param parame 给JS的参数
/// @param dataCallback 给OC的传值
- (void)JS2OCRegisterHandlerWithHandlerName:(NSString *)handlerName parame:(id)parame dataCallback:(nullable void(^)(NSDictionary * backDic))dataCallback;


/// OC调用JS方法(无参)
/// @param handlerName 事件名称
/// @param dataCallback JS收到调用后给OC的响应
- (void)OC2JSCallHandlerWithHandlerName:(NSString *)handlerName dataCallback:(nullable void(^)(id responseData))dataCallback;


/// OC调用JS方法(有参)
/// @param handlerName 事件名称
/// @param parame 给JS的传值
/// @param dataCallback JS收到调用后给OC的响应
- (void)OC2JSCallHandlerWithHandlerName:(NSString *)handlerName parame:(id)parame dataCallback:(nullable void(^)(id responseData))dataCallback;


/// 移除_bridge  !!!!不移除的话，当第二次进入页面时_bridge存在会直接return
- (void)remove_bridge;
```



#### 增加头文件引用

###### <font color=gray>在`AppDelegate.m` 的文件中，增加头文件引用</font>

```objc
#import "HSWebJSBridgeManager.h"
```



#### 初始化JSBridge

```objc
//初始化bridge
[[HSWebJSBridgeManager share] initBridgeWithWebView:webView AndInnerWebView:@"ExampleApp" OrHtmlUrl:nil delegate:self];
```



#### 交互方法

```objc
//注册JS调用OC方法--无参
[[HSWebJSBridgeManager share] JS2OCRegisterHandlerWithHandlerName:@"loginAction1" dataCallback:^(NSDictionary *backDic) {
  NSString *str = [NSString stringWithFormat:@"用户名：%@  姓名：%@",backDic[@"userId"],backDic[@"name"]];
  NSLog(@"JS调用OC，取到参数为： %@",str);
}];


//注册JS调用OC方法--有参
[[HSWebJSBridgeManager share] JS2OCRegisterHandlerWithHandlerName:@"loginAction2" parame:@{@"key":@"value"} dataCallback:^(NSDictionary *backDic) {
  NSString *str = [NSString stringWithFormat:@"用户名：%@  姓名：%@",backDic[@"userId"],backDic[@"name"]];
  NSLog(@"JS调用OC，取到参数为： %@",str);
}];


//OC调用JS方法----无参
[[HSWebJSBridgeManager share] OC2JSCallHandlerWithHandlerName:@"registerAction" dataCallback:^(id responseData) {
  NSLog(@"OC调用JS方法回调：%@",responseData);
}];


//OC调用JS方法---有参
[[HSWebJSBridgeManager share] OC2JSCallHandlerWithHandlerName:@"registerAction" parame:@"传递的参数" dataCallback:^(id responseData) {
  NSLog(@"OC调用JS方法回调：%@",responseData);
}];
```



#### 释放bridge

```objc
//置空_bridge退出页面后_bridge不会马上销毁造成第二次进入页面会直接return，页面无法加载
[[HSWebJSBridgeManager shareManager]remove_bridge];
```

