#### 1.NSMutableDictionary的遍历方式三种

##### 方法一：使用for-each循环遍历字典中的对象

`for (NSString *s in [dictionary allValues]) {`

`NSLog(@"value: %@", s);`

`}`

##### 方法二：NSDictionary函数allValues会返回以数组而非字典形式组织的对象。函数allKeys会将键值作为数组返回：

`for (NSString *s in [dictionary allKeys]) {`

`    NSLog(@"key: %@", s);`

`}`

##### 方法三：通过enumerateKeysAndObjectsUsingBlock:方法针对字典中的每个对象执行代码。可以用来定义代码块，然后应用到字典中的每个对象，同时又不必创建for-each循环或是获得数组版本的字典引用：

`[dictionary enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {`

`    NSLog(@"key = %@ and obj = %@", key, obj);`

`}];`

#### 2.结构体如何存入数组？

需要将CGRect放入NSArray中

`CGRect  rect = CGMakeRect(1, 2, 100, 200);`

`NSValue  *rectValue = [NSValue valueWithBytes:&rect objCType:@encode(CGRect)];`

`[array  addObject:rectValue];`

使用getValue提取数值 传递参数为要存储这个数值的变量的地址

`NSValue *rectValue = [array objectAtIndex: 0];`

`CGRect  rect = [rectValue getValue:&rect];`



