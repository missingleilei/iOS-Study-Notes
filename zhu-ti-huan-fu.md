[http://www.cocoachina.com/ios/20180102/21717.html](http://www.cocoachina.com/ios/20180102/21717.html)



我的实现思路类似微店的实现方式。但是我并没有使用KVO而是使用了通知注册的方式。

APP启动后直接加载对应的皮肤文件，同时另一个线程去请求后台皮肤接口，接口返回了一个zip包的链接，下载zip包，解压后，解析里面的config.json文件，然后我使用通知的方式去触发换肤。具体的思路逻辑相信流程图上已经画的很清楚了。

控制皮肤是否显示的逻辑完全由后台控制，后台返回skinSign为空则关闭皮肤。![](/assets/节日主题换肤.png)

![](/assets/节日主题换肤.png)
