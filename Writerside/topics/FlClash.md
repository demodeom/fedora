# FlClash

Fedora 43 FlClash 代理客户端安装指南

## 概述
FlClash 是一款基于 Clash.Meta 内核的图形化代理客户端，支持多种代理协议和规则配置。本文档详细介绍在 Fedora 43 系统中通过 RPM 包安装 FlClash 的完整流程。

## 环境准备

### 系统要求
- Fedora 43（或兼容的 RPM 发行版）
- AMD64 架构
- 具备 sudo 权限的用户

## 安装步骤

### 方法一：标准安装（推荐）

```bash
# 设置需要安装的 FlClash 版本
version="0.8.92"

# 从 GitHub Releases 页面下载对应版本的 RPM 包
wget https://github.com/chen08209/FlClash/releases/download/v${version}/FlClash-${version}-linux-amd64.rpm

# 使用 DNF 包管理器安装
sudo dnf install ./FlClash-${version}-linux-amd64.rpm
```

### 方法二：使用镜像加速（国内用户推荐）

若 GitHub 下载速度较慢，可使用镜像代理加速：

```bash
# 定义版本号
version="0.8.92"

# 通过 GitHub 代理镜像下载
wget https://gh-proxy.org/https://github.com/chen08209/FlClash/releases/download/v${version}/FlClash-${version}-linux-amd64.rpm

# 安装
sudo dnf install ./FlClash-${version}-linux-amd64.rpm
```

**可用镜像源备选**：
```bash
# 镜像源1：gh-proxy.org
wget https://gh-proxy.org/https://github.com/chen08209/FlClash/releases/download/v${version}/FlClash-${version}-linux-amd64.rpm

# 镜像源2：gh.api.99988866.xyz
wget https://gh.api.99988866.xyz/https://github.com/chen08209/FlClash/releases/download/v${version}/FlClash-${version}-linux-amd64.rpm

# 镜像源3：hub.fgit.cf
wget https://hub.fgit.cf/chen08209/FlClash/releases/download/v${version}/FlClash-${version}-linux-amd64.rpm
```

## 配置代理

### 基础配置步骤
1. **启动 FlClash** 后，在系统托盘找到图标
2. **导入订阅链接**：通过配置文件或订阅链接添加代理节点
3. **选择代理模式**：Rule（规则）、Global（全局）、Direct（直连）
4. **启用虚拟网卡**：开启后无需配置系统代理

## 自动化安装脚本

为方便重复安装，可创建自动化脚本：

```bash
#!/bin/bash
# FlClash 自动安装脚本

# 配置参数
VERSION="0.8.92"
MIRROR="https://gh-proxy.org"  # 可选镜像

# 下载并安装
echo "开始下载 FlClash v${VERSION}..."
wget ${MIRROR}/https://github.com/chen08209/FlClash/releases/download/v${VERSION}/FlClash-${VERSION}-linux-amd64.rpm

if [ $? -eq 0 ]; then
    echo "下载完成，开始安装..."
    sudo dnf install ./FlClash-${VERSION}-linux-amd64.rpm -y
    echo "安装完成！"
else
    echo "下载失败，请检查网络连接或版本号"
    exit 1
fi
```

保存为 `install-flclash.sh`，赋予执行权限后运行：
```bash
chmod +x install-flclash.sh
./install-flclash.sh
```

## 安全建议

1. **验证下载文件**：安装前检查文件完整性
   ```bash
   # 查看文件大小
   ls -lh FlClash-*.rpm
   
   # 可选：验证 GitHub Release 的 SHA256 校验和
   ```

2. **来源可靠性**：仅从官方 GitHub Releases 或可信镜像下载

3. **权限控制**：运行代理软件时注意最小权限原则

## 参考资料
- [FlClash GitHub 仓库](https://github.com/chen08209/FlClash)
- [Clash.Meta 文档](https://wiki.metacubex.one/)
- [DNF 包管理器文档](https://dnf.readthedocs.io/)