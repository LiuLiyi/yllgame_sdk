## 版本更新纪录
### SDK版本1.0.5.6
1. 新增繁体中文
### SDK版本1.0.5.5
1. 新增删除游戏角色api
2. SDK内部逻辑优化
3. SDK新增简体中文
4. 新增设置服务协议和隐私协议api
### SDK版本1.0.5.4
1. 新增获取初始化语区
### SDK版本1.0.5.3
1. 新增推送跳转指定界面功能
2. 新增手机验证码发送新增校验步骤
3. SDK内部优化
### SDK版本1.0.5.2
1. 新增获取用户消息数量变更通知函数
2. 新增展示客服页面函数
### SDK版本1.0.5.1
1. 新增展示举报消息页面api(4.2.3 展示举报消息页面)
2. 新增展示举报语聊房消息页面api(4.2.4 展示举报语聊房消息页面)
3. 新增展示网络检测页面api(11.1 展示网络检测页面)
4. 上传照片函数名修改 ( 上传图片)
5. 新增土耳其语, 语言简称 "tr"
6. SDK内部优化
7. 新增三方库//图片加载库
 ``` 
 //华为推送
 api api 'com.huawei.hms:push:6.5.0.300'
 api 'com.github.bumptech.glide:glide:4.13.2'
 annotationProcessor 'com.github.bumptech.glide:compiler:4.13.2'
 ```
8. 新增华为推送
9. 移除Bugly
10. 新增图片上传权限
11. 更新 
```
api "net.zetetic:android-database-sqlcipher:4.5.2"
```
### SDK版本1.0.5
1. 新增展示手机号绑定页面(4.2.2展示手机号绑定页面)
###  SDK版本1.0.4.3
1. 新增取消注销账号功能
2. SDK内部优化
### SDK版本1.0.4.2
1. 新增Facebook跳转公共界面api、增加获取活动api、增加获取绑定状态api
2. 新增设置界面
3. Facebook更新最新版本适配
```
api 'com.facebook.android:facebook-login:13.0.0'
api 'com.facebook.android:facebook-share:13.0.0'
```
### SDK版本1.0.4.1
1. 新增账号被封禁自助解封
2. 新增推送增根据环境推送
### SDK版本1.0.4
1. 新增语聊举报
2. 新增阿里云图片上传
 ```
api 'com.aliyun.dpa:oss-android-sdk:2.9.9'
```
### SDK版本1.0.3.3
1. 新增举报入口
2. 新增支付订单补单通知
### SDK版本1.0.3.2
1. 新增埋点直接上报到AppsFlyer方法
### SDK版本1.0.3.1
1. 新增埋点直接上报到Firebase和Facebook方法
### SDK版本1.0.3
1. 第三方依赖库升级
```
//Facebook登陆（9.0.0版本升级到11.0.0）
api 'com.facebook.android:facebook-login:11.0.0'
//新增Facebook分享 
api 'com.facebook.android:facebook-share:11.0.0'
```
2. 新增Facebook好友列表和分享
3. 修改支付pay方法增加支付类型skuType
4.  SDK回调设置统一接口
### SDK版本1.0.2.2
1. 增加弱联网模式设置
## SDK版本1.0.2.1  
1. BuglySDK接入 
2. 客户端埋点事件调整 
3. FAQ增加直接跳转至在线客服的入口按钮 
4. 增加静默游客登录接口
5. 数据统计SDK版本更新 
```
api 'com.appsflyer:af-android-sdk:6.2.3@aar' 
api 'com.appsflyer:oaid:6.2.4' 
api 'com.android.installreferrer:installreferrer:2.2'
```
## SDK版本1.0.2 
1. 增加手机号绑定登陆 
2. 调整登陆弹窗的样式 
3. 增加了客户端埋点统计 
4. 华为渠道登陆、支付接入 
5. gradle 增加配置 api 'com.google.android.material:material:1.3.0'库
## SDK版本1.0.1 
1. 新增登陆界面,登陆广播新增用户昵称修改通知和退出登录，type更新，删除切换账号，检查账号状态
2. 新增FCM推送
3. 新增设置界面
4. 新增设置SDK语言
5. 新增在线客服
6. SDK限制minSdkVersion到安卓5.0+（Api 21+）和更新第三方SDK库
