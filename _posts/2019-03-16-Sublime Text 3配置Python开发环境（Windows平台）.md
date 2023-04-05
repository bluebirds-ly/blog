---
title: Sublime Text 3配置Python开发环境（Windows平台）
tags: SublimeText3 Python Windows
key: ST3PyWin
aside:
  toc: true
mathjax: true
mermaid: true
chart: true
published: true
sharing: true
show_author_profile: true
show_subscribe: true
lightbox: true
---

{:toc}

本文面向非计算机专业人士、Python初学者、希望通过一款合适编辑器提高生产力的群体撰写，行文描述尽可能细致，提供傻瓜式操作。
{:.info}

<!-- # Windows10安装Sublime Text 3及配置 python3 开发环境 -->

## 1 插件安装

> (全部配置完成大概30min~1h)(所有工作请在校园网及英文输入法下完成)

### 1.1 Package Control包管理

> 是安装后续所有插件的基础

1. 打开 Sublime Text 3 (简称*ST3*)，点击菜单栏preferences$\to$browse  packages，应当有一个 user 文件夹，点进去检查是否有名为*Package Control*的文件夹，若有则删除；返回上层目录至*Sublime Text 3*目录下，进入*Installed Packages*文件夹，再次检查，若有则删除；

2. 解压文件[Package Control.zip][]，请确保解压后的文件夹名称为*Package Control*，复制此文件夹；

3. 再次点击 ST3$\to$preferences$\to$browse packages ，在此目录下粘贴，完成后应当如下图所示：![img2.1.3][]

4. 重启ST3，进入 preferences 菜单，如果如下图所示：
   ![img2.1.4.1][]

   出现最后两项，点击 package control，如下所示：

   ![img2.1.4.2][]

   证明 package control 插件安装成功（按<kbd>ctrl+shift+p</kbd>关闭上述对话框）；

