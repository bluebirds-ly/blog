---
title: Problems between Linux kernel 5.4+ and Parallel Desktop 14
tags: parallel_desktop Linux Manjaro_Linux
key: pd14_linux_kernel
aside:
  toc: false
published: true
sharing: true
show_author_profile: true
show_subscribe: true
lightbox: true
---

由于之前的Arch Linux桌面环境一直存在问题，最近试水Manjaro，出现和Arch安装及使用中类似的问题（可以预见），经过反复试验，印证了之前的猜想：Linux内核5.4之后的版本（5.2可用，5.3未知）所做的更新，将会导致在Parallel Desktop 14（15未知）中的安装过程出现种种问题，主要表现为桌面环境异常，以及Parallel Tools无法安装。前者主要原因为Xorg Server无法识别到有效的screen，`/var/log/Xorg.0.log`可见；对于后者，Parallel Tools 14本身一直存在bug，更新版本的内核加剧了该问题，具体原因及解决方案尚且未知。

**Conclusion**
如果使用Parallel Desktop 14安装Arch系Linux桌面环境，请使用较旧版本的Linux kernel（5.2以下）。
{:.warning}

---

**更新**：使用Parallel Desktop 15成功安装Manjaro 19.0，以下记录安装要点。

OS: Manjaro 19.0 Kyria  
Kernel: x86_64 Linux 5.4.44-1-MANJARO
{:.info}

1. Parallel Desktop 15版本能够支持Linux kernel 5.4，尚未支持5.6，请注意选用合适的Manjaro发行版。
2. 安装过程中同样会出现screen not found的问题，解决方案参见[Manjaro LiveCD virtual machine shows black screen after boot](https://kb.parallels.com/en/124751)，Method 1成功解决，其余未尝试。注意在PD15中，要更改的配置文件内容如下：

```xml
<Video VideoMemorySize_patch="1" dyn_lists="">
    <Enabled>1</Enabled>
    <Type>1</Type>
    <VideoMemorySize>64</VideoMemorySize>
    <FifoSize>1048576</FifoSize>
    <EnableDirectXShaders>1</EnableDirectXShaders>
    <ScreenResolutions dyn_lists="ScreenResolution 0">
        <Enabled>0</Enabled>
    </ScreenResolutions>
    <Enable3DAcceleration>0</Enable3DAcceleration>
    <EnableVSync>1</EnableVSync>
    <MaxDisplays>0</MaxDisplays>
    <EnableHiResDrawing>0</EnableHiResDrawing>
    <UseHiResInGuest>1</UseHiResInGuest>
    <HostScaleFactor>2</HostScaleFactor>
    <NativeScalingInGuest>0</NativeScalingInGuest>
    <ApertureOnlyCapable>1</ApertureOnlyCapable>
</Video>
```
3. 参考[Parallel Desktop and Manjaro Linux](https://www.linux.org/threads/parallels-desktop-and-manjaro-linux.29439/)，安装Parallel Tools 15。该版本不再需要修改PD Tools内容。

> 参考资料：
>
> 1. [最受欢迎的Linux发行版, Manjaro折腾全记录（超长超详细）](https://cloud.tencent.com/developer/article/1390999)
