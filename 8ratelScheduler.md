# ratel调度任务
调度任务是ratel底层支持的一套可以在目标app上面实现定期执行任务的一种机制。这主要是给SupperAppium提供便利支持，也即为UI驱动型任务提供框架功能。
不过实际上ratel scheduler本身这是一个框架功能，到底能做什么还是依赖使用方对框架的理解和对业务的想象。

## ratelScduler设计目的
如果仅仅是提供一种定时任务的功能，依靠Android本身的timer，或者AlarmManager完全可用。但是对于UItask来说并不足够。主要原因如下：
 1. 考虑任务长期稳定执行,(app保活，进程永生)。带有代码注入的app相对于普通app来说重很多，包括多套api使用，hook方式导致的状态和对象维护管理。hook侵入导致app本身闪退了增加等。以及可能不合理的代码导致app卡死。如果发生app闪退或者卡死，那么我们的框架同样无法运行。框架无法突破Android本身系统权限，无法自动重启自身，或者检测自己处于僵死状态然后自杀。
 2. 一机多号机制下，设备切换导致的app重启限制。app设备切换必须重启app，这是ratel工作机制限制。ratel框架运行在目标app内部，如果app由于重启被杀死，那么没有人可以把app重新拉起。

 考虑如下场景：
我们有一个抢单业务，通过ratel一机多号放大设备，需保持多账号并行登陆，提供设备指纹模拟，位置模拟等功能。实际流程如下描述：
1. 通过服务器下发配置，获取任务详情信息，以及设备基础信息。包括登陆账号、登陆密码、代理ip配置、业务参数
2. app端收到上述信息，根据代理出口ip，确定模拟的真实经纬度定位（考虑定位、GPS、IP出口、Wi-Fi列表、基站出口多端统一问题）、根据模拟经纬度查询该位置环境数据（包括Wi-Fi列表，基站出口）。
3. app根据账号信息创建和该账号绑定的设备，并灌入模拟设备指纹内容。
4. 启动app，app将会运行于模拟沙箱环境下，app将会表现为第一次打开app，即设备首次激活。
5. app启动后，ratel插件功能生效，驱动app进行账号注册（或者登陆）
6. 执行抢单任务

可以看到，（1、2、3）并不是在app运行的时候执行的。此时由于都没有代码运行，这些步骤理论上是无法实现的。所以ratel调度任务主要提供(1、2、3）相关功能支持，他将插件机制引入到ratelManager，借用RatelManager进程身份执行插件扩展任务。并和后续流程无缝衔接。

## 使用

配置文件：在插件apk项目的asset目录增加文件：``ratel_scheduler.json``,配置内容如下
```json
{
    //任务实现类，是java的class，比如是com.virjar.ratel.api.scheduler.RatelTask的实现类
    "taskImplementationClassName": "com.virjar.ratel.mt.MtTaskScheduler",
    //任务执行的时间规则，配置使用cron表达式规则，cron表达式规则可以网上查询。有一个特殊的规则就是ratelScheduler不支持秒级调度，对于秒级配置无法起到效果。这是因为正常一个app打开就是秒级耗时的，秒级调度应该使用进程内的调度方案。ratelScheduler的调度时间间隔以分钟为单位
    "cronExpression": "* * * * * ?",
    //任务超时时间，单位为秒，也即当任务执行达到超时时间后，框架仍然没有收到任务结束消息，那么框架会强行干预。
    //框架动作包括强行进行下一次调度、强杀目标app进程等。该配置不是强制填写，默认值为10分钟
    "maxDuration":600,
    //该任务是否需要强制重启app，如果为true，那么任务调度前会杀死app.该配置默认为true
    "restartApp": true
}
```
如果想要要配置多个定时任务，那么通过json数组配置上述json即可。需要注意的是，如果多个定时任务在时间点存在冲突，那么定时任务无法并行执行。框架可能随机选择某一个定时任务。


## ``com.virjar.ratel.api.scheduler.RatelTask``接口定义
定义描述如下：
```
public interface RatelTask {

    /**
     * 加载调度任务参数，每次调度任务执行之前，可能需要初始化任务相关参数。
     * <br>
     * 非常重要的是，这次调用发生在manager进程,你不能将加载好的数据放到静态变量或者当前进程内存中，否则整个调度过程可能由于app重启导致内存数据丢失
     *
     * @return 任务参数map, KV均为字符串,可为空
     */
    Map<String, String> loadTaskParams();

    /**
     * 执行调度任务
     * <br>
     * 该任务发生在slave app中
     *
     * @param params 调度任务参数
     */
    void doRatelTask(Map<String, String> params);
}
```

## 任务结束
ratelSchedulerTask定义的调度任务是异步执行的，所以任务调用触发成功并不会标记任务执行完成。任务结束需要业务放主动通知。
通知方法如下：``RatelToolKit.schedulerTaskBeanHandler.finishedMTask();``

如果任务执行完成，但是业务并没有主动通知框架任务执行结束，那么框架将会等待到任务超时

## 其他
RatelScheduler模块不能在embed模块生效，且不能在全局模块生效(需要配置for_ratel_apps='singleAppPackage')。
RatelScheduler模块工作在 RatelEngineVersion>=1.3.4 和 RatelManagerVersion>=1.0.7 和RatelApiVersion>=1.3.2

## 关于doRatelTask生命周期
doRatelTask调用时机发生在两个时间点
1. 如果是restartApp=true，也即app重启类型任务，doRatelTask发生在app启动后，app逻辑执行之前。和xposed模块代码回调发生在同一时机。你可以在doRatelTask完成任何Xposed模块加载时的任务逻辑(实际上在框架层面doRatelTask调用就在Xposed模块加载之后)
2. 如果是restartApp=false,也即app即时类型任务，doRatelTask发生在app运行的任何时机。框架会通过IPC机制，直接调用目标进程的回调函数