#### 1.微信登陆授权

1.授权微信三方登录，请求 code ——&gt;通过 code 获取 access\_token 以及 openId ——&gt;拿到 openId 进行登录再次请求 ——&gt;

access\_token 未过期 ——&gt;直接拿到 openId 进行登录

2.或 再次请求 ——&gt;access\_token 过期 ——&gt;重新通过 refresh\_token 进行刷新 access\_token 和 openId ——&gt;再次拿到 openId 进行登录

3.或 再次请求 ——&gt;access\_token 过期 &&refresh\_token 过期 ——&gt;重新进行授权请求

#### 2.class、superclass、super区别

class：获取当前方法调用者的类

superclass：获取当前方法调用者的父类

super：仅仅是一个编译指示器，就是给编译器看的，不是一个指针。

本质：只要编译器看到super这个标志，就会让当前对象去调用父类的方法，本质上还是当前对象在调用。

#### 3.const与宏的区别？

1.苹果推荐使用const而不是宏\(Swift取消宏\)。

2.编译时刻 宏：预编译             const：编译

3.编译检查 宏：没有编译检查  const：有编译检查

4.宏的好处 可以定义函数.方法 const不可以

5.宏的坏处 大量使用宏会导致预编译时间过长

const作用

1.修饰右边基本变量或指针变量

2.被const修饰变量只读

开发过程中的使用

1.修饰全局变量 ==&gt;全局只读变量 ==&gt;代替宏 NSString \*const name = @“name” 

2.修饰方法中的参数

#### 4.static和extern区别与联系

static

修饰局部变量

1.修饰局部变量，被static修饰的局部变量会延长生命周期，生命周期与应用程序相关。

2.被static修饰的局部变量，只会分配一次内存。

3.被static修饰的局部变量什么时候分配内存？ 程序一旦运行就会给static修饰的变量分配内存。

修饰全局变量

1.修饰全局变量，被static修饰的全局变量，作用域会修改，只能在当前文件中使用。

extern

1.声明外部全局变量，注意extern只能用于声明不能用于定义。

工作原理：先去当前文件下有没有对应的全局变量，如果没有再去其他文件查找。

Static  NSString \*const name = @“name”

Extern NSString \*const name = @“name”

#### 5.App运行启动逻辑

使用Xcode打开一个项目，很容易会发现一个文件－－main.m文件，此处就是应用的入口了。程序启动时，先执行main函数，main函数是ios程序的入口点，内部会调用UIApplicationMain函数，UIApplicationMain里会创建一个UIApplication对象 ，然后创建UIApplication的delegate对象 —–（您的）AppDelegate ，开启一个消息循环（main runloop），每当监听到对应的系统事件时，就会通知AppDelegate。

程序启动的完整过程如下：

1.main 函数

2.UIApplicationMain

* 创建UIApplication对象
* 创建UIApplication的delegate对象
* delegate对象开始处理\(监听\)系统事件\(没有storyboard\)
* 程序启动完毕的时候, 就会调用代理的application:didFinishLaunchingWithOptions:方法
* 在application:didFinishLaunchingWithOptions:中创建UIWindow
* 创建和设置UIWindow的rootViewController
* 显示窗口

3.根据Info.plist获得最主要storyboard的文件名,加载最主要的storyboard\(有storyboard\)

* 创建UIWindow
* 创建和设置UIWindow的rootViewController
* 显示窗口

#### 6.KVO底层机制

KVO是基于runtime机制实现的。当某个类的对象第一次被观察时，系统就会在运行期动态的创建该类的一个派生类，在这个派生类中重写基类的中任何被观察属性的setter方法。派生类在被重写的setter方法实现真正的通知机制（Person -&gt;NSKVONotifying\_Person

#### 7.Runtime简介

RunTime简称运行时，OC就是运行时机制，也就是运行时候的一些机制，其中最重要的就是消息机制。

C语言的函数调用在编译的的时候决定调用哪个函数。

OC属于动态调用过程，在编译的时候并不能决定真正调用哪个函数，只有在真正运行的时候才会根据函数的名称找到对应的函数进行调用。

1.RunTime如何利用发送消息的方法创建类  可以用RunTime调用私有方法。

`id objc = objc_msgSend([NSObject class],@selector(alloc));`

`id objc = objc_msgSend(objc_getClass(“NSObject”),objc_registerName”alloc");`

`objc = objc_msgSend(objc,@selector(init));`

`objc = objc_msgSend(objc,objc_registerName”init");`

相当于

`NSObject *objc = [[NSObject alloc] init];`

2.方法的交换 给系统的imageNamed添加功能只能用RunTime\(交换方法\)

//把类加载进内存时调用，只调用一次

`+(void)load{// 交换方法`

`  Method imageNamedMethod class_getClassMethod(self,@selector(imageNamed:));`

`  Method xmg_imageNamedMethod  class_getClassMethod(self,@selector(xmg_imageNamed:));  `

`  method_exchangeImplementations(imageNamedMethod,xmg_imageNamedMethod);`

`}`

#### 8.Runloop简介

RunLoop基本作用:

1.保持程序持续运行

2.处理App中的各种事件\( 比如触摸事件、定时器事件、Selectot事件\) 

3.节省CPU资源，提高程序性能，该做事的时候做事该休息的时候休息

RunLoop与线程关系

1.每条线程都有唯一与之对应的RunLoop对象

2.主线程的RunLoop已经自动创建好，子线程的RunLoop需要手动创建

3.RunLoop在第一次获取的时候创建，在线程结束时销毁

  
一个RunLoop包含若干个Mode，每一个Mode又包含若干个Source/Timer/Observer

KCFRunLoopDefaultMode 默认 主线程在此模式下运行

UITrackingRunLoopMode  界面追踪Mode

UIInitializationRunLoopMode 刚启动App进入的第一个Mode，启动完成后就不再使用

GSEventReceiveRunLoopMode 接收系统事件内部Model

KCFRunLoopCommonModes  占位Mode

每一个RunLoop有多重运行模式，但同一时间只能存在一种运行模式，每一个运行模式下有很多source\(事件源 需要RunLoop来处理的\)、timer、observer。

每次RunLoop启动时只能指定一种运行模式Mode，如果需要切换Mode只能退出RunLoop，再指定另一个Mode进入。

使用GCD创建定时器！

RunLoop运行流程

1.通知observer，即将进入RunLoop。

2.通知observer，将要处理Timer事件或Souce事件。循环执行完毕后。

3.通知observer，线程即将进行休眠并进行休眠。

4.唤醒observer后重新执行2.

通过observer监听RunLoop状态

自动释放池autoreleasepool什么时候释放？

第一次创建：启动RunLoop

最后一次销毁：RunLoop退出的时候

其他的时候创建和销毁：当RunLoop即将睡眠的时候销毁之前的的释放池，并重新创建一个新的释放池

RunLoop的使用场景？

1.常驻线程

2.子线程开启定时器

3.在子线程中进行一些长期监控



