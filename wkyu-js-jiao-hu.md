

```
WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc]init];
_userContentController =[[WKUserContentController alloc]init];
configuration.userContentController= _userContentController;
    
_loanWeb = [[WKWebView alloc] initWithFrame:CGRectMake(0,0, KScreenWidth, KScreenHigh - KNavBar-KTabBar) configuration:configuration];
[_userContentController addScriptMessageHandler:self name:@"login"];

_loanWeb.scrollView.scrollEnabled = NO;
_loanWeb.UIDelegate = self;
_loanWeb.navigationDelegate = self;
[_loanWeb loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:[NSString stringWithFormat:@"%@",kH5HostURL]]]];
[self.view addSubview:_loanWeb];

#pragma mark - WKScriptMessageHandler
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message{
    
    //message.body  --  Allowed types are NSNumber, NSString, NSDate, NSArray,NSDictionary, and NSNull.
    if ([message.name isEqualToString:@"login"]) {

    }
}
```