5. 对此插件进行配置

   - st3$\to$preferences$\to$browse packages，打开 user 目录，将文件[channel_v3.json][]复制到此处，然后请在命令行中查询并复制此文件的完整路径(将文件拖拽到命令行中即可)；

   - 点击st3$\to$preferences$\to$package settings$\to$package control$\to$settings-user，请删除此文件中的全部内容，然后粘贴以下文本：

    ~~~json
    {
        "bootstrapped": true, 
        "channels":
      [ 
    
      ] 
    } 
    ~~~
    注意请在`"channels"`后的方括号中粘贴上述提到的完整路径（带引号），粘贴后请将路径中的所有单斜杠`\`替换为双斜杠`\\`，保存此文件，至此 Package Control 插件配置全部完成。

6. 使用 package control 插件安装其他插件的方法：使用<kbd>ctrl+shift+p</kbd>组合快捷键（或者点击preferences$\to$package control）调出面板（再次按快捷键一次或多次即可关闭此面板），输入`install`按回车，随后输入想要安装的插件名称按回车即可安装。

### 1.2 BracketHighlighter

> 括号匹配插件，使代码结构更清晰可见

1. 按照[1.1.6](#11-package-control包管理)中提到的方法，输入`BracketHighlighter`按回车，注意在窗口左下方为状态栏，显示插件安装状态，如下图：

    ![img2.2.1.1][]

    静待插件安装完成（安装过程中可能会出现如下页面，此为提示信息，忽略即可，请等到状态栏处恢复正常，才可确定插件安装成功）；

    ![img2.2.1.2][]

2.  对此插件进行配置：ST3$\to$preferences$\to$browse packages，进入 user 目录，将文件[bh_core.sublime-settings][]复制到此处，重启 ST3 即可。 

### 1.3 主题颜色

> 让你的 ST3 更漂亮 
>
> 以下为本人采用的配色方案，应当为目前 ST3 最佳通用配色方案

#### 1.3.1 Theme-Soda

  同 [1.1.6](#11-package-control包管理)，直接安装即可，安装完成后请点击 preferences$\to$theme，然后选中`Soda Dark 3.sublime-theme`。

#### 1.3.2 Tomorrow Color Schemes

  直接安装即可，安装完成后请点击 preferences$\to$color scheme，然后选中`Tomorrow-Night-Eighties`。

*以上完成后请重启 ST3。*

### 1.4 PackageResourceViewer

> 为了使 BracketHighlighter 插件显示效果更棒，我们需要使用此插件来更改主题中的某些配置

1. 直接安装插件；

2. 安装完成后，<kbd>ctrl+shift+p</kbd>调出面板，然后输入 `PackageResourceViewer:Open Resource`（不用输全，它会自动出现匹配结果）回车，然后请往下翻，找到`Tomorrow Color Schemes`并选中，然后再找到`Tomorrow-Night- Eighties.tmTheme`（名称比较相似，请注意区分）并选中，在打开的文件中请下翻到最后，在`</array>`之前插入以下文本：

    ~~~xml
    <dict> 
      <key>name</key>
      <string>Bracket Default</string>
      <key>scope</key>
      <string>brackethighlighter.default</string>
      <key>settings</key>
        <dict>
            <key>foreground</key> 
        <string>#25C6FC</string> 
        </dict> 
    </dict> 
    ~~~

    请注意插入后进行调整对齐文本，完成后应当如下图所示：![img2.4.2][]红框中即为新增加的内容，保存此文件。

### 1.5 Anaconda

> 强大的代码补全提示功能，ST3 开发 python 必备插件  

1. 直接安装，重启 ST3；

2. 配置：ST3$\to$preferences$\to$browse packages，进入 user 目录，将文件 [Anaconda.sublime-settings][] 复制到此目录中，然后请在 ST3 中打开此复制后的文件（直接将文件 拖拽到 ST3 窗口中即可打开）；打开命令行，使用`where python`命令查询 python 编译器所在完整路径并复制，将刚刚打开的文件中第一行`"python_interpreter"`后面的`"python"`替换为刚刚复制的路径（带引号）（同样请将所有单斜杠替换为双斜杠），保存此文件，重启 ST3。

### 1.6 SublimeREPL

> 提供 python 交互式运行环境，可利用此插件进行输入输出

1. 直接安装；

2. 快捷键配置：为了快速便捷地使用此插件，我们需要对 SublimeREPL 的快捷键进行配置，ST3$\to$preferences$\to$Key Bindings，在打开的文件右侧 --User 中（应当为空文件）插入以下内容: 

    ~~~json
    [
        { 
        "keys": ["ctrl+alt+b"],
        "caption": "SublimeREPL: Python - RUN current file", 
            "command": "run_existing_window_command", 
            "args": { 
          "id": "repl_python_run", 
          "file": "" 
            } 
      } 
    ] 
    ~~~

    请点击 preferences$\to$browse packages，依次进入 SublimeREPL$\to$config$\to$Python 文件夹，然后在命令行中查询 Main.sublime-menu 文件的完整路径，然后在上述插入的内容中 `"file"`后面的引号中粘贴此路径（同样替换单双斜杠），保存此文件，重启 ST3，快捷键配置完成，以后即可使用组合快捷键<kbd>ctrl+alt+b</kbd>调用此插件（即运行 python程序）。 

### 1.7 ST3综合配置

> 对 ST3 配置进行优化，使它看起来更顺眼 

Preferences$\to$browse packages，进入 user 文件夹，将文件[Preferences.sublime-settings][]粘贴／替换到此文件夹下，重启 ST3，大功告成！

## 2 ST3开发Python综述

> 经过以上安装配置，开发 python 所需的基本插件都已安装完成，可以愉快的进行 python 开发了，下面是对 ST3 使用方法的一些简介

1. 新建文件，在状态栏右下角可以进行开发语言的选择：![img3.1.1][]

   点击Plain Text，选择 python，即可进行编辑，正在进行编辑而未保存的文件会在窗口上有所提示：

   ![img3.1.2][] ==> ![img3.1.3][]

   编辑完成后<kbd>ctrl+s</kbd>保存，然后有以下几个选项：

   - <kbd>Ctrl+Shift+b</kbd>：进行选择，是只编译（检查语法错误）还是编译并运行

   - <kbd>Ctrl+b</kbd>：执行上次操作（上次只编译则此次也只编译，上次为运行则此次同样为运行）

   - <kbd>Ctrl+alt+b</kbd>：调用*SublimeREPL*插件来运行此 python 程序。 以上前两种方法，结果会在窗口下方的面板中显示，且只能输出结果而无法进行输入；第三种方法会打开一个新的窗口显示结果，此法可以在新窗口中进行输入；

2. 编辑时输入少许字母，候选补全代码自动显示，按<kbd>enter</kbd>或<kbd>tab</kbd>即可快速补全；

3. 在 view$\to$layout 中可以选择窗口视图，例如 columns:2，将以两个并列窗口的方式显示；

4. 更多彩蛋请自行摸索~祝开发愉快。

<!-- ---

> ***Author:lanyan***

> ***2019.3.16***

> ***©All Rights Reserved***

---
 -->
[img2.1.3]: /blog/assets/images/ST3PyWin/2.1.3.png
[img2.1.4.1]:/blog/assets/images/ST3PyWin/2.1.4.1.png
[img2.1.4.2]:/blog/assets/images/ST3PyWin/2.1.4.2.png
[img2.2.1.1]:/blog/assets/images/ST3PyWin/2.2.1.1.png
[img2.2.1.2]:/blog/assets/images/ST3PyWin/2.2.1.2.png
[img2.4.2]:/blog/assets/images/ST3PyWin/2.4.2.png
[img3.1.1]:/blog/assets/images/ST3PyWin/3.1.1.png
[img3.1.2]:/blog/assets/images/ST3PyWin/3.1.2.png
[img3.1.3]:/blog/assets/images/ST3PyWin/3.1.3.png
[安装包]:docs/Sublime%20Text%20Build%203200%20x64%20Setup.exe
[Package Control.zip]:docs/Package%20Control.zip
[channel_v3.json]:docs/channel_v3.json
[bh_core.sublime-settings]:docs/bh_core.sublime-settings
[Anaconda.sublime-settings]:docs/Anaconda.sublime-settings
[Preferences.sublime-settings]:docs/Preferences.sublime-settings