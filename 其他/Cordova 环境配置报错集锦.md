#Cordova 环境配置报错集锦
##cordova build android 打包报错
报错提示：

    Error: Could not find an installed version of Gradle either in Android Studio,
    or on your system to install the gradle wrapper. Please include gradle
    in your path, or install Android Studio

解决方案：

1. 手动下载gradle
`gradle-x.x-bin.zip` （x.x代表版本） 
根据需要下载某一版本 
地址：

    https://services.gradle.org/distributions

2. 添加系统环境变量

    path=D:\Program Files\gradle-4.3-rc-3\bin

路径为gradle实际存放路径
3. 关闭`cmd`或`powershell`窗口
4. 重新打开，输入`gradle -v`，查看`gradle`安装成功与否
5. 重新`build`


##gradle build
    You have not accepted the license agreements of the following SDK components

这个是由于sdk的版本不对称，根据报错提示更新相对应的sdk即可