---
title: homebrew:build failure for macOS Sierra
tags: homebrew macOS
key: homebrew_build_failure
aside:
  toc: false
published: true
sharing: true
show_author_profile: true
show_subscribe: true
lightbox: true
---

最近使用homebrew尝试安装package时，遇到安装缓慢的问题，在安装过程中发现使用的是国外源，因此访问缓慢。此前曾使用清华大学的镜像源，完整替换分为两阶段：

1. [Homebrew的formula索引](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)，执行如下命令：

~~~bash
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

brew update
~~~
2. [Homebrew-bottles二进制预编译包](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew-bottles/):

~~~bash
# for bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

#for zsh
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
~~~

formula索引仅提供`brew update`更新时的仓库内容，使用`brew install package`时将首先查找对应的bottles预编译包，如果找到则直接使用，否则将下载源文件在本地执行编译工作（即build from source）。

此次检查并未发现镜像设置问题，但install时仍然提示build from source。搜索[相关博客](https://www.cnblogs.com/olenji/p/8196358.html)的过程中，发现有如下描述：“*homebrew是基于ruby解析你的下载安装脚本，如果你ruby版本不对还是会国外源去下载*”，开始怀疑本地ruby版本问题。

> homebrew 完全基于git和ruby，参考[官方网站](https://brew.sh)
  
`本地环境`{:.info}

**macOS Sierra 10.12.6**  
Homebrew: 2.2.5-28  
ruby:  ruby-2.4.5 [ x86_64 ]  
  &emsp;=* ruby-2.6.3 [ x86_64 ]  
  &emsp;# => - current  
  &emsp;# =* - current && default  
  &emsp;#  * - default
{:.info}

尝试更新ruby、brew版本均无果。

---

直接查看镜像源网站，翻找对应的软件包，发现所需软件的最新版本未提供macOS Sierra版本的预编译包（所提供的最低版本为macOS High Sierra），至此终于确定镜像源并未失效，而是镜像源根本未提供当前系统版本的bottles(xxx🙃)。（同时想到Homebrew官方早已不再提供对macOS Sierra的支持）

~~最终采用升级系统的暴力方式解决问题。~~

*p.s.后续发现当初基于jekyll搭建blog环境时，依赖的是ruby 2.4.5，上述切换ruby版本会导致jekyll命令失效，~~原地爆炸~~。*

