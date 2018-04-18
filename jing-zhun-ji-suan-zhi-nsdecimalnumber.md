## NSDecimalNumber简介

顾名思义这是一个十进制数字类，继承自NSNumber，苹果针对浮点类型计算精度问题提供出来的计算类，基于十进制的科学计数法来计算，同时可以指定舍入模式，一般用于货币计算。

##### 初始化方法

```
/**
 创建并返回一个十进制对象
 @param dcm 十进制的结构体
 */
- (instancetype)initWithDecimal:(NSDecimal)dcm;

/**
 @param mantissa 十进制尾数 （如：123）
 @param exponent 指数      （如：3）
 @param flag 是否为负      （如：YES）
 return 十进制数字对象       (如：-123000)
 */
- (instancetype)initWithMantissa:(unsigned long long)mantissa exponent:(short)exponent isNegative:(BOOL)flag;

/**
 @param numberValue 数字字符串
 */
- (instancetype)initWithString:(nullable NSString*)numberValue;

/**
 @param numberValue 数字字符串,可以根据locale来解析 (如:@"123,456")
 @param locale 解析<numberValue>的字典(特别是NSLocaleDecimalSeparator:分割整数跟小数)
                                                (如 @{NSLocaleDecimalSeparator:@","})
 @return 十进制数字对象                           (如：123.456)
 */
- (instancetype)initWithString:(nullable NSString*)numberValue locale:(nullable id)locale;

```

#### NSDecimalNumberHandler

这是一个NSDecimalNumber的公共协议处理类，可以设置舍入模式以及计算错误的处理；配合NSDecimalNumber来使用，将这个类的实例当做NSDecimalNumber相应API的参数来控制数字处理的结果。

```
/**
 初始化方法

 @param roundingMode 舍入方式
 @param scale 小数点后舍入值的位数。
 @param exact 精度错误处理；YES:如果出现错误，将引发异常，NO:忽略错误并将控制权放回给调用者。
 @param overflow 溢出错误处理；YES:如果出现错误，将引发异常，NO:忽略错误并将控制权放回给调用者。
 @param underflow 下溢错误处理；YES:如果出现错误，将引发异常，NO:忽略错误并将控制权放回给调用者。
 @param divideByZero 除以0的错误处理；YES:如果出现错误，将引发异常，NO:忽略错误并将控制权放回给调用者。
 @return NSDecimalNumberHandler对象
 */

+ (instancetype)decimalNumberHandlerWithRoundingMode:(NSRoundingMode)roundingMode 
                                        scale:(short)scale
                                        raiseOnExactness:(BOOL)exact
                                        raiseOnOverflow:(BOOL)overflow
                                        raiseOnUnderflow:(BOOL)underflow
                                        raiseOnDivideByZero:(BOOL)divideByZero


/**
 快速构造方法
 roundingMode:NSRoundPlain
 scale:全精度
 exact:NO
 overflow:NO
 underflow:NO
 divideByZero:NO
 */
+ (NSDecimalNumberHandler*)defaultDecimalNumberHandler;
```

**NSRoundingMode的几种方式：**

* NSRoundPlain:四舍五入
* NSRoundDown:只舍不入
* NSRoundUp：只入不舍
* NSRoundBankers: 在四舍五入的基础上加了一个判断：当最后一位为5的时候，只会舍入成偶数。比如：1.25不会返回1.3而是1.2，因为1.3不是偶数。

##### 加法运算

```
/**
 @param decimalNumber 相加的十进制对象
 */

- (NSDecimalNumber*)decimalNumberByAdding:(NSDecimalNumber*)decimalNumber;

/**
 @param decimalNumber 相加的十进制对象
 @param behavior NSDecimalNumberHandler对象
 */

- (NSDecimalNumber*)decimalNumberByAdding:(NSDecimalNumber*)decimalNumber withBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior

```

##### 减法运算

```
/**
 @param decimalNumber 减去的十进制对象
 */

- (
NSDecimalNumber*)decimalNumberBySubtracting:(NSDecimalNumber*)decimalNumber;

/**
 @param decimalNumber 减去的十进制对象
 @param behavior NSDecimalNumberHandler对象
 */

- (NSDecimalNumber*)decimalNumberBySubtracting:(NSDecimalNumber*)decimalNumber withBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior;

```

##### 乘法运算

```
/**
 @param decimalNumber 相乘的十进制对象
 */

- (
NSDecimalNumber*)decimalNumberByMultiplyingBy:(NSDecimalNumber*)decimalNumber;

/**
 @param decimalNumber 相乘的十进制对象
 @param behavior NSDecimalNumberHandler对象
 */

- (NSDecimalNumber*)decimalNumberByMultiplyingBy:(NSDecimalNumber*)decimalNumber withBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior;

```

##### 除法运算

```
/**
 @param decimalNumber 除以的十进制对象
 */

- (
NSDecimalNumber*)decimalNumberByDividingBy:(NSDecimalNumber*)decimalNumber;


/**
 @param decimalNumber  除以的十进制对象
 @param behavior NSDecimalNumberHandler对象
 */

- (NSDecimalNumber*)decimalNumberByDividingBy:(NSDecimalNumber*)decimalNumber withBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior;

```

##### 次方运算

```
/**
 @param power 指数
 return 调用者的power次方
 */

- (NSDecimalNumber*)decimalNumberByRaisingToPower:(NSUInteger)power;


/**
 @param power 指数
 @param behavior NSDecimalNumberHandler对象
 return 调用者的power次方
 */
- (NSDecimalNumber*)decimalNumberByRaisingToPower:(NSUInteger)power withBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior;

```

##### 10为底的乘方运算

```
/**
 调用者乘以10的power次方
 @param power 10的指数
 */
 
- (NSDecimalNumber*)decimalNumberByMultiplyingByPowerOf10:(short)power;


/**
 调用者乘以10的power次方
 @param power 10的指数
 @param behavior NSDecimalNumberHandler对象
 */

- (NSDecimalNumber*)decimalNumberByMultiplyingByPowerOf10:(short)power withBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior;

```

##### 舍入运算

```
/**
 @param decimalNumber NSDecimalNumberHandler对象
 */
- (NSDecimalNumber*)decimalNumberByRoundingAccordingToBehavior:(nullable id<NSDecimalNumberBehaviors>)behavior;

```



