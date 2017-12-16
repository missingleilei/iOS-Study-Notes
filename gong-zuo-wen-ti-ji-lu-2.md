##### 1. WKWebView自适应高度

- \(**void**\)webView:\(**WKWebView** \*\)webView didFinishNavigation:\(**WKNavigation** \*\)navigation  {  

\[webView evaluateJavaScript:@"document.body.offsetHeight"completionHandler:^\(id\_Nullable result,**NSError** \*\_Nullable error\) {  

       CGFloat documentHeight = \[resultdoubleValue\];

       CGRect frame = webView.frame;

       frame.size.height = documentHeight;

       webView.frame = frame;

        NSLog\(@"webView.frame = %@",NSStringFromCGRect\(webView.frame\)\);

    }\];

}





