2.如果页面 A 跳转到 页面 B，A 的 viewDidDisappear 方法和 B 的 viewDidAppear 方法哪个先调用？

1.若APushB

A:viewWillDisappear--B:viewWillAppear—A:viewDidDisAppaer—B:viewDidAppaer

2.若APresentC

A:viewWillDisappear--C:viewWillAppear—C:viewDidAppaer—A:viewDidDisAppaer

#### 3.使用 GCD 如何实现这个需求：A、B、C 三个任务并发，完成后执行任务 D

1.异步串行

`dispatch_async(dispatch_get_main_queue(),^{`

`});`

2.组队列

`dispatch_group_async(group, dispatch_get_global_queue(0,0),^{`

`});`

`dispatch_group_notify(group, dispatch_get_main_queue(),^{`

`});`

#### 4.Block循环引用的原因？

两个对象相互引用对方作为自己的成员变量，只有自己销毁的时候才能将成员变量的引用计数减1。对象的销毁相互依赖，谁都无法销毁，从而造成循环引用。

weak表示的是一个弱引用，这个引用不会增加对象的引用计数，并且在所指向的对象被释放之后，weak指针会被设置的为nil。

weak比assign多了一个功能就是当属性所指向的对象消失的时候（也就是内存引用计数为0）会自动赋值为nil，这样再向weak修饰的属性发送消息就不会导致野指针操作crash。

#### 5.iOS11适配

1.启动图添加iOS11尺寸后模拟器才可以根据iOS11进行展示。

注意:iPhoneX的屏幕比例非16:9，所以启动页广告注意左右留一部分距离。

2.导航栏图层变化，如果是自定义导航栏和Tabbar需重新适配。

3.新增安全区概念iPhoneX的SafeAreaInsets值为\(88,0,34,0\)

4.Controller的automaticallyAdjustsScrollViewInsets属性被废弃且scrollView新增的两个属性：adjustContentInset和contentInsetAdjustmentBehavior

\#define NaviHeight Is\_Iphone\_X ? 88: 64

\#define TabbarHeight Is\_Iphone\_X ? 83: 49

\#define BottomHeight Is\_Iphone\_X ? 34: 0

##### iOS11 TableView适配

`if (@available(iOS 11.0, *)) {`

`self.tableView.contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever;//UIScrollView也适用`

`}else {`

`self.automaticallyAdjustsScrollViewInsets = NO;`

`}`

##### iOS11 地图不想提示是否允许定位

`Privacy - Location Always Usage Description   //删掉这个iOS11可提示`

`Privacy - Location When In Use Usage Description`

#### 6.NSString为何用copy而不是strong修饰？

主要是为了防止NSString被修改。当NSString的赋值来源也是NSString时，strong和copy的作用相同，都是给复制来源的引用计数加1；当NSStrig的赋值来源是NSMutableString时，copy会做深拷贝，即重新生成一个新的对象，修改赋值来源不会影响NSString的值。对源头是NSMutableString的字符串，retain仅仅是指针引用，增加了引用计数器，这样源头改变的时候，用这种retain方式声明的变量（无论被赋值的变量是可变的还是不可变的），它也会跟着改变;而copy声明的变量，它不会跟着源头改变，它实际上是深拷贝。对源头是NSString的字符串，无论是retain声明的变量还是copy声明的变量，当第二次源头的字符串重新指向其它的地方的时候，它还是指向原来的最初的那个位置，也就是说其实二者都是指针引用，也就是浅拷贝。

总结就是：当修饰可变类型的属性时用strong，当修饰不可变类型的属性时用copy。

copy：产生的对象副本是不可变的

mutableCopy：产生的对象副本是可变的

浅拷贝复制对象本身，对象里的属性、包括的对象不做复制。

深拷贝既复制对象本身，对象的属性也会复制一份。

#### 7.支付宝支付开发流程？

##### 支付流程：

1.生成订单信息\(客户端传一部分信息给服务端，服务端拼接号订单信息加密后返回给客户端\)。

2.通过服务端返回字符串及配置好URLScheme调用支付宝接口发送订单信息。

3.支付宝根据狗没结果返回支付结果并在AppDelegate里处理结果。

##### 公钥私钥

RSA非对称加密算法

由商户自己生成的RSA公钥（与应用私钥必须匹配），商户需上传应用公钥到支付宝开放平台，以便支付宝使用该公钥验证该交易是否是商户发起的。

##### 应用私钥：

由商户自己生成的RSA私钥（与应用公钥必须匹配），商户开发者使用应用私钥对请求字符串进行加签。

##### 支付宝公钥：

支付宝的RSA公钥，商户使用该公钥验证该结果是否是支付宝返回的。

PID：合作身份ID，2088开头的十六位数组

SellerID：商家唯一标识符

#### 8.微信支付开发流程？

支付流程：

1.用户在App中选择商品提交订单并选择微信支付。

2.服务端收到客户端所传用户支付信息并调用微信支付统一下单接口。

3统一下单接口返回正常prepay-id，再根据签名规范生成签名返回给App

\(appid，partnerid，prepay\_id，noncestr，timestamp，package\)

4.App调起微信支付。

5.返回App接收到支付结果处理。

关于微信支付结果页面返回程序左上角按钮点击返回不走微信回调问题？

通过AppDelegate的applicationWillEnterForeground方法，只需要在这方法里验证订单号是否已经支付完成\(和后台协商定一个接口\)

#### 9.野指针及如何定位野指针？

当所指向的对象被释放或者回收，但是对该指针没有作任何的修改，以至于该指针仍旧指向已经被回收或释放的内存地址，此情况下该指针被称为野指针。

Edit Scheme diagnostics 诊断中

Malloc Scribble对已经释放内存进行数据填充，从而确保野指针的访问必然崩溃。

Zombie Objects将释放的对象标记为Zombie对象，再次给Zombie对象发送消息时，发生Crash并且输出相关调试信息。

#### 10.App内存如何管理分析？

##### \(Xcode运行程序期间\)

1.导航栏Debug navigation里面可以查看Menory使用情况

2.Instrument—Activity Monitor Allocations Leaks

##### Leaks是检测内存泄漏的工具

Allocations时检测堆栈内存\(没有统计虚拟内存\)

Activity Monitor看手机整体内存情况

