##### 1.SourceTree问题记录

`[user]`

`name = ioszhangyang`

`email = zhangyang@mimidai.com`

##### 2.手机分辨率

| 设备 |
| :--- |


|  | 屏幕尺寸 | 分辨率（pt） | Reader | 分辨率（px） | 渲染后 | PPI |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| iPhone 3GS | 3.5吋 | 320x480 | @1x | 320x480 |  | 163 |
| iPhone 4/4s | 3.5吋 | 320x480 | @2x | 640x960 |  | 330 |
| iPhone 5/5s/5c | 4.0吋 | 320x568 | @2x | 640x1136 |  | 326 |
| iPhone 6 | 4.7吋 | 375x667 | @2x | 750x1334 |  | 326 |
| iPhone 6Plus | 5.5吋 | 414x736 | @3x | 1242x2208 | 1080x1920 | 401 |
| iPhone 6s | 4.7吋 | 375x667 | @2x | 750x1334 |  | 326 |
| iPhone 6sPlus | 5.5吋 | 414x736 | @3x | 1242x2208 | 1080x1920 | 401 |
| iPhone 7 | 4.7吋 | 375x667 | @2x | 750x1334 |  | 326 |
| iPhone 7Plus | 5.5吋 | 414x736 | @3x | 1242x2208 | 1080x1920 | 401 |

##### 3.获取控件XY

CGRectGetMaxY\(View\)

**CGRectGetMinX方法的作用得到目前view在当前屏幕中相对于整个View的最小值（位于屏幕的最左边）**

**CGRectGetMaxX方法的作用得到目前view在当前屏幕中相对于整个View的最大值（位于屏幕的最右边）**

**CGRectGetMinY方法的作用得到目前view在当前屏幕中相对于整个View的最小值（位于屏幕的最上边）**

**CGRectGetMaxY方法的作用得到目前view在当前屏幕中相对于整个View的最大值（位于屏幕的最下边）**

##### 4.导航栏、TableView常见问题相关

###### 1.调整contentInset

`//NO：不调整，按设定的frame、contentInset的显示`

`//YES：会调整contentInset.top的高，让显示的顶在导航栏下面，【有滑过半透明效果】`

`self.automaticallyAdjustsScrollViewInsets =NO;`

###### 2.调整frame

`//UIRectEdgeNone//会顶在导航栏下面【没有滑过半透明效果】`

`//UIRectEdgeTop//对齐原点`

`//UIRectEdgeLeft//对齐左边`

`//UIRectEdgeBottom//对齐顶部`

`//UIRectEdgeRight//对齐右边`

`//UIRectEdgeAll//对齐所有`

`self.edgesForExtendedLayout = UIRectEdgeNone;`

属性edgesForExtendedLayout，意思大概是边缘向四周展开

edgesForExtendedLayout 值是结构体，默认值是 UIRectEdgeAll，

也就是说，当一个控制器要往父控制器添加的时候，上下左右填充满整个屏幕。

3.导航栏半透明

`self.navigationController.navigationBar.translucent = YES;`

###### 4.隐藏navigationBar\(1、它推过的所有的VC共用1个Bar；2、用继承View的hidden属性，隐藏不了！\)

###### `self.navigationController.navigationBarHidden=YES;`

###### 5.关于iOS11适配

```
1.关于检测是否开启定位的判断.在iOS11之前,一直都是使用kCLAuthorizationStatusAuthorizedAlways来进行检测是否开启定位服务。
if ([CLLocationManager authorizationStatus] != kCLAuthorizationStatusAuthorizedAlways)但是到了IOS11  苹果禁止了always
这个枚举值,但是并没有废弃,这就尴尬了,所以换了另一个枚举值if ([CLLocationManager authorizationStatus] != 
kCLAuthorizationStatusAuthorizedWhenInUse),这样就可以检测了.
```

ScrollView新增安全区域。

* 如果之前让TabelView顶住屏幕，然后设置顶部内边距 = 20+44，刚好在导航栏下面的话，会被系统向下偏移64的 SafeAreaInsets，再加上自己设置的64，就出现下移64问题。

* 同理，没导航栏的时候，也会下移20 -&gt; 状态栏的高度。

* 以前若设置 automaticallyAdjustsScrollViewInsets  = YES 让系统自动调整，不会有问题

  解决方案：添加下面，相当于 automaticallyAdjustsScrollViewInsets = NO

`if (@available(iOS 11.0, *)) {`

`_PersonalTableView.contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever;//UIScrollView也适用`

`}else {`

`self.automaticallyAdjustsScrollViewInsets =NO;`

`只对滚动视图有效。去除tableview的视图自动往下偏移。`

`}`

5.TableView分割线满屏显示

`-(void)viewDidLayoutSubviews{`

`if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {`

`[self.tableView setSeparatorInset:UIEdgeInsetsMake(0,0,0,0)]`

`}`

`if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {`

`[self.tableView setLayoutMargins:UIEdgeInsetsMake(0,0,0,0)];`

`}}`

`-(void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath{`

`if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {`

`[cell setSeparatorInset:UIEdgeInsetsZero];`

`}`

`if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {`

`[cell setLayoutMargins:UIEdgeInsetsZero];`

`}}`

##### 5.NSUserDefaults数据保存报错：Attempt to set a non-property-list object...

这种错误的原因是插入了不识别的数据类型，NSUserDefaults支持的数据类型有

NSString、 NSNumber、NSDate、 NSArray、NSDictionary

、BOOL、NSInteger、NSFloat等系统定义的数据类型。

自定义的类型需要转成NSData再存入。

我原来认为NSUserDefaults存不了数组，又换成了字典，但是还是报错，最后求助网友大神解决了，是我里边的数据结构有"&lt;null&gt;"，而NSUserDefaults是不能被成功解析并存入的，所有在存入之前需要将里边的"&lt;null&gt;"改成""即可。

##### 6.AlertView弹出问题？MMDAlertViewController

当一个提示框需要弹出另一个提示框时需要延时否则可能无法正确弹出。

