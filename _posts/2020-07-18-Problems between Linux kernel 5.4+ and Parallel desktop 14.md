---
title: Problems between Linux kernel 5.4+ and Parallel Desktop 14
tags: parallel_desktop Linux
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