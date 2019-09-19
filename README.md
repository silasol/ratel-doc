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


请注意，暂时不支持 android<=4.4 or android>=10 重要的事情说三遍
# 分模块文档
ratel 大概分为以下模块

## [ratel server感染apk的后台服务](2.ratelServer.md)
## [ratel管理器，类似xposed intalller](3.ratelManager.md)
## [ratelAPI 通过API编程进行模块开发和ratel功能定制](4.ratelAPI.md)

# 基本使用流程
1. 使用ratel在线平台感染app： http://ratel.virjar.com/
在这里上传apk，等待系统处理完成，下载apk，安装到手机即可
2. 下载并安装ratelManager
ratelManager的apk，在ratel server的首页有下载地址
3. 编写Xposed模块插件（也可以叫做Ratel模块插件）,安装到手机，并在RatelManager中配置即可

# 发布记录
## 引擎发布记录

### V1.2.2
1. 支持通过ratel-api实现指纹数据填充
2. 支持通过ratel-manager获取真实的设备唯一id
3. 深度设备指纹对抗
4. 跨进程IPC过程，签名替换，解决授权登陆出现非正版应用的问题(请注意，跨进程调用需要同步感染双方apk，且建议安装RatelManager，且RatelManager版本大于1.0.4)


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
4. 跨进程IPC过程，签名替换，解决授权登陆出现非正版应用的问题(请注意，跨进程调用需要同步感染双方apk，且建议安装RatelManager，且RatelManager版本大于1.0.4)
5. 迁移sandhook代码，支持AndroidQ
6. 迁移sandhook代码，支持safe hook static method

## server发布记录
### V1.0
预计2019年09月14日正式上线，敬请期待。官网地址：http://ratel.virjar.com