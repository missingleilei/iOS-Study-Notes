```
/**
* 判断设备是否越狱，依据是否存在apt和Cydia.app
 */
 + (BOOL)isJailbroken;
```

#### https://www.jianshu.com/p/8e5bf711f9f0 简书IPA详解

#### 一、支付宝支付流程

![](/assets/2122663-a1112187db75bb54.png)

```
第一步：我们的 APP 发起一笔支付交易，此时，第一件事，我们要去我们自己的服务器上创建一个订单信息。同时服务器会组装好一笔交易交给我们。
关于组装交易信息，有两种做法，第一种就是支付宝推荐我们做的，由我们服务器来组装交易信息，服务器加密交易信息，并保存签名信息；
另一种做法是，服务器返回商品信息给 APP，由 APP 来组装交易信息，并进行加密处理等操作。显然我们应该采用第一种方式。
第二步：服务器创建好交易信息以后，返回给 APP，APP 不对交易信息做处理。
第三步：APP 拿到交易信息，开始调起支付宝的 SDK，支付宝的 SDK 把交易信息传给支付宝的服务器。
第四步：验证通过以后，支付宝服务器会告诉支付宝 SDK 验证通过。
第五步：验证通过以后，我们的 APP 会调起支付宝 APP，跳转到支付宝 APP。
第六步：在支付宝 APP 里，用户输入密码进行交易，和支付宝服务器进行通讯。
第七步：支付成功，支付宝服务器回调支付宝 APP。
第八步：支付宝回到我们自己的 APP，并通过 - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
 方法处理支付宝的回调结果，对应的进行刷新 UI 等操作。
第九步：支付宝服务器会回调我们的服务器并把收据传给我们服务器，如果我们的服务器没有确认已经收到支付宝的收据信息，
那么支付宝服务器就会一直回调我们的服务器，只是回调时间间隔会越来越久。
第十步：我们的服务器收到支付宝的回调，并回调支付宝，确认已经收到收据信息，此时早餐买完了。
```

#### 二、IPA支付

```
第一步：用户开始购买，首先会去我们自己的服务器创建一个交易订单，返回给 APP。
第二步：APP 拿到交易信息，然后开始调起 IAP 服务创建订单，并把订单推入支付队列。
第三步：IAP 会和 IAP 服务器通讯，让用户确认购买，输入密码。
第四步：IAP 服务器回调 APP，通知购买成功，并把收据写入到 APP 沙盒中。
第五步：此时，APP 应该去获取沙盒中的收据信息（一段 Base 64 编码的数据），并将收据信息上传给服务器。
第六步：服务器拿到收据以后，就应该去 IAP 服务器查询这个收据对应的已付款的订单号。
第七步：我们自己的服务器拿到这个收据对应的已付款的订单号以后，就去校验当前的已付款订单中是否有要查询的那一笔，如果有，就告诉 APP。
第八步：APP 拿到查询结果，然后把这笔交易给 finish 掉。
```

#### 三、IPA支付代码

```
#import <StoreKit/StoreKit.h>
@interface BLPaymentManager ()<SKPaymentTransactionObserver, SKProductsRequestDelegate>
@end

@implementation BLPaymentManager

- (void)dealloc {
    [[SKPaymentQueue defaultQueue] removeTransactionObserver:self];
}
- (void)init {
    self = [super init];
    if(self) {
    [[SKPaymentQueue defaultQueue] addTransactionObserver:self];
    }
    return self;
}
- (void)buyProduction {
    if ([SKPaymentQueue canMakePayments]) {
    [self getProductInfo:nil];
    } else {
    NSLog(@"用户禁止应用内付费购买");
    }
}

#pragma mark 从Apple查询用户点击购买的产品的信息.
- (void)getProductInfo:(NSString *)productIdentifier {
    NSSet *identifiers = [NSSet setWithObject:productIdentifier];
    SKProductsRequest *request = [[SKProductsRequest alloc] initWithProductIdentifiers:identifiers];
    request.delegate = self;
   [request start];
}

#pragma mark - SKPaymentTransactionObserver
// 购买操作后的回调.
- (void)paymentQueue:(SKPaymentQueue *)queue updatedTransactions:(NSArray<SKPaymentTransaction *>*)transactions {
     // 这里的事务包含之前没有完成的.
    for (SKPaymentTransaction *transcation in transactions) {
      switch (transcation.transactionState) {
         case SKPaymentTransactionStatePurchasing:
              [self transcationPurchasing:transcation];
              break;
         case SKPaymentTransactionStatePurchased:
              [self transcationPurchased:transcation];
              break;
         case SKPaymentTransactionStateFailed:
              [self transcationFailed:transcation];
              break;
         case SKPaymentTransactionStateRestored:
              [self transcationRestored:transcation];
              break;
         case SKPaymentTransactionStateDeferred:
              [self transcationDeferred:transcation];
              break;
          }
     }
}
#pragma mark - TranscationState
// 交易中.
- (void)transcationPurchasing:(SKPaymentTransaction *)transcation {
    NSURL *receiptURL = [[NSBundle mainBundle] appStoreReceiptURL];
    NSData *receipt = [NSData dataWithContentsOfURL:receiptURL];
    if (!receipt) {
        NSLog(@"没有收据, 处理异常");
        return;
    }

    // 存储到本地先.
    // 发送到服务器, 等待验证结果.
    [[SKPaymentQueue defaultQueue] finishTransaction:transcation];
}
// 交易成功.
- (void)transcationPurchased:(SKPaymentTransaction *)transcation {

}

// 交易失败.
- (void)transcationFailed:(SKPaymentTransaction *)transcation {

}

// 已经购买过该商品.
- (void)transcationRestored:(SKPaymentTransaction *)transcation {

}

// 交易延期.
- (void)transcationDeferred:(SKPaymentTransaction *)transcation {

}
#pragma mark - SKProductsRequestDelegate
// 查询成功后的回调.
- (void)productsRequest:(SKProductsRequest *)request didReceiveResponse:(SKProductsResponse *)response {
     NSArray<SKProduct *>*products = response.products;
     if (!products.count) {
     NSLog(@"没有正在出售的商品");
     return;
     }
     SKPayment *payment = [SKPayment paymentWithProduct:products.firstObject];
     [[SKPaymentQueue defaultQueue] addPayment:payment];
     }
     @end
```

```
将公司服务器生成的订单号 orderNo绑定到苹果的交易paymentTransaction上。第一篇文章中说了，苹果的规范是用一个product生成一个payment
，然后将这个payment推入到paymentQueue之中，最后我们成为交易事务的监听者，在监听方法里拿到交易的paymentTransaction，我们放进去一个
苹果的payment实例，最后得到的是一个paymentTransaction。
```





