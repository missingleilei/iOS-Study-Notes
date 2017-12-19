##### 1. WKWebView自适应高度

* \(**void**\)webView:\(**WKWebView** \*\)webView didFinishNavigation:\(**WKNavigation** \*\)navigation  {  

\[webView evaluateJavaScript:@"document.body.offsetHeight"completionHandler:^\(id\_Nullable result,**NSError** \*\_Nullable error\) {

```
   CGFloat documentHeight = \[resultdoubleValue\];

   CGRect frame = webView.frame;

   frame.size.height = documentHeight;

   webView.frame = frame;

    NSLog\(@"webView.frame = %@",NSStringFromCGRect\(webView.frame\)\);

}\];
```

}

##### 2.Xib 约束 UIScrollerView

1、在使用xib定制界面时，我们经常会用到AutoLayout自动布局，或者不用AutoLayout，自己在xib属性的地方进行约束。

2、xib中，ScrollView的自动约束并不像我们平常约束其他view一样，由于ScrollView的滚动范围是由有属性ContentSize决定的，所以在xib中使用AutoLayout约束的时候会相对复杂一下。

3、在xib中拖动一个scrollerView到xib中，scrollerView直接在View下面，界面结构如下：![](/assets/20160425170513070.png)



4、对srollerView进行约束，并更新位置![](/assets/20160425170830284.png)

5、添加约束完成之后，会发现一切正常，并没有报错，接下来我们往srollerView中添加一个View，会发现立马报错，咱不处理返回去就可以了，然后选中View，添加约束。上下左右约束后还需水平或垂直约束看滑动方向，然后我们再选中View，添加height约束，这里约束的是600，这是会发现，没有报错了，说明可以正常使用了。

6、如果想要在控制器中动态的设置ScrollView的ContentSize，可以将view的高度拉线设置成属性，然后在控制器合适的计算地方设置ScrollView的垂直滚动范围。

7、如果想要往ScrollView上添加需要跟随ScrollView滚动的控件（比如label），只需要将label添加到view（上面高度约束为600的那个view）上，成为view的subView就可以了，简单来说就是忽略掉ScrollView，把需要的控件放到view上，按照正常的约束来约束控件就可以了。

8.跟随内容的高度设置constant，

使用ScrollView的情况一般都是内容可以滚动，并且滚动的范围是计算出来的，在xib里面约束ScrollView，也可以在代码中在计算出滚动范围之后再设置constant，

注意：在计算滚动范围之后立即根据某一个控件设置constant时，有的时候不起作用，这是因为界面只是加载了控件和计算出了范围，并没有刷新界面，某些控件的高度还没有被调整，这里建议在一个方法中写：

`- (void)viewDidLayoutSubviews{ `

`[super viewDidLayoutSubviews];`

`self.scrollerViewHeight.constant = CGRectGetMaxY(self.sendMessageBtn.frame) +50;`

` }`

