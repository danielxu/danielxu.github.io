---
title: 小米平台检测APP自启动行为审核被拒绝
comments: false
date: 2023-04-23 10:02:23
tags:
 - android
 - 自启动
categories: android
keywords: 'android,xiaomi,自启动'
description: 小米平台检测APP自启动行为审核被拒绝
top_img:
cover:
---

{% note blue 'fas fa-bullhorn' %}

 因为国内 Android App 隐私政策的调整，有些平台会审核不过。

{% endnote %}

***

由于国内 Android App 隐私政策的收紧，在使用很多第三方手机厂商或平台的 SDK 时，会无意的被引入一些特性，导致一些 APP 审核不通过，此处记录一下处理结果。

## umeng sdk 

在使用 umeng sdk 的时候，比如：share，push 等，会引入很多第三方 sdk，带入了一些 app 自启动的配置，会被有些平台审核拒绝app上架。所以需要关闭这些自启动。

sdk 版本: 
```gradle
com.umeng.umsdk:push:6.5.0
com.umeng.umsdk:common:9.4.7
com.umeng.umsdk:asms:1.6.0
com.umeng.umsdk:push:6.5.0

com.umeng.umsdk:xiaomi-umengaccs:1.2.8
com.umeng.umsdk:xiaomi-push:4.9.1
com.umeng.umsdk:huawei-umengaccs:1.3.6
com.huawei.hms:push:6.1.0.300
com.umeng.umsdk:meizu-umengaccs:1.1.5
com.umeng.umsdk:meizu-push:4.1.4
com.umeng.umsdk:oppo-umengaccs:1.0.8-fix
com.umeng.umsdk:oppo-push:3.0.0
com.umeng.umsdk:vivo-umengaccs:1.1.6
com.umeng.umsdk:vivo-push:3.0.0.4

```

### 若需移除自启动能力，在AndroidManifest.xml中添加

```xml
<uses-permission
    android:name="android.permission.RECEIVE_BOOT_COMPLETED"
    tools:node="remove" />
<application>
	 <receiver android:name="com.taobao.accs.EventReceiver" tools:node="remove" android:exported="false"
            tools:replace="android:exported" >
            <intent-filter tools:node="remove">
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
            <intent-filter tools:node="remove">
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
            <intent-filter tools:node="remove">
                <action android:name="android.intent.action.PACKAGE_REMOVED" />
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.taobao.agoo.AgooCommondReceiver" android:exported="false"
            tools:replace="android:exported">
            <intent-filter tools:node="remove">
                <action android:name="android.intent.action.PACKAGE_REMOVED" />
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
</application>
```

### 若需移除关联启动能力，在AndroidManifest.xml中添加

```xml
<service
            android:name="com.taobao.accs.ChannelService"
            android:exported="false"
            tools:replace="android:exported" />
        <service
            android:name="com.taobao.accs.data.MsgDistributeService"
            android:exported="false"
            tools:replace="android:exported" />

        <receiver
            android:name="com.taobao.accs.ServiceReceiver"
            android:exported="false"
            tools:replace="android:exported" />
        <service
            android:name="org.android.agoo.accs.AgooService"
            android:exported="false"
            tools:replace="android:exported" />
        <service
            android:name="com.umeng.message.UmengIntentService"
            android:exported="false"
            tools:replace="android:exported" />
        <service
            android:name="com.umeng.message.XiaomiIntentService"
            android:exported="false"
            tools:replace="android:exported" />

```

## 小米平台提示小米 push 自启动

```xml
<receiver
            android:name="com.xiaomi.push.service.receivers.NetworkStatusReceiver"
            android:exported="true"
            tools:node="remove">
            <intent-filter tools:node="remove">
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </receiver>
```

这样基本上对于自启动的审核拒绝，应该就可以解决了。