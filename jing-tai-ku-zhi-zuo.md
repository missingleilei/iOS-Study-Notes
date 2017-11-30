连编工程中 Allow Non-modular Includes In Framework Modules 改为YES 否则无法编译FMDB

相关网址:

[http://www.cocoachina.com/ios/20150127/11022.html](http://www.cocoachina.com/ios/20150127/11022.html)

[http://blog.csdn.net/ma762614600/article/details/50057651](http://blog.csdn.net/ma762614600/article/details/50057651)

[http://www.cocoachina.com/ios/20141126/10322.html](http://www.cocoachina.com/ios/20141126/10322.html)

[http://www.cocoachina.com/bbs/read.php?tid-282490-keyword-framework.html](http://www.cocoachina.com/bbs/read.php?tid-282490-keyword-framework.html)

[http://blog.csdn.net/xyxjn/article/details/42527341](http://blog.csdn.net/xyxjn/article/details/42527341)

[http://www.jianshu.com/p/656ba8094d1d](http://www.jianshu.com/p/656ba8094d1d)

#### 1 .a文件版本

1、新建项目，点击iOS—Framework&—Cocoa Touch Static Library。

2、系统自动生成以工程名命名的.h和.m文件，可自定义的在目录下添加或删除文件，注意目录下Products文件夹有一个.a文件为红色，说明文件并不存在。这里我们将系统生成的.h和.m文件删除。

3、点击Build Phases—Copy Files，左下角点击+号按钮，添加你需要暴露的接口头文件。如果你在静态库工程中使用了category，那么你可能会碰到链接问题，解决的办法就是需要同时在生成静态库的工程和使用静态库的工程中使用“-all\_load”编译选项，即在对应target的"Build Settings"中的“Other Linker Flags”选项添加“-all\_load”，注意：使用静态库的工程中是一定要加该编译选项的！至于生成静态库的工程中加不加没有试过，不过建议还是加上该编译选项。

4、然后点击左上角，选择Edit Scheme，Build Configuration下选择Release，先注意检查下面Release是否为NO：Yes表示只编译选中模拟器设备对应的架构，No则为编译所有模拟器设备支持的cup架构（Debug版本同理）,选择NO，然后分别在模拟器和真机下Command+B编译一下，会看到Products文件夹下的.a文件变为黑色，这个.a文件就是我们想要得到的静态库。

模拟器：iphone4s~5 : i386 iphone5s~6plus : x86\_64

真机：iphone3gs~4s : armv7  iphone5~5c : armv7s （静态库只要支持了armv7，就可以跑在armv7s的架构上） iphone5s~6plus : arm64

特殊注意

在xcode6下，combine\_hidpi\_images的默认设置项为yes，需要手动改为no，否则bundle中的png图片经过编译打包会变成tiff的文件，进而导致对应图片找不到。

#### 2.bundle的相关配置 

1 修改Bundle的属性，让他能用在iOS上。

2 Target：并选择 Build Settings -&gt;Architectures -&gt;Base SDK  改为Latest iOS（ios 8.1）

3 Target：在 Deployment 下，

将 “Mac OS X Deployment Target”改成”Compiler Default”，

将 “Targeted Device Family”改成”iPhone/iPad”,根据自己的需要改。如果你只想在iPhone上用，那选iPhone就行

将  “iOS Deployment Target”，改成 “iOS 7.0”:也就是说这个静态库支持7.0以上版本。

#### 3.真机版和模拟器版的库合并解决方法：

在framework文件夹下，你会看到一个白板文件，这个文件名和的的framework静态库名相同，只是没有后缀名。 这个文件在被引入到其他工程时候是看不见的。只有在Show In Finder下能看到。

打开终端，输入命令：中文换成你的真实目录。

lipo -create “……真机/目录/那个白板文件“ "……/模拟器/目录/那个白板文件" -output “…..另保存的/目录/文件”

合并好的新文件，覆盖掉原来的framework中的文件即可。这个framework就会支持所有设备和真机、模拟器全部版本。

其实这个方法就是用\*.a文件的合并方法。在framework一样有效。

framework中的白板文件就好像\*.a文件一样。

