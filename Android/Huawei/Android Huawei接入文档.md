# Android Huawei接入文档

## SDK结构
支持平台：Android</br>
系统要求: Android6.0+ </br>
环境要求: Android Studio</br>
支持语言：阿语，英语，土耳其语，简体中文</br>

## 1.接入流程
### 1.1集成AAR包
首先把AAR包复制到项目的lib目录下，然后在主项目APP的工程下build.gradle 中的dependencies加入
 ``` Groovy
 //SDK基础库
 implementation(name: 'YllGameSdk', ext: 'aar') 
 //geetest
 api(name: 'geetest_captcha_android', ext: 'aar')
 ```
 ### 1.2设置项目的libs文件目录和过滤so
 在主项目APP的工程下build.gradle 中的android加入
  ``` Groovy
 repositories {
        flatDir {
            dirs 'libs'
        }
    }
    packagingOptions {
        doNotStrip "*/*/libijiami*.so"
    }
 ```
 ### 1.3 导入SDK中所需第三方库
   ``` Groovy
   //Android X支持库  必须添加
    api 'androidx.appcompat:appcompat:1.2.0'
    api 'com.google.android.material:material:1.3.0'
    //okhttp网络请求库 必须添加
    api("com.squareup.okhttp3:okhttp:4.9.0")
    //gson数据解析库 必须添加
    api 'com.google.code.gson:gson:2.8.5'
    //Facebook登陆依赖库 必须添加
    api 'com.facebook.android:facebook-login:13.0.0'
    //Facebook分享
    api 'com.facebook.android:facebook-share:13.0.0'
    //数据库依赖库 必须添加
    def room_version = "2.2.5"
    api "androidx.room:room-runtime:$room_version"
    annotationProcessor "androidx.room:room-compiler:$room_version"
    api "net.zetetic:android-database-sqlcipher:4.5.2"
    //数据统计依赖库 必须添加
    api 'com.appsflyer:af-android-sdk:6.2.3@aar'
    api 'com.appsflyer:oaid:6.2.4'
    api 'com.android.installreferrer:installreferrer:2.2'
    api 'com.aliyun.dpa:oss-android-sdk:2.9.9'
    //华为登陆 hms
    api 'com.huawei.hms:hwid:5.2.0.300'
    api 'com.huawei.hms:ads-identifier:3.4.39.302'
    //华为支付 hms
    api 'com.huawei.hms:iap:5.1.0.300'
    //华为推送
    api 'com.huawei.hms:push:6.5.0.300'
    //图片加载库
    api 'com.github.bumptech.glide:glide:4.13.2'
    annotationProcessor 'com.github.bumptech.glide:compiler:4.13.2'
    //华为carsh
    api 'com.huawei.agconnect:agconnect-core:1.7.2.300'
    api 'com.huawei.agconnect:agconnect-crash:1.7.2.300'
    api 'com.huawei.agconnect:agconnect-crash-native:1.7.2.300'
 ```
 ### 1.4 设置项目build.gradle
   ``` Groovy
   buildscript {
    repositories {
        google()
        jcenter()
        maven {url 'https://developer.huawei.com/repo/'}
    }
    dependencies {
        ...
       // 增加agcp配置。 华为
        classpath 'com.huawei.agconnect:agcp:1.4.2.300'
        classpath 'org.jetbrains.kotlin:kotlin-gradle-plugin:1.5.30'
    }
}

allprojects {
    repositories {
        google()
        jcenter()
        maven {url 'https://developer.huawei.com/repo/'}
    }
}
 ```
## 2.项目配置，初始化

### 2.1初始化Application
在项目的application的onCreate函数中调用SDK的初始化函数，并且调用SDK的设置语言函数。
- SDK初始化函数：``` YllGameSdk.getInstance().init(); ```
``` java 
    /**
     * 初始化
     *
     * @param application
     * @param appId            游戏的gameAppId
     * @param googleClientId   游戏的googleClientId
     * @param appsFlyersDevKey 游戏的appsFlyersDevKey
     */
    public void init(Application application, String appId, String googleClientId, String appsFlyersDevKey)
 ```
 - SDK 设置语言集合函数：``` YllGameSdk.setLanguageList(); ```
