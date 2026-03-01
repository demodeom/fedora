# NVIDIA

Fedora 43 NVIDIA 显卡驱动安装与配置指南

## Fedora 系统安装概述

Fedora 操作系统的安装流程主要包含两个核心阶段：

1. **系统基础安装**：包括硬盘分区规划、磁盘加密配置等底层系统设置
2. **用户环境配置**：涵盖语言选择、键盘布局、时区设定、用户账户创建及密码设置等个性化配置

## NVIDIA 显卡驱动注意事项 {id="nvidia_1"}

### 驱动类型说明
Fedora 系统默认采用开源的 **Nouveau** 驱动，而非 NVIDIA 官方闭源驱动。在**用户环境配置**阶段，由于开源驱动的兼容性问题，系统可能出现无响应或卡死现象。

### 故障处理方案
若遇到系统卡死情况，请按以下步骤处理：
1. 将显示器连接线切换至主板的集成显卡接口（而非独立显卡接口）
2. 执行强制关机：**长按电源键 10 秒**直至设备完全关闭
3. 重启系统并继续安装流程

## NVIDIA 显卡驱动状态查询 {id="nvidia_2"}

### 查看当前显卡驱动类型
使用以下命令检测 NVIDIA 显卡设备信息：
```bash
lspci | grep -iE 'VGA|3D|nvidia'
```

该命令将显示显卡控制器信息，可用于确认显卡型号及当前使用的驱动类型。

## NVIDIA 官方闭源驱动安装 {id="nvidia_3"}

### 步骤一：配置 RPM Fusion 软件源
首先需要添加 RPM Fusion 仓库，该仓库提供 Fedora 官方源中未包含的第三方软件包：

```bash
# 添加 RPM Fusion free 仓库（开源软件）
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# 添加 RPM Fusion nonfree 仓库（专有软件）
sudo dnf install https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### 步骤二：安装 NVIDIA 驱动 {id="nvidia_4"}
```bash
# 安装 NVIDIA 驱动核心包（使用 akmod 框架，支持内核更新后自动重建模块）
sudo dnf install akmod-nvidia

# 安装 CUDA 支持包（提供 nvidia-smi 工具及 CUDA 计算支持）
sudo dnf install xorg-x11-drv-nvidia-cuda
```

### 安装后验证
安装完成后，重启系统并通过以下命令验证驱动安装状态：
```bash
# 查看 NVIDIA 驱动版本及 GPU 状态
nvidia-smi

# 或检查已加载的 NVIDIA 内核模块
lsmod | grep nvidia
```

## 内核升级与 NVIDIA 驱动维护指南

Linux 内核升级是系统维护的常规操作，但在使用 NVIDIA 闭源驱动的系统中，内核更新可能导致驱动与内核版本不兼容，进而引发显示异常（如分辨率错误）。

### 问题现象
内核升级后常见症状

- 显示器分辨率异常（如只能使用 1024x768）

### 根本原因

NVIDIA 闭源驱动以内核模块形式运行，当内核版本更新后，原有驱动模块需要重新编译以适配新内核。

#### 1. 更新系统（包含内核）

bash

```
# 更新所有软件包，包含新内核
sudo dnf update
```



#### 2. 重建 NVIDIA 驱动模块

**方法 A：完整重装（推荐）**



```bash
# 移除现有 NVIDIA 驱动
sudo dnf remove xorg-x11-drv-nvidia-cuda
sudo dnf remove akmod-nvidia

# 重新安装 NVIDIA 驱动
sudo dnf install akmod-nvidia
sudo dnf install xorg-x11-drv-nvidia-cuda
```



**方法 B：仅重建模块（更快）**



```bash
# 触发 akmod 自动重建
sudo akmods --force

# 重建 initramfs
sudo dracut --force
```



#### 3. 验证重建结果



```bash
# 检查 NVIDIA 模块是否为新内核编译
modinfo nvidia | grep vermagic

# 尝试加载模块
sudo modprobe nvidia

# 查看驱动状态
nvidia-smi
```



#### 4. 重启系统



```bash
# 使新内核和驱动生效
sudo reboot
```



## 参考资料
- [NVIDIA on Fedora desktops - 详细安装指南](https://github.com/Comprehensive-Wall28/Nvidia-Fedora-Guide)

---

> **注**：本文基于 Fedora 43 版本编写。不同 Fedora 版本的软件源 URL 中的版本号会自动适配，无需手动修改。




