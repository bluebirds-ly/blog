---
title: Parallel desktop安装Arch Linux
aside: 
   toc: true
tags: parallel_desktop Arch_Linux
published: true
key: PdArch
sharing: true
show_author_profile: true
show_subscribe: true
---


1. 切换镜像源：

   ~~~bash
   vim /etc/pacman.d/mirrorlist
   ~~~

   增加：

   ~~~bash
   ##Tsinghua
   Server = http://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
   ##USTC
   Server = http://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
   ~~~

2. 新建分区：

   ~~~bash
   fdisk /dev/sda
   n
   p
   1
   enter
   enter
   w
   ~~~

   格式化新分区：

   ~~~bash
   mkfs.ext4 /dev/sda1
   ~~~

   挂载：

   ~~~bash
   mount /dev/sda1 /mnt
   ~~~

3. 安装：

   ~~~bash
   pacstrap /mnt base base-devel linux linux-firmware
   ~~~

   如果出现如下类似错误：

   ~~~bash
   invalid or corrupted package (PGP Signature)
   ~~~

   可依次执行`pacman -S archlinux-keyring` & `pacman -S ca-certificates`，随后再次进行尝试。

4. 配置：

   ~~~bash
   genfstab -U /mnt >> /mnt/etc/fstab
   ~~~

   切换进新系统：

   ~~~bash
   arch-chroot /mnt
   ~~~

   - 时区设置：

     ~~~bash
     ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
     #设置与硬件时间同步
     hwclock --systohc --utc
     ~~~

     使用`timedatectl status`查看时间状态。

   - 设置语言：

     ~~~bash
     echo 'LANG=en_US.UTF-8' >> /etc/locale.conf
     ~~~

   - 设置本地化：

     ~~~bash
     vim /etc/locale.gen
     #反注释
     en_US.UTF-8 UTF-8
     zh_CN.UTF-8 UTF-8
     ~~~

     生成本地化：

     ~~~bash
     locale-gen
     ~~~

   - 设置网络：

     ~~~bash
     echo 'yourhostname' >> /etc/hostname
     ~~~

     ~~~bash
     vim /etc/hosts
     >>>
     127.0.0.1 localhost
     ::1 localhost
     127.0.1.1 yourhostname.localdomain yourhostname
     <<<
     ~~~

     安装有线网络模块：

     ~~~bash
     pacman -S dhcpcd
     ~~~

     安装无线网络模块：

     ~~~bash
     pacman -S netctl iw wpa_supplicant dialog
     ~~~

     安装网络管理器：

     ~~~bash
     pacman -S networkmanager
     ~~~

     启用网络管理服务：

     ~~~bash
     systemctl enable NetworkManager
     ~~~

     `/etc/resolv.conf`中包含DNS域名解析服务，由上述NetworkManager负责配置。如发现无法连接网络（域名解析出错），可尝试手动修改上述文件，设置DNS服务提供商。

   - 设置root用户密码：

     ~~~bash
     passwd root
     ~~~

     创建普通用户：

     ~~~bash
     useradd -m -G wheel,audio,video your_user_name
     ~~~

     设置普通用户密码：

     ~~~bash
     passwd your_user_name
     ~~~
     
     为普通用户添加sudo权限：
     
     ~~~bash
     visudo /etc/sudoers
     # 在root ALL=(ALL) ALL下添加
     your_user_name ALL=(ALL) ALL
     ~~~

5. 安装系统启动器：

   ~~~bash
   pacman -S grub
   grub-install --recheck /dev/sda
   grub-mkconfig -o /boot/grub/grub.cfg
   ~~~

6. 重启系统：

   ~~~bash
   reboot
   ~~~

7. 安装桌面环境（以xfce4为例）：

   1. 安装xorg：

      ~~~bash
      pacman -S xorg
      ~~~

   2. 安装xfce4桌面环境：

      ~~~bash
      pacman -S xfce4 xfce4-goodies
      ~~~

   3. 安装桌面显示管理器（以sddm(Simple Desktop Display Manager)为例）：

      ~~~bash
      pacman -S sddm
      systemctl enable sddm
      ~~~

   4. 安装字体：

      - 中文字体：

        ~~~bash
        pacman -S wqy-microhei
        ~~~

      - 英文字体：

        ~~~bash
        pacman -S ttf-dejavu
        ~~~

   在此过程中，可使用<kbd>(Fn+)ctrl+alt+F2</kbd>从图形登陆页面切换回命令行，以进行必要操作。

   在grub引导界面，相关启动项后添加项`3`可强制使用终端模式启动。

   使用`systemctl --type=service`查看系统服务状态。

8. 安装配置zsh：

   - 安装zsh

     ~~~bash
     pacman -S zsh
     ~~~

   - 设置默认shell

     ~~~bash
     vim /etc/passwd
     # 将要修改的用户的shell路径改为 /usr/bin/zsh
     # 重启系统生效
     ~~~

   - 安装oh-my-zsh（首先安装`git`）

     ~~~bash
     sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
     ~~~

   - 配置oh-my-zsh

     ~~~bash
     git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
     git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
     ~~~

     ~~~bash
     vim ~/.zshrc
     # 常用配置
     ZSH_THEME="random"
     plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
     ~~~

9. 安装parallels tools：

   1. 挂载安装光盘

      ~~~bash
      mkdir /mnt/cdrom
      mount /dev/cdrom /mnt/cdrom
      cd /mnt/cdrom
      ~~~

   2. 复制安装目录，修改文件内容

      ~~~bash
      cp -r /mnt/cdrom ~/Desktop
      cd ~/Desktop/cdrom/kmods
      tar -xzf prl_mod.tar.gz
      rm prl_mod.tar.gz
      tar -zcvf prl_mod.tar.gz . dkms.conf Makefile.kmods
      cd installer/
      sudo ./install-cli.sh -i --verbose
      # 根据报错信息直接注释错误行
      # 需安装linux-headers
      ~~~


> 参考资料：
>
> 1. [Xfce4 hangs at logon](https://bbs.archlinux.org/viewtopic.php?id=235604)
> 2. [Wireless network configuration](https://wiki.archlinux.org/index.php/Wireless_network_configuration_(简体中文)#设备驱动)
> 3. [Installation guide](https://wiki.archlinux.org/index.php/installation_guide#Update_the_system_clock)
> 4. [Parallels+Arch Linux](https://wiki.archlinux.org/index.php/Parallels)
> 5. [Installing Arch Linux OS in Parallels Desktop](https://kb.parallels.com/en/124124?language=en)
> 6. [csdn](https://blog.csdn.net/r8l8q8/article/details/76516523)
> 7. [pd archlinux](https://wiki.archlinux.org/index.php/Parallels_Desktop)
> 8. [修改pd tools适应ubuntu](https://gist.github.com/mag911/1a5583a766467d6023584d738cee0d98)
> 9. [parallel forum]( https://forum.parallels.com/threads/parallels-vm-tools-on-gentoo-vm-gcc-install-error.346287/) 
> 10. [Arch_Linux_Archive](https://wiki.archlinux.org/index.php/Arch_Linux_Archive#/packages)
> 11. [Downgrading_packages](https://wiki.archlinux.org/index.php/Downgrading_packages)