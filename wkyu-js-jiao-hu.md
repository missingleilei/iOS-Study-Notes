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

//这里需要注意，前面增加过的方法一定要remove掉。 有点问题
[_userContentController removeScriptMessageHandlerForName:@"login"];

UIWebView加载word文档

1.打开本地的word文档，这里word文档的文件名为：sqlite数据库.docx

NSString *path = [[NSBundle mainBundle] pathForResource:@"sqlite数据库" ofType:@"docx"];
NSURL *url = [NSURL fileURLWithPath:path];
UIWebView *webView = [[UIWebView alloc] initWithFrame:CGRectMake(0, 0, Width, Height)];
[webView loadRequest:[NSURLRequest requestWithURL:url]];
[webView sizeToFit];
 webView.scalesPageToFit = YES;
 webView.delegate = self;
 [self.view addSubview:webView];
 self.webView = webView;

2.通过URL加载word文档
NSURL *url = [NSURL URLWithString:@"http://test.yzp.art-d.com.cn:88/static/upload/111_1.doc"];
NSData *data = [[NSData alloc] initWithContentsOfURL:url];
UIWebView *webView = [[UIWebView alloc] initWithFrame:CGRectMake(0, 0, Width, Height)];
[webView loadRequest:[NSURLRequest requestWithURL:url]];
[webView sizeToFit];
webView.scalesPageToFit = YES;
webView.delegate = self;
[self.view addSubview:webView];
self.webView = webView;
另外，通过webView的 loadData: MIMEType: textEncodingName: baseURL:这个方法也可以加载出来。

此外，我想拿到word显示的文字内容，可以通过执行js代码
-(void)webViewDidFinishLoad:(UIWebView *)webView{
    NSString *strings = [webView stringByEvaluatingJavaScriptFromString:@"document.body.innerText"];
    NSLog(@"%@",strings);
｝
这里的strings就是word文档文字内容。
另外，通过NSString *strings = [webView stringByEvaluatingJavaScriptFromString:@"document.body.innerHTML"];
这里获得的strings为word文档内容的html格式。
```



