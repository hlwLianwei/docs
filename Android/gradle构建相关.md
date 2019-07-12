# 构建apk版本号字符串添加Git提交次数  
在项目app文件夹下的build.gradle文件里，在apply plugin: 'com.android.application'的下面，添加代码  
```android
// 获取git提交的次数
def gitCommits() {
    return 'git rev-list HEAD --first-parent --count'.execute().text.trim().toInteger()
}
```
然后在 defaultConfig里的版本名称添加  
```android
android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "com.xxx.xxx"
        minSdkVersion 21
        targetSdkVersion 28
        versionCode 10
        versionName "1.0.10" + "  g" + gitCommits() // 添加git提交次数小尾巴
    }
}
```

# 构建时自动签名 release及debug  
在项目app文件夹下的build.gradle文件里，添加release和debug的签名文件信息配置代码  
```android
android {
    compileSdkVersion 28
    defaultConfig {
        applicationId "com.xxx.xxx"
        minSdkVersion 21
        targetSdkVersion 28
        versionCode 10
        versionName "1.0.10" + "  g" + gitCommits() // 添加git提交次数小尾巴
    }
    
    // 自动签名配置
    signingConfigs {
        // 填写创建release时的信息, 推荐每个项目都自己生成一个签名文件
        release {
            keyAlias 'xxx'
            keyPassword 'xxx'
            storeFile file('../app/signed_key_release.jks')     // 签名文件位置
            storePassword 'xxx'
        }

        debug {
            storeFile file("../app/signed_key_debug.keystore")  // 签名文件位置
        }
    }
}
```
然后在AndroidStudio, File->Project Structure, 在左侧的Modules里选择app, 在右侧选择Build Types选页， 
Build Types选页里配置debug的Signing Config项为debug，配置release的Signing Config项为release， 然后按钮OK按钮保存。

# AndroidStudio 编译签名apk的另一种方法  
![编译签名apk的方法](https://raw.githubusercontent.com/hlwLianwei/docs/master/Android/images/编译签名apk的方法.png)  

