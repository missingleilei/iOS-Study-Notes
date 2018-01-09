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



