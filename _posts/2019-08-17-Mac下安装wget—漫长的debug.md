---
title: Mac安装wget
tags: Mac
published: true
aside:
   toc: false
---


### Mac 下安装wget—漫长的debug

1. 下载wget源码包

2. tar zxvf ...

3. cd

4. ./config 报error，找不到openssl相关头文件，于是查找目录/usr/local/include和/usr/include，其中均未找到openssl，因此从本地找到openssl安装位置，将/include/openssl中的文件复制到/usr/local/include中，并在./config时添加- -with-ssl=openssl，成功

5. make 报error，一番折腾，得以继续make，在此过程中尝试升级了本地openssl版本，

   `brew upgrade openssl`，并连带upgrade了python等...

6. make继续报error，一番折腾，依据相关[blog](https://blog.csdn.net/terminatorsong/article/details/52332583)，添加环境变量：

   ```bash
   export LDFLAGS="-L/usr/local/opt/openssl/lib"
   export CPPFLAGS="-I/usr/local/opt/openssl/include" 
   export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig"
   ```

   尝试继续make，依然失败...

7. 灵机一动（xxx），重新./config - -with-ssl=openssl，无error，make，无error！！！

8. make install

9. 后知后觉：

   - 有blog说，在Mac下用clang编译openssl极其xxx，建议使用gcc编译

   - 结合之前开发过程中曾多次遇到的openssl问题，怀疑Mac下的openssl是个大坑（xxx），本次debug过程中发现，brew升级openssl已经到了1.0.2s版本，但`openssl version`命令显示的却是`OpenSSL 0.9.8zh`，在相关blog中找到如此描述“Mac下的Xcode自带openssl”，结合本地的Xcode一直未升级，保持在9.x版本，综上：

     ​	*怀疑：安装Xcode时自带了openssl，并一直未升级，后续开发过程中极有可能又手动安装了openssl，并为最新版本，导致本地的openssl版本不一致*


