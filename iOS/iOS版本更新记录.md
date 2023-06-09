
## SDK版本更新纪录 
- v1.0.5.7
1. 新增 `获取商品本地化信息` 函数
2. 新增协议更新
3. SDK内部优化

- v1.0.5.6
1. SDK新增繁体中文语言
2. SDK语言枚举名称调整
3. SDK内部优化

- v1.0.5.5
1. 新增 `删除游戏角色` 函数
2. 新增 `游戏服务协议` 和 `游戏隐私协议` 配置属性
3. SDK新增简体中文语言
4. SDK登录页面调整
5. SDK相册调整
6. SDK最低支持版本提升至`iOS11.0`
7. SDK内部逻辑优化


- v1.0.5.4 
1. 新增 `获取初始化语区` 函数
2. pod GoogleSignIn 版本升级, 新版本为 `pod 'GoogleSignIn', '6.2.4'`


- v1.0.5.3 
1. 新增 `推送功能` 及相关函数
2. 移除 `- (void)yg_application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler;` 函数
3. 手机验证码发送新增校验步骤
4. SDK 内部优化

- v1.0.5.2
1. 新增 `获取用户消息数量变更通知` 函数
2. 新增 `展示客服页面` 函数
3. SDK 移除 armv7 架构
4. Xcode 需要升级到 Release 14.0+

- v1.0.5.1
1. 添加 `展示举报消息页面` 函数
2. 添加 `展示举报语聊房消息页面` 函数
3. 添加 `展示网络检测页面` 函数
4. `上传照片函数名` 修改
5. 新增 `Firebase/Crashlytics` 功能 
6. 新增 `定位权限` 
7. 移除 `pod 'Bugly', '~> 2.5.90'`, SDK移除 YllGameSDK 文件下的 buglyAppId 属性, 工程移除shell脚本(具体路径: 对应工程target->Build Phases->Run Script)
8. 新增 `土耳其语`, 语言简称 "tr"
9. 提升 Firebase pod版本, `Firebase/Analytics` 和 `Firebase/Messaging` Pod 版本从 v6.34.0 提升至 v8.13.0
10. SDK内部优化

- v1.0.5
1. 添加 `展示手机号绑定页面` 函数
2. SDK内部优化

- v1.0.4.3
1. 新增 `注销账号功能`
2. SDK内部优化

- v1.0.4.2
1. 新增 `获取活动接口` 函数 
2. 新增 `获取手机号绑定状态` 函数 
3. 新增 `跳转到指定用户的facebook主页` 函数 
4. 新增 `展示账号绑定页面` 函数 
5. 提升 facebook pod版本, `FBSDKLoginKit` 和 `FBSDKShareKit` 从 v9.1.0 提升至 v12.3.2
6. 删除 `打开客服接口`函数

- v1.0.4.1
8. `创建订单新增orderType参数`
9. SDK内部优化

- v1.0.4.0 
1. 新增 `房间和房间用户举报`函数 
2. 新增 `相册选择图片上传图片`函数 

- v1.0.3.3  
1. 新增 `举报`函数 
2. 新增 `补单成功回调` 函数

- v1.0.3.2  
1. 新增 `埋点直接上报到AppFlyer`函数和`同时上传到Appflyer&Firebase&Facebook`函数 

- v1.0.3.1  
1. 新增 `埋点直接上报到Firebase和Facebook`函数 

- v1.0.3
1. 新增 `订阅功能` 函数

- v1.0.2.2  
1. 新增 `强弱网判断` 功能

- v1.0.2.1  
1. `BuglySDK接入` 
2. 客户端埋点事件调整 
3. FAQ增加直接跳转至在线客服的入口按钮
4. 新增 `游客静默登录`函数 
5. 新增 `获取SDKVersion`函数和`SDKBuild`函数 
          
- v1.0.2 
1. 新增 `手机号绑定登陆`
2. 新增`客户端埋点统计`
3. 调整登陆弹窗的样式
  
- v1.0.1 
1. FAQ&在线客服模块
2. SDK设置界面整合
3. 推送功能
4. 增加登陆弹窗
5. 增加Yalla联动活动入口
