---
title: "Gnome桌面macOS主题介绍"
date: 2024-02-20T00:00:00+08:00
draft: false
author: "GeekerHWH"
zh_tags:
    - Themes
    - Debian
image: /imagesInBlogs/macTheme/macbook.png
description: "How to make your Gnome Desktop like macOS"
toc: true
---

感谢[@vinceliuice's repository](https://github.com/vinceliuice)的开源仓库
以下是本教程中将使用的一些Github仓库：
- [https://github.com/vinceliuice/WhiteSur-gtk-theme](https://github.com/vinceliuice/WhiteSur-gtk-theme)
- [https://github.com/vinceliuice/WhiteSur-icon-theme](https://github.com/vinceliuice/WhiteSur-icon-theme)
- [https://github.com/vinceliuice/WhiteSur-wallpapers](https://github.com/vinceliuice/WhiteSur-wallpapers)

## 前提说明
本教程基于**Gnome桌面环境**
在这种情况下，Debian并非必需，但我将使用apt作为安装进度的示例

## 安装gnome扩展和git
```bash
sudo apt install gnome-tweaks gnome-shell-extensions gnome-shell-extension-manager git
```

## 克隆仓库并运行安装脚本
一条条复制下面脚本并运行即可
```bash
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git --depth=1
chmod -R +x WhiteSur-gtk-theme
WhiteSur-gtk-theme/install.sh -l -N mojave
sudo WhiteSur-gtk-theme/tweaks.sh -g

git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git --depth=1
chmod +x WhiteSur-icon-theme/install.sh
WhiteSur-icon-theme/install.sh

git clone https://github.com/vinceliuice/WhiteSur-wallpapers.git --depth=1
chmod +x WhiteSur-wallpapers/install-wallpapers.sh
WhiteSur-wallpapers/install-wallpapers.sh -t whitesur -c light
```

## 自定义你的gnome！
### 打开tweaks
tweaks用于设置标题栏按钮（左侧）和更改我们将要安装的桌面主题（右侧）

<div style="display: flex; justify-content: space-between;">
    <img src="/imagesInBlogs/macTheme/tweaks1.png" alt="tweaks1" style="width: 50%;">
    <img src="/imagesInBlogs/macTheme/tweaks2.png" alt="tweaks2" style="width: 50%;">
</div>

1. 在外观--图标中选择WhiteSur-Dark；Shell中选择WhiteSur-Dark；传统应用程序中选择WhiteSur-Dark
2. 打开窗口标题栏--最大化和最小化

### 打开extension-manager
1. 在系统扩展中找到“用户主题”，打开它，并将其设置为WhiteSur-Dark
2. 点击浏览
    1. 搜索并下载“Blur my Shell”
    2. 搜索并下载“X11 Gestures”（您应该按照Github上的说明操作）
    3. 搜索并下载“Dash to Dock”
3. 自行调整到自己喜欢的样子
### 更改壁纸
然后...我们就结束了！
![macbook](/imagesInBlogs/macTheme/macbook.png)

## 后话
我已经在我的CustomizeDebian12项目中支持了装机时直接安装macOS主题的脚本，欢迎各位装机党使用！