---
title: gnome桌面美化和优化
abbrlink: ac136077
date: 2025-05-27 23:27:42
updated: 2025-05-27 23:27:42
tags:
  - linux
  - gnome
  - 美化
categories: 笔记
keywords:
cover:
---

本文章基于 fedora42

## gnome 扩展

1. 安装 gnome 优化和扩展

   ```bash
   sudo dnf install gnome-tweaks gnome-extensions-app
   ```

2. 添加扩展

   推荐扩展：

   - [**AppIndicator and KStatusNotifierItem Support**](https://extensions.gnome.org/extension/615/appindicator-support/)支持显示传统托盘图标（如网络管理器、音量控制等），兼容旧版应用通知。
   - [**Blur my Shell**](https://extensions.gnome.org/extension/3193/blur-my-shell/)为 Gnome Shell 界面添加高斯模糊效果，包括顶部状态栏、活动概览和应用窗口背景，增强视觉美感 。
   - [**Dash to Dock**](https://extensions.gnome.org/extension/307/dash-to-dock/)将左侧的应用启动器转换为底部或侧边的 Dock 栏，支持自定义图标大小、动画效果、位置等，提升操作便捷性 。
   - [**User Themes**](https://extensions.gnome.org/extension/19/user-themes/)允许用户安装和使用第三方主题、图标和光标样式，丰富桌面个性化设置 。
   - [**Caffeine**](https://extensions.gnome.org/extension/517/caffeine/)阻止屏幕自动锁屏、休眠或进入屏保状态，适用于观看视频、演示文稿等场景 。
   - [**Vitals**](https://extensions.gnome.org/extension/1460/vitals/)在顶部状态栏实时显示 CPU 使用率、内存占用、网络流量等系统信息，方便监控系统状态 。

3. gnome 升级后扩展不支持
   **问题现象**：Gnome 升级后，部分扩展可能因版本不兼容而失效，界面显示"`此扩展与您的 Gnome 版本不兼容`"。

   ```bash
    gsettings set org.gnome.shell allow-extension-updates true
   ```

## 主题美化

- gtk 主题:[Colloid gtk theme](https://github.com/vinceliuice/Colloid-gtk-theme)
- 图标主题:[papirus-icon-theme](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme)
- 鼠标主题:[Bibata Cursor](https://github.com/ful1e5/Bibata_Cursor)
- 终端字体:[Maple Mono NF CN](https://github.com/subframe7536/Maple-font)

## 性能优化

- 禁用缓解措施

  可以提高多线程系统的性能。CPU 核心数越多，性能提升越大，范围在 5-30% 之间。如果你通过网络共享服务和文件，或在虚拟机中使用 Fedora，请不要执行此操作。

  现代 Intel CPU（10 代以上）在禁用缓解措施后性能提升不明显。因此，禁用缓解措施可能会带来一些安全风险，但仍可能提高系统的 CPU 性能。

  ```bash
  sudo grubby --update-kernel=ALL --args="mitigations=off"
  ```

- 现代待机

  可能会在笔记本电脑进入睡眠状态时提高电池续航时间。

  ```bash
  sudo grubby --update-kernel=ALL --args="mem_sleep_default=s2idle"
  ```

  如果 "s2idle" 对你不起作用（例如使用 Alder Lake CPU 的用户），你可能需要参考这个

- 启用 nvidia-modeset

  对于配备 Nvidia GPU 的笔记本电脑很有用。对某些 PRIME 相关的互操作性功能是必需的。

  ```bash
  sudo grubby --update-kernel=ALL --args="nvidia-drm.modeset=1"
  ```

- 禁用 NetworkManager-wait-online.service

  禁用它可以将启动时间至少减少 15-20 秒：

  ```bash
  sudo systemctl disable NetworkManager-wait-online.service
  ```

- 从启动应用中禁用 Gnome Software

  由于某些原因，Gnome Software 会在启动时自动运行，即使在每次启动时并不需要它（除非你希望它在后台进行更新）。这会占用至少 100MB 到 900MB 的内存（根据非正式报告）。你可以通过以下方式阻止它自动启动：

  ```bash
  sudo rm /etc/xdg/autostart/org.gnome.Software.desktop
  ```
