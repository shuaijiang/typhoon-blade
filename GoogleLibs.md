很多用户反映，Blade 集成了对一些 google 的开源库和工具的支持，但是用户要用这些库，还需要自己准备，为何没有便利的方式。
现在我们把这些库整合起来，贡献给大家。

注意
  * 所有的库都在 thirdparty 目录下，引用时也需要带 "thirdparty/" 前缀。
  * 所有的库都有两个目录，libname-1.6.0 和 libname，前者是为了升级使用的，请不要用，只用不带版本的目录。
  * protobuf 依赖 zlib-dev 包（不同的发行版可能名字不同），请自行确保已安装。
  * 请参考 examples 目录，学习使用方法。
  * 我们所做的工作只是整合这些开源的库，所有原始版权属于 google。
  * BLADE\_ROOT 里是使用这个库需要的 Blade 配置信息，如果你决心所有的项目都使用这个库，直接写到你的 blade.conf 或者 .bladerc 文件即可。
  * 配置文件用的 protobuf 编译器为 thirdparty/protobuf/bin/protoc，已经在里面，如果在你的机器上不能运行，下载原始的 protobuf-2.4.1 包，configure make 后，放到这个位置替换掉即可。


包含的各个库及版本：
  * gflags 2.0
  * glog 0.3.2
  * protobuf 2.4.1 (我们内部用的还是2.3.0，众网友幸福！)
  * gtest 1.6.0
  * gmock 1.6.0
  * gperftools 2.0 (我们内部用的还是1.7，悲惨)

gflags 的修改：
  * --help 时，帮助信息以彩色输出，就像 gtest 那样
  * gflags.h 中的 guard 宏 BASE\_COMMANDLINEFLAGS\_H_跟perftools 中的冲突，改回旧版本里的 GFLAGS\_GFLAGS\_H_

glog 的修改：
  * 使用 gflags
  * 以彩色输出日志到终端，警告为黄色，错误为红色
  * 没有设置 log\_dir 时，优先使用当前目录，原来输出到 /tmp，很容易造成磁盘满告警。
  * 输出完整的源文件路径，方便查问题，原来只输出文件名部分。用 bool 类型参数 --strip\_source\_path 来控制是否开启
  * 避免在 FlushLogFilesUnsafe 里分配内存，否则在信号处理或者 tcmalloc 失败 abort 时会死锁。

protobuf 的修改：
  * 使用 glog 替代 protobuf 自己定义的简单的 log
  * RAW\_LOG(FATAL) 在开启 Strip 时，调用 exit，如果本身是在 exit中，就会造成 exit 递归调用，应该跟不 strip 一样调 abort()

gtest 的修改：
  * gtest main 中，额外地初始化了 gflags, glog。

gperftools 的修改：
  * 使用 gflags
  * 修改了和 glog 一起使用，flag同名而导致冲突的问题。

这些修改有一部分是来自台风平台内部使用中的所做的修改，将会在适当的时机（说人话就是指我比较闲的时候）提交给 google 这些库的维护者。

最初平台的开发启动时，整合这些库参考了陈硕同学的 [整合 Google 开源 C++ 代码](http://blog.csdn.net/Solstice/article/details/5497699)一文，再次表示感谢。