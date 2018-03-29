# 场景

block和delegate是iOS开发者经常用到的技术，也常常出现在各种面试题里，你经常听到他们之间的对比。

我的态度是每个成熟的技术并没有明显的优劣，不应该用谁好谁劣来评判他们，而应该看谁更适合应用场景，在合适的场合选择合适的技术。

本篇文章将讨论在 **网络层调用和回调** 这个场景下的技术选择。

Block回调

一个常见的Block回调，通常是业务代码调用请求，然后在回调中获得返回的数据，然后执行业务逻辑，如下：

```
// 业务层代码
- (void)blockDemo {
	[self.service requestWithParms:nil WithResult:^(id data, NSError *error) {
	    // 处理业务逻辑代码
	    // ...
		NSLog(@
"Result from block:%@"
, data);
	}];
}

```

## 考虑点1：内存

之所以会考虑这个问题，是因为用户使用时常常会刚进入一个页面，就立刻点返回，此时网络请求刚发出去，数据返回有可能还没回来，那么这个网络请求会怎么样呢？

当前页面controller能够被pop出栈，释放掉吗？毕竟网络请求还没结束

我们一一来验证

### controller销毁

如何验证内存释放已经释放，很简单，首先我写的网络请求并不是真的网络请求，他只会延时5s返回一个假的数据，用来方便模拟网络请求

```
- (void)requestWithParms:(NSDictionary *)parms WithResult:(ResultBlock)result {
	
	int delay = NET_DELAY; // 默认是5s，可以传参数改变
	
	
if
 ([parms valueForKey:@
"delayTime"
] != nil) {
		delay = (int)[parms valueForKey:@
"delayTime"
];
	}
	
	dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delay * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
		NSString *resultData = @
"This is a Mock Data!!"
;
		NSLog(@
"Network Finish:%@"
,resultData);
		
if
 (result) {
			result(resultData, nil);
		}
	});
}

```

再在viewcontroller的`dealloc`方法里打印log，就能知道`dealloc`是否被调用，如果调用说明可以释放

```
- (void)dealloc {
	NSLog(@
"NextPageViewController has been dealloc!"
)
}

```

这样验证起来就是很简单，只需要执行`blockDemo`方法，然后立刻点返回，退出当前vc，等5秒后如果看结果

结果是vc可以释放的

```
2018-03-25 19:36:07.345523+0800 NetworkCallback[4580:3600834] NextPageViewController has been dealloc!

```

### Block捕获外界变量销毁

我们再进一步想想，Block有个最大的特点是可以访问当前的作用域，我们随便创建一个数组，重复上面操作，是否能够销毁

```
- (void)blockDemo {
	NSArray *outsideArray = @[@1, @2, @3];
	
	[self.service requestWithParms:nil WithResult:^(id data, NSError *error) {
		// 处理业务逻辑
		// ...
		
		NSLog(@
"Result from block:%@"
, data);
		NSLog(@
"outsideArray :%@"
, outsideArray);
	}];
}

```

打印结果：

```
2018-03-25 19:55:40.997535+0800 NetworkCallback[4970:3641450] NextPageViewController has been dealloc!
2018-03-25 19:55:44.831721+0800 NetworkCallback[4970:3641450] outsideArray :(
    1,
    2,
    3
)

```

神奇不？！

注意，vc先销毁了，但是5s后，这个临时变量竟然还没有销毁。那么这个变量存储在哪里呢？留个悬念

考虑一下，如果你在Block代码里访问了一个超大的文件，这个文件必然是保存内存的，然后此时你遇上了网络慢，接口好久没有返回，那么这个超大的文件就会一直占用内存

继续，如果我在Block中访问self呢？此时的self就是当前的controller，这时候可以销毁吗？

```
- (void)blockDemo {
	NSArray *outsideArray = @[@1, @2, @3];
	[self.service requestWithParms:nil WithResult:^(id data, NSError *error) {
		// 处理业务逻辑
		// ...
		
		NSLog(@
"Result from block:%@"
, data);
		NSLog(@
"outsideArray :%@"
, outsideArray);
		NSLog(@
"self :%@"
, self);
	}];
}

```

打印结果：

