---
title: "ArchLinux+Gnome简录"
date: 2023-01-17T17:39:19+08:00
draft: false
---

本篇文章中使用**Arch Linux** + **Gnome**
## Arch系统安装
1. **Arch Linux**下载镜像网站（ustc）: https://mirrors.ustc.edu.cn/archlinux/iso/
2. 下载启动盘制作工具**Rufus**: https://rufus.ie/en/
3. 使用Rufus将自己的U盘制作成启动盘，然后重启在Bios中选择启动项为usb设备，进入livecd进行下一步操作。
4. 进入livecd后，修改其镜像源为ustc。执行`vim /etc/pacman.d/mirrorlist`，删除所有内容，并加入: 
    ```toml
    Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
    ```
5. 执行`archinstall`脚本进行自定义、自动化安装（桌面选择Gnome。对于显卡驱动，如果使用intel核显，安装开源intel驱动；若是nvidia独显，安装闭源nvidia驱动）。
6. 安装完成后，执行`exit`，然后`reboot`即可。
7. 进入默认Gnome桌面。

## 基本配置

### Yay
aur（archlinux user repository）中有很多官方没有的软件包: https://aur.archlinux.org/
aur中的软件包需要自己编译安装，**yay**能够使这个过程自动化。
安装yay: 
执行`sudo vim /etc/pacman.conf`，在最后添加: 
```toml
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```
然后执行`sudo pacman -S archlinux-keyring`。安装完成后即可安装yay: `sudo pacman -S yay`。

### 开机自启
在Tweaks中可以设置开机自启的*桌面程序*。要使终端程序自启可以下载**gnome-session-properties**: `yay -S gnome-session-properties`。安装完成后的软件名为**Startup Applications**。

### proxy
下载clash: `sudo pacman -S clash`，启动: `clash`。待其数据库下载完成后，修改`~/.config/clash/config.yaml`。

将clash（/usr/bin/clash）添加到开机自启中。

### shell
下载zsh: `sudo pacman -S zsh`。进入`zsh`，执行`vim ~/.zshrc`，添加: 
```sh
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
```
git添加代理: 
`git config --global http.proxy http://127.0.0.1:7890`
`git config --global https.proxy http://127.0.0.1:7890`
~/.ssh/config中修改: 
```sh
Host github.com
   User "kennyissak@gmail.com"
   Hostname ssh.github.com
   PreferredAuthentications publickey
   Port 443
```

下载ohmyzsh: `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

插件: 
- autosuggestions: `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
- Syntax-highlighting: `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
  
主题: `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`

修改配置: `vim ~/.zshrc`
```sh
ZSH_THEME="powerlevel10k/powerlevel10k"

plugins=(
	git
	zsh-autosuggestions
	zsh-syntax-highlighting
)
```

## 常用软件安装

> 尽管通过yay一样可以下载官方源中的软件，个人更习惯pacman下载官方源软件，yay只下载aur软件。
### 命令行工具
**zoxide, exa, bat**（替代cd, ls, cat的shell工具）: `sudo pacman -S zoxide exa bat`
`vim ~/.zshrc`，添加: 
```sh
eval "$(zoxide init zsh)"
```

### 终端
**Alacritty**: `sudo pacman -S alacritty"
配置: https://github.com/Jarvisyy/dotfiles/blob/archlinux/alacritty/alacritty.yml
主题: https://github.com/eendroroy/alacritty-theme

### 编辑器
**neovim**: `sudo pacman -S neovim`
**neovide** (Gui): `sudo pacman -S neovide`
配置: https://github.com/Jarvisyy/dotfiles/tree/archlinux/nvim

**vscode**: `yay -S visual-studio-code-bin`

### 浏览器
**Firefox**: `sudo pacman -S firefox`
**Chrome**: `yay -S google-chrome`

### 音乐
**listen1**: `yay -S listen1-desktop-appimage`

### 通讯
**qq**: `yay -S linuxqq`
**wemeet**: `yay -S wemeet-bin`
**telegram**: `sudo pacman -S install telegram-desktop`

## Gnome配置

### 桌面缩放
我的桌面缩放使用1.25是完美的，在wayland下可以使用: `gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
`来解锁125缩放，但是会导致很多软件变“糊”。故本人使用X11: 在设置中设置缩放为200。然后写一个脚本: `mkdir ~/Documents/sh`，`vim ~/Documents/sh/display.sh`
```sh
#!/bin/sh
xrandr --output DP-2 --scale 1.6x1.6 --mode 1920x1080
```
将该脚本添加到开机自启中。

### MacOS风格美化

进入浏览器下载**Gnome Shell integration**插件，安装
- User Theme
- Applications Menu
- Blur my Shell
- Dash to Dock


**主题下载**: `git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git`。进入目录然后`./install.sh`
美化Firefox: `./tweaks.sh -f monterey`
美化GDM: `sudo ./tweaks.sh -g`

**图标下载**: `git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git`。进入目录: `./install.sh`

**壁纸下载**（随时间变化）: `git clone https://github.com/vinceliuice/WhiteSur-wallpapers.git`。进入目录: `sudo ./install-gnome-backgrounds.sh`
