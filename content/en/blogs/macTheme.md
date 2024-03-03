---
title: "How to make your Gnome Desktop like macOS"
date: 2024-01-20T00:00:00+08:00
draft: false
author: "GeekerHWH"
category:
    - Technology
en_tags:
    - Debian
    - Linux
    - Themes
image: /imagesInBlogs/macTheme/macbook.png
description: "How to make your Gnome Desktop like macOS"
toc: true
---
Thanks for [@vinceliuice's repository](https://github.com/vinceliuice)
Here are some repos that will be used in this tutorial:
- [https://github.com/vinceliuice/WhiteSur-gtk-theme](https://github.com/vinceliuice/WhiteSur-gtk-theme)
- [https://github.com/vinceliuice/WhiteSur-icon-theme](https://github.com/vinceliuice/WhiteSur-icon-theme)
- [https://github.com/vinceliuice/WhiteSur-wallpapers](https://github.com/vinceliuice/WhiteSur-wallpapers)

## Prerequisite
This tutorial is based on **[Gnome Desktop Environment](https://www.gnome.org/)**
Debian is not essential in this case, but I will use `apt` as the example in intalling progress

### Install gnome extensions and git
```bash
sudo apt install gnome-tweaks gnome-shell-extensions gnome-shell-extension-manager git
```

### Clone the repository and Run install scripts
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

## Customize your gnome!
### open tweaks
tweaks is used to set up your TitleBar Bottons (Left) and
 to change the Desktop theme that we will install (Right)
<div style="display: flex; justify-content: space-between;">
    <img src="/imagesInBlogs/macTheme/tweaks1.png" alt="tweaks1" style="width: 50%;">
    <img src="/imagesInBlogs/macTheme/tweaks2.png" alt="tweaks2" style="width: 50%;">
</div>

1. In Appearance--Icons choose WhiteSur-Dark; Shell choose WhiteSur-Dark; Legacy Applications choose WhiteSur-Dark
2. Turn on Window Titlebars--Maximize & Minimize

### open extension-manager
1. find "User Themes" in System Extensions, turn on it, set it to WhiteSur-Dark
2. Click Browse
   1. search and download "Blur my Shell"
   2. search and download "X11 Gestures" (you should follow the instructions in Github)
   3. search and download "Dash to Dock"
3. Tuning the settings yourself!
### change the background!
and ... we are done!
![macbook](/imagesInBlogs/macTheme/macbook.png)

## prompt
I've already support install macOS theme in my projects CustomizeDebian12, please check it out if you are interested in it! : )