```
2018-03-25 20:04:21.012135+0800 NetworkCallback[5224:3659292] Network Finish:This is a Mock Data!!
2018-03-25 20:04:21.012476+0800 NetworkCallback[5224:3659292] Result from block:This is a Mock Data!!
2018-03-25 20:04:21.013186+0800 NetworkCallback[5224:3659292] outsideArray :(
    1,
    2,
    3
)
2018-03-25 20:04:21.013675+0800 NetworkCallback[5224:3659292] self :
<
NextPageViewController: 0x7f993662cf20
>

2018-03-25 20:04:21.013858+0800 NetworkCallback[5224:3659292] NextPageViewController has been dealloc!

```

看到了吗？dealloc是最后打印出来的，也就是说Block不返回，controller就释放不了了！

看起来是在Block内访问谁，谁就无法释放啊！

有人会想这是不是就是循环引用呢？

请大家回忆一下：Block循环引用是self强引用Block，Block里面再强引用self，这里Block确实强引用了self，但是self并没有强引用Block，这个Block是一个参数传给了NetService，跟self并无关系

而且是循环引用的话，那么vc会一直释放不掉，但看上面的log，其实是可以释放的，只是释放的时机被延后了

但是，无论如何，我们试试换成 `weakself` 会怎么样呢？ 打印结果：

```
2018-03-25 20:07:10.944557+0800 NetworkCallback[5294:3665537] NextPageViewController has been dealloc!
2018-03-25 20:07:15.228961+0800 NetworkCallback[5294:3665537] Network Finish:This is a Mock Data!!
2018-03-25 20:07:15.230836+0800 NetworkCallback[5294:3665537] Result from block:This is a Mock Data!!
2018-03-25 20:07:15.231074+0800 NetworkCallback[5294:3665537] outsideArray :(
    1,
    2,
    3
)
2018-03-25 20:07:15.231190+0800 NetworkCallback[5294:3665537] weakSelf :(null)

```

没问题，果然换成weakself就解决了

### 原因是什么？

为什么在Block内访问谁，谁就无法释放呢？为什么用weakself就解决了呢？

#### Block的本质是个对象

Block看起来像一个函数，其实在objectice-c中，它是个对象，之所以Block可以捕获外部变量，正是因为它是个对象，他有自己的属性，他用属性强引用了外部变量，导致外部变量（就是上面的self和outsideArray）的引用计数不为0，也就不能释放了

#### weakself做了什么

当Block中访问weakself的时候，强引用并没有指向self，而是指向weakself，所以self可以被释放

### 内存小结

1. 使用Block无论是否有循环引用的可能，都要使用
   `weakself`
   ，来防止vc被持有，而延迟释放
2. Block会导致对象的生命周期被延长，特别是当某些大文件被Block访问时，有几率导致内存不足

## 考虑点2：代码安全

这是基于上面的考虑，我们已经知道要用`weakself`来保证controller被及时释放，也可以在上面log中看到weakself变成了nil，此时有可能导致crash，因为我们正在操作一个nil对象 想象一个业务场景：分页请求，你拉取了前面几页，比如page=3，然后去拉下一页数据，此时网络请求尚未返回，用户就退出当前页面，此时

1. 如果页面能够被释放，那么Block中的业务逻辑代码被执行吗?
2. 如果可以执行会有什么危险？

其实第一个问题上面的log已经回答了，log之所以被打印出来，其实就是Block中的代码被执行了嘛。

也就是说即使controller已经销毁，Block中的代码还是会被执行

第二个问题，执行了会有什么危险

1. 通常这里会做json转model，会做某些数据转换，如果返回数据很大，比如是个三千多个元素的数组，那么势必浪费CPU去执行，注意，此时controller已经销毁了，执行代码是无意义的，这里的CPU是确确实实的浪费掉了。
2. 想像一下，此时weakself是nil，如果是分页请求的数据，你通常是把新的数据加到某个数组里，然后你就crash了，因为你把nil加到数组去了

```
- (void)blockDemo {
	__weak typeof(self) weakSelf = self;
	NSArray *outsideArray = @[@1, @2, @3];
	[self.service requestWithParms:nil WithResult:^(id data, NSError *error) {
		// 处理业务逻辑
		// ...
		[data addObject:weakSelf.pageArray]; // weakSelf是nil
	}];
}

```

因此，你必须小心翼翼，写上的保护代码

