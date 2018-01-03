#怎么使用命令对APK包进行签名

##一、生成签名文件
windows中打开cmd，在命令模式中，进入JDK的安装目录的Bin子目录下。（我的JDK安装在E盘，所以先进入E盘，然后再进入JDK安装目录，如果jds配置了环境变量则不用进入到jdk相对应的安装目录）
如果使用的mac，系统集成了jdk，安装路径为`/Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home/bin`或者命令打开`open /Library/Java/`

通过`keytool.exe`工具来创建`keystore`库.
输入以下命令：
`keytool -genkeypair -alias -mydemo.keystore -keyalg RSA -validity 100 -keystore /Users/kayak/Desktop/mydemo.keystore`

命令说明如下：
* -genkeypair：指定生成数字证书
* -alias：指定生成数字证书的别名
* -keyalg：指定生成数字证书的算法  这里如RSA算法
* -validity：指定生成数字证书的有效期
* -keystore：指定生成数字证书的存储路径。
* -keystore默认在keytool.exe目录下，这里可以指定自己想要生成的路径/Users/kayak/Desktop/mydemo.keystore
回车
* 出现如图交互式界面
* 输入数字证书费密码
* 您的名字与姓氏是什么
* 您的组织单位名称是什么
* 您的组织名称是什么
* 您所在的城市或区域名称是什么
* 您所在的省/市/自治区名称是什么

完成后，keystore库创建完成，你可以在指定的保存目录下找到
使用jarsigner命令对未签名的APK安装包进行签名。使用JDK安装目录下bin子目录下的jarsigner.exe工具来进行签名。
然后把未签名的apk也拷贝到此目录

keytool用法：

-certreq     [-v] [-protected]
             [-alias <别名>] [-sigalg <sigalg>]
             [-file <csr_file>] [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-changealias [-v] [-protected] -alias <别名> -destalias <目标别名>
             [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-delete      [-v] [-protected] -alias <别名>
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-exportcert  [-v] [-rfc] [-protected]
             [-alias <别名>] [-file <认证文件>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-genkeypair  [-v] [-protected]
             [-alias <别名>]
             [-keyalg <keyalg>] [-keysize <密钥大小>]
             [-sigalg <sigalg>] [-dname <dname>]
             [-validity <valDays>] [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-genseckey   [-v] [-protected]
             [-alias <别名>] [-keypass <密钥库口令>]
             [-keyalg <keyalg>] [-keysize <密钥大小>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-help

-importcert  [-v] [-noprompt] [-trustcacerts] [-protected]
             [-alias <别名>]
             [-file <认证文件>] [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-importkeystore [-v]
             [-srckeystore <源密钥库>] [-destkeystore <目标密钥库>]
             [-srcstoretype <源存储类型>] [-deststoretype <目标存储类型>]
             [-srcstorepass <源存储库口令>] [-deststorepass <目标存储库口令>]
             [-srcprotected] [-destprotected]
             [-srcprovidername <源提供方名称>]
             [-destprovidername <目标提供方名称>]
             [-srcalias <源别名> [-destalias <目标别名>]
             [-srckeypass <源密钥库口令>] [-destkeypass <目标密钥库口令>]]
             [-noprompt]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-keypasswd   [-v] [-alias <别名>]
             [-keypass <旧密钥库口令>] [-new <新密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-list        [-v | -rfc] [-protected]
             [-alias <别名>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

-printcert   [-v] [-file <认证文件>]

-storepasswd [-v] [-new <新存储库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]

jarsigner用法： [选项] jar 文件别名
jarsigner -verify [选项] jar 文件

[-keystore <url>]           密钥库位置
[-storepass <口令>]         用于密钥库完整性的口令
[-storetype <类型>]         密钥库类型
[-keypass <口令>]           专用密钥的口令（如果不同）
[-sigfile <文件>]           .SF/.DSA 文件的名称
[-signedjar <文件>]         已签名的 JAR 文件的名称
[-digestalg <算法>]    摘要算法的名称
[-sigalg <算法>]       签名算法的名称
[-verify]                   验证已签名的 JAR 文件
[-verbose]                  签名/验证时输出详细信息
[-certs]                    输出详细信息和验证时显示证书
[-tsa <url>]                时间戳机构的位置
[-tsacert <别名>]           时间戳机构的公共密钥证书
[-altsigner <类>]           替代的签名机制的类名
[-altsignerpath <路径列表>] 替代的签名机制的位置
[-internalsf]               在签名块内包含 .SF 文件
[-sectionsonly]             不计算整个清单的散列
[-protected]                密钥库已保护验证路径
[-providerName <名称>]      提供者名称
[-providerClass <类>        加密服务提供者的名称
[-providerArg <参数>]] ... 主类文件和构造函数参数

##二、使用签名文件签名
使用如下命令进行签名：
`jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA -keystore mydemo.keystore -signedjar android-release-unsigned.apk test.apk mydemo.keystore` 

以上命令的说明：
* -verbose：指定生成详细输出
* -keystore：指定数字证书存储路径
* -signedjar：该选项的三个参数为1.签名后的apk包2.未签名的apk包3.数字证书别名
注意有效期哦。

##三、对签名apk包进行优化
sdk目录下tool目录下使用zipalign.exe工具优化APK安装包。
将已经签名的apk包放在zipalign.exe同目录下

使用如下命令：
`zipalign -f -v 4 -Note.apk -Notes.apk`
命令说明：
* -f：指定强制覆盖已有文件
* -v：指定生成详细输出
* 4：指定档案整理基于的字节数一般是4,也有基于32位的。
* -Note.apk：优化前APK
* -Notes.apk：优化后的APK
运行命令后，在该目录下生成一个-Notes.apk,这个就是优化过的APK安装包
，该安装包可以对外发布。