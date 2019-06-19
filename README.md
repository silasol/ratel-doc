# ratel-doc

# 捐赠
求请我和咖啡，不要瑞幸优惠券，要钱
![alipay](img/reward.jpg)

# 文档正文
平头哥（ratel）是一个能够在免root手机中使用xposed的框架。支持5.0-9.0。使用ratel可以任意控制app的行为，如同开启上帝模式一样。他有如下特性：

1. 完全支持9.0（理论上Android10也是支持的，只是没有测试），对比xposed框架本身在8.0的时候就各种使用不顺畅了
2. 免root运行，ratel运行，不需要root后者解锁bootloader。现在新手机获得root权限越来越困难。ratel可以实现免root下DIY你的手机
3. 痕迹隐藏，对于root，xposed等常见风险操作检测，ratel框架从原理上就避免了他们。就我个人测试，一些银行类app可以正常被ratel控制。
4. 无法影响全局，受限于实现原理，ratel无法影响系统。只能影响正常安装在手机里面的app。
5. ratel模块开发，无需重启手机即可生效（就xposed框架本身来说，也可以通过一定手段达到这个效果，参考我的另一个项目[ucrack](https://gitee.com/virjar/ucrack)）
6. API扩展，ratel在xposed framework层面之上，提供了ratel独有的一些特殊API。可以实现一些xposed本身不提供的强大功能。当然，如果你按照标准的xposed API开发，那么ratel支持绝大部分xposed特性
7. external&embed模式共存，在免root环境下，如果把模块植入到app内部，那么可以使得这个app可以被广大普通用户直接使用。因为DIY功能的使用门槛降低到了只需要下载AndroidAPK了。可以理解为您家老人都可以正常使用ratel了
9. 内置设备模拟+一健新机+多账号共存功能。这个功能各位不要滥用，比较狂暴

# Ratel Manager

RatelManager的作用类似XposedInstaller，实现对Ratel模块的管理，不过需要注意的是，embed模式下的模块，可以在没有RatelManager的环境下直接运行。

![img](img/ratelManager1.jpg)![img](img/ratelManager2.jpg)

# 开发文档
如果只是使用标准的xposed功能，那么引入标准xposed依赖即可
```
dependencies {
    compileOnly 'de.robv.android.xposed:api:82'
}
```

如果希望使用ratel提供的扩展api，那么可以选择依赖ratelAPI
```
repositories {
    maven {
        name "contralSnapshot"
        url "https://oss.sonatype.org/content/repositories/snapshots/"
    }
}
dependencies {
 compileOnly 'com.virjar:ratel-api:1.0.0-SNAPSHOT'
}
```
请注意，ratel的依赖发布到mavenCentral的。生产环境依赖可以选择
```
repositories {
mavenCentral()
}
```
或者使用阿里云镜像提速
```
    repositories {
        maven {
            name "aliyunmaven"
            url "https://maven.aliyun.com/repository/public"
        }
        maven {
            name "aliyunGoogle"
            url "https://maven.aliyun.com/repository/google"
        }

    }
```

## ratel定制API总线
接口一览
```
package com.virjar.ratel.api;

import android.annotation.SuppressLint;
import android.content.Context;

public class RatelToolKit {
    /**
     * 1。 以下对象，是暴露给调用方的额外API，可以通过他们操作ratel提供的额外功能（除开xposed本身功能之外）
     */
    //全局的一个context，context是调用Android系统功能的重要对象。有这个对象之后，无需手动通过拦截attach的方式获取context
    @SuppressLint("StaticFieldLeak")
    public static Context sContext = null;

    /**
     * ratel框架的配置信息，代表了ratel编码、打包、运行过程产生的一些特定flag
     */
    public static RatelConfig ratelConfig = null;

    /**
     * ratel支持对文件进行重定向
     */
    public static IORelocator ioRelocator = null;

    /**
     * 当前进程名称
     */
    public static String processName = null;

    /**
     * 当成packageName
     */
    public static String packageName = null;


    /**
     * 虚拟化环境功能支持
     */
    public static VirtualEnv virtualEnv = null;

    /**
     * 虚拟化环境下，sdcard将会被隔离，导致无法往sdcard写入数据。但是如果ratel模块期望通过sdcard和其他app交换数据，那么需要通过一个sdcard白名单进行放行
     */
    public static String whiteSdcardDirPath = null;
    /**
     * @hidden
     */
    public static RatalStartUpCallback ratalStartUpCallback = null;

    public static void setOnRatelStartUpCallback(RatalStartUpCallback ratelStartUpCallback) {
        RatelToolKit.ratalStartUpCallback = ratelStartUpCallback;
    }


    /**
     * 2。 以下以下对象，是用户层不需要关心的。我也不会做解释
     */
    public static HookProvider usedHookProvider;

    public static String TAG = null;


}

```

# 和ratel类似的产品

竞争使得我们更加优秀，即时竟品还不认为我们是他竞品。

1. 本方案始祖，太极: https://taichi.cool/README_CN.html
2. Xpatch，没有开源，但是可能是最容易反编译出来的一个产品： https://github.com/WindySha/Xpatch
3. 傀儡管理器,作者很搞笑： http://qssq666.cn/2019/04/19/puppet_publish/
4. 应用转生，目前还在作者自己的技术群里演示界面，实际上方案效果不明，暂无法评判。

# 如何使用
1. 使用ratel在线平台感染app： 
http://ratel.virjar.com/#/login?redirect=%2F

在这里上传apk，等待系统处理完成，下载apk即可

2. 下载并安装ratelManager
https://virjar-comon.oss-cn-beijing.aliyuncs.com/RatelManager_1.0.0.apk

3. 编写Xposed模块插件（也可以叫做Ratel模块插件）,安装到手机，并在RatelManager中配置即可


# 关于内测
ratel目前处于内测阶段，仅对小部分用户开放。如想获得内测资格，联系qq:1076208143 weixin:odengweijia
