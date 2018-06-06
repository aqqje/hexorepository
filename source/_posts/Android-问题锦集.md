---
title: Android 问题锦集
date: 2018-06-06 20:31:57
tags: android
---

Android 问题锦集

<!-- more -->

## android studio新建项目时出现Error:Execution failed for task ':app:preDebugAndroidTestBuild'.


1.在app下的build.gradle文件中的dependences {}中添加如下代码：

androidTestCompile('com.android.support:support-annotations:26.1.0') {
    force = true
}

```gradel

dependencies {
    androidTestCompile('com.android.support:support-annotations:26.1.0') {
        force = true
    }
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation"org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:26.1.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'

}
```

<a href="https://www.cnblogs.com/fqfzs/p/9117520.html">参考</a>

## android studio 出现 "Unsupported method: BaseConfig.getApplicationIdSuffix()"

找到项目下的 gradle -- > build.gradle 修改如下

```gradle
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        // 原先错误原因请删除或注释掉
		//classpath 'com.android.tools.build:gradle:1.0.0' 
		// 修改如下:
        classpath 'com.android.tools.build:gradle:2.3.2'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

<a href="https://blog.csdn.net/rjc_lihui/article/details/78434864">参考</a>


## Android studio 出现 "Error:Failed to open zip file. Gradle’s dependency cache may be corrupt (this sometimes occurs after a network connection timeout.)"

 