```
- (void)blockDemo {
	__weak typeof(self) weakSelf = self;
	NSArray *outsideArray = @[@1, @2, @3];
	[self.service requestWithParms:nil WithResult:^(id data, NSError *error) {
		
		// 保护代码
		
if
 (weakSelf == nil) {
			
return
;
		}
		
		// 处理业务逻辑
		// ...
	}];
}

```

### 代码安全总结

1. Block会有执行无意义代码的可能，浪费CPU
2. Block会有操作nil对象导致crash的可能，因此要写保护代码

# Delegate回调

经过上面的验证，看起来好像Block有挺多麻烦了，那么delegate怎么样呢？我们也来试一试

首先是模拟网络请求，然后通过delegate回调

```
- (void)requestWithParms:(NSDictionary *)parms {
	int delay = NET_DELAY;
	
	
if
 ([parms valueForKey:@
"delayTime"
] != nil) {
		delay = (int)[parms valueForKey:@
"delayTime"
];
	}
	
	dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delay * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
		
		// 判断成功
		// 判断失败
		
		NSString *resultData = @
"This is a Mock Data!!"
;
		NSLog(@
"Network Finish:%@"
,resultData);
		
		// ***这里加了判断***
		
if
 (self.delegate 
&
&
 [self.delegate respondsToSelector:@selector(networkFinishWithSuccess:AndError:)]) {
			[self.delegate networkFinishWithSuccess:resultData AndError:nil];
		}
	});
}

```

注意我用\*\*\*标注的注释，这里有个delegate的判断，这里保证了Block考虑点2不会出现，因为当delegate为nil的时候，绝对不会执行delegate的方法.

演示一下，代码如下

```
#pragma mark - Delegate request

- (void)delegateDemo {
	self.service.delegate = self;
	[self.service requestWithParms:nil];
}


#pragma mark - NetWorkDelegate

- (void)networkFinishWithSuccess:(id)data AndError:(NSError *)error {
	NSLog(@
"Result from Delegate:%@"
, data);
}

```

## 验证block存在的问题

同样的，一进入Nextpage就立刻点返回，等5s看看代码会不会执行

```
2018-03-25 21:08:23.422103+0800 NetworkCallback[6399:3784145] NextPageViewController has been dealloc!

```

只有一条log，说明内存释放没有问题，而且在回调前对delegate的判断，使得我们非常方便的得知业务层是否还存在了，而如果用Block来实现就很麻烦，在网络回调前是无法得知的，一定要在Block里面加判断代码

总结：

1. 在业务层delegate比Block更加优雅，可以在网络层回调前就中断逻辑，把错误发生的可能提前中断，而不必进入业务层才做判断，这是一个很好的隔断
2. 没有延长某个对象生命周期，代码更加清晰，易于管理

## delegate自己的问题

那么难道delegate就没有缺点了吗？

### 多个业务层请求

之前的demo中只有一个业务层，工程中绝对不会只有一个，而NetService的delegate只能指向一个对象，岂不是只有一个请求能够拿到回调，这岂不是滑天下之大稽？

当然不能这样，如果使用delegate，就必须对每个请求封装成一个对象，而不能统一的用一个NetService

```
@interface RequestAPI : NSObject

@property (nonatomic, weak, nullable) id
<
NetWorkDelegate
>
 delegate;

/**
 模拟Delegate请求方法
 
 @param parms 请求参数
 */
- (void)requestWithParms:(NSDictionary *)parms;

```

可是每个对象都去实现一篇请求逻辑岂不是很傻？！

所以底层还是调用NetService

```
#import "RequestAPI.h"


@implementation RequestAPI

- (void)requestWithParms:(NSDictionary *)parms {
	__weak typeof(self) weakSelf = self;
	[[NetService alloc] requestWithParms:parms WithResult:^(id  _Nonnull data, NSError * _Nonnull error) {
		
if
 (weakSelf 
&
&
 [weakSelf respondsToSelector:@selector(networkFinishWithSuccess:AndError:)]) {
			[self.delegate networkFinishWithSuccess:resultData AndError:nil];
		}
	}];
}

```

### 总结

所以其实，使用delegate和Block的结合使用，由此我们可以看出

1. Block适合做集约型调用，每个业务逻辑不一样，但是我们可以通过把代码封装在Block中，然后发给统一的方法来处理，实现了统一方法处理不同的逻辑
2. delegate适合离散型调用，每次返回是同样的逻辑
3. 网络层调用要delegate和Block结合使用，在业务层回调适合delegate，在底层网络处理适合Block



