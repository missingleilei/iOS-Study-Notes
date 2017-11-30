#### 1.tableView局部刷新

NSIndexPath \*path = \[NSIndexPath indexPathForRow:row inSection:0\];

\[self.tableView reloadRowsAtIndexPaths:@\[path\] withRowAnimation:UITableViewRowAnimationBottom\];

#### 2.静态库中含有分类

1.在该静态库的Taraget-&gt;Build Settings-&gt;Other Linker Flags-&gt;加上 -ObjC.

2.在使用该静态库的工程Taraget-&gt;Build Settings-&gt;Other Linker Flags-&gt;加上-all\_load或-force\_load.

#### 3.&lt;libxml/tree.h&gt;file not found

在 Build Settings -&gt;Header Search Paths里设置正确就没问题了,添加路径

${SDKROOT}/usr/include/libxml2

#### 4.判断邮箱格式是否正确的代码

` -( BOOL )isValidateEmail:( NSString  *)email  {  `

`    NSString  *emailRegex =  @"[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,4}" ;  `

`    NSPredicate  *emailTest = [ NSPredicate   predicateWithFormat : @"SELF MATCHES%@" ,emailRegex];  `

`    return  [emailTest  evaluateWithObject :email];  `

`}` 

#### 5.截取屏屏幕图片

`// 创建一个基于位 图的图形上下文并指定大小为CGSizeMake(200,400) `

`UIGraphicsBeginImageContext(CGSizeMake(200,400));  `

//renderInContext  呈现接受者及其子范围到 指定的上下文 

`[self.view.layer renderInContext:UIGraphicsGetCurrentContext()]; `

`// 返回 一个基于当前图形上下文的图片 `

`UIImage *aImage = UIGraphicsGetImageFromCurrentImageContext(); `

`// 移除栈顶 的基于当前位图的图形上下文 `

`UIGraphicsEndImageContext(); `

`// 以 png 格式 返回指定图片的数据 `

`imageData = UIImagePNGR epresentation(aImage); `

#### 6.IOS中一段文字设置多种字体颜色代码

`[self fuwenbenLabel:contentLabel FontNumber:[UIFont systemFontOfSize:15] AndRange:NSMakeRange(6, 1) AndColor:RGBACOLOR(34, 150, 253, 1)];`

`-(void)fuwenbenLabel:(UILabel *)label FontNumber:(id)font AndRange:(NSRange)range AndColor:(UIColor *)vaColor{`

`　　NSMutableAttributedString *str = [[NSMutableAttributedString alloc] initWithString:labell.text];`

`　　[str addAttribute:NSFontAttributeName value:font range:range];`

`　　[str addAttribute:NSForegroundColorAttributeName value:vaColor range:range];`

`　　label.attributedText = str;`

`}`

#### 7.日期比较

`+(int)compareOneDay:(NSDate *)oneDay withAnotherDay:(NSDate *)anotherDay{`

`    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];`

`    [dateFormatter setDateFormat:@"dd-MM-yyyy"];`

`    NSString *oneDayStr = [dateFormatter stringFromDate:oneDay];`

`    NSString *anotherDayStr = [dateFormatter stringFromDate:anotherDay];`

`    NSDate *dateA = [dateFormatter dateFromString:oneDayStr];`

`    NSDate *dateB = [dateFormatter dateFromString:anotherDayStr];`

`    NSComparisonResult result = [dateA compare:dateB];`

`    NSLog(@"date1 : %@, date2 : %@", oneDay, anotherDay);`

`    if (result == NSOrderedDescending) {`

`        //NSLog(@"Date1  is in the future");`

`        return 1； }`

`    else if (result == NSOrderedAscending){`

`        //NSLog(@"Date1 is in the past");`

`        return -1;`

`    }`

`    //NSLog(@"Both dates are the same");`

`    return 0;`

`}`



