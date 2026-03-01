# GnomeDesktop

Fedora 43 GNOME 桌面环境优化指南

## 概述

GNOME 是 Fedora 的默认桌面环境，以其简洁优雅的设计著称。通过安装适当的工具和扩展，可以大幅提升用户体验和工作效率。本文档详细介绍 GNOME 桌面的优化配置流程。

## 基础优化工具

### GNOME Tweaks（优化工具）

GNOME Tweaks 是定制 GNOME 桌面的核心工具，提供图形化界面调整隐藏设置。

#### 安装

```bash
sudo dnf install gnome-tweaks
```

#### 主要功能

| 配置类别       | 可调整项                       |
| -------------- | ------------------------------ |
| **外观**       | 主题、图标、光标、Shell 主题   |
| **字体**       | 界面字体、文档字体、等宽字体   |
| **键盘与鼠标** | 按键延迟、鼠标加速、触摸板设置 |
| **窗口管理**   | 窗口吸附、焦点跟随、标题栏按钮 |
| **顶部栏**     | 星期显示、秒数显示、电池百分比 |
| **开机启动**   | 管理开机自启程序               |

#### 启动方式

```bash
# 命令行启动
gnome-tweaks

# 或通过应用程序菜单
# 应用程序 → 工具 → 优化
```

### Extension Manager（扩展管理器） {id="extension-manager_1"}

Extension Manager 是管理 GNOME Shell 扩展的图形化工具，提供浏览、安装、配置一站式服务。

#### 安装

```bash
flatpak install -y flathub com.mattjakeman.ExtensionManager
```

#### 启动方式

```bash
# 命令行启动
flatpak run com.mattjakeman.ExtensionManager

# 或通过应用程序菜单
# 应用程序 → 系统工具 → Extension Manager
```

## 推荐 GNOME Shell 扩展

### 扩展安装方式

#### 通过 Extension Manager（推荐）

1. 打开 Extension Manager
2. 在「浏览」标签页搜索扩展
3. 点击「安装」按钮
4. 在「已安装」标签页配置

### 个人常用扩展详解

#### 1. AppIndicator and KStatusNotifierItem Support

**扩展 UUID**：`appindicatorsupport@rgcjonas.gmail.com`

**功能说明**：

- 向 GNOME Shell 顶部栏添加应用程序指示器支持
- 兼容 KStatusNotifierItem 和旧版系统托盘图标
- 使 Telegram、Dropbox、Skype 等应用图标正常显示

**配置建议**：

- 无需额外配置，安装即用
- 图标自动显示在顶部栏右侧

#### 2. Dash to Panel

**扩展 UUID**：`dash-to-panel@jderose9.github.com`

**功能说明**：

- 将 GNOME 的应用程序启动器（Dash）合并到顶部面板
- 创建类似于 Windows 7+ 或 KDE Plasma 的任务栏
- 整合应用程序启动器、运行中应用和系统托盘

**核心特性**：

| 功能           | 说明                           |
| -------------- | ------------------------------ |
| **任务栏样式** | 图标式任务栏，显示运行中的应用 |
| **应用分组**   | 相同应用图标自动分组           |
| **预览窗口**   | 悬停显示窗口缩略图             |
| **自定义位置** | 可调整面板位置（顶部/底部）    |
| **时钟合并**   | 可选择是否保留原顶部栏时钟     |

**常用配置**：

- 面板位置：底部（更接近传统桌面习惯）
- 图标大小：32px
- 启用窗口预览
- 设置智能隐藏

#### 3. Input Method Panel

**扩展 UUID**：`inputmethodpanel@extensions.gnome-shell.chinying.com`

**功能说明**：

- 优化输入法切换器的显示效果
- 提供更美观的输入法状态指示
- 改善 Fcitx5 等输入法框架的视觉体验

**配置建议**：

- 安装后自动生效
- 配合 Fcitx5 使用效果最佳
- 可在扩展设置中调整显示位置

#### 4. User Themes

**扩展 UUID**：`user-theme@gnome-shell-extensions.gcampax.github.com`

**功能说明**：

- 允许用户加载自定义 Shell 主题
- 扩展 GNOME Tweaks 的主题定制能力
- 支持从 `~/.themes` 目录加载主题

**使用步骤**：

```bash
# 创建主题目录
mkdir -p ~/.themes

# 下载主题到此目录（示例）
cd ~/.themes
git clone https://github.com/主题仓库

# 通过 GNOME Tweaks 选择主题
# 外观 → Shell → 选择下载的主题
```

#### 5. Bilingual App Search

**扩展 UUID**：`bilingual-app-search@diegovargas.com.br`

**功能说明**：

- 增强 GNOME 应用搜索功能
- 支持以英文和系统语言同时搜索应用
- 解决中文环境下英文应用名搜索不到的问题

**应用场景**：

| 输入关键词 | 可找到的应用     |
| ---------- | ---------------- |
| "火狐"     | Firefox          |
| "Firefox"  | Firefox          |
| "文件"     | Files (Nautilus) |
| "终端"     | Terminal         |

## 扩展配置最佳实践

### 安装顺序建议

1. 首先安装 **Extension Manager**（管理工具）
2. 安装 **User Themes**（主题基础）
3. 安装 **AppIndicator**（系统托盘）
4. 安装 **Dash to Panel**（界面改造）
5. 安装其他功能扩展

### 配置文件备份

```bash
# 备份当前扩展配置
dconf dump /org/gnome/shell/extensions/ > ~/gnome-extensions-backup.conf

# 恢复扩展配置
dconf load /org/gnome/shell/extensions/ < ~/gnome-extensions-backup.conf
```

### 性能优化建议

- 避免同时启用过多扩展（建议不超过10个）
- 定期检查扩展更新
- 禁用不常用的扩展
- 使用 `gnome-extensions-app` 监控性能影响

## 主题美化进阶

### 安装 GNOME Look 主题

```bash
# 安装主题所需依赖
sudo dnf install gnome-shell-extensions-user-theme

# 下载主题
wget https://dl.opendesktop.org/dl/主题文件.tar.xz

# 解压到主题目录
tar -xf 主题文件.tar.xz -C ~/.themes/
```

### 推荐主题资源

- [GNOME Look](https://www.gnome-look.org/) - 最大的 GNOME 主题社区
- [Adwaita 主题变体](https://github.com/指定主题)
- [WhiteSur 主题](https://github.com/vinceliuice/WhiteSur-gtk-theme)

## 故障排查

### 性能问题

```bash
# 查看 Shell 性能
gnome-shell --perf-tool

# 列出所有已启用扩展
gnome-extensions list --enabled
```



## 参考资料

- [GNOME Shell Extensions 官网](https://extensions.gnome.org/)
- [GNOME Tweaks 文档](https://wiki.gnome.org/Apps/Tweaks)
- [Fedora 桌面指南](https://docs.fedoraproject.org/en-US/fedora/latest/desktop/)