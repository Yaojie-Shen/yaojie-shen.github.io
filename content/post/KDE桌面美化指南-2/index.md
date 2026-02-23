---
title: KDE桌面美化指南 Part 2
date: 2021-02-20 22:45:49
updated: 2021-02-20 22:45:49
tags:
  - 主题
categories:
  - Linux
    - Theme
aliases:
  - /2021/02/20/KDE桌面美化指南-2/
enableComment: true
---

距离上次写美化过了好久，又回到了Manjaro。  
当前KDE已经更新到了5.20，自带的性能监控小插件功能更全也更好看了还多了一些新的插件修复了一些bug然后产生了新的bug，是时候更新下一篇了。

![2021-02-20T225737](images/2021-02-20T225737.png)

## 顶栏更新

首先是顶栏更新了一下，效果如下。

![2021-02-20T231002](images/2021-02-20T231002.png)

和之前主要的不同仅在于性能监控使用了KDE更新到5.20后自带的性能监控Widgets，想要监控什么性能指标自己加就行，啥都有，很全很KDE。自行设置了一下样式，怎么配置应该试试就会了吧～（大概）  
另外Weather Widgets的一个天气数据来源`yr.no`由于站点更新，当前插件已经不支持了，改用`OWM`吧。  

## 字体调整

Linux下字体渲染要比Windows下号很多，所以字号即使比较小看起来效果也比Windows下的大字要舒服些（个人觉得吧），中文字体推荐使用Noto Sans SC，不同的系统下命名可能不同，比如Manjaro下就叫Noto Sans CJK SC。字号稍微调大一些，同时把字体渲染拉满就行，凭个人感觉来。  

![2021-02-20T232309](images/2021-02-20T232309.png)

如果你装系统的时候选的是英文的话（事实上推荐这么做），这个中文字体很可能是缺失的，安装好语言的支持包即可，只要不去改变系统语言就行。或者单独安装Noto Sans字体。

```shell
~$ sudo pacman -Ss noto-fonts
extra/noto-fonts 20201226-1 [installed]
    Google Noto TTF fonts
extra/noto-fonts-cjk 20201206-1 [installed]
    Google Noto CJK fonts
extra/noto-fonts-emoji 20200916-1 [installed]
    Google Noto emoji fonts
extra/noto-fonts-extra 20201226-1
    Google Noto TTF fonts - additional variants
community/noto-fonts-compat 20151217-1 [installed]
    Google Noto TTF fonts (compat-package)
```

至于某些应用中，比如Chrome中的字体，在应用自身的设置中是可以调整的，可以独立配置。

## Shell相关的配置工作

用Linux怎么会少的了用Shell呢。个人来讲更多是通过下拉式的Shell，比如yakuake，对应Gnome下一般就是Guake。毕竟开个Konsole还要多一个窗口，窗口一多就乱了...

### Yakuake主题

yysy，yakuake就没找到好看的主题QwQ。所以最后是魔改了`transparent-tabs`这一个主题，一路用到现在，反正只要够简洁就行了，效果如下。

![2021-02-20T234156](images/2021-02-20T234156.png)

直接把主题中的白边删了，弄成了简单的一条透明的标签栏。[下载连接在此](images/https://drive.google.com/file/d/1qsMTPaod6x1N6C0_agJh9LKn0BDIJ6dh/view?usp=sharing)。解压后将主题文件夹放到`~/.local/share/yakuake/kns_skins/`即可，文件夹没有则自行创建。  

Warning: 使用这个主题后，你会发现Yakuake的设置没地方打开了！使用快捷键`Ctrl+Shift+,`可以打开设置。

### Yakuake终端颜色主题

除了Yakuake的边框外观，还有Shell的颜色配置需要设置，不然是没有上面的透明度的。直接在Yakuake终端的空白处`右键>Edit Current Profile...>Appearance`配置颜色主题，选择一个黑色主题即可，并点`Edit`拉低其中的透明度即可。

![2021-02-21T002141](images/2021-02-21T002141.png)

### ZSH主题和插件

ZSH不必多说吧，反正对于我来说已经习惯成自然了，必须装一个。

安装了ZSH之后首先装一下Oh My Zsh，安装方法见[官网](images/https://ohmyz.sh/#install)或者这边：

```shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

安装后会发现`.zshrc`更新了。接下来安装下ZSH的主题，配置一下常用的插件即可。主题推荐使用[powerlevel10k](images/https://github.com/romkatv/powerlevel10k)，因为我们用的是Oh My Zsh，所以安装方法也用Oh My Zsh对应的方法，引用下项目的README中的安装方法：

> **Oh My Zsh**  
> ```
> git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
> ```
> Users in mainland China can use the official mirror on gitee.com for faster download.
> 中国大陆用户可以使用 gitee.com 上的官方镜像加速下载.
> ```
> git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
> ```
> Set ZSH_THEME="powerlevel10k/powerlevel10k" in ~/.zshrc.

很贴心给了镜像。简单来讲就是运行给出的指令，然后改一下`.zshrc`就行了，下一次打开zsh时会跳出主题的一个配置过程，按照指示依次选定想要的样式，最后保存配置，ZSH主题就配置完成了，效果就和上面yakuake截图中一样。

至于插件，只需要autojump, zsh-autosuggestions和zsh-syntax-highlighting就行，安装方法可以参照另一个博主的[这篇文章](images/https://www.zrahh.com/archives/167.html)，个人觉得这几个足矣。其中`autojump`在Linux下的安装不用那么麻烦，而且还可能不见得装的上，manjaro直接从AUR装就可以了，或者`archlinuxcn`中带了。  

三个ZSh插件功能如下：

- **autojump** 任何位置使用`j xxx`即可跳转到对应的目录，而且不用写全目录名称。前提是这个目录之前访问过。
- **zsh-autosuggestions** 会记住之前的命令输入历史，在输入命令时自动显示出过去输入过的命令，按右方向键`>`即可自动填充。
- **zsh-syntax-highlighting** 会给输入的命令高亮，输错了的命令会显示成红色。

效果如下图：

![2021-02-21T003034](images/2021-02-21T003034.png)

## 输入法配置和主题

最后来讲下输入法的问题吧，毕竟打字还是要个中文输入法的。个人使用的是Fcitx，虽然Fcitx 5已经出了，但还是用的4。  

安装就不说了，主题依然是使用了一个魔改后的主题，受害者是Material主题，来源已经不清楚了，应该是从Github上找到。魔改后的效果如下图，附[下载链接](images/https://drive.google.com/file/d/192uBLNoPpNklZr8RbhuOxnrVmIXvvMAu/view?usp=sharing)，解压后放置到`~/.config/fcitx/skin/`，然后去Fcitx设置中改主题即可。

![2021-02-21T003807](images/2021-02-21T003807.png)

至于输入法的配置，个人使用的是LibPinyin，另外在LibPinyin的设置中开了一些模糊音和词库，以及添加了搜狗的细胞词库，总的来说还凑合，词的提示不怎么智能就是了。云拼音建议打开。

另外，一定要更新Fcitx以及其他的包到新版，旧的版本可能会有问题，比如细胞词库导入不了，或者云拼音出不来等等。
