---
title: Parallel desktop安装Arch Linux
aside: 
   toc: true
tags: parallel_desktop Arch_Linux
published: false
---


1. 切换镜像源：

   ```
   vim /etc/pacman.d/mirrorlist
   ```

   增加：

   ```
   ##Tsinghua
   Server = http://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
   ##USTC
   Server = http://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
   ```

2. 新建分区：

   ```
   fdisk /dev/sda
   n
   p
   1
   w
   ```

   格式化新分区：

   ```
   mkfs.ext4 /dev/sda1
   ```

   挂载：

   ```
   mount /dev/sda1 /mnt
   ```

3. 安装：

   ```
   pacstrap /mnt base base-devel
   ```

4. 配置：

   ```
   genfstab -U /mnt >> /mnt/etc/fstab
   ```

   切换进新系统：

   ```
   arch-chroot /mnt
   ```

   Balabala...

5. 安装系统启动器：

   ```
   pacman -S grub
   grub-install --recheck /dev/sda
   grub-mkconfig -o /boot/grub/grub.cfg
   ```

6. 安装桌面环境

7. <kbd>(Fn+)ctrl+alt+F2</kbd>从图形登陆页面切换回命令行

8. 安装parallels tools：

   mount <-> umount 

   参考[该链接]( https://forum.parallels.com/threads/parallels-vm-tools-on-gentoo-vm-gcc-install-error.346287/) 

   共修改3处，除上述链接外，(tmp/kmods/)prl_fs/SharedFolders/Guest/Linux/prl_fs/super.c

   MS -> IS   …. SB   S   170L+220L  注释三行
   
   另：ubuntu下参考[链接](https://gist.github.com/mag911/1a5583a766467d6023584d738cee0d98)