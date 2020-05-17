---
title: 在React-Native项目中引入react-native-sqlite-storage
date: 2017-03-30 21:42:18
categories: 技术
tags: [React Native]
---
#### 前言

最近在使用React Native做毕业设计，因为第一次接触踩了不少坑，不过好在都解决了，这篇主要记录一下在React-Native中引入'react-native-sqlite-storage'的方法：
<!--more-->
目前在react-native中主要有两种数据存储方案，第一种是官方的AsyncStorage，AsyncStorage是一个简单的、异步的、持久化的Key-Value存储系统，它对于App来说是全局性的
它适用于存储些系统设置、全局变量等简单的key-value数据，不适用于过于庞大的数据，也不适用于一些包含数据结构等复杂数据，对于复杂的数据结构，我们需要使用SQLite，轻量的数据库，但RN并没有提供，不过有这种需求的肯定不止我一个，所以现在拿来直接使用，我在这里用的是react-native-sqlite-storage。且由于没有ios设备，这里只说安卓平台。

#### 1. 命令行安装

```
npm install --save react-native-sqlite-storage
```

#### 2.  全局Gradle的设置
文件目录：android/settings.gradle
```
...
 
include ':react-native-sqlite-storage'
project(':react-native-sqlite-storage').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-sqlite-storage/src/android')
```

#### 3. 修改android/app/build.gradle
目录：android/app/build.gradle
```
...
dependencies {
    ...
    compile project(':react-native-sqlite-storage')
}
```

#### 4. 在MainActivity.java中注册模块
目录：android\app\src\main\java\com\项目名\MainActivity.java
```
import android.app.Activity;
import android.os.Bundle;
import com.facebook.react.modules.core.DefaultHardwareBackBtnHandler;
import com.facebook.react.ReactInstanceManager;
import com.facebook.react.ReactRootView;
import com.facebook.react.shell.MainReactPackage;
import com.facebook.react.common.LifecycleState;

import com.facebook.react.ReactActivity;
import org.pgsqlite.SQLitePluginPackage;

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {

    private ReactInstanceManager mReactInstanceManager;
    private ReactRootView mReactRootView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mReactRootView = new ReactRootView(this);
        mReactInstanceManager = ReactInstanceManager.builder()
                .setApplication(getApplication())
                .setBundleAssetName("index.android.bundle")  // this is dependant on how you name you JS files, example assumes index.android.js
                .setJSMainModuleName("index.android")        // this is dependant on how you name you JS files, example assumes index.android.js
                .addPackage(new MainReactPackage())
                .addPackage(new SQLitePluginPackage(this))   // register SQLite Plugin here
                .setUseDeveloperSupport(BuildConfig.DEBUG)
                .setInitialLifecycleState(LifecycleState.RESUMED)
                .build();
        mReactRootView.startReactApplication(mReactInstanceManager, "AwesomeProject", null); //这里把AwesomeProject
        setContentView(mReactRootView);
    }
    
     @Override
     public void invokeDefaultOnBackPressed() {
          super.onBackPressed();
     }
}
```
#### 5. 引入
这样应该就引入成功了，具体的使用还在学习，后续再补充上来。

#### 6. 坑
今天跑起来后发现出事儿了，开发者菜单调不出来了，而且我用来测试的alert语句会发出一个警告，如图：
![http://oj056g1gy.bkt.clouddn.com/IMG20170331221314.jpg](http://oj056g1gy.bkt.clouddn.com/IMG20170331221314.jpg)
在网上没有找到同样的问题，但是sqlite能工作，而且只发出警告有感觉是因为版本的问题，试着将配置方式改了一下，结果就成了。
将MainActivity.java中添加和修改的内容都删除了，还原到原来的内容：
```
package com.项目名;

import com.facebook.react.ReactActivity;

public class MainActivity extends ReactActivity {

    /**
     * Returns the name of the main component registered from JavaScript.
     * This is used to schedule rendering of the component.
     */
    @Override
    protected String getMainComponentName() {
        return "项目名";
    }
}
```
MainApplication文件中的内容：
```
package com.weidao;

import android.app.Application;
import android.util.Log;

import com.facebook.react.ReactApplication;
import com.facebook.react.ReactNativeHost;
import com.facebook.react.ReactPackage;
import com.facebook.react.shell.MainReactPackage;
import com.facebook.soloader.SoLoader;

import org.pgsqlite.SQLitePluginPackage;

import java.util.Arrays;
import java.util.List;

public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    public boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }

    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
          new SQLitePluginPackage()
      );
    }
  };

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }

  @Override
  public void onCreate() {
    super.onCreate();
    SoLoader.init(this, /* native exopackage */ false);
  }
}

```
其他没变，这样终于可以了>_<_
