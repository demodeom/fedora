# Node.js

Fedora 43 Node.js 开发环境搭建指南

## 概述
本文档详细介绍在 Fedora 43 系统中使用 NVM (Node Version Manager) 搭建 Node.js 开发环境的完整流程，包括版本管理、镜像源配置等核心内容。

## Node.js 版本管理工具 NVM

### 什么是 NVM {id="nvm_1"}
NVM 是一个用于管理多个 Node.js 版本的命令行工具，允许开发者在不同项目间灵活切换 Node 版本，避免版本冲突问题。

### 安装 NVM {id="nvm_2"}

#### 下载安装脚本
从官方 GitHub 仓库获取最新安装脚本：

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

#### 加载环境配置
安装完成后，需要重新加载 Shell 配置文件使 NVM 命令生效：

```bash
# Bash 用户
source ~/.bashrc

# Zsh 用户
source ~/.zshrc
```

#### 验证安装
```bash
# 查看 NVM 版本，确认安装成功
nvm --version
```

## NVM 核心命令详解 {id="nvm_3"}

### Node.js 版本管理 {id="node-js_1"}

#### 安装 Node.js {id="node-js_2"}
```bash
# 安装最新 LTS 版本（推荐用于生产环境）
nvm install 24

# 安装指定具体版本
nvm install 20.18.0

# 安装最新稳定版
nvm install node
```

#### 卸载 Node.js {id="node-js_3"}
```bash
# 卸载指定版本
nvm uninstall 24

# 卸载多个版本（空格分隔）
nvm uninstall 20 22
```

#### 版本切换
```bash
# 临时切换到指定版本
nvm use 20

# 切换并使用指定版本（若未安装则自动安装）
nvm use --lts  # 切换到最新 LTS 版本
```

#### 默认版本设置
```bash
# 设置默认使用的 Node 版本（新终端窗口生效）
nvm alias default 24

# 设置默认使用 LTS 版本
nvm alias default 'lts/*'
```

### 版本查询命令

| 命令 | 功能说明 |
|------|---------|
| `nvm ls` | 查看本地已安装的所有 Node 版本 |
| `nvm ls-remote` | 查看所有可安装的 Node 版本（包括历史版本） |
| `nvm current` | 显示当前正在使用的 Node 版本 |
| `nvm which 20` | 查看指定版本 Node 的安装路径 |

## NPM 镜像源管理工具 NRM

### 什么是 NRM {id="nrm_1"}
NRM (NPM Registry Manager) 是一个用于管理和快速切换 NPM 镜像源的命令行工具，可有效提升依赖包下载速度。

### 安装 NRM {id="nrm_2"}

#### 通过 NPM 全局安装
```bash
# 标准安装
npm install nrm -g

# 若安装速度慢，可使用淘宝镜像加速安装
npm install nrm -g --registry=https://registry.npmmirror.com/
```

### NRM 核心命令 {id="nrm_3"}

#### 查看可用镜像源
```bash
nrm ls
```

执行后显示类似以下列表：
```
  npm ---------- https://registry.npmjs.org/
  yarn --------- https://registry.yarnpkg.com/
  tencent ------ https://mirrors.tencent.com/npm/
  cnpm --------- https://r.cnpmjs.org/
* taobao ------- https://registry.npmmirror.com/
  npmMirror ---- https://skimdb.npmjs.com/registry/
  huawei ------- https://repo.huaweicloud.com/repository/npm/
```
> **注**：带 `*` 号的为当前使用的镜像源

#### 切换镜像源
```bash
# 切换到淘宝镜像（国内推荐）
nrm use taobao

# 切换到官方源
nrm use npm
```

成功切换提示：
```
SUCCESS  The registry has been changed to 'taobao'.
```

#### 其他 NRM 命令 {id="nrm_4"}
```bash
# 测试各镜像源响应速度
nrm test

# 添加自定义镜像源
nrm add myregistry https://my.custom.registry.com/

# 删除镜像源
nrm del myregistry
```

## 开发环境验证

### 验证 Node.js 安装
```bash
# 查看 Node 版本
node --version

# 查看 NPM 版本
npm --version

# 验证 NPM 镜像源
npm config get registry
```

### 完整配置示例
```bash
# 1. 安装 LTS 版本并设为默认
nvm install 24
nvm alias default 24

# 2. 配置淘宝镜像源
npm install -g nrm
nrm use taobao

# 3. 验证配置
node -v
npm -v
npm config get registry  # 应显示 https://registry.npmmirror.com/
```

## 常见问题排查

### NVM 命令未找到
```bash
# 手动加载 NVM 脚本
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

### 安装 Node 速度慢
```bash
# 设置 NVM 使用国内镜像
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node
nvm install 24
```

### NRM 切换失败
```bash
# 手动设置 NPM 镜像源
npm config set registry https://registry.npmmirror.com/
```

## 最佳实践建议

1. **版本选择**：生产环境优先使用 LTS 版本，开发环境可使用最新版本体验新特性
2. **镜像配置**：国内开发者建议配置淘宝镜像源，显著提升依赖安装速度
3. **全局工具**：使用 `-g` 安装的全局工具（如 NRM）会跟随当前 Node 版本，切换版本后需重新安装
4. **项目本地化**：通过 `nvm use` 和项目级 `.nvmrc` 文件确保团队使用统一 Node 版本

## 参考资料
- [NVM 官方文档](https://github.com/nvm-sh/nvm)
- [NRM GitHub 仓库](https://github.com/Pana/nrm)
- [Node.js 官网](https://nodejs.org/)