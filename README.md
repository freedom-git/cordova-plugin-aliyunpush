# cordova-plugin-aliyunpush

## Install


- 通过 Cordova Plugins 安装，要求 Cordova CLI 5.0+：

  ```shell
cordova plugin add @freedom_sky/cordova-plugin-aliyunpush
  ```

## 注意事项
* 安卓默认通道名“-1”，安卓8以上推送时需要指定通道才能收到
* ios阿里云插件版本1.9.9
* 目前只集成了华为、小米通道
* 安卓9以上需要动态获取READ_PHONE_STATE权限

### Android Preferences

```xml
<edit-config file="app/src/main/AndroidManifest.xml" mode="merge" target="/manifest/application" xmlns:android="http://schemas.android.com/apk/res/android">
            <application android:name="com.alipush.PushApplication" />
</edit-config>
<config-file parent="/manifest/application" target="app/src/main/AndroidManifest.xml" xmlns:android="http://schemas.android.com/apk/res/android">
            <meta-data android:name="AliyunAppKey" android:value="value=XXXXXX" />
            <meta-data android:name="AliyunAppSecret" android:value="value=XXXXXX" />
            <meta-data android:name="XiaoMiAppId" android:value="value=XXXX" />
            <meta-data android:name="XiaoMiAppKey" android:value="value=XXXX" />
            <meta-data  android:name="com.huawei.hms.client.appid" android:value="appid=XXXXX" />
            <activity android:name="com.alipush.PopupPushActivity" android:exported="true" android:theme="@android:style/Theme.Translucent.NoTitleBar" />
            <receiver android:name="com.alipush.PushMessageReceiver" android:exported="false">
                <intent-filter>
                    <action android:name="com.alibaba.push2.action.NOTIFICATION_OPENED" />
                </intent-filter>
                <intent-filter>
                    <action android:name="com.alibaba.push2.action.NOTIFICATION_REMOVED" />
                </intent-filter>
                <intent-filter>
                    <action android:name="com.alibaba.sdk.android.push.RECEIVE" />
                </intent-filter>
            </receiver>
</config-file>
```
### IOS Preferences
```xml
<!-- 需要先将阿里云下载得到的AliyunEmasServices-Info.plist文件放置在resources/ios/plist/目录下 -->
<resource-file src="resources/ios/plist/AliyunEmasServices-Info.plist" />
```


## Usage

### API

```
    /**
     * 获取设备唯一标识deviceId，deviceId为阿里云移动推送过程中对设备的唯一标识（并不是设备UUID/UDID）
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}  
     */
    getRegisterId: function(successCallback, errorCallback)

    /**
     * 阿里云推送绑定账号名
     * @param  {string} account         账号
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void} 
     */
    bindAccount: function(account, successCallback, errorCallback)

    /**
     * 阿里云推送解除账号名,退出或切换账号时调用
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void} 
     */
    unbindAccount: function(successCallback, errorCallback)

    /**
     * 阿里云推送绑定标签
     * @param  {string[]} tags            标签列表
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}  
     */
    bindTags: function(tags, successCallback, errorCallback) 

    /**
     * 阿里云推送解除绑定标签
     * @param  {string[]} tags            标签列表
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}               
     */
    unbindTags: function(tags, successCallback, errorCallback)

    /**
     * 阿里云推送解除绑定标签
     * @param  {Function} successCallback 成功回调
     * @param  {Function} errorCallback   失败回调
     * @return {void}           
     */
    listTags: function(successCallback, errorCallback) 

    /**
      * 没有权限时，请求开通通知权限，其他路过
      * @param  string msg  请求权限的描述信息
      * @param {} successCallback 
      * @param {*} errorCallback 
      */
    requireNotifyPermission:function(msg,successCallback, errorCallback)
    
    /**
    * 阿里云推送消息透传回调
    * @param  {Function} successCallback 成功回调
    */
    onMessage:function(sucessCallback) ;

    # sucessCallback:调用成功回调方法，注意没有失败的回调，返回值结构如下：
    #json: {
      type:string 消息类型,
      title:string '阿里云推送',
      content:string '推送的内容',
      extra:string | Object<k,v> 外健,
      url:路由（后台发送推送时，在ExtParameters参数里写入url如{url:'demoapp://...'}）
    }

    #消息类型
    {
      message:透传消息，
      notification:通知接收，
      notificationOpened:通知点击，
      notificationReceived：通知到达，
      notificationRemoved：通知移除，
      notificationClickedWithNoAction：通知到达，
      notificationReceivedInApp：通知到达打开 app
    }

```


