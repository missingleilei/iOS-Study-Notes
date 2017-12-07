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