``` java 
    /**
     * 设置SDK支持语言，必须调用在init之前
     *
     * @param languageList 游戏支持语言集合 现支持 ar 阿语 en英语 tr土耳其语 zh简体中文 该集合默认第一个是SDK的默认语言
     */
    public static void setLanguageList(List<String> languageList) 
 ```
 - SDK设置语言函数函数：``` YllGameSdk.setLanguage(); ```
``` java 
    /**
     * 设置SDK默认语言
     *
     * @param localLanguage ar 阿语 en英语 tr土耳其语 zh简体中文
     */
    public static void setLanguage(String localLanguage)
 ```
 - 调用设置弱联网函数为：``  YllGameSdk.setNetMode(int mode); ``
``` java
     /**
     * 设置SDK联网默认模式 默认强联网模式
     *
     * @param mode  YGConstants.SDK_STRONG_NET 强联网 YGConstants.SDK_WEAK_NET 弱联网
     */
    public static void setNetMode(int mode)
```
- 调用当前游戏服务协议函数为：``  YllGameSdk.setTermsService(String privacyPolicyURL); ``
``` java
    /**
     * 当前游戏服务协议
     *
     * @param termsServiceURL 游戏服务协议网址
     */
    public static void setTermsService(String privacyPolicyURL)
```
- 调用当前游戏隐私政策函数为：``  YllGameSdk.setPrivacyPolicy(String privacyPolicyURL); ``
``` java
    /**
     * 当前游戏隐私政策
     *
     * @param privacyPolicyURL 游戏隐私政策网址
     */
    public static void setPrivacyPolicy(String privacyPolicyURL)
```
**（注：项目的application要在AndroidManifest中注册，项目中初始化参数的key 找运营方）**
### 2.2配置Facebook
在项目中的AndroidManifest中添加
``` xml
        <activity
            android:name="com.facebook.FacebookActivity"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name" />
        <activity
            android:name="com.facebook.CustomTabActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="@string/fb_login_protocol_scheme" />
            </intent-filter>
        </activity>
        <meta-data
            android:name="com.facebook.sdk.ApplicationId"
            android:value="@string/facebook_app_id" /> 
```
``` xml
    <string name="facebook_app_id" translatable="false">15793xxxxxxxxxx</string>
    <string name="fb_login_protocol_scheme" translatable="false">fb15793xxxxxxxxxx</string>
```
**（注：facebook_app_id和fb_login_protocol_scheme 在Android项目string里，需要接入者自行变更为游戏的Facebook的APPID）**
### 2.3注册登陆Receiver
在项目中的AndroidManifest中注册
``` xml
        <receiver
            android:name="游戏包名.ygapi.YGLoginReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="com.yllgame.sdk.loginReceiver" />
            </intent-filter>
        </receiver>
```
``` java
public class YGLoginReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        //Constants.BROADCAST_RECEIVER_LOGIN_ACTION SDK中登陆的action
        if (intent.getAction() == YGConstants.BROADCAST_RECEIVER_LOGIN_ACTION) {
            //拿到登陆之后的用户信息
            GameUserInfoEntity userInfoEntity = (GameUserInfoEntity) intent.getExtras().getSerializable(YGConstants.BROADCAST_RECEIVER_LOGIN_INFO_KEY);
            //该示例中通过EventBus通知并且更新主界面的更新 具体的结合自身需求修改
            if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_LOGIN_ACCOUNT_SUCCESS) {
                //登陆成功
                //需要设置语言
                YllGameSdk.setLanguage(userInfoEntity.getLastRegion());
            } else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_LOGIN_ACCOUNT_FAIL) {
                //登陆失败
            } else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_FAIL_ACCOUNT_REMOTE) {
                //账号异地登录 SDK内部会有弹窗 必须退出到登陆界面清除用户信息
            } else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_FAIL_ACCOUNT_BLOCKED) {
                //账号被封 SDK内部会有弹窗 退出到登陆界面
            } else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_FAIL_TOKEN_OVERDUE) {
                //账号Token过期 退出到登陆界面
            } else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_ACCOUNT_CHANGE_USERNAME) {
                //修改昵称成功
            } else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_ACCOUNT_LOGIN_OUT) {
                //退出登录
            }else if (userInfoEntity.getType() == GameUserInfoEntity.TYPE_FAIL_ACCOUNT_EXPIRED) {
                //账号过期
            }
        }
    }
}
```
注：项目中所有登陆以都会通过广播通知并且在下发用户信息，YGLoginReceiver为固定写法，该广播放在项目包名.ygapi下
退出登录要退出到登陆界面并且清除本地用户信息
### 2.4注册YGReceiver
在项目中的AndroidManifest中注册
``` xml
        <receiver
            android:name="游戏包名.ygapi.YGReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="com.yllgame.sdk.payDropOrderReceiver" />
             <action android:name="com.yllgame.sdk.accountBindReceiver" />
            </intent-filter>
        </receiver>
```
``` java
public class YGReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        if (intent.getAction() == YGConstants.BROADCAST_RECEIVER_PAY_DROP_ORDER_ACTION) {
            String orderId = intent.getExtras().getString(YGConstants.BROADCAST_RECEIVER_PAY_ORDER_ID_KEY);
            LogUtils.logEForDeveloper("游戏充值订单编号：" + orderId);
        } else if (intent.getAction() == YGConstants.BROADCAST_RECEIVER_ACCOUNT_BIND_ACTION) {
            GameUserAccountBindEntity gameUserAccountBindEntity = (GameUserAccountBindEntity) intent.getExtras().getSerializable(YGConstants.BROADCAST_RECEIVER_ACCOUNT_BIND_INFO_KEY);
            /**
             *     private int isBindFacebook = 1 绑定facebook
             *     private int isBindGoogle = 1 绑定谷歌
             *     private int isBindPhone = 1 绑定手机
             *     private int isBindHuaWei = 1 绑定华为
             */
        }else if (intent.getAction() == YGConstants.BROADCAST_RECEIVER_LANGUAGE_ACTION) {
            String region = intent.getExtras().getString(YGConstants.BROADCAST_RECEIVER_LANGUAGE_INFO_KEY);
            LogUtils.logEForDeveloper("获取到初始化语言：" + region);
        }
    }
}
```
注：YGReceiver为固定写法，该广播放在项目包名.ygapi
### 2.5 设置allowBackup配置
``` android:allowBackup="false" ```</br>
**注：新生成的项目allowBackup为true须在项目的AndroidManifest中application设置allowBackup为false**
### 2.5 华为Carsh
#### 1. 设置agcp参数
```
...
//app的
agcp{
    symbolUpload = true
    debug = false
    //设置so文件
    debugSoDirectory = '../FirebaseNDKTest/app/build/intermediates/merged_native_libs/debug/out/lib'
    //Android studio ndk
    ndkDirectory = 'D:/AndroidSDK/ndk/22.1.7171670'
}
android{
...
}
```
#### 2. 执行gradle任务
![image](https://user-images.githubusercontent.com/19358621/199694652-f9ee68be-3962-4dae-8362-8f14f4c32991.png)
**看见控制台输出：--I- Upload symbol file success, appVersion is , appid is , variantName is release
则表示成功**
### 2.7 YallaGameSDK横竖屏设置
 * 如果游戏为竖屏，在AndroidManifest.xml内添加如下配置
```
        <activity
            android:name="com.yllgame.sdk.ui.AccountManagerActivity"
            android:configChanges="orientation|screenSize"
            android:exported="false"
            android:screenOrientation="portrait"
            android:theme="@style/YllGameSdkActivityTheme" >
        </activity>
        <activity
            android:name="com.yllgame.sdk.ui.AccountBindActivity"
            android:configChanges="orientation|screenSize"
            android:screenOrientation="portrait"
            android:exported="false"
            android:theme="@style/YllGameSdkActivityTheme" >
        </activity>
```
 * 如果游戏为横屏，在AndroidManifest.xml内添加如下配置
```
        <activity
            android:name="com.yllgame.sdk.ui.AccountManagerActivity"
            android:configChanges="orientation|screenSize"
            android:exported="false"
            android:screenOrientation="landscape"
            android:theme="@style/YllGameSdkActivityTheme" >
        </activity>
        <activity
            android:name="com.yllgame.sdk.ui.AccountBindActivity"
            android:configChanges="orientation|screenSize"
            android:screenOrientation="landscape"
            android:exported="false"
            android:theme="@style/YllGameSdkActivityTheme" >
        </activity>
```
 * 如果游戏为跟随传感器横屏，在AndroidManifest.xml内添加如下配置
```
        <activity
            android:name="com.yllgame.sdk.ui.AccountManagerActivity"
            android:configChanges="orientation|screenSize"
            android:exported="false"
            android:screenOrientation="sensorLandscape"
            android:theme="@style/YllGameSdkActivityTheme" >
        </activity>
        <activity
            android:name="com.yllgame.sdk.ui.AccountBindActivity"
            android:configChanges="orientation|screenSize"
            android:screenOrientation="sensorLandscape"
            android:exported="false"
            android:theme="@style/YllGameSdkActivityTheme" >
        </activity>
```
## 3.SDK Api接口
### 3.1 登陆
- SDK调起登陆函数：``` YGLoginApi.getInstance().login(); ```
``` java
    /**
     * 登陆界面
     *
     * @param activity 当前Activity
     */
    public void login(Activity activity)
```
### 3.2 游客静默登陆
- SDK调起静默游客登陆函数：``` YGLoginApi.getInstance().silentGuestLogin(); ```
``` java
    /**
     * 静默游客登录
     */
    public void silentGuestLogin()
```
### 3.3 同步角色与回调
 - SDK调起同步用户角色的函数：``` YGUserApi.getInstance().syncRoleInfo(); ```
``` java 
    /**
     * 同步角色信息
     *
     * @param roleId：角色id              ：int 必要参数
     * @param roleName：角色名称          ：string 必要参数
     * @param roleLevel：角色等级         ：int 必要参数
     * @param roleVipLevel：角色Vip等级   ：int 必要 没有默认0
     * @param serverId：角色所在服务器id   ：int 必要参数
     * @param roleCastleLevel：城堡等级   ：int 必要 没有默认0
     * @param callback：同步角色回调:       true 同步成功 false 同步失败
     */
    public void syncRoleInfo(String roleId,
                             String roleName,
                             String roleLevel,
                             String roleVipLevel,
                             String serverId,
                             String roleCastleLevel,
                             YGBooleanCallBack callback)
```
**注：建议游戏每次登陆、角色创建、用户角色升级，用户VIP升级必须调用该函数**
### 3.4 华为内购充值与回调
####  导入华为json文件 配置清单文件信息
- 导入agconnect-services.json文件，文件需找运营方要 </br>
![image](https://user-images.githubusercontent.com/19358621/119936162-4a397080-bfbb-11eb-9364-55d80fee0af8.png)
- 在APP的AndroidManifest.xml 中配置appid和cpid，appid和cpid在json中获取
``` xml
        <meta-data
            android:name="com.huawei.hms.client.appid"
            android:value="appid=xxx"></meta-data>
        <meta-data
            android:name="com.huawei.hms.client.cpid"
            android:value="cpid=xxx"></meta-data>
```
####  导入华为plugins
- 在APP的app/build.gradle文件的plugins目录导入华为插件
```
id 'com.huawei.agconnect'
```
#### 配置华为HMS Core Maven仓库
- 在项目的build.gradle文件夹</br>
![image](https://user-images.githubusercontent.com/19358621/119936866-68ec3700-bfbc-11eb-9db9-e725f3bc63ad.png)
```
maven {url 'https://developer.huawei.com/repo/'}
classpath 'com.huawei.agconnect:agcp:1.4.2.300'
```
- SDK调起华为支付的函数为：`` YGPayApi.pay() ``
``` java 
    /**
     * 支付
     *
     * @param activity      当前activity
     * @param roleId        角色id
     * @param roleServiceId 角色服务器Id
     * @param sku           商品的sku
     * @param cpNo          支付的订单号
     * @param cpTime        支付订单的创建时间
     * @param number        支付的数量 目前为1
     * @param amount        支付的金额
     * @param pointId       支付的充值点
     * @param listener      支付的回调
     */
    public static synchronized void pay(Activity activity, String roleId,
                                        String roleServiceId, String sku,
                                        String cpNo, String cpTime,
                                        String number, String amount,
                                        String pointId, YGPaymentListener listener)
```
### 3.6 打开SDK设置界面
- SDK调起设置界面的函数为：`` YGUserApi.getInstance().showSettingsView ``
``` java
    /**
     * 设置界面
     *
     * @param activity  当前activity
     * @param serviceId 角色服务器id
     * @param roleId    角色id
     */
    public void showSettingsView(Activity activity, String serviceId, String roleId)
```
### 3.7 打开修改昵称界面
 - SDK调起修改昵称的函数：``` YGUserApi.getInstance().showUpdateNickNameDialog() ```
``` java 
    /**
     * 修改用户昵称
     *
     * @param activity
     * @param listener 修改昵称回调
     */
    public void showUpdateNickNameDialog(Activity activity, UpdateUserNameListener listener)
```
### 3.8 打开用户管理界面
- SDK调起用户管理界面的函数：``` YGUserApi.getInstance().openAccountManager(this) ```
``` java 
    /**
     * 修改用户昵称
     *
     * @param activity
     */
    public void openAccountManager(Activity activity)
```
### 3.9 检查账号绑定
- SDK调起账号升级的函数为：`` YGUserApi.getInstance().checkBindStat ``
``` java 
    /**
     * 检查绑定状态 如果未绑定 则会显示账户绑定弹窗
     *
     * @param activity
     * @return 是否绑定 true 绑定 false 未绑定
     */
    public boolean checkBindStat(Activity activity)
```
### 3.10 自定义埋点
- SDK提供埋点功能, 分别上报到YallaGame, Firebase、Facebook和Appflyer 数据平台
``` java
// 上报到YallaGame
YGEventApi.onEvent()
// 上报到Firebase
YGEventApi.onFireBaseEvent(String eventName, Map params)
// 上报到Facebook
YGEventApi.onFacebookEvent(String eventName, Map params)
// 上报到Appflyer
YGEventApi.onAppsFlyerEvent(String eventName, Map params)
// 上报到Firebase&Facebook&Appflyer事件方法
YGEventApi.onThirdEvent(String eventName, Map params)
```
event_name分为游戏通用埋点和自定义埋点的事件名称
通用事件埋点event_name 都需要按规定书写
由YGEventConstants提供，列如YGEventConstants.YG_GAME_UPDATE_BEGIN

自定义事件埋点event_name 由游戏方约束
详情文档可参考[Yll_Android埋点](https://github.com/yllgame2021/yllgamesdk/blob/master/%E5%9F%8B%E7%82%B9%E9%9C%80%E6%B1%82/Android/%E7%BB%9F%E8%AE%A1%E5%9F%8B%E7%82%B9Android.md)

### 3.11 华为获取推送token
-调用获取token调用方式：YGMessageApi.getInstance().getPushToken() 
 ``` java
     /**
     * 获取推送token
     *
     * @param callBack 回调的token
     */
    public void getPushToken(@NonNull YGCallBack<String> callBack)
 ```
#### 在value下的strings.xml添加 华为id
``` xml
<string name="yll_game_sdk_huawei_appid" translatable="false">appid</string>
```
``` xml
<!--华为id-->
        <meta-data
            android:name="com.yllgame.sdk.huawei.ApplicationId"
            android:value="@string/yll_game_sdk_huawei_appid" />
        <service
            android:name=".MyHmsMessageService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.huawei.push.action.MESSAGING_EVENT" />
            </intent-filter>
        </service>
```
#### 清单文件配置推送Service
``` java
public class MyHmsMessageService extends HmsMessageService {
    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        super.onMessageReceived(remoteMessage); 
    }
}
```
**注：YGMessageApi.getInstance().handlePushMessage(remoteMessage);函数必须接入，在推送消息的service的onMessageReceived里。**
### 3.12华为推送处理
#### 1.设置游戏Activity action 
``` xml
        <activity
            android:name=".游戏Activity"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:exported="true">
            <intent-filter>
                <!--        设置action        -->
                <action android:name="游戏包名.push.intent.action.custom" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
```
 ``` java
 public class 游戏Activity {
@Override
    protected void onNewIntent(Intent intent) {
        LogUtils.logEForDeveloper("LoginActivity>>>>>onNewIntent");
        //设置intent
        setIntent(intent);
        //获取通知栏点击data
        getData(intent);
        super.onNewIntent(intent);
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //获取通知栏点击data
        getData(getIntent());
        setContentView(R.layout.activity_login);
    }

    private void getData(Intent intent) {
        if (intent == null || intent.getExtras() == null) return;
        Bundle bundle = intent.getExtras();
        //获取通知的类型和内容
        String type = bundle.getString("ClickActionGoType", "1");
        String content = bundle.getString("ClickActionGoContent", "");
        //调用SDK的点击函数
        YGMessageApi.getInstance().clickPushMessage(SPStaticUtils.getRoleId(), SPStaticUtils.getRoleServiceId(), getIntent().getExtras());
        LogUtils.logEForDeveloper("push>>>>>type:" + type + ">>>>>content:" + content);
    }}
 ```
**注：用户点击通知栏的时候会默认进入设置action的SampleActivity，在当前界面获取传递的数据并调SDK的clickPushMessage函数**
### 3.13 获取SDK版本
- 调用获取SDK函数为：`` YGCommonApi.getSDKVersionName() ``
``` java
     /**
     * @return 返回版本名称
     */
    public static String getSDKVersionName()
```
### 3.14 获取SDKBuild
- 调用埋点函数为：``YGCommonApi.getSDKVersionCode() ``
``` java
    /**
     * @return 返回版本号
     */
    public static int getSDKVersionCode()
```
### 3.15 检查SDK版本(非必要)
- SDK调起用户版本更新的函数为：`` YGUserApi.getInstance().getVersionInfo() ``
 ``` java 
     /**
     * 获取版本
     */
    public void getVersionInfo()
 ```  
### 3.16 Facebook 分享
AndroidManifest.xml
``` xml
        <!--将 ContentProvider 添加至 AndroidManifest.xml 文件，并将 {APP_ID} 设置为您的应用编号：-->
        <provider
            android:name="com.facebook.FacebookContentProvider"
            android:authorities="com.facebook.app.FacebookContentProvider{APP_ID}"
            android:exported="true" />
        <!--如果您的应用程序面向 Android 11 或更高版本，请向 AndroidManifest.xml 文件添加以下查询块，以使 Facebook 应用对您的应用可见-->
        <queries>
            <provider android:authorities="com.facebook.katana.provider.PlatformProvider" />
        </queries>
```
- Facebook分享链接的函数为：`` YGTripartiteApi.getInstance().shareLink ``
``` java 
    /**
     * 分享链接
     *
     * @param activity 当前的activity
     * @param quote    分享的标题
     * @param url      分享的url
     * @param callback 分享回调
     */
    public void shareLink(Activity activity, String quote, String url, FacebookCallback<Sharer.Result> callback)
```
### 3.17获取Facebook好友列表
- 获取Facebook好友列表的函数为：`` YGTripartiteApi.getInstance().getFacebookFriends ``
``` java 
    /**
     * 获取Facebook好友
     *
     * @param activity 当前的activity
     * @param callBack 好友列表回调
     */
    public void getFacebookFriends(Activity activity, YGCallBack<List<GameFacebookFriendEntity>> callBack)
```
### 3.18 打开举报页面
- SDK调起游戏举报的函数为：`` YGUserApi.getInstance().showReportView ``
``` java 
    /**
     * 游戏举报
     *
     * @param activity       当前activity
     * @param reportRoleId   举报者角色id
     * @param reportedRoleId 被举报者角色id
     * @param serviceId      角色服务器id
     */
    public void showReportView(Activity activity, String reportRoleId, String reportedRoleId, String serviceId)
```
### 3.19设置activity回调（必接）
- 调用设置activity函数为：``YGCommonApi.setCallback() `` 
``` java
    /**
     * 设置回调
     *
     * @param requestCode
     * @param resultCode
     * @param data
     */
    public static void setCallback(int requestCode, int resultCode, Intent data)
```
注：该函数必须接入，有些第三方依赖库必须依赖Activity的onActivityResult的回调来传值和回调。在Activity 重写onActivityResult
``` java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        YGCommonApi.setCallback(requestCode, resultCode, data);
        super.onActivityResult(requestCode, resultCode, data);
    }
```
### 3.20 打开展示房间/房间用户举报页面
- SDK调起语聊举报的函数为：`` YGUserApi.getInstance().showRoomReportView ``
``` java 
    /**
     * 语聊举报
     *
     * @param activity          当前activity
     * @param reportType        1房间 2角色
     * @param reportRoleId      举报者角色id
     * @param reportServiceId   举报者区服Id
     * @param reportedRoleId    被举报者角色id
     * @param reportedServiceId 被举举报者区服Id
     * @param roomId            房间Id
     */
    public void showRoomReportView(Activity activity, String reportType, String reportRoleId, String reportServiceId, String reportedRoleId, String reportedServiceId, String roomId)
```
### 3.21 上传图片
文件读写权限获取：
``` java 
    /**
     * 请求存储权限代码
     *
     * @param activity
     * @param requestCode 请求的requestCode
     * @return 有权限返回true 反之false
     */
    public boolean hasStoragePermission(Activity activity, int requestCode)
```
- 阿里云上传图片的函数为：`` YGTripartiteApi.getInstance().updatePicture ``
``` java 
    /**
     * 阿里云上传图片
     *
     * @param activity 当前Activity
     * @param type     类型,
     *                 {@link YGConstants#SDK_IMAGE_SELECT_NONE} 表示选择图片不裁剪
     *                 {@link YGConstants#SDK_IMAGE_SELECT_CLIP_RATIO_1_1} 选择图片裁剪
     *                 {@link YGConstants#SDK_IMAGE_SELECT_PREVIEW} 选择图片和预览
     * @param callBack 上传回调返回图片绝对路径
     */
    public void updatePicture(Activity activity, int type, YGCallBack<String> callBack)
```
注：列表展示、缩略图等展示都可以使用 [阿里云图片处理](https://help.aliyun.com/document_detail/101260.html) ,来让显示效果更快
代码示例
``` java 

    if (YGTripartiteApi.getInstance().hasStoragePermission(this, 12345)) {
                    startUpload();
                }

    private void startUpload() {
        YGTripartiteApi.getInstance().updatePicture(this, new YGCallBack<String>() {
            @Override
            public void onSuccess(String s) {
                LogUtils.logEForDeveloper("图片：" + s);
            }

            @Override
            public void onFail(int code) {

            }
        });
    }

@Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == 12345) {
            if (grantResults.length == 2 && grantResults[0] == PackageManager.PERMISSION_GRANTED && grantResults[1] == PackageManager.PERMISSION_GRANTED) {
                startUpload();
            } else {
                if (!ActivityCompat.shouldShowRequestPermissionRationale(this, permissions[0]) || !ActivityCompat.shouldShowRequestPermissionRationale(this, permissions[1]))
                    YGTripartiteApi.getInstance().goToAppSetting(this);
            }
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        YGCommonApi.setCallback(requestCode, resultCode, data);
        super.onActivityResult(requestCode, resultCode, data);
    }
```
### 3.22 获取活动信息
- SDK调起语聊举报的函数为：`` YGUserApi.getInstance().getActivityInfos ``
``` java 
    /**
     * 获取活动信息
     *
     * @param serviceId 服务器id
     * @param roleId    角色id
     * @param callBack  回调
     */
    public void getActivityInfos(String serviceId, String roleId, YGCallBack<List<GameActivityEntity>> callBack)
```
### 3.23 获取手机号绑定状态
- SDK调起语聊举报的函数为：`` YGUserApi.getInstance().getPhoneBindState ``
``` java 
    /**
     * 获取手机绑定状态
     *
     * @param callBack true 已绑定手机 false 未绑定手机
     */
    public void getPhoneBindState(YGBooleanCallBack callBack)
```
### 3.24 跳转到facebook主页
- Facebook公共页面：`` YGTripartiteApi.getInstance().openFacebookPage ``
``` java 
    /**
     * Facebook公共页面
     *
     * @param pageId 账号id
     */
    public void openFacebookPage(Activity activity,String pageId)
```
### 3.25 展示账号绑定页面
- SDK展示绑定界面的函数为：`` YGUserApi.getInstance().showAccountBindView ``
  ``` java 
     /**
     * 展示绑定界面
     *
     * @param activity
     */
    public void showAccountBindView(Activity activity)
  ```
### 3.26 展示手机号绑定页面
- SDK调起语聊举报的函数为：`` YGUserApi.getInstance().showPhoneBindView ``
``` java 
    /**
     * 显示绑定界面
     *
     * @param activity          当前Activity
     * @param ygBooleanCallBack true：绑定成功
     */
    public void showPhoneBindView(Activity activity, YGBooleanCallBack ygBooleanCallBack)
```
### 3.27 展示举报消息页面
- SDK调起展示举报消息页面的函数为：`` YGUserApi.getInstance().showReportCustomMsgView ``
``` java 

    /**
     * 展示举报消息页面
     *
     * @param activity       当前Activity
     * @param gameServerId   区服id
     * @param reportRoleId   举报者角色Id
     * @param beReportRoleId 被举报者角色Id
     * @param scene          场景（世界、联盟、私聊）根据游戏定
     * @param chatMsgList    消息内容数据集合
     *                       {"roleId":"消息发送者角色id",
     *                       "roleName":"角色名称",
     *                       "msgContent":"消息内容",
     *                       "msgContent":"消息内容",
     *                       "msgType":0 文本消息  1图片消息,
     *                       "msgTime":"发送时间(时间戳到秒)",
     *                       "isReport":1(是否为举报消息1是 0否)}
     */
    public void showReportCustomMsgView(Activity activity,
                                        String gameServerId,
                                        String reportRoleId,
                                        String beReportRoleId,
                                        String scene,
                                        List<GameReportChatEntity> chatMsgList) 
```
### 3.28 展示举报语聊房消息页面
- SDK调起展示举报语聊房消息页面的函数为：`` YGUserApi.getInstance().showReportChatRoomMsgView ``
``` java 
    /**
     * 展示举报语聊房消息页面
     *
     * @param activity             当前Activity
     * @param roomId               房间id
     * @param reportGameServerId   举报者区服id
     * @param beReportGameServerId 被举报者区服id
     * @param reportRoleId         举报者角色Id
     * @param beReportRoleId       被举报者角色Id
     * @param chatMsgList          消息内容数据集合
     *                             {"roleId":"消息发送者角色id",
     *                             "roleName":"角色名称",
     *                             "msgContent":"消息内容",
     *                             "msgType":0 文本消息  1图片消息,
     *                             "msgTime":"发送时间(时间戳到秒)",
     *                             "isReport":1(是否为举报消息1是 0否)}
     */
    public void showReportChatRoomMsgView(Activity activity,
                                          String roomId,
                                          String reportGameServerId,
                                          String beReportGameServerId,
                                          String reportRoleId,
                                          String beReportRoleId,
                                          List<GameReportChatEntity> chatMsgList)
```
### 3.29 展示网络检测页面
- 调用网络检测函数为：`` YGNetDiagnosis.getInstance().showNetCheckView ``
``` java
    /**
     * 网络检测
     * @param activity 当前activity
     * @param userId 用户id
     * @param roleId 角色id
     */
    public void showNetCheckView(Activity activity, String userId, String roleId)
```
### 3.30 获取用户消息数量变更通知
- 调用获取消息信息接口函数为：`` YGUserApi.getInstance().getCustomerMsg ``
``` java
    /**
     * 获取消息信息接口
     *
     * @param roleId    游戏角色Id
     * @param serviceId 角色所在区服Id
     * @param callBack  回调 未读消息数量0没有 大于0表示有
     */
    public void getCustomerMsg(String roleId, String serviceId, YGCallBack<Integer> callBack)
```
### 3.31 展示客服页面
- 调用展示客服页面函数为：`` YGUserApi.getInstance().showCustomerView ``
``` java
    /**
     * 展示客服页面
     *
     * @param activity 当前activity
     */
    public void showCustomerView(Activity activity)
```
### 3.33 获取初始化语区
- SDK会在初始化时通过回调YGReceiver，登陆会在YGLoginReceiver回调。示例代码参考2.3和2.4
### 3.34 删除游戏角色
- 调用删除游戏角色函数为：`` YGUserApi.getInstance().deleteRole ``
``` java
    /**
     * 删除角色
     *
     * @param serviceId 服务器id
     * @param roleId    角色id
     * @param reason    删除原因
     * @param callBack  回调
     */
    public void deleteRole(String roleId, String serviceId, String reason, YGBooleanCallBack callBack)
```
