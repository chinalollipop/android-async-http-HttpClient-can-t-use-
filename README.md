# android-async-http-HttpClient-can-t-use-


android6.0SDK中删除HttpClient的相关类的解决方法

一、出现的情况
在eclipse或 android studio开发，
设置android SDK的编译版本为23时，且使用了httpClient相关类的库项目：如android-async-http等等，会出现有一些类找不到的错误。
二、原因
android 6.0(api 23) SDK，不再提供org.apache.http.*(只保留几个类).

三、解决方法
1.eclipse:
libs中加入
org.apache.http.legacy.jar
上面的jar包在：**\android-sdk-windows\platforms\android-23\optional下（需要下载android 6.0的SDK）
2.android studio:
在相应的module下的build.gradle中加入：
android {
    useLibrary 'org.apache.http.legacy'
}
注意放置的位置：是在android {}中
可以参考:
https://developer.android.com/preview/behavior-changes.html

最新的android-async-http的已经按上面的方法，更新了。

另外：在eclipse中，加入org.apache.http.legacy.jar后，把android sdk版本改为低于6.0也可以正常使用

附加：
u013004268：加了上面的jar，混淆出现问题
 解决方法：
对这个jar，不做混淆处理
下面是混淆配置(eclipse上面测试通过)
混淆配置：
#不混淆android-async-http(这里的与你用的httpClient框架决定)
-keep class com.loopj.android.http.**{*;}
 
 #不混淆org.apache.http.legacy.jar 
 -dontwarn android.net.compatibility.**
 -dontwarn android.net.http.**
 -dontwarn com.android.internal.http.multipart.**
 -dontwarn org.apache.commons.**
 -dontwarn org.apache.http.**
 -keep class android.net.compatibility.**{*;}
 -keep class android.net.http.**{*;}
 -keep class com.android.internal.http.multipart.**{*;}
 -keep class org.apache.commons.**{*;}
 -keep class org.apache.http.**{*;}
 
最后是完整的混淆配置文件的内容：
[plain] view plain copy print?
-ignorewarnings  
  
# 指定代码的压缩级别  
-optimizationpasses 5   
# 不使用大小写混合  
-dontusemixedcaseclassnames  
# 混淆第三方jar  
-dontskipnonpubliclibraryclasses  
# 混淆时不做预校验  
-dontpreverify  
 # 混淆时记录日志  
-verbose  
 # 混淆时所采用的算法  
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*  
  
 # 保持哪些类不被混淆：四大组件，应用类，配置类等等  
-keep public class * extends android.app.Activity  
-keep public class * extends android.app.Application  
-keep public class * extends android.app.Service  
-keep public class * extends android.content.BroadcastReceiver  
-keep public class * extends android.content.ContentProvider  
-keep public class * extends android.app.backup.BackupAgentHelper  
-keep public class * extends android.preference.Preference  
-keep public class com.android.vending.licensing.ILicensingService  
  
# 保持 native 方法不被混淆  
-keepclasseswithmembernames class * {  
    native <methods>;  
}  
  
 # 保持自定义控件类不被混淆  
-keepclasseswithmembers class * {  
    public <init>(android.content.Context, android.util.AttributeSet);  
}  
  
 # 保持自定义控件类不被混淆  
-keepclasseswithmembers class * {  
    public <init>(android.content.Context, android.util.AttributeSet, int);  
}  
  
 # 保持自定义控件类不被混淆  
-keepclassmembers class * extends android.app.Activity {  
   public void *(android.view.View);  
}  
  
 # 保持枚举 enum 类不被混淆  
-keepclassmembers enum * {  
    public static **[] values();  
    public static ** valueOf(java.lang.String);  
}  
  
 # 保持 Parcelable 不被混淆  
-keep class * implements android.os.Parcelable {  
  public static final android.os.Parcelable$Creator *;  
}  
 # 这个主要是在layout中写的onclick方法android:onclick="onClick"，不进行混淆  
 -keepclassmembers class * extends android.app.Activity {                                     
   public void *(android.view.View);   
 }   
   
 #保持注解  
 -keepattributes *Annotation*  
   
#不混淆android-async-http  
-keep class com.loopj.android.http.**{*;}  
   
 #不混淆org.apache.http.legacy.jar   
 -dontwarn android.net.compatibility.**  
 -dontwarn android.net.http.**  
 -dontwarn com.android.internal.http.multipart.**  
 -dontwarn org.apache.commons.**  
 -dontwarn org.apache.http.**  
 -keep class android.net.compatibility.**{*;}  
 -keep class android.net.http.**{*;}  
 -keep class com.android.internal.http.multipart.**{*;}  
 -keep class org.apache.commons.**{*;}  
 -keep class org.apache.http.**{*;}  
   
   
   http://blog.csdn.net/yangqingqo/article/details/48214865
   
