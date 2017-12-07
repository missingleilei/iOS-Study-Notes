##### 1.产生随机数

```
int allTextIndex = arc4random_uniform(30)+1;
```

##### 2.禁止视图自动布局

```
if ([self respondsToSelector:@selector(setAutomaticallyAdjustsScrollViewInsets:)]) {

        self.automaticallyAdjustsScrollViewInsets = NO;
        }
```

#### 3.添加单击事件

```
UITapGestureRecognizer *userTap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(clickUserPhotoImage)]; 
[self.userPhotoImageView addGestureRecognizer:userTap];
```

#### 4.延时加载方法

```
double delayInSeconds = 1;
dispatch_time_t delay = dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delayInSeconds * NSEC_PER_SEC));
               dispatch_after(delay, dispatch_get_main_queue(), ^(void){
               //执行的方法});
```

##### 5.简单弹出框，可直接写在pch文件中

```
#define kTipAlert(_S_, ...)     [[[UIAlertView alloc] initWithTitle:@"提示" message:[NSString stringWithFormat:(_S_), ##__VA_ARGS__] delegate:nil cancelButtonTitle:@"知道了" otherButtonTitles:nil] show]
```

##### 6.通知的简单使用

```
//发送登录成功的通知
[[NSNotificationCenter defaultCenter] postNotificationName:@"UserLoginSuccess" object:nil];
//监听用户登录成功后的通知
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(getUserDetail) name:@"UserLoginSuccess" object:nil];
//移除通知
移除单个通知:[[NSNotificationCenter defaultCenter] removeObserver:self name:@"JPUSHNOTIFICATION" object:self];
移除当前所有通知:[[NSNotificationCenterdefaultCenter]removeObserver:self];
```

##### 7.获取当前国家地区

```
NSString *systemCountryCode  = [[NSLocale currentLocale] objectForKey:NSLocaleCountryCode];
```

##### 8.当有名称相同的代理协议方法时如何解决？

```
[WXApi handleOpenURL:url delegate:(id<WXApiDelegate>)self];
比如微信和QQ的代理中有个方法名称都叫 onResp：
微信：-(void)onResp:(BaseResp*)resp;
QQ: - (void)onResp:(QQBaseResp *)resp;
将参数改成id类型即可- (void)onResp:(id)resp
```

##### 9.隐藏状态栏

```
[[UIApplication sharedApplication]setStatusBarHidden:YES withAnimation:UIStatusBarAnimationFade];
```

##### 10.NSTimer的简单使用

```
self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(refreshLessTime) userInfo:@"" repeats:YES];
[[NSRunLoop currentRunLoop] addTimer:self.timer forMode:UITrackingRunLoopMode];
```

##### 11.使用Masnory 获取Frame

```
[self.rightButton.superview layoutIfNeeded];
CGRect frame = self.rightButton.frame;
CGRect frame2 = self.leftButton.frame;
NSLog(@"%@",NSStringFromCGRect(frame));
```

##### 12.使用UIBezierPath 创建分割线

```
UIBezierPath *path = [[UIBezierPath alloc] init];
//path创建好后，就可以设置其线宽，颜色等属性
[path moveToPoint:CGPointMake(0, 0)];
[path addLineToPoint:CGPointMake(UISCREENWIDTH, 0)];
path.lineCapStyle = kCGLineCapRound;
path.lineJoinStyle = kCGLineJoinRound;    
CAShapeLayer *shapeLayer=[CAShapeLayer layer];
shapeLayer.path=path.CGPath;
shapeLayer.fillColor = [UIColor clearColor].CGColor;//填充颜色
shapeLayer.strokeColor = [ZMColor colorWithRed:210 withGreen:210 withBlue:212 withAlpha:0.45].CGColor;//边框颜色
shapeLayer.lineWidth = 0.5;
[self.layer addSublayer:shapeLayer];
```

##### 13.判断含有某个字符串

```
 if ([actionSheet.title rangeOfString:@"://"].location == NSNotFound 如果为false，表示含有://字符
```

##### 14.Library not loaded: 错误解决方法

