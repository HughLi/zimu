---
title: JenKins在线打包坑点总结
date: 2017-07-16 18:27:28
tags:
---
#### JenKins iOS在线打包 坑点备忘
##### 1. 证书配置
````
1. 文件配置中的版本号要和Xcode所描述的版本号完全一致
2. 设置的provision_profile 是在线平台给配置的,在配置的时候是平台使用平台上面的证书预先为项目申请了bundleID 所以BundleID 和provision_profile都将是由平台提供
3. 项目所能支持的最低的iOS版本是要和Xcode上面的完全一致
4. 目前公司的在线打包平台所配置的对文件结构有比较严格的要求,要求运行包必须在包里面.XCodeProj也就是SVN/Git的文件目录为/Proj/Proj/.xCodeProj...
5. 自动签名需要关闭,不然会导致签名失败
```` 

#### 2. 打包apk报错信息
>Check dependencies
[BEROR]XXXX has conflicting provisioning settings. XXXX is automatically signed, but provisioning profile XXXXXX has been manually specified. Set the provisioning profile value to "Automatic" in the build settings editor, or switch to manual signing in the project editor.
[BEROR]Code signing is required for product type 'Application' in SDK 'iOS 10.0'
[BEROR]Code signing is required for product type 'Application' in SDK 'iOS 10.0'

报错原因为没有取消自动获取签名的选项

>clang: error: linker command failed with exit code 1 (use -v to see invocation)
Ld build/XXXXX.build/Release-iphoneos/XXXXX.build/Objects-normal/armv7/XXXXX normal armv7
    cd /Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp
    export IPHONEOS_DEPLOYMENT_TARGET=6.0
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Library/Frameworks/Python.framework/Versions/3.6/bin:/Library/Frameworks/Python.framework/Versions/2.7/bin:/Library/Frameworks/Python.framework/Versions/2.7/bin:/usr/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin"
    /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang++ -arch armv7 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.0.sdk -L/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/build/Release-iphoneos -L/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/XXXXX/ThirdPart/libWeiboSDK -L/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/XXXXX/ThirdPart/ZBarSDK -L/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/XXXXX/ThirdPart/WeChatSDK -F/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/build/Release-iphoneos -F/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/XXXXX/lib -F/Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/XXXXX -filelist /Users/Bone/.jenkins/jobs/XXXXX_iOS_Release/workspace/tmp/build/XXXXX.build/Release-iphoneos/XXXXX.build/Objects-normal/armv7/XXXXX.LinkFileList -Xlinker -rpath -Xlinker 

报错原因:没有正确配置项目所能支持的最低iOS版本号,该项错误可以获取到.ipa文件,但是.ipa文件的文件大小特别的小

>所有的配置都要做到仔细检查防止在最后打包的时候,不能成功获取到.ipa文件;在打包的时候 需要特别注意打包的文件生成顺序,一般说来会优先生成我们的.app文件,然后才是.ipa文件,目前的jenkins平台生成的ipa文件为两个 外加source文件的备份,一共会有4个文件生成.

