---
title: fedora使用笔记
abbrlink: 35456
date: 2024-12-15 22:17:06
updated:
tags:
  - linux
  - gnome
categories: 笔记
---

## 美化

### 安装 gnome 扩展

1. 优化：GNOME Tweaks，管理 GNOME 桌面的少许区域

   ```bash
   sudo dnf install gnome-tweak-tool
   ```

### VS Code 隐藏顶部标题栏

按下`Ctrl`+`,`。
搜索"window.titlebarstyle"，将"window.titlebarstyle"修改为 custo，重启 vscode 即可生效。

## 配置

### 把用户文件下的文件夹该成中文

```bash
export LANG=en_US
xdg-user-dirs-gtk-update #会弹出一个确认框，选择"Update Names" 来确认更新文件夹名称操作
export LANG=zh_CN.UTF-8
```

### 电池续航

预配置的 power-profiles-daemon 在大多数系统上运行良好，但如果你仍遇到电池续航不佳的问题，可以尝试安装 tlp：

```bash
sudo dnf install tlp tlp-rdw
```

并屏蔽 power-profiles-daemon：

```bash
sudo systemctl mask power-profiles-daemon
```

同时安装 powertop：

```bash
sudo dnf install powertop
sudo powertop --auto-tune
```

### 安装 iBus+雾凇拼音中文输入法

- 安装 ibus-rime

  ```bash
  sudo dnf install ibus-rime
  ```

- 安装 Plum

  ```bash
  curl -fsSL https://raw.githubusercontent.com/rime/plum/master/rime-install | bash
  bash rime-install iDvel/rime-ice:others/recipes/full # 安装雾凇拼音配置文件
  ```

- 修改系统键盘配置
  在系统设置 ⇒ 键盘 ⇒ 输入源 ⇒ 添加 ⇒ 汉语 ⇒ 选择中文（rime）
- 重启

### 中文软件显示异常

```bash
sudo dnf remove google-noto-sans-cjk-vf-fonts
sudo dnf install google-noto-sans-cjk-fonts
```

## 驱动

### NVIDIA 驱动

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm #RPM Fusion
sudo dnf -y update
sudo dnf -y install akmod-nvidia #安装最新驱动
```

然后重启一下系统。
检查一下：

```bash
nvidia-smi
```

### CUDA

如果是 fedora39 可以看[官网](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Fedora&target_version=39&target_type=rpm_network)。
如果是更新的版本：

```bash
sudo wget https://developer.download.nvidia.com/compute/cuda/repos/fedora39/x86_64/cuda-fedora39.repo -P /etc/yum.repos.d/
sudo update
sudo dnf clean all
sudo dnf -y install cuda-toolkit-12-6
```

检查一下：

```bash
nvcc -V
```

如果是 "bash: nvcc: 未找到命令..."
这通常是因为环境变量 PATH 没有正确设置，导致系统无法找到 nvcc 执行文件。

1. 检查 CUDA 安装路径：
   通常 CUDA 工具包会被安装在 /usr/local/cuda- \<version> 目录下，其中 \<version> 是 CUDA 的版本号。如果不知道确切的版本号，可以使用如下命令来查找：

   ```bash
   ls /usr/local/ | grep cuda
   ```

2. 临时添加到 PATH:
   如果您知道 CUDA 安装的路径，您可以临时将它添加到 PATH 环境变量中以测试是否解决了问题。假设 CUDA 被安装到了 /usr/local/cuda-12.6,那么您可以这样做：

   ```bash
   export PATH=/usr/local/cuda-12.6/bin${PATH:+:${PATH}}
   export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
   ```

   然后再次尝试运行

   ```bash
   nvcc -V
   ```

   查看是否能找到 nvcc。

3. 添加 CUDA 的路径到环境变量中:

   ```bash
   sudo nano ~/.bashrc
   ```

   添加 CUDA 路径：
   在文件的末尾添加以下两行，确保将 cuda-12.6 替换为您安装的具体 CUDA 版本号，如果它不同的话。

   ```bash
   export PATH=/usr/local/cuda-12.6/bin${PATH:+:${PATH}}
   export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
   ```

   按下`Ctrl`+`O`保存，并按`Eenter`确认。
   按下`Ctrl`+`X`退出。
   使更改生效：
   您需要让新的环境变量设置立即生效，可以运行以下命令来重新加载 .bashrc 文件：

   ```bash
   source ~/.bashrc
   ```
