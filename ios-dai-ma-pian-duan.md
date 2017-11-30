#### 1.NSMutableDictionary的遍历方式三种

##### 方法一：使用for-each循环遍历字典中的对象

`for (NSString *s in [dictionary allValues]) {`

`NSLog(@"value: %@", s);`

`}`

##### 方法二：NSDictionary函数allValues会返回以数组而非字典形式组织的对象。函数allKeys会将键值作为数组返回：

`for (NSString *s in [dictionary allKeys]) {`

`NSLog(@"key: %@", s);`

`}`

##### 方法三：通过enumerateKeysAndObjectsUsingBlock:方法针对字典中的每个对象执行代码。可以用来定义代码块，然后应用到字典中的每个对象，同时又不必创建for-each循环或是获得数组版本的字典引用：

`[dictionary enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {`

`NSLog(@"key = %@ and obj = %@", key, obj);`

`}];`

#### 2.结构体如何存入数组？

需要将CGRect放入NSArray中

`CGRect  rect = CGMakeRect(1, 2, 100, 200);`

`NSValue  *rectValue = [NSValue valueWithBytes:&rect objCType:@encode(CGRect)];`

`[array  addObject:rectValue];`

使用getValue提取数值 传递参数为要存储这个数值的变量的地址

`NSValue *rectValue = [array objectAtIndex: 0];`

`CGRect  rect = [rectValue getValue:&rect];`

#### 3.远程推送流程

推送过程

1.我的设备发送设备的UDID和Bundle Identifier给苹果APNs服务器。

2.经苹果加密后返回一个device token。

3.例如 QQMine发送当前用户的deviceToken和用户的标志\(比如id或者qq \)到QQ服务器

4.QQ服务器存入数据库。

5.QQYour发送消息给QQ服务器。

6.去数据库查询QQMine的device token 返回device token。

7.QQ服务器发送deviceToken和推送内容给APNs服务器。

8.通过devicetoken找到QQMine现在的设备进行推送。

#### 4.PCH相关知识点

pch文件是一个标准的预编译头文件\( Pre-Compiled Header\). 在Xcode6之前的版本中，系统模板会在Supporting Files文件夹自动创建。但在Xcode6之后的版本中取消了这一文件，如果我们需要使用pch文件，则需要手动创建。

注意事项

1.当Precompile Prefix Header为YES，那么pch会被预编译，预编译后的pch文件会被缓存起来，从而提高编译速度。当Precompile Prefix Header为NO时，那么pch不会被预编译，而是在每一个用到它导入的框架类库的.m文件中编译一次。

2.$\(SRCROOT\)代表你工程根目录，无需更改。后面分别是你的项目名称以及你的pch文件名。$\(SRCROOT\)/MMD/MMDPrefixHeader.pch

#### 5.沙盒文件操作

1.把 图片 写入 沙盒

`-(void)photoFile {`

`    NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask, YES);`

`    NSString *uniquePath=[[paths objectAtIndex:0] stringByAppendingPathComponent:@"pin"];`

`    BOOL blHave=[[NSFileManager defaultManager] fileExistsAtPath:uniquePath];`

`    if (blHave) {`

`        NSLog(@"already have");`

`        return ;`

`    }`

`    NSString *strPathOld = [[NSBundle mainBundle] pathForResource:@"pin" ofType:@"png"];`

`    NSData *data = [NSData dataWithContentsOfFile:strPathOld];`

`    BOOL result = [data writeToFile:uniquePath atomically:YES];`

`    if (result) {`

`        NSLog(@"success");`

`    }else {`

`        NSLog(@"no success");`

`    }  `

`}`

2.删除沙盒里的文件

-\(void\)deleteFile {

  NSFileManager\* fileManager=\[NSFileManager defaultManager\];

  NSArray \*paths = NSSearchPathForDirectoriesInDomains\(NSDocumentDirectory,NSUserDomainMask, YES\);

  NSString \*uniquePath=\[\[paths objectAtIndex:0\] stringByAppendingPathComponent:@"pin.png"\];

  BOOL blHave=\[\[NSFileManager defaultManager\] fileExistsAtPath:uniquePath\];

  if \(!blHave\) {

       NSLog\(@"no  have"\);

      return ;

  }else {

      NSLog\(@" have"\);

      BOOL blDele= \[fileManager removeItemAtPath:uniquePath error:nil\];

      if \(blDele\) {

          NSLog\(@"dele success"\);

      }else {

          NSLog\(@"dele fail"\);}

 }}

