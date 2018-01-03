#Android签名-apksigner对apk进行v2签名


一、前言
在 Android 7.0 Nougat 中引入了全新的 APK Signature Scheme v2签名方式，美团也推出相应的Android渠道包生成工具[Walle](https://tech.meituan.com/android-apk-v2-signature-scheme.html)
360加固后需要重新签名，借助360官方提供的签名工具[qihoo apk signer](http://jiagu.360.cn/qcms/help.html#!id=84)，是采用的7.0以前的v1签名，这时再通过walle打渠道包，是无法成功往apk写入渠道号的。这时我们就必须借助Android SDK提供的apksigner工具对已经打包好的apk进行v2签名。

二、基本使用
Android官方文档已经对apksigner的使用有比较详细的解释。下面说说实际的操作步骤：

1. **ZipAlign**
zip对齐，因为APK包的本质是一个zip压缩文档，经过边界对齐方式优化能使包内未压缩的数据有序的排列，从而减少应用程序运行时的内存消耗 ，通过空间换时间的方式提高执行效率（zipalign后的apk包体积增大了100KB左右）。
打开cmd，把目录切换到SDK的build-tools目录下（例如 E:\SDK\build-tools\25.0.2\），执行：

```
.\zipalign.exe -v -p 4 input.apk output.apk
```

zipalign命令选项不多：

    -f : 输出文件覆盖源文件
    -v : 详细的输出log
    -p : outfile.zip should use the same page alignment for all shared object files within infile.zip
    -c : 检查当前APK是否已经执行过Align优化。
    另外上面的数字4是代表按照4字节（32位）边界对齐。

2. **apksigner**
这个工具位于`SDK`目录的`build-tools`目录下。必须说明的是，`v2`签名方式时在`Android7.0`后才推出的，所以只有版本>25的SDK\build-tools\中才能找到`apksigner.jar`。
打开cmd，把目录切到`SDK\build-tools\版本号\lib`下（例如`E:\SDK\build-tools\25.0.2\lib`），执行：

```
java -jar apksigner.jar sign           //执行签名操作
```

     --ks 你的jks路径                                 //jks签名证书路径
    --ks-key-alias 你的alias           //生成jks时指定的alias
    --ks-pass pass:你的密码          //KeyStore密码
    --key-pass pass:你的密码   //签署者的密码，即生成jks时指定alias对应的密码
    --out output.apk                         //输出路径
    input.apk                                     //被签名的apk

示例：

    java -jar apksigner.jar sign  --ks key.jks  --ks-key-alias releasekey  --ks-pass pass:pp123456  --key-pass pass:pp123456  --out output.apk  input.apk   

apksigner还支持另外的一些选项，[详情点击这里](https://link.jianshu.com/?t=https://developer.android.com/studio/command-line/apksigner.html#options-sign-general)。包括指定min-sdk版本、max-sdk版本、输出详细信息、检查apk是否已经签名等等。

例如检查apk是否已经签名：

    java -jar apksigner.jar verify -v my.apk
