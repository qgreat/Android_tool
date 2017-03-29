### launcher3
* 整体布局在launcher.xml里面hotseat是底部的固定的5个app，qsb_bar删除栏，page_indicator指示点，workspace工作页面。
  整体在一个framelayout里面，位置大小可以在DynamicGrid里面layout（）调整。
* 从application进入LauncherAppState，在里面注册广播

* launcher处理实现LauncherModel.Callbacks（），里面有initScreen()等界面处理逻辑
* launcherModel处理业务逻辑，LauncherModel.Callbacks界面逻辑回调接口在launcher里面实现接口内容。

* 增加绑定增加的app:LauncherModel的addAndBindAddedApps方法里

* cropImageAndSetWallpaper 设置壁纸，适配i8s的大小 launcher cropImageAndSetWallpaper

* DynamicGrid里的DynamicGrid（.....）方法适配i8s、i9  icon大小


* 在launcherModel里通过接收广播"hp.intent.action.APP_VISIBLE_CHANGE"隐藏和显示桌面app。
* 自动排序 ， launcher里面的autoArrangeWorkAndHotseat()方法自动排序页面app和底部hotseat的app，
 autoArrange（）自动排序页面app，
* 壁纸，设置，桌面切换，自动排序等定制功能的入口在launcher的 setupViews（）方法里
### kimi桌面
* 切换桌面功能 在appswitchfrag里面增加radiogroup选项按钮，通过SPUtils.putLauncher存储状态，点击时给快易桌面

  发送广播隐藏或显示app，并保存状态，在桌面启动时判断Sp里数据状态（onResume里面），根据状态启动桌面。
* kimi桌面i8s和i9版本src文件相同，打包时用同一个jar文件，由于分辨率不同，资源文件里不同，unity那边的文件也不同，所以打包文件是分开的，
  如果改动了src里的文件内容，更换jar文件即可。
* 
### 小龙人快速阅读 i9/软件组/SRC/app/studio_project/BLinkCard3
* 音频、txt资源文件全在assets下边，分为小班、中班、大班、速算
* 使用第三方框架
  * 数据库Raizlabs.DBFlow或greendao
  * timber  Log工具
  * leakcanary  内存泄露检测工具
  * umeng.analytics 统计
  * butterknife 依赖注解
  * databinding ui绑定
  * otto  事件总线
  * TastyToast 动画的toast
  
 * 注意点
   * 设置字体选项：需要平板/hpdata/fonts/下有字体文件，没有的话就不显示
   * 设置里采用otto事件总线进行通信  Store.bus.post(msg)发送消息，在 onMsgChange(Message msg)里面接收消息，（不是handle）
   * 拖动排序是自己实现的（[参考这篇文章](http://www.2cto.com/kf/201609/548687.html)），没有用第三方框架，Collections.swap(mDatas, from, target);        交换
   * 设置信息全部封装在SettingBean中，
 
 ### 上网管理服务 
   * 主要工作改bug，幼教家教机都使用 h30项目下的nethandle
   * ApkFilterService findAddr（）方法判断屏蔽的第三方软件安装助手
   * HistoryDb line361 开放网址（例：带cloud的网站）
   * HistoryDb getSPMode()里面通过判断Build.MODEL区分机型，兼容家教机和kimi平板
   
 ### 语音助手
 * 去讯飞语音官网注册账号，开通需要的功能，拿到IFLYTEK_APPKEY，在manifest注册，拷入jar包（参照官方文档、demo），.so文件。
 * 在VoiceHelperActivity里初始化IflytekUnderstander（里面封装mTextUnderstander、SpeechUnderstander）、IflytekSynthesizer（SpeechSynthesizer），通过发送广播通知外部状态改变，RecognizerReceiver接收识别消息，SynthesizerReceiver接收合成消息，
 * 初始化动画animatorView（frame动画，可能造成oom），动画之后设置onTouch事件，在down事件里接收语音识别、结束speak、开始动画、调用understander.understanderVoice();在up方法里判断时间是否足够，否则接收识别，结束动画，通过位置判读是否取消识别，如果正常，结束识别（开始识别）
 * 之后会调用doRecognizerResult（）方法，将返回的json数据封装进FilterResult里面，如果getRc()结果返回0，调用doFilter()处理结果，如果不是0，则拿到文本自己处理。  
   
   
   
   
   