#### 6.iOS App沙盒中的目录

App Bundle ,如xxx.app 其实是一个目录，里面有app本身的二进制数据以及资源文件

Documents, 存放程序产生的文档数据

Library , 下面默认包含下面两个目录 Caches Preferences

tmp, 临时文件目录

###### 获取沙盒路径

NSArray \*NSSearchPathForDirectoriesInDomains\(NSSearchPathDirectory directory, NSSearchPathDomainMask domainMask, BOOL expandTilde\); 

directory 目录类型 比如Documents目录 就是NSDocumentDirectory 

domainMask 在iOS的程序中这个取NSUserDomainMask 

expandTilde YES，表示将~展开成完整路径

#### 7.数据加密相关

1.本地数据加密

对NSUserDefaults，sqlite存储文件数据加密，保护帐号和关键信息。

2.URL编码加密

对程序中出现的URL进行编码加密，防止URL被静态分析

4.网络传输数据加密

对客户端传输数据提供加密方案，有效防止通过网络接口的拦截获取

5.方法体，方法名高级混淆

对应用程序的方法名和方法体进行混淆，保证源码被逆向后无法解析代码

6.程序结构混排加密

对应用程序逻辑结构进行打乱混排，保证源码可读性降到最低

#### 8.自定义UITableViewCell（registerNib: 与 registerClass: 的区别）

自定义UITableViewCell大致有两类方法：

1.使用nib 

1、xib中指定cell的Class为自定义cell类型（注意不是设置File's Owner的class）

2、调用 tableView 的 registerNib:forCellReuseIdentifier:方法向数据源注册cell

\[\_tableView registerNib :\[UINibnibWithNibName:@"xxxxxCell"  bundle:nil\] forCellReuseIdentifier :kCellIdentify\];

3、在cellForRowAtIndexPath中使用dequeueReuseableCellWithIdentifier:forIndexPath:获取重用的cell，若无重用的cell，将自动使用所提供的nib文件创建cell并返回（若使用旧式dequeueReuseableCellWithIdentifier:方法需要判断返回是否为空而进行新建

xxxxxCell \*cell = \[tableView dequeueReusableCellWithIdentifier :kCellIdentify forIndexPath: indexPath\];  

4、获取cell时若无可重用cell，将创建新的cell并调用其中的awakeFromNib方法，可通过重写这个方法添加更多页面内容

2.不使用nib 

1、重写自定义cell的initWithStyle:withReuseableCellIdentifier:方法进行布局 

2、为tableView注册cell，使用registerClass:forCellReuseIdentifier:方法注册（注意是Class） 

\[\_tableView registerClass :\[xxxxxCell class\] forCellReuseIdentifier :kCellIdentify\]; 

3、在cellForRowAtIndexPath中使用dequeueReuseableCellWithIdentifier:forIndexPath:获取重用的cell，若无重用的cell，将自动使用所提供的class类创建cell并返回 

xxxxxCell \*cell = \[tableView dequeueReusableCellWithIdentifier:kCellIdentify forIndexPath :indexPath\];  

4、获取cell时若无可重用cell，将调用cell中的initWithStyle:withReuseableCellIdentifier:方法创建新的cell

另外要注意的：

1、dequeueReuseableCellWithIdentifier:与dequeueReuseableCellWithIdentifier:forIndexPath:的区别：

前者不必向tableView注册cell的Identifier，但需要判断获取的cell是否为nil；

后者则必须向table注册cell，可省略判断获取的cell是否为空，因为无可复用cell时runtime将使用注册时提供的资源去新建一个cell并返回

2、自定义cell时，记得将其他内容加到self.contentView 上，而不是直接添加到 cell 本身上

总结：

1.自定义cell时，

若使用nib，使用 registerNib: 注册，dequeue时会调用 cell 的 -\(void\)awakeFromNib

不使用nib，使用 registerClass: 注册, dequeue时会调用 cell 的 - \(id\)initWithStyle:withReuseableCellIdentifier:

2.需不需要注册？

使用dequeueReuseableCellWithIdentifier:可不注册，但是必须对获取回来的cell进行判断是否为空，若空则手动创建新的cell；

使用dequeueReuseableCellWithIdentifier:forIndexPath:必须注册，但返回的cell可省略空值判断的步骤。





