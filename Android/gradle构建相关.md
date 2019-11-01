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

# android studio生成keystore文件
在android studio的菜单栏，“Build”选择，弹出选择“Generate Signed Bundle / APK..”，弹出对话框勾选“APK”，点下一步，点击“Create new..”创建新的key store，弹出框，选择保存文件位置及填写信息，点击“OK”返回，在返回的对话框点击“Next”下一步，在“Build Type”选择release或debug，同时勾选“V2(Full APK Signature)”，点击“Finish”即可。  
查看keystore方件的 md5 或 sh1，在android studio的终端下，cd到keystore文件所有文件夹，执行如下命令：  
keytool -v -list -keystore signed_key_debug.jks 


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
            keyAlias 'xxx'
            keyPassword 'xxx'
            storeFile file('../app/signed_key_debug.jks')     // 签名文件位置
            storePassword 'xxx'
        }
    }
}
```
然后在AndroidStudio, File->Project Structure, 在左侧的Modules里选择app, 在右侧选择Build Types选页， 
Build Types选页里配置debug的Signing Config项为debug，配置release的Signing Config项为release， 然后按钮OK按钮保存。

# AndroidStudio 编译签名apk的另一种方法  
![编译签名apk的方法](/Android/images/编译签名apk的方法.png)  

