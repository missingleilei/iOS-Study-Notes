#### iOS UIWebView内存泄露问题

`- (void)webViewDidFinishLoad:(UIWebView *)webView{`

`[[NSUserDefaults standardUserDefaults] setInteger:0 forKey:@"WebKitCacheModelPreferenceKey"];`

`[[NSUserDefaults standardUserDefaults] setBool:NO forKey:@"WebKitDiskImageCacheEnabled"];`

`[[NSUserDefaults standardUserDefaults] setBool:NO forKey:@"WebKitOfflineWebApplicationCacheEnabled"];`

`[[NSUserDefaults standardUserDefaults] synchronize];`

`}`

`- (void)dealloc {`

`[webView loadHTMLString:@"" baseURL:nil];`

`[webView stopLoading];`

`[webView removeFromSuperview];`

`webView = nil;`

`[[NSURLCache sharedURLCache] removeAllCachedResponses];`

`[[NSURLCache sharedURLCache] setDiskCapacity:0];`

`[[NSURLCache sharedURLCache] setMemoryCapacity:0];`

`}`

```
 - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(
NSDictionary *)launchOptions{ 
       int cacheSizeMemory = 4*1024*1024; // 4MB int           
       int cacheSizeDisk = 32*1024*1024; // 32MB 
       NSURLCache *sharedCache = [[NSURLCache alloc] initWithMemoryCapacity:cacheSizeMemory diskCapacity:cacheSizeDisk 
       diskPath:@"nsurlcache"];
       [NSURLCache setSharedURLCache:sharedCache]; 
｝ 
- (void)applicationDidReceiveMemoryWarning:(UIApplication*)application { 
       [[NSURLCache sharedURLCache] removeAllCachedResponses];
 } 
```





