连编工程中 Allow Non-modular Includes In Framework Modules 改为YES 否则无法编译FMDB

相关网址:

[http://www.cocoachina.com/ios/20150127/11022.html](http://www.cocoachina.com/ios/20150127/11022.html)

[http://blog.csdn.net/ma762614600/article/details/50057651](http://blog.csdn.net/ma762614600/article/details/50057651)

[http://www.cocoachina.com/ios/20141126/10322.html](http://www.cocoachina.com/ios/20141126/10322.html)

[http://www.cocoachina.com/bbs/read.php?tid-282490-keyword-framework.html](http://www.cocoachina.com/bbs/read.php?tid-282490-keyword-framework.html)

[http://blog.csdn.net/xyxjn/article/details/42527341](http://blog.csdn.net/xyxjn/article/details/42527341)

[http://www.jianshu.com/p/656ba8094d1d](http://www.jianshu.com/p/656ba8094d1d)

#### 1 .a文件版本

1、新建项目，点击iOS—Framework&—Cocoa Touch Static Library。

2、系统自动生成以工程名命名的.h和.m文件，可自定义的在目录下添加或删除文件，注意目录下Products文件夹有一个.a文件为红色，说明文件并不存在。这里我们将系统生成的.h和.m文件删除。

3、点击Build Phases—Copy Files，左下角点击+号按钮，添加你需要暴露的接口头文件。如果你在静态库工程中使用了category，那么你可能会碰到链接问题，解决的办法就是需要同时在生成静态库的工程和使用静态库的工程中使用“-all\_load”编译选项，即在对应target的"Build Settings"中的“Other Linker Flags”选项添加“-all\_load”，注意：使用静态库的工程中是一定要加该编译选项的！至于生成静态库的工程中加不加没有试过，不过建议还是加上该编译选项。

4、然后点击左上角，选择Edit Scheme，Build Configuration下选择Release，先注意检查下面Release是否为NO：Yes表示只编译选中模拟器设备对应的架构，No则为编译所有模拟器设备支持的cup架构（Debug版本同理）,选择NO，然后分别在模拟器和真机下Command+B编译一下，会看到Products文件夹下的.a文件变为黑色，这个.a文件就是我们想要得到的静态库。

模拟器：iphone4s~5 : i386 iphone5s~6plus : x86\_64

真机：iphone3gs~4s : armv7  iphone5~5c : armv7s （静态库只要支持了armv7，就可以跑在armv7s的架构上） iphone5s~6plus : arm64

特殊注意

在xcode6下，combine\_hidpi\_images的默认设置项为yes，需要手动改为no，否则bundle中的png图片经过编译打包会变成tiff的文件，进而导致对应图片找不到。

#### 2.bundle的相关配置

1 修改Bundle的属性，让他能用在iOS上。

2 Target：并选择 Build Settings -&gt;Architectures -&gt;Base SDK  改为Latest iOS（ios 8.1）

3 Target：在 Deployment 下，

将 “Mac OS X Deployment Target”改成”Compiler Default”，

将 “Targeted Device Family”改成”iPhone/iPad”,根据自己的需要改。如果你只想在iPhone上用，那选iPhone就行

将  “iOS Deployment Target”，改成 “iOS 7.0”:也就是说这个静态库支持7.0以上版本。

#### 3.真机版和模拟器版的库合并解决方法：

在framework文件夹下，你会看到一个白板文件，这个文件名和的的framework静态库名相同，只是没有后缀名。 这个文件在被引入到其他工程时候是看不见的。只有在Show In Finder下能看到。

打开终端，输入命令：中文换成你的真实目录。

lipo -create “……真机/目录/那个白板文件“ "……/模拟器/目录/那个白板文件" -output “…..另保存的/目录/文件”

合并好的新文件，覆盖掉原来的framework中的文件即可。这个framework就会支持所有设备和真机、模拟器全部版本。

其实这个方法就是用\*.a文件的合并方法。在framework一样有效。

framework中的白板文件就好像\*.a文件一样。



![](/assets/eed0429d-5bf2-4664-a9b8-47d0307d4493.png)

AVPlayer本身并不能显示视频，而且它也不像MPMoviePlayerController有一个view属性。如果AVPlayer要显示必须创建一个播放器层AVPlayerLayer用于展示，播放器层继承于CALayer，有了AVPlayerLayer之添加到控制器视图的layer中即可。要使用AVPlayer首先了解一下几个常用的类：

AVAsset：主要用于获取多媒体信息，是一个抽象类，不能直接使用。

AVURLAsset：AVAsset的子类，可以根据一个URL路径创建一个包含媒体信息的AVURLAsset对象。

AVPlayerItem：一个媒体资源管理对象，管理者视频的一些基本信息和状态，一个AVPlayerItem对应着一个视频资源。

AVPlayer播放和暂停没有对应的状态，通常情况下可以通过判断播放器的播放速度来获得播放状态。如果rate为0说明是停止状态，1是则是正常播放状态。

AVPlayer和AVPlayerItem（AVPlayer有一个属性currentItem是AVPlayerItem类型，表示当前播放的视频对象）都无法获得播放器状态和媒体加载状态。

AVPlayerItem的相关通知只有AVPlayerItemDidPlayToEndTimeNotification--播放完成通知。

当想获取视频加载情况、缓冲情况、播放情况，这些信息可以通过KVO监控AVPlayerItem的status、loadedTimeRanges属性来获得。

当AVPlayerItem的status属性为AVPlayerStatusReadyToPlay是说明正在播放，只有处于这个状态时才能获得视频时长等信息；当loadedTimeRanges的改变时（每缓冲一部分数据就会更新此属性）可以获得本次缓冲加载的视频范围（包含起始时间、本次加载时长），这样一来就可以实时获得缓冲情况。然后就是依靠AVPlayer的`- (id)addPeriodicTimeObserverForInterval:(CMTime)interval queue:(dispatch_queue_t)queue usingBlock:(void (^)(CMTime time))block`方法获得播放进度。这个方法会在设定的时间间隔内定时更新播放进度，通过time参数通知客户端。相信有了这些视频信息播放进度就不成问题了，事实上通过这些信息就算是平时看到的其他播放器的缓冲进度显示以及拖动播放的功能也可以顺利的实现。

视频切换的功能，在前面介绍的所有播放器中每个播放器对象一次只能播放一个视频，如果要切换视频只能重新创建一个对象，但是AVPlayer却提供了\(void\)replaceCurrentItemWithPlayerItem:\(AVPlayerItem \*\)item方法用于在不同的视频之间切换。

