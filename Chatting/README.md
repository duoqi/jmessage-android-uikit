# jmessage-android-ui-components
IM SDK UI 组件

简单的聊天组件, 实现了单聊和群聊功能.(由于模拟器不支持录音和拍照，所以没有演示)

![demo 演示](https://github.com/KenChoi1992/jchat-android/raw/dev/JChat/screenshots/demogif.gif)

###将聊天功能集成到你的项目（Android Studio平台）:

####方法一：使用jmessage-uikit-chatting-1.0.0.jar：

- 将[jmessage-uikit-chatting-1.0.0.jar](https://github.com/jpush/jmessage-android-uikit/releases/download/1.0.0/jmessage-uikit-chatting-1.0.0.jar)及相关libs复制到你的libs文件夹下
- 配置AndroidManifest，将所需的权限及Receiver, Service等拷贝到你的AndroidManifest中:
```
<permission
        android:name="${applicationId}.permission.JPUSH_MESSAGE"
        android:protectionLevel="signature" />
    <!--Required 一些系统要求的权限，如访问网络等-->
    <uses-permission android:name="${applicationId}.permission.JPUSH_MESSAGE" />
    <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_LOCATION_EXTRA_COMMANDS"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <!-- JMessage Demo required for record audio-->
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>
```
   以下放在application下 
```
            <!--以下放在application下 -->
            <service
                android:name="cn.jpush.android.service.PushService"
                android:enabled="true"
                android:exported="false"
                android:process=":remote">
                <intent-filter>
                    <action android:name="cn.jpush.android.intent.REGISTER" />
                    <action android:name="cn.jpush.android.intent.REPORT" />
                    <action android:name="cn.jpush.android.intent.PushService" />
                    <action android:name="cn.jpush.android.intent.PUSH_TIME" />
                </intent-filter>
            </service>
            <receiver
                android:name="cn.jpush.android.service.PushReceiver"
                android:enabled="true">
                <intent-filter android:priority="1000">
                    <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED_PROXY" />
                    <category android:name="${applicationId}" />
                </intent-filter>
                <intent-filter>
                    <action android:name="android.intent.action.USER_PRESENT" />
                    <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                </intent-filter>
                <!-- Optional -->
                <intent-filter>
                    <action android:name="android.intent.action.PACKAGE_ADDED" />
                    <action android:name="android.intent.action.PACKAGE_REMOVED" />
                    <data android:scheme="package" />
                </intent-filter>
            </receiver>
            <activity
                android:name="cn.jpush.android.ui.PushActivity"
                android:configChanges="orientation|keyboardHidden"
                android:theme="@android:style/Theme.Translucent.NoTitleBar">
                <intent-filter>
                    <action android:name="cn.jpush.android.ui.PushActivity" />
                    <category android:name="android.intent.category.DEFAULT" />
                    <category android:name="${applicationId}" />
                </intent-filter>
            </activity>
            <service
                android:name="cn.jpush.android.service.DownloadService"
                android:enabled="true"
                android:exported="false" />
            <!-- Required Push SDK核心功能 -->
            <receiver android:name="cn.jpush.android.service.AlarmReceiver" />
            <!-- IM Required IM SDK核心功能-->
            <receiver
                android:name="cn.jpush.im.android.helpers.IMReceiver"
                android:enabled="true"
                android:exported="false">
                <intent-filter android:priority="1000">
                    <action android:name="cn.jpush.im.android.action.IM_RESPONSE" />
                    <action android:name="cn.jpush.im.android.action.NOTIFICATION_CLICK_PROXY" />
                    <category android:name="${applicationId}" />
                </intent-filter>
            </receiver>
             <!-- option 可选项。用于同一设备中不同应用的JPush服务相互拉起的功能。 -->
             <!-- 若不启用该功能可删除该组件，将不拉起其他应用也不能被其他应用拉起 -->
             <service
                android:name="cn.jpush.android.service.DaemonService"
                android:enabled="true"
                android:exported="true">
                <intent-filter>
                    <action android:name="cn.jpush.android.intent.DaemonService" />
                    <category android:name="${applicationId}" />
                </intent-filter>
             </service>
            <meta-data
                android:name="JPUSH_CHANNEL"
                android:value="developer-default" />
            <!-- Required. AppKey copied from Portal -->
            <meta-data
                android:name="JPUSH_APPKEY"
                android:value="5fbb6030a7c7b853dc199ea0" />
    
```
别忘了**配置applicationId或者替换为你的包名, AppKey也要替换为你在极光控制台上注册的应用所对应的AppKey**.
配置applicationId需要在build.gradle的defaultConfig中声明（注意将此处"io.jchat.android"更换为你的applicationId）:
![如图](https://github.com/KenChoi1992/jchat-android/raw/dev/JChat/screenshots/screenshot3.png)
注意，AndroidManifest中的package字段值与build.gradle中的ApplicationId需要保持一致。
**如果你使用的是Eclipse，要将applicationId改为你的包名**

另外在AndroidManifest的application标签下需要注册ChatActivity
```
<activity android:name="cn.jmessage.android.uikit.chatting.ChatActivity"
            android:theme="@style/noTitle"
            android:windowSoftInputMode="adjustResize"/>
            
```
- 将你的项目结构修改为与Chatting相似，即src与res、libs、AndroidMainfest同级，并且在build.gradle中的android节点下加入以下代码：
```
sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
            manifest.srcFile 'AndroidManifest.xml'
            jniLibs.srcDirs = ['libs']
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }
    }
    
```
这样就可以兼容Android Studio和Eclipse，还要注意将build.gradle中buildToolsVersion的版本修改为你当前的版本。

- 复制相关资源文件到你的项目中（注意不要漏了某些文件）
- 在你的入口Activity（或者Application与启动Activity，总之先于ChatActivity启动的类中）做相关初始化操作:
```
        //初始化JMessage-sdk
        JMessageClient.init(this);
        //设置Notification的模式
        JMessageClient.setNotificationMode(JMessageClient.NOTI_MODE_NO_NOTIFICATION);
        //初始化SharePreference
        SharePreferenceManager.init(this, JCHAT_CONFIGS);
        
```
- 进入聊天界面：（完整代码可以参考Chatting中的DemoActivity）
```
    //单聊
    intent.putExtra(TARGET_ID, mTargetId);
    //群聊
    //intent.putExtra(GROUP_ID, mGroupId);
    intent.setClass(DemoActivity.this, ChatActivity.class);
    startActivity(intent);
```
其中的Key或者其它代码都可以自行修改（下载Chatting，并作为module导入Android Studio，编辑完成后执行build.gradle中的jarMyLib任务，然后在生成的build/libs中即可得到jar包）

####方法二：使用Chatting源代码：
- 复制chatting文件夹下的文件到你的项目.

- 配置AndroidManifest, 将所需的权限及Receiver, Service等拷贝到你的AndroidManifest中，同上
- 配置build.gradle，同上
- 初始化相关操作，同上
- 复制资源文件到你的项目, 你可以自定义界面的样式
- 在XML文件中将引用路径修改为你当前的路径(红色方框部分) ![如图](https://github.com/KenChoi1992/jchat-android/raw/dev/JChat/screenshots/screenshot6.png)
- 配置用户信息, 在MainActivity这个入口Activity中配置用户信息(包括登录用户, 聊天用户及群聊id), 可以使用Intent传递到ChatActivity. 
- 关于注册：你可以调用JMessageClient.register(username, password, callback)来注册用户, 也可以使用curl的方式注册用户:
```

curl --insecure -X POST -v https://api.im.jpush.cn/v1/users -H "Content-Type: application/json" -u "your AppKey:your master secret" -d '[{"username":"user003", "password": "1111"}]'

```

注册群组, 同样可以使用JMessageClient.createGroup("", "", new CreateGroupCallback(){}), 或者使用curl:

```

curl --insecure -X POST -v https://api.im.jpush.cn/v1/groups -H "Content-Type: application/json" -u "your AppKey:your master secret" -d '{"owner_username":"user001", "name": "example group", "members_username":["user002","user003"],"desc":"example"}'

```
####项目中所使用的开源项目简单说明

- android-shape-imageview [github地址](https://github.com/siyamed/android-shape-imageview) 自定义ImageView的形状

- PhotoView [github地址](https://github.com/chrisbanes/PhotoView) 根据手势缩放图片

- DropDownListView [github地址](https://github.com/Trinea/android-common) 下拉刷新ListView

