# Clash Verge 介绍

## 什么是 Clash Verge

Clash Verge（现维护版本称为 **Clash Verge Rev**）是一款基于 [Clash Meta（mihomo）](https://github.com/MetaCubeX/mihomo) 内核的跨平台代理客户端，使用 **Tauri** 框架构建，界面由 React 驱动。相比其他 Clash 客户端，它更轻量、现代，并持续维护。

> 原版 Clash Verge 已停止维护，当前活跃项目为社区 fork：**Clash Verge Rev**
> GitHub：https://github.com/clash-verge-rev/clash-verge-rev

---

## 核心特性

| 特性 | 说明 |
|------|------|
| 跨平台 | 支持 Windows、macOS、Linux |
| 轻量内核 | 基于 Clash Meta（mihomo）内核，功能强大 |
| 系统代理 | 一键开启/关闭系统代理，支持 PAC 模式 |
| 订阅管理 | 支持导入、更新、切换多个订阅配置 |
| 规则模式 | 支持规则模式、全局模式、直连模式 |
| TUN 模式 | 支持 TUN 虚拟网卡，实现真正全局代理 |
| 脚本支持 | 支持 JavaScript 脚本处理订阅配置 |
| 主题定制 | 支持深色/浅色模式及自定义主题颜色 |
| 多语言 | 支持中文、英文等多语言界面 |

---

## 界面功能模块

### 1. 代理（Proxies）
- 查看当前订阅中的所有代理节点
- 支持手动选择节点或自动选择最低延迟节点
- 显示节点延迟测试结果

### 2. 规则（Rules）
- 查看当前生效的分流规则列表
- 支持 DOMAIN、IP-CIDR、GEOIP 等多种规则类型

### 3. 连接（Connections）
- 实时查看所有当前连接
- 显示连接的目标地址、协议、规则命中情况
- 支持一键断开指定连接

### 4. 日志（Logs）
- 实时显示代理运行日志
- 便于排查连接问题

### 5. 设置（Settings）
- 系统代理设置
- TUN 模式配置
- 启动项、自动更新订阅
- 外观与语言配置

### 6. 订阅（Profiles）
- 导入远程订阅链接（URL）
- 导入本地配置文件
- 配置文件脚本预处理（Merge / Script）
- 定时自动更新订阅

---

## 支持的协议

Clash Meta 内核支持以下协议：

- **Shadowsocks (SS)**
- **ShadowsocksR (SSR)**
- **VMess**
- **VLESS**
- **Trojan**
- **Hysteria / Hysteria2**
- **TUIC**
- **WireGuard**
- **SOCKS5 / HTTP**

---

## 安装方式

### macOS
```bash
# 使用 Homebrew
brew install --cask clash-verge-rev
```

或从 GitHub Releases 下载 `.dmg` 文件手动安装。

### Windows
从 GitHub Releases 下载 `.exe` 或 `.msi` 安装包。

### Linux
从 GitHub Releases 下载 `.AppImage`、`.deb` 或 `.rpm` 包。

---

## 配置文件结构（YAML 示例）

```yaml
mixed-port: 7890
allow-lan: false
mode: rule
log-level: info
external-controller: 127.0.0.1:9090

proxies:
  - name: "节点1"
    type: vmess
    server: example.com
    port: 443
    uuid: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    alterId: 0
    cipher: auto
    tls: true

proxy-groups:
  - name: "PROXY"
    type: select
    proxies:
      - 节点1

rules:
  - GEOIP,CN,DIRECT
  - MATCH,PROXY
```

---

## 脚本预处理（Merge / Script）

Clash Verge 支持对订阅配置进行二次处理：

### Merge 模式
通过 YAML 合并追加或覆盖配置字段：
```yaml
rules:
  - "DOMAIN,example.com,DIRECT"
```

### Script 模式
使用 JavaScript 动态修改配置：
```javascript
function main(config) {
  config.rules.unshift("DOMAIN,example.com,DIRECT");
  return config;
}
```

---

## 与其他客户端对比

| 客户端 | 框架 | 内核 | 平台 | 特点 |
|--------|------|------|------|------|
| Clash Verge Rev | Tauri + React | Clash Meta | Win/Mac/Linux | 轻量现代，活跃维护 |
| Clash for Windows | Electron | Clash | Win/Mac | 功能完善，已停更 |
| ClashX | 原生 Swift | Clash / Meta | macOS | Mac 专属，轻量 |
| Hiddify | Flutter | Sing-box | 全平台 | 新兴，多协议 |

---

## 常见问题

**Q: TUN 模式需要管理员权限吗？**
A: 是的，TUN 模式需要管理员/root 权限才能创建虚拟网卡。

**Q: 订阅更新失败怎么办？**
A: 检查网络连接，或尝试关闭系统代理后再更新（避免循环代理）。

**Q: 如何备份配置？**
A: 配置文件默认存储在 `~/.config/clash-verge`（Linux/macOS）或 `%APPDATA%\clash-verge`（Windows）。

---

## 相关链接

- GitHub（Clash Verge Rev）：https://github.com/clash-verge-rev/clash-verge-rev
- Clash Meta 内核：https://github.com/MetaCubeX/mihomo
- 文档：https://clash-verge-rev.github.io/

---

*最后更新：2026-06-04*