```
在build phases 改为optional 
```

##### 15.Xcode调试不能停在代码区域

```
只要 XCode中 Debug ->Debug Workflow - Always Shaw Disassembly 取消打勾就可以了
```

##### 16.主线程更新UI

```
__weak typeof(self) weakSelf = self;
dispatch_async(dispatch_get_main_queue(), ^{
     weakSelf.tableView.tableHeaderView = weakSelf.headView;
     [weakSelf.tableView reloadData];
});
```

##### 17.判断对象是否为空

```
- (BOOL)isNull{
     if ([self isEqual:[NSNull null]]) {
     return YES;
     }else{
         if ([self isKindOfClass:[NSNull class]]){
         return YES;
         }else{
         if (self==nil){
         return YES;    
          }}}
       if ([self isKindOfClass:[NSString class]]) { if ([((NSString *)self) isEqualToString:@"(null)"]) {
       return YES;
       }}
       return NO;
  }
```

##### 18.cocoapods添加了新库但不想更新之前的库

```
pod install --no-repo-update
```

##### 19.将试图放到最下面 或最上面

```
[self.mainView addSubview:imageView];
[self.mainView sendSubviewToBack:imageView];  底下
[self.mainView bringSubviewToFront :imageView]; 朝上
```

##### 20.IQKeyboardManager键盘库的简单使用

```
// 获取类库的单例变量
IQKeyboardManager *keyboardManager = [IQKeyboardManager sharedManager];
// 控制整个功能是否启用
keyboardManager.enable = YES;
// 控制点击背景是否收起键盘
keyboardManager.shouldResignOnTouchOutside = YES;
// 控制键盘上的工具条文字颜色是否用户自定义
keyboardManager.shouldToolbarUsesTextFieldTintColor = YES;
// 有多个输入框时，可以通过点击Toolbar 上的“前一个”“后一个”按钮来实现移动到不同的输入框
keyboardManager.toolbarManageBehaviour = IQAutoToolbarBySubviews;
// 控制是否显示键盘上的工具条
keyboardManager.enableAutoToolbar = YES;
// 是否显示占位文字
keyboardManager.shouldShowTextFieldPlaceholder = YES;
// 设置占位文字的字体
keyboardManager.placeholderFont = [UIFont boldSystemFontOfSize:17];
// 输入框距离键盘的距离
keyboardManager.keyboardDistanceFromTextField = 10.0f;
keyboardManager.preventShowingBottomBlankSpace = NO;
```

##### 21.跳转到评价页面

```
NSString *str = [NSString stringWithFormat:@"itms-apps://ax.itunes.apple.com/WebObjects/MZStore.woa/wa/viewContentsUserReviews?type=Purple+Software
&id=你的AppId" ];
if( ([[[UIDevice currentDevice] systemVersion] doubleValue]>=7.0)){
str = [NSString stringWithFormat:@"https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewContentsUserReviews?id=你的AppId
&pageNumber=0&sortOrdering=2&type=Purple+Software&mt=8"];}
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:str]];
```

##### 22.自动布局 autoLayout

```
setNeedsLayout：告知页面需要更新，但是不会立刻开始更新。执行后会立刻调用layoutSubviews。 
layoutIfNeeded：告知页面布局立刻更新。所以一般都会和setNeedsLayout一起使用。如果希望立刻生成新的frame需要调用此方法，利用这点一般布局动画可以在更新布局后直接使用这个方法让动画生效。 
layoutSubviews：系统重写布局 setNeedsUpdateConstraints：告知需要更新约束，但是不会立刻开始 
updateConstraintsIfNeeded：告知立刻更新约束 
updateConstraints：系统更新约束
```

##### 23.UIView 使用阴影产生离屏渲染卡顿相关链接

\([http://blog.csdn.net/zixiweimi/article/details/39889623](https://link.jianshu.com/?t=http://blog.csdn.net/zixiweimi/article/details/39889623)\)

```
[self.view layer].shadowPath =[UIBezierPath bezierPathWithRect:self.mainView.bounds].CGPath;
```



