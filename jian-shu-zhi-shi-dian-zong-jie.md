#### 1.UIPasteboard使用

```
UIPasteboard是ios中访问粘贴板的原生控件，可分为系统等级的和app等级的，系统等级的独立于app，可以复制一个app的内容到另一个app；
app等级的只能在app内进行复制和粘贴；它们分别由+(UIPasteboard)generalPasteboard;和+(nullable UIPasteboard )pasteboardWithName:
(NSString )pasteboardName create:(BOOL)create;
这两个类方法进行创建。
```

```
//获取系统级别的剪切板
+ (UIPasteboard*)generalPasteboard;
//获取一个自定义的剪切板 name参数为此剪切板的名称 create参数用于设置当这个剪切板不存在时 是否进行创建
+ (UIPasteboard*)pasteboardWithName:(NSString*)pasteboardName create:(BOOL)create;
//获取一个应用内可用的剪切板
+ (UIPasteboard*)pasteboardWithUniqueName;

赋值
UIPasteboard pasteboard = [UIPasteboard generalPasteboard];
[pasteboard setString:@"复制的字符串内容"];

/*通过名称获取粘贴板并且移除*/
+ (void)removePasteboardWithName:(NSString *)pasteboardName;
 /*从粘贴板中获取数据，pasteboardType是自定义的，说明app可以处理哪种类型的数据*/
- (nullable NSData *)dataForPasteboardType:(NSString *)pasteboardType; 
/*data类型的数据放在粘贴板中，pasteboardType同上*/
- (void)setData:(NSData *)data forPasteboardType:(NSString *)pasteboardType;
/*从粘贴板中取出data*/
- (nullable NSData *)dataForPasteboardType:(NSString *)pasteboardType;

在ios中，支持UIPasteboard原生控件只有UITextField 、UITextView、UIWebView这三个。
如果想自定义一个控件能够使用UIPasteboard，需要在定义的时候重载
-(BOOL)canBecomeFirstResponder和 -(BOOL)canPerformAction:(SEL)action withSender:(id)sender
```

#### 2.终极横竖屏切换解决方案

[https://github.com/newyjp/BLLandscape](https://github.com/newyjp/BLLandscape)

#### 3.搭建Android环境

https://www.jianshu.com/p/bf77cb5ce70b

#### 4.iOS-9.0后跳支付左上角返回键解决办法

`-(void)applicationWillEnterForeground:(UIApplication *)application`

我在这个方法里发通知告诉调起支付的VC，请求服务器，看服务器那边的支付结果，如果没有支付结果，就给支付结果等待页面，一般来说支付宝和微信回调结果会很快的。

但是仅仅这样做事不够的，因为app压后台，再次打开的情况很多，比如分享返回也走这个接口，我怎么会知道是不是支付调起的返回呢，那么我就在发起支付的时候，做了一个标记，这里我用了系统单例NSUserDefaults，这样我在返回app的时候，就知道是不是支付返回的了。  
`[[NSUserDefaults standardUserDefaults] setObject:@"微信返回"forKey:@"支付返回"];`

做完这些感觉是完成了，其实还有个重点问题，就是点击支付宝或微信给的取消或者完成按钮的时候，app返回不仅会走WillEnterForeground方法，而且会走下图的方法，要命的是，下图的方法是后进入的，这样app就会返回两次支付结果。

`- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options{`

```
为了解决以上问题，我就用了GCD，在进入WillEnterForeground方法里的时候，让里面的方法等0.5秒执行，如果是有回调的返回，就利用bool值，
变为ture，这是下面判断这个bool是ture，WillEnterForeground方法里的判断就不进，如果是没有回调的返回，这个bool值是不会改变的，
0.5秒后继续执行WillEnterForeground方法里的判断。
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options{
      _turefalseBack = YES;
      if(){
      }else{
      }
      rrturn YES;
}
-(void)applicationWillEnterForeground:(UIApplication *)application{
      _turefalseBack = NO;
      NSString *BackStr = [[NSUserDefaults standardUserDefaults] objectForKey:@"支付返回"];
      dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(0.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        dispatch_get_main_queue(){
        if(!_turefalseBack){
        }else{
        }
        }
    });
}
！！在每次支付处理结束，要把NSUserDefaults对应的键值清除。
```





