# ratel-doc
# 文档正文
平头哥（ratel）是一个能够在免root手机中使用xposed的框架。支持5.0-9.0。使用ratel可以任意控制app的行为，如同开启上帝模式一样。他有如下特性：

1. 完全支持9.0，对比xposed框架本身在8.0的时候就各种使用不顺畅了
2. 免root运行，ratel运行，不需要root后者解锁bootloader。现在新手机获得root权限越来越困难。ratel可以实现免root下DIY你的手机
3. 痕迹隐藏，对于root，xposed等常见风险操作检测，ratel框架从原理上就避免了他们。
4. 无法影响全局，受限于实现原理，ratel无法影响系统。只能影响正常安装在手机里面的app。
5. ratel模块开发，无需重启手机即可生效（就xposed框架本身来说，也可以通过一定手段达到这个效果，参考我的另一个项目[ucrack](https://gitee.com/virjar/ucrack)）
6. API扩展，ratel在xposed framework层面之上，提供了ratel独有的一些特殊API。可以实现一些xposed本身不提供的强大功能。当然，如果你按照标准的xposed API开发，那么ratel支持绝大部分xposed特性
7. external&embed模式共存，在免root环境下，如果把模块植入到app内部，那么可以使得这个app可以被广大普通用户直接使用。因为DIY功能的使用门槛降低到了只需要下载AndroidAPK了。可以理解为您家老人都可以正常使用ratel了
9. 内置设备模拟+一键新机+多账号共存功能。这个功能各位不要滥用，比较狂暴。设备指纹模块几乎可以对抗专业级指纹解决方案。


# 不支持

*暂时不支持 android<=4.4 , android>=10未测试*

*不支持模拟器，不支持X86架构，仅支持arm or arm64*


# 本月测试证书
```
m3RHrdtLUZ8F/pjKxcsRMqp3aRYXoEBhDW2AML+Pv55dOnNnKM1ctTKoqJkv6PAGMroIFHWo+WZfOAT2ANTpLZRXmrTrnWoezxa5TzQrixr7S9BGjKoPYEnYfhItz06VkN8RtwrjlvovUg+Rb7d5EIdMxF+MVCIdY28+NH7orGgg8QfKC2kda0Z1u/KdU2kTRvwdjwGameW2nbYR6eMf8+SfugvaR8cEGpgfk9dG7cHBzbvrsAIF6Sm4s3eiEMpHANyoEsn/vOFMy2jJqzvfWBk7RmADmijGxCsPBP9dW6D2hNejw2nFqM9PEIWKdnRMnbkNTndKHDXqz1nchqspulim0lU1Z0ny2bky6FOGHZHeKxChsxs7B8L5XP7yKkoe5eqa2sQj0C3BdjsR7b3KQsDgIQfySZV1ULE3t0gD2J/OuqvKXeC+p1gRqTKvxQvYj7QduiB/ycI9nnGY3Cj6UTc0OM/E5B4O2DsnqjgK+FeVMMWkiclrM4p3/qvN0O08AmxGFyX6OClZKCjtxus3xVpUXRjvZdi4LhPV73JlQ68SN7E8T217LVoouu7AzCp/W8gcF7ObVacA9XFaVOIvVoiBAAxfm15x7hZD/9wfIgRvnYji5eo2Liao7PqXVBcJDtQ5s3MDjvcGrSo65jA0RjF3yMRQ817I29ybPoR/DUA=
```

# 分模块文档
ratel 大概分为以下模块

## [ratel server感染apk的后台服务](2.ratelServer.md)
## [ratel管理器，类似xposed intalller](3.ratelManager.md)
## [ratelAPI 通过API编程进行模块开发和ratel功能定制](4.ratelAPI.md)
## [虚拟化相关](5.virtual_env.md)
## [扩展开发包](7.ratelExtension.md)
## [常见问题处理](6.faq.md)

# 基本使用流程
1. 使用ratel在线平台感染app： http://ratel.virjar.com/
在这里上传apk，等待系统处理完成，下载apk，安装到手机即可
2. 下载并安装ratelManager
ratelManager的apk，在ratel server的首页有下载地址
3. 编写Xposed模块插件（也可以叫做Ratel模块插件）,安装到手机，并在RatelManager中配置即可
4. xposed模块插件标准demo可以参考: http://git.virjar.com/ratel/ratel-demo

# 发布记录
## 引擎发布记录

### V1.2.9
1. [improve] 不允许app读取系统systemSetting定义以外的数据，避免app矩阵泄漏设备指纹id
2. [bugfix] rebuildDex如遇寄存器使用超V15，无法直接访问p0,p1寄存器，导致FacebookLite构建失败
3. [improve] 通过DexMerger替换smali整体的dex重编译，大幅提升rebuildDex的构建速度，感谢skyun1314(https://github.com/skyun1314/)
4. [improve] 支持已经被Ratel感染过的APK的重编
5. [improve] 支持在重编后的apk里面植入默认的SmaliLog模块，方便进行APKTool重打包调试
6. [feature] 引入R8转换模块实现class字节码转Dex字节码，支持重编引擎在Android环境下运行。Ratel重编引擎同时支持服务器和Android环境，并将在下个版本集成到RatelManager中
7. [improve] 使用google官方ApkSinger替换java的JarSigner,支持Android高版本的V1,V2,V3 schema证书签名。同时避免和JVM耦合，为重打包引擎兼容RatelManager做准备

### V1.2.8
1. [bugfix] rebuild dex 模式下,cinit代码识别错误&强行继承了带final修饰的attachBaseContext方法导致facebook打开闪退
2. [improve] 移除ratel框架不支持的so

### V1.2.7
1. [improve] sync sandhook : disable forceProcessProfiles in Q
2. [bugfix] 存在弹窗情况下，无法自杀app问题
3. [bugfix] 部分机型设备模拟模块导致文件句柄资源泄漏

### V1.2.6
1. [bugfix] 不应该将mock目录设置为沙箱白名单，这会影响符号反查逻辑。可能导致真实地址泄漏
2. [bugfix] fd符号查询函数，在hook模块下，系统调用存在脏数据。使用新的地址空间接收数据。包括系统调用readlinkat和readlink
3. [improve] 如果开发者错误的对一个函数重复hook，那么打印警告。一般一个逻辑只应该hook一个函数一次。多次hook应该为不同模块功能。大量挂载相同hook点将会导致内存溢出
4. [feature] 开始引入Okio模块，可能在下个版本实现完整的流量监控
5. [bugfix] 修复hook框架在hook函数的时候，出现方法编译前StopTheWorld导致compile函数死锁(大型app上大约5%概率发生)。进而引发app卡死在启动屏的问题。本修改点影响了hook流程，需要做大量兼容测试
6. [feature] ratel日志TAG统一，不再区分java和native
7. [bugfix] 可能是一个代码优化框架的bug导致某些情况下app打开卡屏,所以对一个函数开放混淆白名单
8. [bugfix] 部分app的Application入口配置没有android,导致app代码入口解析失败（追加发布）

### V1.2.5
1. [feature] 在android7.0上，class init 符号未适配导致safe hook static method失效
2. [feature] 无论如何，只要class没有初始化，那么都使用延迟hook方案
3. [feature] 提供感知class加载的监听接口(对加固场景分析特别有效)
4. [bugfix] 64未模式下运行，execeve注入的so地址有误
5. [feature] 增加对网卡地址，bootId的模拟
6. [bugfix] 修复1.2.3版本重构设备指纹模块时，AndroidId模拟失败问题
7. [bugfix] apk2jar过程，dexFile写入了ByteBuf末尾脏数据，导致在Android9上面,DexFile格式校验不通过
8. [bugfix] 修复一个1.2.3版本重构文件系统结构导致的bug，该bug导致app升级的时候一机多号数据丢失

### V1.2.4
1. [bugfix] dex short value  65535 overflow when rebuild entry point dex sometimes
2. 打包过程，清理一个不需要的资源，在输出APK中节约2M空间
3. ratelAPI剥离XposedAPI，在ratelAPI中，无法看到Xposed相关class(为了彻底隐藏xposed在虚拟机的出现)。不过如果你的模块仍然是一个xposed标准模块，那么依然可以运行在ratel环境中
4. 抽取部分逆向工具链
5. 绕过一个多开对抗检测点
6. 代码载体，由apk转化为jar，且对无用的Android相关class进行清理
7. 修复一个1.2.3版本重构文件系统结构导致的bug
8. 迁移sandhook相关class，尽量避免sandhook痕迹泄露

### V1.2.3
1. dexmaker优化，ratel框架hook的method stub资源，通过预构建的方法产生，减少stub资源使用量，ratel framework hook逻辑不再使用dexmaker
2. [bugfix] 当外置插件和内置插件package相同，且外置插件配置生效，之后外置插件被卸载。此时应该回退使用内置插件功能，而非显示文案:"the embed xposed module :{" + embedXposedModuleApkPackage + "} has load from external already ,skip it"
3. [bugfix] 指纹模拟逻辑，部分场景出现local reference overflow导致crash
4. 支持execve io hook
5. 重构虚拟设备和一机多号文件组织结构管理，支持内部文件指向sdcard（可以使得在非root设备调试的时候，观察app内部文件）

### V1.2.2
1. 支持通过ratel-api实现指纹数据填充
2. 支持通过ratel-manager获取真实的设备唯一id
3. 深度设备指纹对抗
4. 跨进程IPC过程，签名替换，解决授权登陆出现非正版应用的问题(请注意，跨进程调用需要同步感染双方apk，且建议安装RatelManager，且RatelManager版本大于1.0.4)
5. 迁移sandhook代码，支持AndroidQ
6. 迁移sandhook代码，支持safe hook static method
7. [bugfix]当模块代码存在so的时候，nativeLibaryDirectory设置错误

### V1.2.1
1. 在xposed函数回调过程，如果发生了异常，需要打印bridge日志堆栈。避免异常被淹没
2. sdcard白名单，自动创建文件夹，白名单提升到父目录
3. 框架hook逻辑，全部替换为rposed
4. 允许从命令行附加额外属性，范例``ratel_properties_virtualEnvModel=START_UP``
5. virtualEnvModel的value值，统一定义为``com.virjar.ratel.api.VirtualEnv.VirtualEnvModel.children``，避免理解误差
6. virtualEvn的log tag，单独剥离，并且只在debug版本打印，避免输出刷屏
7. webview的签名检查服务hook逻辑错误 bugfix
8. 当手机使用VPN的时候，隐藏VPN标记（TODO 还不完善）
9. 自动隐藏Wi-Fi列表BSSID，避免app通过Wi-Fi列表定位


## manager发布记录

### V1.0.3
1. 当不需要守护进程的时候，不引导用户到辅助模式设置页面
2. 申请获得定位信息，申请获得序列号、imei信息，提供给ratelAPI使用
3. 程存活，实时保活。经常自杀后能够马上唤醒
4. 提供一机多号操作面板


## server发布记录
### V1.0
预计2019年09月14日正式上线，敬请期待。官网地址：http://ratel.virjar.com