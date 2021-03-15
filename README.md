# ratel-doc
# 文档正文
平头哥（ratel）是一个能够在免root手机中使用xposed的框架。支持5.0-10.0。使用ratel可以任意控制app的行为，如同开启上帝模式一样。他有如下特性：

1. 支持到Android较新版本(目前到android10)，对比xposed框架本身在8.0的时候就各种使用不顺畅了
2. 免root运行，ratel运行，不需要root后者解锁bootloader。现在新手机获得root权限越来越困难。ratel可以实现免root下DIY你的手机
3. 痕迹隐藏，对于root，xposed等常见风险操作检测，ratel框架从原理上就避免了他们。
4. 无法影响全局，受限于实现原理，ratel无法影响系统。只能影响正常安装在手机里面的app。
5. ratel模块开发，无需重启手机即可生效（就xposed框架本身来说，也可以通过一定手段达到这个效果，参考我的另一个项目[ucrack](https://gitee.com/virjar/ucrack)）
6. API扩展，ratel在xposed framework层面之上，提供了ratel独有的一些特殊API。可以实现一些xposed本身不提供的强大功能。当然，如果你按照标准的xposed API开发，那么ratel支持绝大部分xposed特性
7. external&embed模式共存，在免root环境下，如果把模块植入到app内部，那么可以使得这个app可以被广大普通用户直接使用。因为DIY功能的使用门槛降低到了只需要下载AndroidAPK了。可以理解为您家老人都可以正常使用ratel了
9. 内置设备模拟+一键新机+多账号共存功能。这个功能各位不要滥用，比较狂暴。设备指纹模块几乎可以对抗专业级指纹解决方案。


# 不支持

*暂时不支持 android<=4.4*

*不支持模拟器，不支持X86架构，仅支持arm or arm64*


# 公测证书（20分钟程序退出、三天后程序会永久闪退）
```
fhbzcaYYVEjLc1lNLonVWFMTq+T11NyISHMwsYofh8tlOWnQwDyG6VgvzV9w64hKcsQVVzaR18qGOS8qyIg1hn2kvCquMgcLdgRAqwu5Ndnssdi27SMCXLN3nwxowTD/tNHbQ8/exYLBA/M2he2lbaPmG07sFkHBPjQ5gyFxqcBccBahtp4nk9AIXz6EdZIlfBjtbAVDIaCkF1TprAKgvuAolWzvAMCpp6gltxXz1vHsUdLYyk6+2n3XHI3n8rIktG81YSxYcT0UvKb8BuA7UJMQ6o2n61Mmzc4pSvnF4KJO4fOzNJ5rZoblJStUjX+hqvDLB2Nqlt/1Ay9jaCfVCKYNTI5NAXZ6Bkr6ebtMtXBqK/jkZ96jvoo0Vnsr8EdVlv3jZV4ITSQBBJRX8o71JDVZImkco1uRieRapER9KCMqz2Ar7AnZ55R0Cqtk5mKX1QtkmQt3BrT9sVjQJv9/M+7sHxDYlisLCwpyIQ55PT2izPBIWfVf3CO8uLbtrhvPDgrl0hnEOYFUp8EODkvdlglfKg8jh8xpfYz4/QIf7lGpQVPbjjPD9FSNF3Hg9D7e6Y1rloRmDPsMRSk1+v8eW9Ls4wAXhqf7ov3x8E2aCT82XPZY4Vk4PEu93zqLoDPiPvgtmylpK2dOgGv5gyZJ0/A/M0uyWh3pKE/hcAvCK/ePUeTpYOaGEmQtKgerin2aOtCz3SNzsS+w+aUYYgxcRin1XWS6j4/bizQwy5bTnRAPc6fmNU/t7KiybwLsyYGXE/7tWwSPPSbOka26fMT90epxi5vb5zl0lMfWG5VaYGNU00zVmh2F3+su/+s7gbRsnpmQVjjfx4lwUTW4BBh976sqD6E8WqVY4TnCrQyDNFCY5o8fVv4AssjrRdWTjUKC08x9JP7CGcmHta2qftRr+O9Ou8gPNZ2aXmulY2fms38I2NOjw6sXdL+Dv4pw+2IH84mA46MySrhO1xv8diFgiTm2HHG55DiJrfn4VTYa8tjpogGVIhNA9joBZrFCPL1R
```

# 分模块文档
ratel 大概分为以下模块

- [ratel server感染apk的后台服务](2.ratelServer.md)
- [ratel管理器，类似xposed intalller](3.ratelManager.md)
- [ratelAPI 通过API编程进行模块开发和ratel功能定制](4.ratelAPI.md)
- [虚拟化相关](5.virtual_env.md)
- [扩展开发包](7.ratelExtension.md)
- [ratel调度任务](8ratelScheduler.md)
- [RDP 类似APKtool的代码修改工具](9.RDP.md)
- [Zelda多开分身](10.zelda.md)
- [HotModule热发模块](11.HotModule.md)
- [平头哥公开模块](12.publicModules.md) 
- [常见问题处理](6.faq.md)

# 基本使用流程
1. 使用ratel在线平台感染app： http://ratel.virjar.com/
在这里上传apk，等待系统处理完成，下载apk，安装到手机即可
2. 下载并安装ratelManager
ratelManager的apk，在ratel server的首页有下载地址
3. 编写Xposed模块插件（也可以叫做Ratel模块插件）,安装到手机，并在RatelManager中配置即可
4. xposed模块插件标准demo可以参考: http://git.virjar.com/ratel/ratel-demo
5. 如果你想通过git submodule直接依赖ratel提供的快照API，可以follow相关项目:[http://git.virjar.com/ratel/ratelextension](http://git.virjar.com/ratel/ratelextension) 和 [http://git.virjar.com/ratel/ratelapi](http://git.virjar.com/ratel/ratelapi)

# 发布记录
## 引擎发布记录

### V1.4.9 20210315
1. [bufix] 某很老的壳子的dex格式对抗
2. [feature] 支持给app增加一些权限特有权限声明（入app本身没有申请sdcard，此时ratel插件无权限读写sdcard）
3. [bugfix] appendDex模式，如果application注册了一些生命周期函数，可能导致某些函数回掉不成功
4. [bugfix] 分身模式下，Android7.0可能闪退(HOOK了一些低版本不存在的api导致NPE)
5. [feature] 同步SandHook代码
6. [bugfix] 使用ratle脚本提取apk的原apk，可能导致NPE
7. [bugfix]  Nubia Android5.0 Resource构造函数适配&CellLocation fake aidlAPI参数适配 & sdcard创建ratel相关文件夹，可能存在部分sdcard挂载点没有权限
8. [bugfix] android5.0 java.lang.reflect.Proxy.generateProxy 参数签名兼容
9. [feature] hotmodule switch，支持在rm中临时禁用热发插件，方便调试
10. [bugfix] 对于DexSplit的apk进行二次重打包会导致： Multiple ZIP entries with the same name（addsOn判定逻辑不严谨）
11. [bugfix] 360壳子，Android10，Android64位模式运行闪退
12. [bugfix] android 6.0.1,资源构造方法可能有误导致闪退
13. [bugfix] windows环境下构建apk闪退问题

### V1.4.8
1. [bugfix] 部分场景dex2oat禁用功能失效
2. [bugfix] 脱壳机组装apk可能dex组装失败导致脱壳失败
3. [feature] 使用riru的maps hidden方法
4. [feature] 支持native hook框架切换到Dobby
5. [feature] 支持binder hook
6. [feature] 设备指纹对抗方案升级(核心重点功能)
7. [bugfix] 部分appAndroid10闪退问题修复
8. [bugfix] 弱网环境下，热发插件推送可能不生效
9. [feature] native helper模块，so脱壳，native函数指针定位，Io trace，native函数so文件定位等
10. [feature] ptrace 调试的时，干掉tracePid痕迹，避免tracePid调试对抗。ratel支持免root使用ida调试功能。


### V1.4.7 
1.[feature] kratos引擎，支持root模式下使用ratel，对标太极阳

### V1.4.6
1. [feature] JustTrustMe内置到ratelAPI中
2. [bugfix]  android 10上面 AppComponentFactory兼容性问题fix
3. [bugfix]  不能随便删除不支持的架构so文件
4. [bugfix]  linker failed，can not find libratelnative_32.so
5. [feature] ratel内置脱壳机功能

### V1.4.5
1. [bugfix] 64为手机，运行在32为模式，调用shell命令失效问题
2. [bugfix] 跨进程调用签名生效问题修复
3. [bugfix] 修复在windows系统运行打包程序，字符集未设置utf8导致乱码问题
4. [feature]ratel脚本支持windows环境
5. [feature]Android6.0之前不支持V2签名，所以需要开启V1签名
6. [bugfix]bugfix for  String more than 65535 UTF bytes long

### V1.4.4
1. [feature] 支持xapk(split apk)(仅在命令行模式生效，web站点无法使用)

### V1.4.3
1. [feature] 支持IO access tracer，方便通过文件访问回溯代码关键点
2. [bugfix] 8.0手机卡屏问题
3. [bugfix] 存储卡白名单增加多个挂载点兼容。
4. [bugfix] 删除nativeHook一个不应该的printf


### V1.4.2
1. [feature] RDP功能支持添加补丁dex，支持一键植入扩展代码
2. [feature] 重编引擎支持ratel二次重编后资源迁移，包括dex代码数据
3. [bugfix] 一个多开对抗点
4. [feature] 支持Frida环境：TODO
5. [bugfix] WhatsApp代码注入失败
6. [bugfix] 一个多开对抗点，可能导致iccId泄漏和序列号泄漏

### V1.4.1
1. [bugfix] 隔离Ratel模块和原生Xposed模块的调用时机。否则多账号切换和Xposed模块(微X模块)可能冲突

### V1.4.0
1. [bugfix] batch hook status change failed
2. [bugfix] RatelSchduler模块 RM安装但是没有打开的情况下，可能导致app闪退。华为部分机型会出现
3. [bugfix] 修复华为Android9首次打开链家崩溃的问题
4. [bugfix] Proguard代码优化导致sandhook ArtMethodSizeTest调用被优化逻辑认为是无效调用，进而在vivo Android 6.0出现artMethod数据结构紊乱，出现hook卡屏
5. [bugfix] android 10访问一个HiddenApi导致app崩溃

### V1.3.9
1. 外放引擎模式API
2. batch hook for ratel framework hook task to decrease stopTheWorld times
3. 设备指纹一个对抗点
4. 热发模块服务器地址修改为https，否则在Android9之后无法访问http接口
5. bugfix: zelda模式下根据pkgname授权问题
6. [bugfix] 修复zelda可能无法加载模块的bug
7. sync sandhook :AndroidQ: disable FastInterpreter
9. [sync sandHook] AndroidQ: filter thread construction hook
10. [sync Sandhook] AndroidR: support Android R dev-preview-1
11. [bugfix] 删除Ratel运行文件之后需要重构Ratel目录结构(在目录结构缓存之后)，否则引发：fake_maps: cannot create tmp file, errno = 2
12. [bugfix] 纯64位模式下，命令行运行32位可执行文件，需要注入32位so文件。比如dex2oat


### V1.3.8
1. [feature] 支持访问模块布局资源
2. [bugfix] 追加发布，fix下厨房Dex DebugInfo重构对抗，下厨房Axml stringPool stringItem增加双Null导致Axml解析错误
3. [bugfix] mac native fake 导致zelda引擎在IO重定向之前访问虚拟空间，引发权限问题，访问失败。app崩溃
4. [feature] 正式支持支持Android10

### V1.3.7
1. [feature] HotModule，热发模块，可远程批量部署模块，便于集群管理
2. [improve] 修复Zelda引擎若干bug


### V1.3.6
1. [improve] 架构优化，移除ArtHook兼容路由层，移除在存在Xposed环境上对Ratel框架的支持，ArtHook框架唯一指定SandHook
2. [improve] 架构优化，抽象RatelRuntime多种引擎入口封装，为Zelda实现提供支持
3. [feature] 支持zelda引擎：https://github.com/virjar/zelda 请注意这是Zelda商业版，拥有超越开源版本的稳定性、安全性、扩展性
4. [bugfix]  追加:兼容xposed module使用AndroidAppHelper,上一个版本由于代码优化模块导致AndroidAppHelper被移除(兼容微X模块)
5. [improve] 支持Xposed模块被ratel感染(可以兼容部分Xposed模块依靠自身是否被hook来判定Xposed环境是否生效)(兼容微X模块)
6. [bugfix]  只在rebuildDex模式下，必须存在entry point class，appendDex和zelda模式下不需要有这个判定(兼容微X模块)
7. [bugfix]  appendDex模式下，无Entry Application需要通过TextUtil判定，以防配置传递长度为0的字符串(兼容微X模块)

### V1.3.5
1. [feature] RDP模块支持
2. [feature] support settings provider
3. [improve] 基站id漂移规则不合理导致百度定位API在省电定位模式下，通过网络定位失败
4. [improve] appendDex模式下，如遇无application场景，需还原application为``android.app.Applicaion``
5. [improve] 默认UncaughtExceptionHandler可能为null，做UncaughtExceptionHandler托管时增加null判断
6. [bugfix] AXml(Panxiaobo)处理UTF8类型的StringPool的时候，AXML格式解析有误，导致当字符串偏长的时候出现``Negative initial size: -43``


### V1.3.4
1. [feature] 支持调度任务，以提供脱离USB控制的UI驱动型自动化任务

### V1.3.3
1. [improve] 网卡硬件地址泄漏
2. [bugfix] rebuild failed when "bogus opcode"
3. [feature] apk打包，支持zipalign
4. [improve] 重构serial fake模块,根据serial在Android不同版本下的规则不同单独抽取serial模拟模型，而非之前和system properties get 逻辑耦合。这可能导致部分情况存在手机序列号信息丢失

### V1.3.2
1. [improve] 框架在android9之后，bypass hidden API policy痕迹处理
2. [bugfix] keygen模块command重构错误
3. [bugfix] 今日头条7.5.5构建失败 java.nio.BufferOverflowException
4. [bugfix] system_properties_hook，导致在华为某机型上面卡白屏

### V1.3.1
1. [bugfix] 部分无application的应用，MainActivity入口解析失败
2. [improve] ROM厂商设备指纹泄漏问题，目前适配小米+华为
3. [improve] 支持contentProvider拦截
4. [bugfix] 修复luckin coffee闪退
5. [sync] sync sandhook: fix can not get threadId when hook Thread

### V1.3.0
1. [bugfix] rebuildDex如遇method带annotation，将会导致指令解析不成功，引发构建失败
2. [bugfix] 插件解析过程，未将配置内容存储到ratel全局配置文件，Android解析meta过程将会猜测配置项类型，导致数字格式的字符串无法被识别
3. [bugfix] dex index overflow的时候，将要进行dex拆分。ratel拆分过程识别supperclass逻辑有误
4. [bugfix] #13 有道词典某加固版本构建失败,该加固版本存在baksmali对抗混淆。dex中存在错误的smali指令，但是该指令不会被执行，却会导致baksmali解码指令异常
5. [improve] #12 使用AXmlConverter替代四哥的XmlEditor  并且支持将app的debug开关打开，ratel app开始支持免root调试


### V1.2.9
1. [improve] 不允许app读取系统systemSetting定义以外的数据，避免app矩阵泄漏设备指纹id
2. [bugfix] rebuildDex如遇寄存器使用超V15，无法直接访问p0,p1寄存器，导致FacebookLite构建失败
3. [improve] 通过DexMerger替换smali整体的dex重编译，大幅提升rebuildDex的构建速度，感谢skyun1314
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