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


